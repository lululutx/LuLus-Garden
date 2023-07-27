nginx官方文档说明： [http://nginx.org/en/linux_packages.html#RHEL-CentOS](http://nginx.org/en/linux_packages.html#RHEL-CentOS) <br />**一、安装前准备：** <br />yum install yum-utils <br />**二、添加源** <br />到 cd /etc/yum.repos.d/ 目录下 <br />新建 vim nginx.repo 文件 <br />输入以下信息 <br />[nginx-stable] <br />name=nginx stable repo <br />baseurl=http://nginx.org/packages/centos/$releasever/$basearch/ <br />gpgcheck=1 <br />enabled=1 <br />gpgkey=https://nginx.org/keys/nginx_signing.key <br />[nginx-mainline] <br />name=nginx mainline repo <br />baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/ <br />gpgcheck=1 <br />enabled=0 <br />gpgkey=https://nginx.org/keys/nginx_signing.key 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468108008-f48a2e3f-6a26-4b27-8d88-38e335a59798.png#averageHue=%23242322&clientId=u298bda8d-98ac-4&from=paste&id=u313b08a9&originHeight=342&originWidth=805&originalType=url&ratio=1&rotation=0&showTitle=false&size=35625&status=done&style=none&taskId=u94fdd3c6-17e6-4a40-9270-e0b06a1c6b1&title=)<br />**三、安装Nginx** <br />通过yum search nginx看看是否已经添加源成功。如果成功则执行下列命令安装nginx。 <br />yum install nginx <br />安装完后，rpm -qa | grep nginx 查看 <br />启动nginx：systemctl start nginx <br />加入开机启动：systemctl enable nginx <br />查看nginx的状态：systemctl status nginx 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468108088-963c87be-283d-4db5-8c19-419280ed1e69.png#averageHue=%230d0b0a&clientId=u298bda8d-98ac-4&from=paste&id=u928f23da&originHeight=442&originWidth=825&originalType=url&ratio=1&rotation=0&showTitle=false&size=67216&status=done&style=none&taskId=uad8d223f-929e-41b9-b3e5-2b0f3bf4ca8&title=)<br />在浏览器输入自己服务器的IP地址即可访问到nginx，如下图所示，nginx服务的默认端口为80（这里需要注意防火墙的限制和端口冲突）。 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468108046-801fb581-878f-4066-b94d-22318dbbe4d5.png#averageHue=%23f8f7f5&clientId=u298bda8d-98ac-4&from=paste&id=ua79501c2&originHeight=385&originWidth=964&originalType=url&ratio=1&rotation=0&showTitle=false&size=54748&status=done&style=none&taskId=u2fca0b94-8e83-415f-8786-2d45164c7a1&title=)<br />用命令lsof -i:80，可查看80端口被那个进程占用。 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468108015-43f1d87c-4476-4ef0-a20c-63b20becee38.png#averageHue=%230e0c0a&clientId=u298bda8d-98ac-4&from=paste&id=u3ae67f73&originHeight=102&originWidth=805&originalType=url&ratio=1&rotation=0&showTitle=false&size=16014&status=done&style=none&taskId=ue37d84ae-bb58-4d06-a1e3-379957bf91d&title=)<br />nginx服务的默认配置文件在 vim /etc/nginx/conf.d/default.conf ，打开可看到，默认端口为80，项目部署目录为/usr/share/nginx/html/。 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468108090-24295314-91a9-4905-8366-56c57b8c47fb.png#averageHue=%23131211&clientId=u298bda8d-98ac-4&from=paste&id=u68a25c35&originHeight=960&originWidth=825&originalType=url&ratio=1&rotation=0&showTitle=false&size=86788&status=done&style=none&taskId=ubf3628c6-ba48-4b9b-8a7d-da7f54766de&title=)<br />往 /usr/share/nginx/html/ 目录下上传一个JavaScript写的飞机大战。 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468108858-00179bd9-ee64-41e3-afad-897d33a349d6.png#clientId=u298bda8d-98ac-4&from=paste&id=ue9eb389f&originHeight=142&originWidth=805&originalType=url&ratio=1&rotation=0&showTitle=false&size=17639&status=done&style=none&taskId=u0ec1afc3-9815-471f-bf6c-f05cc2944e1&title=)<br />在浏览器里输入http://192.168.0.146/Plane/test/，即可访问到。 



来自 <[https://www.cnblogs.com/opsprobe/p/10773582.html](https://www.cnblogs.com/opsprobe/p/10773582.html)>  

