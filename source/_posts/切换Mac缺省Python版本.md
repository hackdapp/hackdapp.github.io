title: 切换Mac缺省Python版本
date: 2015-11-05 11:09:16
tags:
---
### 1. 进入用户目录

        >>> cd ~
        
### 2. 创建.bash_aliases文件并设置别名

        >>> touch .bash_aliases
        
编辑.bash_aliases   
        
        alias python='python3'
        
### 3. 重新打开命令窗口测试python版本
     
        python --version

---
`注：` 如若每次进行命令窗口发现此文件未生效，可进行如下设置:

- 情况一: 命令窗口为bash, 那么把`. ~/.bash_profile`追加到`~/.bashrc`末尾;
- 情况二: 命令窗口为zsh, 那么把`. ~/.bash_profile`追加到`~/.zshrc`末尾.