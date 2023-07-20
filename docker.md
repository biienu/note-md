# docker 安装mysql

```
docker run -p 3306:3306 --name mysql -v /mydata/mysql/log:/var/log/mysql -v /mydata/mysql/data:/var/lib/mysql -v /mydata/mysql/conf:/etc/mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:8.0.27
```

# docker 安装redis

```
mkdir -p /mydata/redis/conf
touch /mydata/redis/conf/redis.conf

docker run -p 6379:6379 --name redis -v /mydata/redis/data:/data -v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf -d redis redis-server /etc/redis/redis.conf
```

# docker 安装nacos

```shell
docker run -p 8848:8848 -p 9848:9848 -p 9849:9849 -v ./nacos/logs/:/home/nacos/logs -v ./nacos/data/:/home/nacos/data --name nacos -d nacos/nacos-server:2.0.2
```



# docker常用命令

```tex
docker version    # 显示 docker 的版本信息
docker info       # 显示 docker 的详细信息
docker 命令 --help # 帮助命令
```



# 镜像命令

docker images 查看所有镜像



docker serarch 搜索镜像

```shell
docker search 镜像 -f 过滤
```

docker pull 下载镜像

```shell
docker pull mysql #下载最新版本
docker pull mysql:5.7  # 下载指定版本
```

docker rmi 删除镜像

```shell
docker rmi 镜像
docker rmi -f 强制删除
docker rmi -f 镜像 $(docker images -aq) 全部删除
```

# 容器命令

说明: 有了镜像才可以创建容器

```shell
#下载个centos镜像
docker pull centos
```



```shell
# 新建容器并启动
docker run [可toptions] 镜像id

# 参数说明
--name "name" 容器名字 
-d             后台方式运行
-it             使用交互方式运行
-p             指定容器端口 
	-p ip:主机端口:容器端口
	-p 主机端口:容器端口
	-p 容器端口
	容器端口
-P  随机映射端口

# 从容器退出
exit 

# 列出正在运行的容器
docker ps -a  # 列出正在运行的容器（包括历史运行）
-n=?   # 显示最近创建的容器，-n=2 显示最近2个容器
-q   # 只显示容器的编号 

# 退出容器不停止运行
ctrl + p + Q


# 删除容器 
docker rm 容器id
docker rm -f $(docker ps -aq)   # 删除所有容器
docker ps -a -q|xargs docker rm # 删除所有容器

# 启动和停止容器的操作
docker start 容器id    # 启动容器
docker restart 容器id  # 重启容器
docker stop 容器id	 # 停止容器
docker kill 容器id	 # 杀掉容器

```

# 查看日志

```shell
docker logs 

docker logs -tf --tail=n 
```

# 查看容器中的进程信息

```shell
docker top 容器id
```

# 查看镜像的元数据

```shell
docker inspect 容器id
```

# 进入当前正在运行的容器

```shell
# 通常容器都是使用后台方式运行的，需要进行容器修改一些配置

# 命令:
docker exec -it 容器id /bin/bash    # 以交互方式进入容器
docker attach 容器id  

# 区别
# docker exec     # 进入容器后开启一个新的终端（常用）
# docker attach   # 进入容器正在执行的终端，不会启动新的进程
```

# 从容器内拷贝文件到主机上

```shell
docker cp 容器id:容器路径 主机路径
```

# 服务器重启时开启容器

```shell
docker update --restart=always mysql
```



# docker stats 容器id 查看这个容器消耗cpu情况

原   $2a$10$EuWPZHzz32dJN7jexM34MOeYirDdFAZm2kuWj7VEOJhhZkDrxfvUu



原   $2a$10$EuWPZHzz32dJN7jexM34MOeYirDdFAZm2kuWj7VEOJhhZkDrxfvUu



新   $2a$10$luWQ0k4VCAIlrJ.37hBpvORO71eGzbBQ/oJHEKzDjd38FDAj0ptgy

$2a$10$nTKpduMtz3r7aH4DDJMfOOaNghIw7Sm.nGEbIw0YUoHDRU8fUA7le

# commit 自己的镜像

```sehll
docker commit 提交容器成为一个新的版本

# 命令
docker commit -m "提交信息描述" -a "作者" 容器id 目标镜像名[:tag]

```



# 数据卷

## 使用数据卷

> 命令

```shell
# -v 数据卷挂载
docker run -it -v Linux目录:容器目录 镜像名字

# 官方下载mysql
# 解释说明
# -e (environment)  用于配置环境
 docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```



