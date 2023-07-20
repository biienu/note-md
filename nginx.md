```shell
root@8dc888ed229f:/usr/share/nginx/html# ls
50x.html  index.html
root@8dc888ed229f:/usr/share/nginx/html# pwd
/usr/share/nginx/html     #ls
访问页面


root@8dc888ed229f:/etc/nginx# ls           # 配置文件
conf.d          mime.types  nginx.conf   uwsgi_params
fastcgi_params  modules     scgi_params

/var/log/nginx/access.log   # 访问日志
```





```shell
# docker 安装 tomcat
docker run -d --name tomcat8080 -p 8080:8080 -v /mydata/tomcat/conf:/usr/local/tomcat tomcat
```

