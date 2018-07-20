# 实例创建之Redis

```docker
docker run -d -p 6379:6379 --name redis1 
    -v D:\CODEROOT\DockerSpace\imageRedis\redis1\data:/data 
    redis:3.2 redis-server --appendonly yes
```