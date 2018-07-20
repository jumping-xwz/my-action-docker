# Docker学习笔记

## 容器

#### 1. 唯一标识符ID：

16进制编码的1024位数字

#### 2. 创建和启动一个容器：

```docker
docker run --detach --name web nginx:latest
```

#### 3. 创建但不启动容器：

```docker
docker create nginx
```

#### 4. 将创建的容器ID写入文件：

```docker
docker create --cidfile /tmp/web.cid nginx
```

#### 5. 运行交互式容器:

```docker
docker run --interactive --tty --link web:web --name web_test busybox:latest /bin/sh

docker run -it --link web:web --name web_test busybox:latest /bin/sh
```

#### 6. 获取容器ID:

```docker
docker ps  -q
```

#### 7. 获取最后创建的那个容器的截断ID:

```docker
    CID=$(docker ps -l -q)
    echo $CID
```

#### 8. 查看正在运行的容器:

```docker
docker ps
docker ps -a
```

#### 9. 重启web容器:

```docker
docker restart web
```

#### 10. 停止web容器

```docker
docker stop web
```

#### 11. 查看web容器的日志:

```docker
docker logs web
docker logs -f web
```

#### 12. 运行web容器中的其它命令:

```docker
docker exec web ls
docker run --pid host busybox:latest ps
```

#### 13. 查看Docker容器正在运行的进程:

```docker
docker top lamp-test
docker exec lamp-test ps
```

#### 14. 创建没有PID命名空间的容器:
设置 --pid=host

```docker
docker run --pid host busybox:latest ps
```

#### 15. 重命名已存在的web容器：

```docker
docker rename webid webid-old
```

#### 16. 使用只读文件系统：

```docker
docker run -d --name wp --read-only wordpress:4
```

#### 17. 检查容器元数据：

```docker
docker inspect --format "{{.State.Running}}" wp
```

#### 18. 注入环境变量：

```docker
docker create \
    --env WORDPRESS_DB_HOST=<my database hostname> \
    --env WORDPRESS_DB_USER=site_admin \
    --env WORDPRESS_DB_PASSWORD=<password> \
    wordpress:4
```

#### 19. 设置容器启动入口点命令：

```docker
docker run --entrypoint="act" wordpress:4 /entrypoint.sh
```

#### 20. 清理容器：

```docker
docker rm wp
```

#### 21. 强制删除容器：

```docker
docker rm -f wp
docker rm -vf $(docker ps -a -q)
```

#### 22. 容器退出时自动清理：

```docker
docker run --rm --name auto-exit-test busybox:latest echo hello world
```

#### 23. 运行特权容器：

```docker
docker run --rm --privileged ubuntu:latest id
```

# 镜像

#### 1. 在仓库中查找镜像：

>公开脚本构建的镜像在 AUTOMATD 列有一个OK标记

```docker
docker search mysql
```

#### 2. 删除镜像:

```docker
docker rmi quay.io/dockerinaction/ch3_hello_registry
docker rmi busybox
```

#### 3. 将镜像保存为文件:

```docker
docker save -o myfile.tar busybox:latest
```

#### 4. 从Dockerfile 安装镜像:

```docker
docker build -t dia_ch3/dockerfile:latest ch3_dockerfile
```

#### 5. 从文件中加载镜像:

```docker
docker load -i myfile.tar
```

#### 6. 显示仓库里的镜像:

```docker
docker images
docker images -a
```

#### 7. 从容器构建镜像:

```docker
docker commit hw_container hw_image
```

带上作者信息：

```docker
docker commit -a "@dockerinaction" -m "Added git" image-dev ubuntu-git
```

指定入口和命令：

```docker 
docker run --name cmd-git --entrypoint git ubuntu-git
docker run --name rich-image_example-2 --entrypoint "/bin/sh" rie -c "echo \$ENV_EXAMPLE1 \$ENV_EXAMPLE2
```

#### 8. 查看容器改动:

```
docker diff mod_ubuntu
```

#### 9. 复制一个已有镜像:

```docker
docker tag myuser/myfirstrepo:mytag myuser/mod_ubuntu
```

#### 10. 查看镜像的所有层:

```docker
docker history ubuntu-git:removed
```

#### 11. 镜像导出:

```docker
docker export --output contents.tar export-test
```

#### 12. 镜像导入:

```docker
docker import -c "ENTRYPOINT [\"/hello\"]" - dockerinaction/ch7_static < static_hello.tar
```

#### 13. 使用Dockerfile：

```docker
docker build -tag ubuntu-git:auto .
```

# 存储卷

#### 1. 绑定挂载存储卷:

```docker
docker run -d --name bmweb \
    -v ~/example-docs:/usr/local/apache2/htdocs \
    -p 80:80 \
    httpd:latest
```

#### 2. 创建一个存储卷容器:

