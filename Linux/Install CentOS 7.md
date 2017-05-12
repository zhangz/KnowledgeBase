## 安装CentOS 7.0

http://www.osyunwei.com/archives/7829.html

#### 设置IP地址、网关DNS

CentOS 7.0默认安装好之后是没有自动开启网络连接的。

1.  进入网络配置文件目录 `cd /etc/sysconfig/network-scripts/` 

    `vi ifcfg-ens192`  编辑配置文件，添加修改以下内容：

```
TYPE=Ethernet
BOOTPROTO=static #启用静态IP地址
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens192
UUID=c150d939-b376-4b60-9504-f6133afc4a48
DEVICE=ens192
ONBOOT=yes #开启自动启用网络连接
HWADDR=00:50:56:b4:20:c3
IPADDR=10.23.179.87 #设置IP地址
NETMASK=255.255.255.0 #设置子网掩码
GATEWAY=10.23.179.254  #设置网关
ZONE=
```

*Centos下查看网卡Mac地址`HWADDR`，输入命令:*

*`ifconfig -a`*

*ens192 ether 00:e4:56:2E:D8:20*
*00:e4:56:2E:D8:20即是MAC地址。*

2.  重启网络 `service network restart `

`ping www.baidu.com` 测试网络是否正常

`ip addr `查看IP地址

3.  配置DNS `vi /etc/resolv.conf`

    ```
    nameserver 10.23.134.7
    nameserver 10.23.134.9
    ```

4.  **设置主机名**

    编辑配置文件 `vi /etc/hosts ` 增加 `10.23.163.106 机器名`

5.  重启系统 `shutdown -r now`