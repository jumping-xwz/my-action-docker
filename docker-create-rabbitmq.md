# 实例创建之rabbitMq

1. 查找 
```docker
docker search rabbitmq:management
```
2. 拉取
```docker
docker pull rabbitmq:management
```


3. 创建rabbitmq1容器
```docker
docker run --name rabbitmq1 -d -p 5671:5671 -p 5672:5672 -p 4369:4369 -p 25672:25672 -p 15671:15671 -p 15672:15672 rabbitmq:management
```

4. 访问:
```http
http://localhost:15672
```
用户名：guest, 密码：guest