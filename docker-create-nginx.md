# 实例创建之Nginx

```docker
docker run -d -p 80:80 --name nginx1 
    -v D:\CODEROOT\DockerSpace\imageNginx\html:/usr/share/nginx/html 
    -v D:\CODEROOT\DockerSpace\imageNginx\conf\nginx.conf:/etc/nginx/nginx.conf 
    -v D:\CODEROOT\DockerSpace\imageNginx\logs:/wwwlogs
    nginx
```