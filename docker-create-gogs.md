# 实例创建之gogs

```docker
docker run --name gogs1 -d -p 10022:22 -p 10080:3000 
    -v D:\CODEROOT\DockerSpace\imageGogs\gogs1\data:/data 
    --link mysql1:db gogs/gogs
```