# 实例创建之jenkins

```docker
docker run -p 8080:8080 -p 50000:50000 
    -v D:\CODEROOT\DockerSpace\imageJenkins\jenkins1\jenkins_home:/var/jenkins_home 
    -d --name jenkins1 jenkins/jenkins:lts
```
