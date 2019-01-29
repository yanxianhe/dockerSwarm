#### docker 之 swarm

##### 快速安装docker

~~~~~~~~
 > https://get.docker.com/

curl -fsSL https://get.docker.com -o get-docker.sh

sh get-docker.sh
~~~~~~~~




##### 安装docker 后初始化 swarm
初始化 swarm 指定网卡
~~~~~~~~
docker swarm init --advertise-addr em2
或
docker swarm init --advertise-addr 10.0.254.100
~~~~~~~~
##### 查看 swarm token

~~~~~~~~
docker swarm join-token worker
~~~~~~~~
##### 如果有个node 每个node上都执行 
~~~~~~~~
docker swarm join --token .....
~~~~~~~~

##### 使用swarm 查看信息
~~~~~~~~
docke info #docker info来查看当前swarm集群的状态
docker node ls #运行docker node ls来查看节点信息
~~~~~~~~

##### 创建服务

创建服务后节点ip+port都可以访问
~~~~~~~~
docker service create --name nginx --replicas 3 --publish 8080:80 nginx:1.15
~~~~~~~~

##### 指定在固定node上运行
https://docs.docker.com/engine/reference/commandline/service_create/

> Specify service constraints (--constraint)
~~~~~~~~
docker service create --name nginx --replicas 2 --constraint node.hostname==vms4 nginx:1.15
~~~~~~~~

##### 服务升级(更新)

服务，配置10s的更新间隔
~~~~~~~~
docker service create --replicas 2 --name nginx --update-delay 10s nginx:1.15
~~~~~~~~
##### 查看服务状态
~~~~~~~~
docker service  inspect  --pretty nginx
~~~~~~~~
##### 查看服务在那个节点上运行
~~~~~~~~
docker service ps nginx
~~~~~~~~


##### 更新镜像
~~~~~~~~
docker service update --image nginx:1.15+ nginx
~~~~~~~~

###### 查看服务 
~~~~~~~~
docker service ls #查看当前服务
~~~~~~~~

###### 查看删除服务 
~~~~~~~~
docker service rm nginx #删除服务 .根据服务名删除
~~~~~~~~

#### 在swarm集群上部署一个服务

#### Node Label 管理
##### [vms1]节点操作
[升级]
~~~~~~~~
docker node promote vms1
~~~~~~~~

[降级] 
~~~~~~~~
docker node demote vms1
~~~~~~~~

###### 添加标签

key 为 yxh value nginx
~~~~~~~~
docker node update --label-add yxh=nginx node1
~~~~~~~~
###### 删除标签
~~~~~~~~
docker node update --label-rm yxh node1
~~~~~~~~
###### 查看node上标签信息
~~~~~~~~
docker node inspect vms0
~~~~~~~~


