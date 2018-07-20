# 实例创建之Rancher

访问路径: http://10.34.135.122:8081

```docker
docker run -d --name rancher1 
    -v D:\CODEROOT\DockerSpace\imageRancher\rancher1\mysql:/var/lib/mysql 
    --restart=unless-stopped -p 8081:8080 rancher/server
```