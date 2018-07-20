# 实例创建之Apache

```docker
docker run -p 80:80 
    -v D:\CODEROOT\DockerSpace\imageApache\apache1\htdocs:/usr/local/apache2/htdocs 
    -v D:\CODEROOT\DockerSpace\imageApache\apache1\conf:/usr/local/apache2/conf 
    -v D:\CODEROOT\DockerSpace\imageApache\apache1\logs:/user/local/apache2/logs 
    -d --name apache1 httpd
```