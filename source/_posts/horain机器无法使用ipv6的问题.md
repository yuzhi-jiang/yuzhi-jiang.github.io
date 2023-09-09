---
title: horain机器无法使用ipv6的问题
tags: linux
categories: []
date: 2022-10-12 23:31:22
---

## 前言

> 手上有一台horain的机器，有次有个需求需要用到ipv6，提交工单分配ipv6后却无法ping 通外网（ipv6）同时外网也ping不通主机，后来找到一遍文章解决了，不过后来又一次遇到问题，却记不得细节，找了很久才重新找到该文章，所以这里记录以下，以便以后再次遇到

如图：

![image-20221012233816828](https://raw.githubusercontent.com/yuzhi-jiang/imgbed/main/img/image-20221012233816828.png)

## 尝试一

> 根据ipw.cn 的做法尝试开启ipv6

```text
vim /etc/sysconfig/network-scripts/ifcfg-eth0
```

新增一行配置 `DHCPV6C=yes`, 可以自动获取 IPv6 地址。

![image-20221012234210471](https://raw.githubusercontent.com/yuzhi-jiang/imgbed/main/img/image-20221012234210471.png)

接下来，增加默认 IPv6 静态路由

```
vim /etc/sysconfig/network-scripts/route6-eth0
```

> 新增一行配置 `default dev eth0 via fe80::feee:ffff:feff:ffff`

以上为 为以后方便不用跳来跳去直接复制，以下贴出原地址和截图

[腾讯云 cvm 开启 IPv6](https://ipw.cn/doc/ipv6/server/tencent_cloud_cvm_ipv6.html)



![image-20221012234540393](https://raw.githubusercontent.com/yuzhi-jiang/imgbed/main/img/image-20221012234540393.png)

### 结果

![image-20221012235524436](https://raw.githubusercontent.com/yuzhi-jiang/imgbed/main/img/image-20221012235524436.png)

> 结果能查询到ipv6说明，说明dns应该是没问题的，但是卡在那里动不了





## 尝试二

期间也尝试过其他办法，都没有管用，最后在一篇文章《 [解决ubuntu下ipv6连接一段时间后就network is unreachable](https://www.polarxiong.com/archives/%E8%A7%A3%E5%86%B3ubuntu%E4%B8%8Bipv6%E8%BF%9E%E6%8E%A5%E4%B8%80%E6%AE%B5%E6%97%B6%E9%97%B4%E5%90%8E%E5%B0%B1network-is-unreachable.html)》找到大佬的分析和解决方案

> 其实应该就是ipv6的架构问题

大佬的排错步骤如下:

考虑到底是哪个地方网络断了

```
traceroute ipv6.google.com
```

显示:

```
network is unreachable
```

> 分析为不可达网关

尝试

```
ip -6 route show
```

在IPv6可用时显示

```
2001:250:5002:8100::/64 dev eth0  proto kernel  metric 256
fe80::/64 dev eth0  proto kernel  metric 256
default via fe80::ae85:3dff:feb2:c0b4 dev eth0  proto ra  metric 1024  expires 821sec
```

然后

```
route -6
```

显示

```
Kernel IPv6 routing table
Destination                    Next Hop                   Flag Met Ref Use If
2001:250:5002:8100::/64        ::                         U    256 0     0 eth0
fe80::/64                      ::                         U    256 0     0 eth0
::/0                           fe80::ae85:3dff:feb2:c0b4  UGDAe 1024 0     0 eth0
::/0                           ::                         !n   -1  1    19 lo
::1/128                        ::                         Un   0   1     5 lo
2001:250:5002:8100::/128       ::                         Un   0   1     0 lo
2001:250:5002:8100::3cee/128   ::                         Un   0   1    13 lo
fe80::/128                     ::                         Un   0   1     0 lo
fe80::ba27:ebff:fe10:41e4/128  ::                         Un   0   1    23 lo
ff00::/8                       ::                         U    256 1     0 eth0
::/0                           ::                         !n   -1  1    19 lo
```

那个`fe80::ae85:3dff:feb2:c0b4`就是默认网关了。

**然后等到IPv6不可用时再尝试上述命令，就再也找不到`fe80::ae85:3dff:feb2:c0b4`了，意味着默认网关消失了。**

**注意到`ip -6 route show`中默认网关后有`expires 821sec`，以及`route -6`中默认网关后的`UGDAe`，感觉找到问题在哪了。**

`Flag`中各个字母代表的意思：

```
UP U
GATEWAY G
REJECT !
HOST H
REINSTATE R
DYNAMIC D
MODIFIED M
DEFAULT d
ALLONLINK a
ADDRCONF c
NONEXTHOP o
EXPIRES e
CACHE c
FLOW f
POLICY p
LOCAL l
MTU u
WINDOW w
IRTT i
NOTCACHED n
```

`D`代表`DYNAMIC`，`e`代表`EXPIRES`。有些疑惑网关是固定地址，为啥连接时采用的有效时间为30分钟的动态形式

## 分析问题所在

`**既然是有时间限制，那就一定有更新时间的方法，而网关消失就应该是时间没有更新，症结就应该在如何让有效时间自动更新了**`

## 前面的情况和我基本一直，但作者后面的分析我确实不太懂，所以就原封贴下了（如果有大佬知道还请指点下）

> 直接上tcpdump抓包
>
> 抓包就不说了，东西太杂，不过关于IPv6的，倒是有`Route Advertize`和`ICMPv6`一直出现，根据之前折腾openwrt的ipv6的经验，应该就是这两个报文的问题了。
>
> 接下来就像是碰运气了，因为没有系统学习IPv6的细节，只能靠感觉找了。不过也算是我脑洞大开，直接往路由器方向找，毕竟那边折腾协议这种经验多些，很快找到了我感兴趣的：
>
> ```plain
> net.ipv6.conf.all.forwarding
> net.ipv6.conf.default.accept_ra
> ```
>
> 然后找到了这两个参数的详细介绍：
>
> forwarding
>
> Type: BOOLEAN
>
> Default: FALSE if global forwarding is disabled (default), otherwise TRUE
>
> Configure interface-specific Host/Router behaviour.
>
> Note: It is recommended to have the same setting on all interfaces; mixed router/host scenarios are rather uncommon.
>
> Value FALSE: By default, Host behaviour is assumed. This means:
>
> IsRouter flag is not set in Neighbour Advertisements.
> Router Solicitations are being sent when necessary.
> If accept_ra is TRUE (default), accept Router Advertisements (and do autoconfiguration).
> If accept_redirects is TRUE (default), accept Redirects.
> Value TRUE: If local forwarding is enabled, Router behaviour is assumed. This means exactly the reverse from the above:
>
> IsRouter flag is set in Neighbour Advertisements.
> Router Solicitations are not sent.
> Router Advertisements are ignored.
> Redirects are ignored.
>
> ------
>
> accept_ra
>
> Type: BOOLEAN
>
> Accept Router Advertisements; autoconfigure using them.
>
> Possible values are:
>
> 0: Do not accept Router Advertisements.
> 1: Accept Router Advertisements if forwarding is disabled.
> 2: Overrule forwarding behaviour. Accept Router Advertisements even if forwarding is enabled.
>
> 解决方法就出来啦！



## 解决方法

编辑sysctl.conf

```
vim /etc/sysctl.conf
```

将   `#net.ipv6.conf.all.forwarding=1`   注释去掉并添加 `net.ipv6.conf.eth0.accept_ra=2`  



文件如下：(如原本没有 `#net.ipv6.conf.all.forwarding=1` 不要死板，没有就自己加上)



![image-20221013001639954](image-20221013001639954-1665592928766-1.png)





然后重启，就解决问题了

![image-20221013001533514](https://raw.githubusercontent.com/yuzhi-jiang/imgbed/main/img/image-20221013001533514.png)



## 参考文章下：
[腾讯云 cvm 开启 IPv6](https://ipw.cn/doc/ipv6/server/tencent_cloud_cvm_ipv6.html)

 [解决ubuntu下ipv6连接一段时间后就network is unreachable](https://www.polarxiong.com/archives/%E8%A7%A3%E5%86%B3ubuntu%E4%B8%8Bipv6%E8%BF%9E%E6%8E%A5%E4%B8%80%E6%AE%B5%E6%97%B6%E9%97%B4%E5%90%8E%E5%B0%B1network-is-unreachable.html)