# 具名和匿名挂载

```shell
# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx

# 查看所有 volume(卷)的情况
docker volume ls

# 所谓匿名挂载就是在 -v 时只写了容器内路径，没有写容器外路径


# 具名挂载
[root@VM-4-16-centos ttt]# docker run -d -P --name nginx02 -v juming-nginx:/etc/
nginx nginx
a2602d74fd2f677ef6ed81d0a58dac9476a37688c6ed59df5d1f8eca464c883b

#################################################
[root@VM-4-16-centos ttt]# docker volume ls
DRIVER    VOLUME NAME
local     177e907eaa230940b1f5628d8fba470bd67f201c218c162e9f8423fd58f9936c
local     juming-nginx
[root@VM-4-16-centos ttt]#

# 查看具名挂载的地址:
[root@VM-4-16-centos ttt]# docker volume inspect juming-nginx
[
    {
        "CreatedAt": "2022-06-19T15:42:34+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]

```

所有的docker 容器内的卷，没有指定目录的情况下都是在`/var/lib/docker/xxx/_data`

我们可以通过具名挂载可以方便的找到我们的一个卷，大多数情况使用**具名挂载**

```shell
# 如何区分具名挂载和匿名挂载，还是指定路径挂载
-v 容器内路径   	# 匿名挂载
-v 卷名:容器内路径	# 具名挂载
-v /宿主机路径:容器内路径   # 指定路径挂载
```

**拓展：**

```shell
# 通过 -v 容器内路径:ro rw 改变读写权限 
ro     readonly  # 只读
rw     readwrite # 可读可写
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 juming-nginx:/etc/nginx:rw nginx

# ro 只要看到这个，只能通过宿主机来操作，不能在容器内修改
```

# dockerFile 的指令



**指令必须大写，指令从上到下执行**

```shell
FROM 		# 基本镜像，一切从这里开始构建 
MAINTAINER	# 镜像是谁写的，姓名+邮箱
RUN			# 镜像构建的时候需要运行的命令
ADD			# 步骤，tomcat镜像，这个tomcat压缩包, 添加内容
WORKDIR 	# 镜像的工作目录
VOLUME		# 挂载的目录
EXPOSE		# 暴露端口配置
CMD			# 指定这个容器启动时候要运行的命令，(相同命令)只有最后一个会生效，可被替代
ENTRYPOINT	# 指定这个容器启动时候要运行的命令，可以追加命令
ONBUILD		# 当构建一个被 继承 DockerFile 这个时候就会运行 ONBUILD 的指令，触发指令
COPY		# 类似 ADD ，将我们文件拷贝到镜像中  
ENV			# 构建的时候设置环境变量
```



# Dockfile 创建镜像

dockerfile 是用来构建docker镜像的文件！命令参数脚本！

构建步骤：

1、编写一个dockerfile文件

2、docker build 构建成为一个镜像

3、docker run 运行镜像

4、docker push 发布镜像(DockerHub、阿里云镜像仓库)



```shell
# 1、 编写一个自己的DockerFile的文件
# 文件内容:
FROM centos
MAINTAINER biienu<biienu@163.com>
ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "-----end-----"
CMD /bin/bash

# 2、构建命令
docker build -f dockerfile文件 -t 镜像名:[tag] .   # 注意：最后一个点
```

# docker history 镜像id 查看这个镜像创建记录





# 提交镜像到dockerHub或阿里云

```shell
# docker tag 命令:  给一个镜像设置tag
docker tag 镜像id 镜像名字:[tag]

# docker login -u : 登录到dockerhub
-u 用户名

# docker push 镜像    提交镜像到DockerHub
```



# docker网络



![image-20220714094719992](D:/ProgramFiles/typora/typora-images/image-20220714094719992.png)



![image-20220713214145883](D:/ProgramFiles/typora/typora-images/image-20220713214145883.png)



```shell
docker run -d -P --name tomcat01 tomcat


# 查看容器的内部网络地址
# 进入正在运行的容器 + ip addr 
docker exec -it 容器id ip addr
============================================
 "Networks": {
     "bridge": {
         "IPAMConfig": null,
         "Links": null,
         "Aliases": null,
         "NetworkID": "9e3241f418e9c02f976e1a
fe30f638365ec",
         "EndpointID": "e0c71cce10919a67fd5e6
9cb4f4cce4cba6",
         "Gateway": "172.17.0.1",
         "IPAddress": "172.17.0.2",
         "IPPrefixLen": 16,
         "IPv6Gateway": "",
         "GlobalIPv6Address": "",
         "GlobalIPv6PrefixLen": 0,
         "MacAddress": "02:42:ac:11:00:02",
         "DriverOpts": null
============================================

# linux 可以ping通容器内部

```

