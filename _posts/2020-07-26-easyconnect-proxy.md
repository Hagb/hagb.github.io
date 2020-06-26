---
layout: post
title:  "记折腾容器化 EasyConnect 的全局透明代理"
date:   2020-06-26 17:30:00 +0800
---
> 也算是水了第一篇技术博客吧……鄙人不是计算机科班出生，计算机方面的东西只是业余爱好。如果有什么不对或者可以改进的地方，望读者（如果有的话）不吝赐教！

前一阵子折腾 [EasyConnect 的容器化](https://github.com/Hagb/docker-easyconnect)，用来连接学校内网。当时是在 Docker 容器内运行 EasyConnect（以下简称 EC），同时开一个 Socks5 代理服务供宿主机使用。

后来又有了使用 EC 全局透明代理的需要，最直接的方法就是使用`--net host`参数让容器直接在宿主机中设置网络。但是 EC 会把路由表设得很乱，不方便管理（而且还容易产生冲突）。

也可以从容器中开出 PPTP 服务，然后在宿主机中使用 NetworkManager 连接，但是这样

1. 需要专门设置 EC 容器的路由指向物理网卡以防止路由回环，并且网络环境变更之后又需要重新设置；
2. 在镜像中加入 PPTP 服务端会使镜像体积增大，使用 PPTP 也会占用一定的资源。

于是改变思路，将容器作为一个网关来转发宿主机（lo）发出的流量到 EC。

## 结果

每次开机后，运行一次，即可设置 EC 全局透明代理

``` shell
{% raw %}#!/bin/bash
NAME='easyconnect'
NET='svpn'

docker network create "$NET"

xhost +LOCAL:
docker run -d --network svpn --rm --name "$NAME" --device /dev/net/tun \
    --cap-add NET_ADMIN -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v $HOME/.Xauthority:/root/.Xauthority -e DISPLAY=$DISPLAY \
    -e TYPE=x11 -v $HOME/.ecdata:/root -p 127.0.0.1:1080:1080 \
    hagb/docker-easyconnect
while docker exec $NAME [ ! -d /sys/class/net/tun0 ]
do
sleep 5
done
docker exec "$NAME" iptables -t nat -A POSTROUTING -o tun0 -j MASQUERADE

IP="$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' "$NAME")"
MTU="$(docker exec "$NAME" cat /sys/class/net/tun0/mtu)"

sudo ip route flush table 3
sudo ip route add table 3 default via "$IP" mtu "$MTU"
sudo ip rule add iif lo table 3 {% endraw %}
```

然后路由是这样的：

```
(EC in docker)    [tun0] <---- route table of EC <----------+
===[svpn]===========|============|==========================|=====
                    |            |                  table 3 |
(host)              V            |                      request(lo)
               tasble main <-----+
                    V
                物理网卡
```

不走 EC 的流量可以在路由表 table 3 中定义。

或者亦可将`sudo ip route add table 3 default via "$IP" mtu "$MTU"`代之以需要走 EC 的流量的路由。

如果要开热点或者用网线共享 EC 网络，那么还需要运行（以下以共享网络所用网卡名为`wlp2s0`，网段为`10.42.0.0/24`为例）

```
sudo ip route add table 3 10.24.0.0/24 dev wlp2s0
sudo ip rule add iff wlp2s0 table 3
```

## 折腾过程中的坑

### MTU

其实折腾过程中耗费的大部分时间都是因为 MTU。在笔者折腾的环境下，EC 容器中创建出来的虚拟网卡`tun0`的 MTU 为 1400，而其他各个虚拟或实体的网卡默认 MTU 均是 1500。一开始是进行试配置之后使用`www.baidu.com`的 ip 地址`182.61.200.6`来验证，但因为`tun0`的 MTU 小于默认的 MTU，因此一直无法连上（详细地说是能够建立连接，但无法完成握手过程），只能反复试验。

后来发现许多别的网站能够访问，但访问速度和容器内明显有差异，而 <http://www.baidu.com> 则完全无法访问。仔细排查，发现 MTU 不一致的情况，于是尝试将`tun0`的 MTU 值和路由表，问题解决。
