# 实例创建之Mongodb

1.
```docker
docker run -d -p 27017:27017 -v D:\CODEROOT\DockerSpace\imageMongo\mongo1\data:/data --name mongo1 mongo --auth
```

2.
docker exec -it mongo1 mongo admin

3. 
创建管理员用户wjpdev
```
db.createUser({ user: 'wjpdev', pwd: 'wjpdev', roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] });
```


4.
创建test数据库和test用户
```
docker exec -it mongo1 mongo admin
db.auth("wjpdev","wjpdev");
use test；
db.createUser({ user: 'test', pwd: '123456', roles: [{ role: "readWrite", db: "test" }] });
```