> 每启动一个docker 容器, docker就会给docker 容器分配一个ip，我们只要装了docker，就会有一个docker0网卡。
>
> docker 采用的是 桥接模式，使用的技术是 **evth-pair**



![image-20220714101509815](D:/ProgramFiles/typora/typora-images/image-20220714101509815.png)



再启动一个`tomcat02`

tomcat01 的ip地址

![image-20220714101821569](D:/ProgramFiles/typora/typora-images/image-20220714101821569.png)





![image-20220714102106676](D:/ProgramFiles/typora/typora-images/image-20220714102106676.png)



tomcat01 和 tomcat02 是否可以ping通？

容器和容器之间可以ping通。



### --link 通过容器名访问

```shell
#启动一个tomcat03
[root@ls dockerfile-test]# docker run -d -P --name tomcat03 diytomcat
dd1af1c6e44fc009c6bbac402e007930fa884a38c310997d841208d9c63c8f63

# 启动一个tomcat04 并 --link tomcat03
[root@ls dockerfile-test]# docker run -d -P --name tomcat04 --link tomcat03 diytomcat
7f2ec8899976e8e1fbf2f6ef54c6b56a14068a45d38537eb44cda19da6662b62

#tomcat04 ping tomcat03
[root@ls dockerfile-test]# docker exec -it tomcat04 ping tomcat03
PING tomcat03 (172.17.0.4) 56(84) bytes of data.
64 bytes from tomcat03 (172.17.0.4): icmp_seq=1 ttl=64 time=0.109 ms
64 bytes from tomcat03 (172.17.0.4): icmp_seq=2 ttl=64 time=0.059 ms

# 发现tomcat04 可以通过tomcat03ping通，但是tomcat03 不能通过tomcat04ping通。
```



**为什么tomcat03不能ping通tomcat04?**

以下为tomcat04的hosts文件，可以看到 这个信息：

`172.17.0.4      tomcat03 dd1af1c6e44f`

```shell
[root@ls dockerfile-test]# docker exec -it tomcat04 cat /etc/hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.17.0.4      tomcat03 dd1af1c6e44f
172.17.0.5      7f2ec8899976

```



### 自定义网络

**网络模式**：

* bridge : 桥接模式       		docker
* none   : 不配置网络
* host    :  和宿主机共享一个网络
* container:  



![image-20220714142755904](D:/ProgramFiles/typora/typora-images/image-20220714142755904.png)



docker0 特点，默认，不支持域名访问，可以通过--link



**创建网络**：

```shell
[root@ls ~]# docker network create --driver bridge --subnet 192.168.0.0/24 --gateway 192.168.0.1 myself-network

[root@ls ~]# docker network inspect 9786ae72b16a
[
    {
        "Name": "myself-network",
        "Id": "9786ae72b16a1547834a6297948568923514b3c9a72fca79c11b3c8b
46f456d6",
        "Created": "2022-07-14T14:32:00.176010044+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/24",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
[root@ls ~]#
```



**使用自己创建的网络**：

```shell
[root@ls ~]# docker run -d -P --name tomcat-net01 --net myself-network diytomcat
6d3fdc18703a2eabf54a9d58ba0bce02ec8214cd0b510300d5d24262a82dde6b

[root@ls ~]# docker run -d -P --name tomcat-net02 --net myself-network diytomcat


# tomcat-net01 和tomcat-net02 可以ping通
[root@ls ~]# docker exec -it tomcat-net01 ping tomcat-net02
PING tomcat-net02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat-net02.myself-network (192.168.0.3): icmp_seq=1 ttl=64 time=0.071 ms
64 bytes from tomcat-net02.myself-network (192.168.0.3): icmp_seq=2 ttl
```



**网络联通**

![image-20220714145458036](D:/ProgramFiles/typora/typora-images/image-20220714145458036.png)



```shell
[root@ls ~]# docker network connect myself-network tomcat01


[root@ls ~]# docker exec -it tomcat01 ping tomcat-net01
PING tomcat-net01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat-net01.myself-network (192.168.0.2): icmp_seq=1 ttl=64 time=0.085 ms
64 bytes from tomcat-net01.myself-network (192.168.0.2): icmp_seq=2 ttl=64 time=0.060 ms

```



