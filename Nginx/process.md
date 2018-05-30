1. 问题：nginx启动的时候会报丢失pid的错误:
    ```c
    nginx: [error] open() “/usr/local/var/run/nginx.pid” failed```
    *解决方案:
```C 
sudo nginx -c /usr/local/etc/nginx/nginx.conf
sudo nginx -s reload```
1. [代理服务器](http://www.cnblogs.com/kevingrace/p/6086652.html)

[返回目录](README.md)