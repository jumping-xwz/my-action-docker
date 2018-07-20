# 实例创建之wekan

```docker
docker run -d --restart=always --name wekan-db mongo
```

```docker
docker run -d --restart=always --name wekan 
    --link "wekan-db:db" 
    -e "MONGO_URL=mongodb://db" 
    -e "ROOT_URL=http://localhost:8089" 
    -p 8089:80 
    wekanteam/wekan
```