---
title: 巧用HistoryApiAction实现对链数据的存储与查询
categories: EOS完全开发指南
url: eosdev_datastorage_historyaction
tags:
  - EOS完全开发指南
  - 合约数据存储
  - history action
  - inline action
keywords:
  - EOS完全开发指南
  - 合约数据存储
  - history action
  - inline action
description:
  - 在开发EOS DApp智能合约时，我们都知道可以使用`multi\\_index\_`来对合约数据的存储与查询，但合约的存储是需要消耗一定的资源的，而且随着用户的增长往往会导业务数据会越来越大，从而导致合约需要更多的资源来支撑其数据增长。那么是否有一种更好的方式来降低某些场景的资源消耗呢？那么，今天将为你分享一种数据存储方案： 借用EOS链提供的`history_api_action\_`插件、`内部合约Action调用`来完成对业务数据的存储与查询。HackDApp愿与你分享！
abbrlink: 63840
date: 2019-03-13 16:36:13
---

在开发EOS DApp智能合约时，我们都知道可以使用`multi_index `来对合约数据的存储与查询，但合约的存储是需要消耗一定的资源的，而且随着用户的增长往往会导业务数据会越来越大，从而导致合约需要更多的资源来支撑其数据。

有时我们可以通过业务设计，让业务数据得到即时清理及资源释放。但大多数情况下，业务数据是不允许清理的，那么针对这种情况是否有其他办法来降低对合约存储资源的消耗呢？

那么，今天分享的方案就是：借用EOS链提供的`history_api_action `插件服务、内部合约Action调用来完成对业务数据的存储与查询。

举个🌰️：
在去中心化交易所中，往往搓合成功会产生大量的成交订单，那保存在合约数据库中肯定是不合适的，所以可以在搓合方法中通过调用内部日志方法的形式，通过交易日志来将订单数据写入区块中；然后通过EOS节点提供的[查询历史action接口](https://developers.eos.io/eosio-nodeos/v1.4.0/reference#get_actions-1)，查询合约日志数据并增量同步到中心化数据库。

下面，我们将通过具体的代码示例来帮助大家理解整个过程：

----

**首先**，我们新创建一个合约(dexchange.hpp/dexchange.cpp)；
```bash
//dexchange.hpp
#include <eosiolib/eosio.hpp>
#include <eosiolib/print.hpp>

using namespace eosio;

CONTRACT dexchange : public contract {
  public:
    using contract::contract;
    dexchange(eosio::name receiver, eosio::name code, datastream<const char*> ds):contract(receiver, code, ds) {}

    [[eosio::action]]
    void executetrade(uint64_t pair_id, uint64_t sell_order_id, uint64_t buy_order_id);

    [[eosio::action]]
    void log(uint64_t deal_price, uint64_t quantity, uint64_t sell_order_id, uint64_t buy_order_id);

};

EOSIO_DISPATCH(dexchange, (executetrade)(log))

//https://gist.github.com/2b68242019242bdd12f174208e39e7d2

```
**然后**，定义并实现两个方法：executetrade、log。 `executetrade `合约方法负责搓合业务，当搓合业务处理完之后调用`log`方法，通过交易信息将参数调用数据写入区块中；

```bash
//filename: dexchange.cpp

#include "dexchange.hpp"

void dexchange::executetrade(uint64_t pair_id, uint64_t sell_order_id, uint64_t buy_order_id){
  uint64_t deal_price = 1200;
  uint64_t quantity = 10000;

  action(
      permission_level{ _self, "active"_n },
      _self, "log"_n,
      std::make_tuple(deal_price, pair_id, sell_order_id, buy_order_id)
  ).send();
}

void dexchange::log(uint64_t deal_price, uint64_t quantity, uint64_t sell_order_id, uint64_t buy_order_id){
  require_auth( _self );
}

//https://gist.github.com/ea6ec431a57faee3a2823cfeee406efd

```
从以上示例可以看出，合约log日志方法其实并不需要做任何业务逻辑处理。只需要间接被调用，便可将我们需要的业务数据通过交易的形式记录在区块中，而不会浪费我们的合约存储空间，也不需要担心资源释放的问题。

**下一步**、发布智能合约，并调用一次`executetrade`合约方法

```bash
//发布合约至hackdappexch合约帐户
> cleos set contract hackdappexch contracts/ -p  hackdappexch@active

//执行合约方法
> cleos push action hackdappexch executetrade '[1,2,3]' -p hackdappexch@active

```

**最后**，通过EOS链节点提供的[RPC服务](https://developers.eos.io/eosio-nodeos/v1.4.0/reference#get_actions-1)，进行历史action数据查询，通过数据过滤找到我们的日志方法及参数数据。

在确保之前的操作都成功之后，我们使用curl命令查询其对应服务：

```bash
curl --request POST \
  --url https://localhost:8888/v1/history/get_actions \
  --header 'content-type: application/x-www-form-urlencoded; charset=UTF-8' \
  --data '{"pos":-1,"offset":-10,"account_name":"hackdappexch"}'
```

通过此接口查询出来的数据不仅仅是`log`合约方法数据，可能还会存在该合约的其他方法事件，需要根据情况再过滤一次数据。

![](http://cdn.hackdapp.com/2019-03-13-080641.jpg)

**补充说明**

如果你在本地曾经搭建过EOS私链的化，那么或许看到过，EOS启动时是可以配置不同插件。而其中有一个插件`history_api_plugin `，就是用于监听并存储合约方法的调用信息；另外启动链节点时，是可以按规划自由指定所要监听的合约帐户及方法，如： `--filter-on hackdappexch::log `, 该参数配置表示只监听hackdappexch合约中的log方法。

```bash
nodeos -e -p eosio -d /mnt/dev/data \
  --config-dir /mnt/dev/config \
  --http-validate-host=false \
  --plugin eosio::producer_plugin \
  --plugin eosio::chain_api_plugin \
  --plugin eosio::http_plugin \
  --plugin eosio::history_api_plugin \
  --http-server-address=0.0.0.0:8888 \
  --access-control-allow-origin=* \
  --contracts-console \
  --filter-on hackdappexch:log: \
  --max-transaction-time=1000 \
  --verbose-http-errors &
```

如以上EOS节点启动命令，就展示了启动一个EOS节点的具体参数配置。其中，`--filter-on `参数便是指定只监听记录`hackdappexch `合约的`log `方法调用数据。

所以，当我们要通过链节点RPC服务查询合约方法历史调用数据时，需要先确认提供RPC服务的节点是否开启了`history_api_plugin`插件，以及自己所要查询的合约是否在其过滤规则之中。

----
**小结**

通过本篇文章，我们学会了利用**内部合约方法调用** ➕️ **链节点历史Action查询**的方式实现业务数据的另一种数据存储与查询方案。

----

> 在教程中如出现错误🐛或不易理解的知识点，欢迎加我微信指正!
> Name: zhangliang | WeChat: rushking2009 | Mail: zhangliang@cldy.org

<img class="nofancybox" src="http://cdn.hackdapp.com/2019-03-11-IMG_1625.JPG-blog" style="display: inline;"><img class="nofancybox" src="http://cdn.hackdapp.com/2019-03-11-IMG_1626.jpg-blog" style="display: inline; ">

----

### **changelog**
2019-03-13 [zhangliang](mailto:zhangliang@cldy.org)
  - 初次发稿
