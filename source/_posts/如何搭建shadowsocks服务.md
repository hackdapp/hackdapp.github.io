title: 如何搭建shadowsocks服务
date: 2016-06-09 20:21:39
tags:
---

#yum install epel-release 
#yum update 
#yum install python-setuptools m2crypto supervisor 
#easy_install pip 
#pip install shadowsocks

#vi /etc/shadowsocks.json

{
    "server":"0.0.0.0",
    "server_port":8388,
    "local_port":1080,
    "password":"yourpassword",
    "timeout":600,
    "method":"aes-256-cfb"
}



#vi /etc/supervisord.conf

[program:shadowsocks]
command=ssserver -c /etc/shadowsocks.json
autostart=true
autorestart=true
user=root
log_stderr=true
logfile=/var/log/shadowsocks.log


#echo “service supervisord start” >> /etc/rc.local

{
    "server":"0.0.0.0",
    "port_password":{
        "8388":"password1",
        "8389":"password2",
        "8390":"password3",
        "8391":"password4"
    },
    "timeout":300,
    "method":"aes-256-cfb"
}