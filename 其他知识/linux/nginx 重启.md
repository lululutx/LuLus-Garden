目录 

一、启动/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf 

二、停止 

1、从容停止 

（1）查看进程号: ps -ef|grep nginx 

（2）杀死进程: kill -quit xxxx 

2、快速停止 

（1）查看进程号: ps -ef|grep nginx 

（2）杀死进程:  kill -term xxxx/ kill -int xxxx 

3、强制停止: pkill -9 nginx 

三、重启 

1、验证nginx配置文件是否正确 

（1）方法一：进入nginx安装目录sbin下，输入命令./nginx -t 

（2）方法二：在启动命令-c前加-t 

2、重启nginx服务 

（1）方法一：进入nginx安装目录sbin下，输入命令./nginx -s reload 即可 

（2）方法二：查找当前nginx进程号，然后输入命令：kill -HUP 进程号 实现重启nginx服务 

一、启动/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf 启动代码格式：nginx安装目录地址 -c nginx配置文件地址 

例如： 

[root@localhost ~]# /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf 二、停止 nginx的停止有三种方式： 

1、从容停止 （1）查看进程号: ps -ef|grep nginx [root@localhost ~]# ps -ef|grep nginx 


（2）杀死进程: kill -quit xxxx 142804是进程的编号 

[root@localhost ~]# kill -quit 142804 


2、快速停止 （1）查看进程号: ps -ef|grep nginx [root@localhost ~]# ps -ef|grep nginx  

（2）杀死进程:  kill -term xxxx/ kill -int xxxx 142804是进程的编号 

[root@localhost ~]# kill -term 142804  或   

[root@localhost ~]# kill -int 142804 


3、强制停止: pkill -9 nginx [root@localhost ~]# pkill -9 nginx 三、重启 1、验证nginx配置文件是否正确 （1）方法一：进入nginx安装目录sbin下，输入命令./nginx -t 看到如下显示nginx.conf syntax is ok 

nginx.conf test is successful 

说明配置文件正确！ 

[root@localhost ~]# cd /usr/local/nginx/sbin  [root@localhost sbin]# ./nginx -t 

（2）方法二：在启动命令-c前加-t [root@localhost sbin]# /usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf 


2、重启nginx服务 （1）方法一：进入nginx安装目录sbin下，输入命令./nginx -s reload 即可 [root@localhost ~]# cd /usr/local/nginx/sbin [root@localhost sbin]# ./nginx -s reload 


（2）方法二：查找当前nginx进程号，然后输入命令：kill -HUP 进程号 实现重启nginx服务  ———————————————— 版权声明：本文为CSDN博主「乞力马扎罗の黎明」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。 原文链接：https://blog.csdn.net/qq_39715000/article/details/119919823 
