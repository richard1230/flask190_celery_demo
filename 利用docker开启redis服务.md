[TOC]


## 拉取镜像
```
$docker pull redis:latest
latest: Pulling from library/redis
8ec398bc0356: Retrying in 1 second
da01136793fa: Download complete
cf1486a2c0b8: Download complete
188200a8c82e: Downloading
9391ca24f5d0: Download complete
6ed21f46fa2d: Download complete
latest: Pulling from library/redis
8ec398bc0356: Pull complete
da01136793fa: Pull complete
cf1486a2c0b8: Pull complete
188200a8c82e: Pull complete
9391ca24f5d0: Pull complete
6ed21f46fa2d: Pull complete
Digest: sha256:21b037b4f6964887bb12fd8d72d06c7ab1f231a58781b6ca2ceee0febfeb0d36
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
$docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
redis latest c33c9b2541a8 7 hours ago 98.2MB
counter-app_web-fe latest 23dc2794e600 12 days ago 84.5MB
memcached latest 291823545098 2 weeks ago 82.2MB
redis alpine a49ff3e0d85f 5 weeks ago 29.3MB
ubuntu latest 775349758637 8 weeks ago 64.2MB
alpine latest 965ea09ff2eb 2 months ago 5.55MB
tensorflow/tensorflow latest c9a0882cbdbc 8 months ago 1.05GB
python 3.4-alpine c06adcf62f6e 9 months ago 72.9MB

```

## 本地创建以下内容
```
mkdir  ~/docker/redis 
mkdir  ~/docker/redis/data 
touch  ~/docker/redis/redis.conf '
touch  ~/docker/redis/redis.bash
```

## 配置文件redis.conf
```
# Redis配置文件

# Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
daemonize no

# 指定Redis监听端口，默认端口为6379
port 6379

# 绑定的主机地址，不要绑定容器的本地127.0.0.1地址，因为这样就无法在容器外部访问
bind 0.0.0.0

# 持久化
appendonly yes


```


## 编辑redis.bash
```
docker run -p 6379:6379 --name myredis -v  ~/docker/redis:/etc/redis/redis.conf -v ~/docker/redis/data:/data -d redis redis-server /etc/redis/redis.conf
```
说明
```
docker run redis # 从redis镜像运行容器
-p 6379:6379 # 映射本地6379端口到容器6379端口，前为本地端口
--name redis # 设置容器名称为redis，方便以后使用docker ps进行管理
-v ~/docker/redis/redis.conf:/etc/redis/redis.conf # 关联本地~/docker/redis/redis.conf文件到容器中/etc/redis/redis.conf，同样，前为本地
-v  ~/docker/redis/data:/data # 关联本地~/docker/redis/data到容器内/data目录，此为存放redis数据的目录，为方便以后升级redis，而数据可以留存
-d # 后台启动，使用此方式启动，则redis.conf中daemonize必须设置为no，否则会无法启动
redis-server /etc/redis/redis.conf # 在容器内启动redis-server的命令，主要是为了加载配置


```


## 给与执行权限
```
#这里是在当前目录
$sudo chmod 777 ./redis.bash
```
## 启动
```
$~/docker/redis/redis.bash
3bde98dfa5ded686918c273d4d78ccf96918047a1585f45107d9a3d24e25ba7c
$docker ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
3bde98dfa5de redis "docker-entrypoint.s…" 16 seconds ago Up 15 seconds 0.0.0.0:6379->6379/tcp my_redis
```

## 停止
```
$docker container stop 3bde98dfa5de
3bde98dfa5de
$docker ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
$


```