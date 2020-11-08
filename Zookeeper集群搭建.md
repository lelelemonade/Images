# 前期准备

多台能互相ping通的主机(docker容器/虚拟机/云服务器均可)，下面以三台云服务器为例

[zookeeper安装包](http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.6.2/apache-zookeeper-3.6.2-bin.tar.gz)

# 搭建Java环境

`sudo apt install openjdk-8-jdk`

或者

`yum install java-1.8.0-openjdk`

以上在三台主机均要配置

# 安装zookeeper

以下操作都在/root/zookeeper目录下执行，也以此目录为例

### 将之前的zookeeper安装包解压

`tar -xzvf apache-zookeeper-3.6.2-bin.tar.gz`

### 准备一个数据目录

`mkdir data`

### 切入解压缩的出来的zookeeper目录

`cd apache-zookeeper-3.6.2-bin/`

### 将模板配置文件copy成实际读取的配置文件

`cp conf/zoo_sample.cfg conf/zoo.cfg`

### 修改配置文件

`vim conf/zoo.cfg`

##### 必要配置

修改

```bash
dataDir=/root/zookeeper/data
```
添加
```bash
server.1=localhost:2888:3888
server.2=localhost:2888:3888
server.3=localhost:2888:3888
```
将localhost改为三台主机的ip地址，这段配置里1、2、3用来标识三台主机，如果有更多主机请继续添加

##### 可选配置

tickTime：集群主机间互相发送心跳的周期，数值为ms
clientPort：客户端连接的端口
initLimit：允许follower连接并同步到Leader的初始化连接时间，数值为tickTime的倍数
syncLimit：如果follower在设置时间内不能与leader通信，那么此follower将会被丢弃，数值为tickTime的倍数

quorumListenOnAllIPs：设置为true表示接受公网ip，如果你和我一样有这种蠢蠢的需求的话

### 在dataDir目录添加myid文件，

三台主机的内容为1、2、3，这是主机间的标识符

```bash
echo 1 > /root/zookeeper/data/myid
```
三台主机的配置只有这一步不同
# 配置zookeeper

# 启动服务并检查状态

在三台主机执行以下命令启动zookeeper，出现以下截图表示集群启动成功

`./bin/zkServer.sh start`

![](https://raw.githubusercontent.com/lelelemonade/Images/main/Screen%20Shot%202020-11-08%20at%201.28.48%20AM.png)

在三台主机执行以下命令，检查zookeeper集群状态

 `./bin/zkServer.sh status`
 ![](https://raw.githubusercontent.com/lelelemonade/Images/main/Screen%20Shot%202020-11-08%20at%201.33.59%20AM.png)

![](https://raw.githubusercontent.com/lelelemonade/Images/main/Screen%20Shot%202020-11-08%20at%201.32.52%20AM.png)


以上是主从节点状态截图