```docker
docker run -d \
    --volume /var/lib/cassandra/data \
    --name cass-shared \
    alpine echo Data Container
```
>卷容器并不需要运行，因此停止时容器仍能保证存储卷的引用。

#### 3. 继承存储卷:

```docker
docker run -d \
    --volumes-from cass-shared \
    --name cass1 \
    cassandra:2.2
```

#### 4. 创建容器使用存储卷:

```docker
docker run -it --rm \
    --link cass1:cass \
    cassandra:22 cqlsh cass
```

#### 5. 查看存储卷在主机上的真实路径:

```docker
docker inpsect -f "{{json .Volumes}}" cass-shared
```

#### 6. 删除管理卷，使用-v:

```docker
docker rm -v student
```

# 运行个人Registry

```docker
docker run -d --name personal_registry -p 5000:5000 --restart=always registry:2
```

# Dockerfile

#### 1.忽略文件:

创建一个.dockerignore文件

#### 2. 元数据指令:

`FROM`、`MAINTAINER`、`RUN`、`ENV`、`LABEL`、`ENTRYPOINT`、`EXPOSE`、`WORKDIR`、`USER`、`CMD`

#### 3. 文件系统指令：

`COPY`、`VOLUME`、`ADD`、`ONBUILD`

# Run-as用户

#### 1. 查看默认用户:

```docker
docker run --rm --entrypoint "" busybox:latest whoami
docker run --rm --entrypoint "" busybox:latest id
```

#### 2. 查看镜像可用用户名列表:

```docker
docker run --rm busybox:latest awk -F: '$0=$1' /etc/passwd
```

#### 3. 设置run-as用户:

```docker
docker run --rm  --user nobody busybox:latest id
docker run --rm -u nobody:default busybox:latest id
docker run --rm -u 10000:20000 busybox:latest id
```

# 资源分配

#### 1. 内存限制:

```docker
docker run -d --name ch6_mariadb --memory 256m  dockerfile/mariadb
```

#### 2. CPU限制:

```docker
docker run -d -P --name ch6_wordpress --cpu-shares 512 --link ch6_mariadb wordpress:4.1
```

#### 3. 设备访问权:

```docker
docker -it --rm --device /dev/video0:/dev/video0 ubuntu:latest ls -al /dev
```

#### 4. 共享内存:

```docker
docker -d --name ch6_ipc_consumer \
    --ipc container:ch6_ipc_producer \
dockerinaction/ch6_ipc -consumer
```

#### 5. 开放内存容器:

```docker
docker -d --name ch6_ipc_producer --ipc host dockerinaction/ch6_ipc -producer
```

# 跨容器信赖

#### 1. 示例:

```docker
docker run -d --name importantData --expose 3306 dockerinaction/mysql_noauth service mysql_noauth start
docker run -d --name importantWebApp --link importantData:db \
dockerinaction/ch5_web startapp.sh -db tcp://db:3306
```

#### 2. 链接别名:

```docker
docker run --link a:alias-a --link b:alias-b --linkk c:alias-c ...
```

#### 3. 创建链接并列出所有环境变量:

```docker
docker run -it --rm --link mydb:database dockerinaction/ch5_ff env
```

# 网络访问

## 单主机虚拟网络

### Closed容器

```docker
docker run --rm --net none alpine:latest ip addr
```

### Joined容器

```docker
docker run -d --name brady \
    -net none alphine:latest \
    nc -l 127.0.0.1:3333

docker run -it -net container:brady \
    alphine:latest netstat -al
```

### Bridge容器

#### 1. 访问外部网络:

```docker
docker run --rm  alpine:latest ping -w 2 8.8.8.8
```

#### 2. 自定义命名解析:

```docker
docker run --rm --hostname barker alpine:latest nslookup barker
```

#### 3. 自定义DNS服务器:

```docker
docker run --rm --dns 8.8.8.8 alpine:latest nslookup docker.com
docker run --rm --add-host test:10.10.10.255 alpine:latest nslookup test
```

#### 4. 开放对容器的访问:

暴露端口:

```docker
docker run -p 3333 ...
docker run -p 3333:3333 ...
docker run -p 192.168.0.32::2222 ...
docker run -p 192.168.0.32:1111:1111 ...
```

暴露所有端口:

```docker
docker run -d --name dawson -p 5000 -p 6000 -p 7000 dockerinaction/ch5_expose
docker run -d --name dawson -P dockerinaction/ch5_expose
```

增加端口暴露:

```docker
docker run -d --name philbin --expose 8000 -P dockerinaction/ch5_expose
```

查看端口映射：

```docker
docker port philbin
```

#### 5. 关闭容器之间的网络连接：

```docker
docker -d -icc=false ...
```

### Open容器

```docker
docker run --rm --net host alpine:latest ip addr
```


