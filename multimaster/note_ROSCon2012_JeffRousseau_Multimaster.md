# Multi-Master/Multi-Robot ROS, Present and Future
> by HoriSun   
> 
> 这是对 Aptima Inc 的 Jeff Rousseau 在 ROSCon2012 上演讲的简单笔记。当时他们刚开始尝试做实验性的 Multimaster，虽然做了一些 demo，但通用性和可靠性仍然难以保证。Jeff 同时是 ROS 的 Multi-Master Special Interest Group (MM-SIG) 的成员。  
> 
> 目前主要可见到的 multimaster 实现有由 rocon 主导开发的 rocon\_multimaster，以及由 FKIE 开发的 multimaster\_fkie。


## 1. 多机器人系统：通讯能力的局限

理想的多机器人系统：网络从不断线，机器永不宕机，带宽无限，延迟无存。   
醒醒，骚年。
  - 小车可能会没电
  - 小车跑出WiFi覆盖范围
  - 带宽拥挤： TF, 点云，地图， costmap， 视频数据

目前 ROS （先不考虑 ROS 2.0，那还没开发完） 主要使用 TCP 通讯（ROS 的 C++ API 提供 UDP 通讯功能，但不太好用）。在某些场景下，TCP 并不是最佳选择：  
  1. TCP 主要保证可靠性，却不能保证即时性。对于音视频传输和遥控，像 UDP 这类不强调可靠性的反而更合适；
  2. TCP 为一对一通讯，当场景中需要一对多通讯（一机发多机收）时，使用多播（multicast）协议如 PGM 等更合适。
    
    
## 2. ROS master 是什么  ~~道是什么~~

- 一个 XMLRPC 服务器
- 与 parameter server 同时启动
- 以目录方式管理资源（topic / service / param），类似 DNS
  - 对于一个 topic，其 advertiser 和 subscriber 会在 ROS master 处注册以供查询
  - subscriber 往 master 发送注册请求，返回的信息找到 advertiser 并获取数据
  - 对于 service 而言， client 从 master 获取的是 service 的 URI

由于这种类 DNS 机制，ROS 存在一个问题：当 roscore 被单独关闭后，连接同一 topic 的发布 node 和订阅 node （在已经建立连接的情况下）依然能正常工作，数据仍然能被发送和接收。若此时再开一个新的 roscore（也就是新的 master）, 这些节点不会被 master 所知， rosnode kill -a 也无法关闭它们。
    
换言之，对于一个 topic, 它的 subscriber 和 publisher 是各自单独建立连接的。 master 只担任资源注册/查找的工作。**_一旦 master 关闭/掉线_**：
  - ROS service 和 parameter 无法获取， topic 的 subscriber 无法找到 publisher
  - 已有的 topic / node 等列表亦无法查询
  - 若新开 master (roscore/roslaunch)，已有的 node 会被新开的 master 孤立 



## 3. 从一到多：为何需要运行多个 master？

ROS 自身有在网络里多机共享单一 master 的能力，但在某些场景下我们需要允许多个 master 共存并进行通讯：
  1. 各自拥有 master 的多台机器人相互通讯
  2. 一台建筑内有多台移动机器人，一个或多个同样运行 ROS 的管理设备（building manager 或 fleet manager）为它们提供必要的局部信息和进行调度。机器人连接到新的管理设备以获取诸如地图和任务请求等信息。
    
为什么不用一个 master? 首先机器人需要能够独立运行，其次，即使所有机器是在同一楼层里，但无线网络连接的可靠性和覆盖范围也是一个问题。

在机器人移动到新地点连接新固定设备的场景中，**固定设备上运行的 ROS master 会有不同于机器人上的配置（如新的参数/服务），而这些配置在机器人启动的时候是不存在的**。

如果是唯一 master 架构，在机器人脱离 master 的无线连接范围后，机器人上的 node **能否顺利地发现连接断开并正常终止**也是一个问题。因为在获取所用信息后，这些 node 不会再去关心 master 的死活。

基于这些原因，可以考虑采用多 master 方案：一个可靠的本地 master 加上对外部 master 的自动发现与连接机制。


## 4. Foreign Relay 

Foreign Relay 是一个命令行工具，用于将运行 master 的两台机上中继消息。

```
$ rosrun foreign_relay foreign_relay.py (adv|sub) topic foreign_master_uri
```

事实上 Foreign Relay 没有使用 rospy 库，而是直接通过 XML-RPC 直接与 master 通讯，因此绕过了 Python/C++ ROS API 下 node 必须绑定一个 master 的限制。

foreign_relay 的限制：
  - Node 无法知道外部 master 是否掉线（资源注册不会超时，见 TTL issue #682 (???)）
  - 不能以可编程的方式部署
  
    静态 launch 文件是个问题。整个系统的状态在配置时就要决定好，不能在运行时修改
    
    在这种情况下， 使用网络发现协议如 zeroconf 等就变得困难
  - 可以 hack （比如 catch exception 以判断）但显然不优雅也不合适，后续会难以维护
  
  
  
  
  
つづく
  
  