![image-20220714145833283](D:/ProgramFiles/typora/typora-images/image-20220714145833283.png)





# ####docker compose####



官方文档：[Overview of Docker Compose | Docker Documentation](https://docs.docker.com/compose/)

# docker compose是什么？

![image-20220809205034605](D:/ProgramFiles/typora/typora-images/image-20220809205034605.png)



# docker compose使用的三个步骤：

**使用 Compose 基本上有三个步骤:**

1. 用 Dockerfile 定义应用程序的环境，这样就可以在任何地方复制它。

2. 在 docker-compose. yml 中定义组成应用程序的服务，以便它们可以在一个独立的环境中一起运行。

3. 运行 Docker 组合，Docker 组合命令启动并运行整个应用程序。您可以选择运行 docker-Compose 使用 Compose 独立(docker-Compose 二进制)。



docker-compose.yml文件类似这样：

```yaml
version: "3.9"  # optional since v1.27.0
services:  
  web:
    build: .
    ports:
      - "8000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
  
# services的配置：
# https://docs.docker.com/compose/compose-file/compose-file-v3/#service-configuration-reference
```



![image-20220930125512253](D:/ProgramFiles/typora/typora-images/image-20220930125512253.png)



![image-20220809205519214](D:/ProgramFiles/typora/typora-images/image-20220809205519214.png)

![image-20220930132627378](D:/ProgramFiles/typora/typora-images/image-20220930132627378.png)

![image-20220930132705828](D:/ProgramFiles/typora/typora-images/image-20220930132705828.png)



![image-20220930132920416](D:/ProgramFiles/typora/typora-images/image-20220930132920416.png)







# 安装docker-compose：



官网安装教程(Linux)：[Install Docker Compose CLI plugin | Docker Documentation](https://docs.docker.com/compose/install/compose-plugin/#installing-compose-on-linux-systems)



















# #### docker Swarm ############





# #### K8s #########

























```shell
docker run -p 3306:3306 --name mysql -v ./mysql/logs:/var/log/mysql -v ./mysql/data:/var/lib/mysql -v ./mysql/conf/my.cnf:/etc/my.cnf -v ./mysql/init:/docker-entrypoint-initdb.d/ -e MYSQL_ROOT_PASSWORD=shangke6 -d mysql:8.0.25
```







docker run  \
-e TZ="Asia/Shanghai" \
-e MODE=standalone \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-e MYSQL_DATABASE_NUM=1 \
-e MYSQL_SERVICE_HOST=8.130.19.144 \
-e MYSQL_SERVICE_PORT=3306 \
-e MYSQL_SERVICE_USER=root \
-e MYSQL_SERVICE_PASSWORD=shangke6\
-e MYSQL_SERVICE_DB_NAME=nacos_config \
-p 8848:8848 \
--name nacos \
--restart=always \
-d nacos/nacos-server
————————————————
版权声明：本文为CSDN博主「我只会跳一下」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/m0_58049961/article/details/122852485





```shell
docker run -e TZ="Asia/Shanghai" -e MODE=standalone -e SPRING_DATASOURCE_PLATFORM=mysql -e MYSQL_DATABASE_NUM=1 -e MYSQL_SERVICE_HOST=8.130.19.144 -e MYSQL_SERVICE_PORT=3306 -e MYSQL_SERVICE_USER=root -e MYSQL_SERVICE_PASSWORD=shangke6 -e MYSQL_SERVICE_DB_NAME=nacos_config -p 8848:8848 --name nacos --restart=always -d nacos/nacos-server:1.4.2
```





```txt
com.alibaba.nacos.api.exception.NacosException: failed to req API:/nacos/v1/ns/instance after all servers([8.130.19.144:8848]) tried: java.net.ConnectException: Connection refused (Connection refused)
```



# docker 安装 rabbitMQ

```shell
docker run -d --hostname my-rabbit --name rabbitmq -p 5672:5672 -p 15672:15672 -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password -e RABBITMQ_DEFAULT_VHOST=/ rabbitmq:3-management
```

# docker 安装 nginx

```shell
docker run -d -p 80:80 --name studentOrganizationFront -v /mydata/student-club/nginx/conf.d:/etc/nginx/conf.d -v /mydata/student-club/nginx/html:/usr/share/nginx/html -v /mydata/student-club/nginx/log:/var/log/nginx nginx
```

