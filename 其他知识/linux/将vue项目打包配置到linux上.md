使用Vue做[前后端分离](https://so.csdn.net/so/search?q=%E5%89%8D%E5%90%8E%E7%AB%AF%E5%88%86%E7%A6%BB&spm=1001.2101.3001.7020)项目时，通常前端是单独部署，用户访问的也是前端项目地址，因此前端开发人员很有必要熟悉一下项目部署的流程与各类问题的解决办法了。Vue项目打包部署本身不复杂，不过一些前端同学可能对服务器接触不多，部署过程中还是会遇到这样那样的问题。本文介绍一下使用nginx服务器代理前端项目的方法以及项目部署的相关问题，内容概览： <br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468135035-f3a73a7c-8178-4e77-8cef-746d21413213.png#clientId=ucfa03b16-79fd-4&from=paste&id=uc50f655a&originHeight=242&originWidth=568&originalType=url&ratio=1&rotation=0&showTitle=false&size=30753&status=done&style=none&taskId=u9aa47078-b256-415a-9350-5f3b107002b&title=)<br />Vue项目打包发布问题汇总 <br />**一、准备工作——服务器和**[nginx](https://so.csdn.net/so/search?q=nginx&spm=1001.2101.3001.7020)**使用** <br />**1. 准备一台服务器** <br />我的是ubuntu系统，linux系统的操作都差不多。没有服务器怎么破？ <br />如果你只是想体验一下，可以尝试各大厂的[云服务器](https://so.csdn.net/so/search?q=%E4%BA%91%E6%9C%8D%E5%8A%A1%E5%99%A8&spm=1001.2101.3001.7020)免费试用套餐，比如华为云免费试用，本文相关操作即是在华为云上完成的。不过如果想时常练练手，我觉得可以购买一台云服务器，比如上面的华为云或者阿里云都还挺可靠。我的个人网站就是部署在阿里云，你可以点击我的推广链接进行购买，近期有活动首次购买不到100块/年。 <br />**2. nginx安装和启动** <br />轻装简行，这部分不作过多赘述（毕竟网上相关教程一大堆），正常情况下仅需下面两个指令： 

1. # 安装，安装完成后使用nginx -v检查，如果输出nginx的版本信息表明安装成功 
2. sudo apt-get install nginx 
3. # 启动 
4. sudo service nginx start 

启动后，正常情况下，直接访问 [http://服务器ip](http://xn--ip-fr5c86lx7z/) 或 [http://域名](http://xn--eqrt2g/) （本文测试用的服务器没有配置域名，所以用ip，就本文而言，域名和ip没有太大区别）应该就能看到nginx服务器的默认页面了——如果访问不到，有可能是你的云服务器默认的http服务端口（80端口）没有对外开放，在服务器安全组配置一下即可。 <br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468135040-36951664-b011-462d-a15a-e06377bb4701.png#clientId=ucfa03b16-79fd-4&from=paste&id=ub134039b&originHeight=279&originWidth=512&originalType=url&ratio=1&rotation=0&showTitle=false&size=33391&status=done&style=none&taskId=uc5a85f92-b08d-416f-87f6-bd345c26b33&title=)<br />Vue项目打包发布问题汇总 <br />**3. 了解nginx: 修改nginx配置，让nginx服务器代理我们创建的文件** <br />查看nginx的配置，linux系统下的配置文件通常会存放在/etc目录下，nginx的配置文件就在/etc/nginx文件夹，打开文件/etc/nginx/sites-available/default（nginx可以有多个配置文件，通常我们配置nginx也是修改这个文件）： <br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468135036-11ba3201-2a93-4279-9438-e7f25452940f.png#clientId=ucfa03b16-79fd-4&from=paste&id=u226991fb&originHeight=341&originWidth=669&originalType=url&ratio=1&rotation=0&showTitle=false&size=21173&status=done&style=none&taskId=ucf489c9e-3bf0-4574-bf23-6bf25ae66f2&title=)<br />Vue项目打包发布问题汇总 <br />可以看到默认情况下，nginx代理的根目录是/var/www/html，输入 [http://服务器ip会访问这个文件夹下的文件，会根据index的配置值来找默认访问的文件，比如index.html、index.htm之类](http://%E6%9C%8D%E5%8A%A1%E5%99%A8ip%E4%BC%9A%E8%AE%BF%E9%97%AE%E8%BF%99%E4%B8%AA%E6%96%87%E4%BB%B6%E5%A4%B9%E4%B8%8B%E7%9A%84%E6%96%87%E4%BB%B6%EF%BC%8C%E4%BC%9A%E6%A0%B9%E6%8D%AEindex%E7%9A%84%E9%85%8D%E7%BD%AE%E5%80%BC%E6%9D%A5%E6%89%BE%E9%BB%98%E8%AE%A4%E8%AE%BF%E9%97%AE%E7%9A%84%E6%96%87%E4%BB%B6%EF%BC%8C%E6%AF%94%E5%A6%82index.html%E3%80%81index.htm%E4%B9%8B%E7%B1%BB/)。 <br />我们可以更改root的值来修改nginx服务代理的文件夹： 

1. 创建文件夹/www，并创建index.html，写入"Hello world"字符串 <br /> 
2. mkdir /www 
3. echo 'Hello world' > /www/index.html 
4. 修改root值为 /www 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468135043-5d8b9e32-1845-4842-b003-5380002dfc74.png#clientId=ucfa03b16-79fd-4&from=paste&id=u03d89d1c&originHeight=288&originWidth=651&originalType=url&ratio=1&rotation=0&showTitle=false&size=20003&status=done&style=none&taskId=u648e796d-1233-4c5f-9a03-4f789307855&title=)

1. sudo nginx -t 检查nginx配置是否正确 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468135054-9815bcb6-a9fe-4c3c-b33d-6b7b108b5f7d.png#clientId=ucfa03b16-79fd-4&from=paste&id=u4a15f35c&originHeight=113&originWidth=905&originalType=url&ratio=1&rotation=0&showTitle=false&size=28685&status=done&style=none&taskId=u36c415ff-5718-4934-8517-bfe485fecff&title=)

1. 加载nginx配置：sudo nginx -s reload 

再次访问页面，发现页面内容已经变成了我们创建的index.html: <br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468135722-c08232f4-e5ab-4d86-b69e-46690812c488.png#clientId=ucfa03b16-79fd-4&from=paste&id=u207a2ba7&originHeight=89&originWidth=362&originalType=url&ratio=1&rotation=0&showTitle=false&size=6712&status=done&style=none&taskId=u4bbddc8e-7166-4d13-80b9-457888fbf6b&title=)<br />Vue项目打包发布问题汇总 <br />**二、Vue项目打包同步文件到远程服务器** <br />**1. 打包** <br />默认情况下，使用vue-cli创建的项目，package.json里的script应该已经配置了build指令，直接执行yarn build 或者 npm run build即可。 <br />**2. 同步到远程服务器** <br />我们使用nginx部署Vue项目，实质上就是将Vue项目打包后的内容同步到nginx指向的文件夹。之前的步骤已经介绍了怎样配置nginx指向我们创建的文件夹，剩下的问题就是怎么把打包好的文件同步到服务器上指定的文件夹里，比如同步到之前步骤中创建的/www。同步文件可以在git-bash或者powershell使用scp指令，如果是linux环境开发，还可以使用rsync指令: 

1. scp -r dist_/* root@117.78.4.26:/www_ 
2. _或_ 
3. _rsync -avr --delete-after dist/* root@117.78.4.26:/www _ 
4. <br />

**注意这里以及后续步骤是root使用用户远程同步，应该根据你的具体情况替换root和ip(ip换为你自己的服务器IP)。** <br />为了方便，可以在package.json脚本中加一个push命令，以使用yarn为例（如果你使用npm，则push命令中yarn改成npm run即可）： 

1. "scripts": { 
2. "build": "vue-cli-service build", 
3. "push": "yarn build && scp -r dist/* root@117.78.4.26:/www" 
4. }, 

这样就可以直接执行yarn push 或者npm run push直接发布了。不过还有一个小问题，就是命令执行的时候要求输入远程服务器的root密码（这里使用root来连接远程的，你可以用别的用户，毕竟root用户权限太高了）。 <br />为了避免每次执行都要输入root密码，我们可以将本机的ssh同步到远程服务器的authorized_keys文件中。 <br />**3. 同步ssh key** 

1. 生成ssh key：使用git bash或者powershell执行ssh-keygen可以生成ssh key。会询问生成的key存放地址，直接回车就行，如果已经存在，则会询问是否覆盖： 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662468135903-8fc074db-7a9c-449e-8df9-2c1dfd9d5614.png#clientId=ucfa03b16-79fd-4&from=paste&id=u4c2a9f1a&originHeight=146&originWidth=707&originalType=url&ratio=1&rotation=0&showTitle=false&size=39141&status=done&style=none&taskId=ue0eff91b-cb13-4022-9a67-5108d3edd3f&title=)

1. 同步ssh key到远程服务器，使用ssh-copy-id指令同步 <br />ssh-copy-id -i ~/.ssh/id_rsa.pub root@117.78.4.26 

输入密码后，之后再次同步就不需要输入密码了。其实ssh_key是同步到了服务器（此处是root用户家目录）~/.ssh/authorized_keys文件里： 

当然你也可以手动复制本地~/.ssh/id_rsa.pub（注意是pub结尾的公钥）文件内容**追加**到服务器~/.ssh/authorized_keys的后面（从命名可以看出该文件可以存储多个ssh key） <br />**注意：这里全程使用的是root用户，所以没有文件操作权限问题。如果你的文件夹创建用户不是远程登录用户，或许会存在同步文件失败的问题，此时需要远程服务器修改文件夹的读写权限（命令 chmod）。** <br />创建了一个测试项目（点击本链接可以在gihub查看）[1]试一下，打包、文件上传一句指令搞定啦： 

Vue项目打包发布问题汇总 <br />访问一下，果然看到了我们熟悉的界面： 

Vue项目打包发布问题汇总 <br />至此，常规情况下发布Vue项目就介绍完了，接下来介绍非域名根路径下发布以及history路由模式发布方法。 <br />**三、非域名根路径发布** <br />有时候同一台服务器同一端口下可能会根据目录划分出多个不同的项目，比如我们希望项目部署到http://a.com/test下，这样访问http://a.com/test访问到的是项目的首页，而非test前缀的地址会访问到其它项目。此时需要修改nginx配置以及Vue打包配置。 <br />**1. nginx配置** <br />只需要添加一条location规则，分配访问路径和指定访问文件夹。我们可以把/test指向之前创建的/www文件夹，这里因为文件夹名称和访问路径不一致，需要用到alias这个配置： 

Vue项目打包发布问题汇总 <br />如果文件夹名称与访问路径一致都为test，那这里可以用root来配置： 

Vue项目打包发布问题汇总 <br />这里要将/test配置放到/之前，意味着在路由进入的时候，会优先匹配/test。如果根路径/下的项目有子路由/test，那http://xxxx/test只会访问到/www里的项目，而不会访问该子路由。 <br />**2. 项目配置** <br />为了解决打包后资源路径不对的问题，需要在vue.config.js中配置publicPath，这里有两种配置方式，分别将publicPath配置为./和/test： 

Vue项目打包发布问题汇总 <br />更新nginx配置，发布后即可正常访问啦。这里的两种配置方式是有区别的，接下来会看一下它们的区别。如果不进行项目配置，直接发布访问会出现JS、CSS等资源找不到导致页面空白的问题： 

Vue项目打包发布问题汇总 <br />该问题原因是资源引用路径不对，页面审查元素可以看到，页面引用的js都是从根路径下引用的： 

Vue项目打包发布问题汇总 <br />查看打包后的文件结构，可以看到js/css/img/static等资源文件是与index.html处于同级别的： 

Vue项目打包发布问题汇总 <br />对于两种配置方式，看看都是怎么生效的： 

1. publicPath配置为./， 打包后资源引用路径为相对路径： 

1. publicPath配置为/test，打包后资源相对路径为从域名根目录开始的绝对路径： 

两种配置都可以正确地找到JS、CSS等资源。不过还有个问题，那就是static中的静态资源依旧会找不到。 <br />**3. 绝对路径引用的静态资源找不到的问题** <br />因为在打包过程中，public下的静态资源都不会被webpack处理，我们需要通过绝对路径来引用它们。当项目部署到非域名根路径上时，这点非常头疼，你需要在每个引用的URL前面加上process.env.BASE_URL（该值即对应上文配置的publicPath），以使得资源能被正常访问到。我们可以在main.js把这个变量值绑定到Vue.prototype，这样每个Vue组件都可以使用它： <br />Vue.prototype.$pb = process.env.BASE_URL <br /> <br />在模板中使用： <br /><img :src="`${$pb}static/logo.png`"> <br /> <br />然而，更加头疼并且没有良好解决方案的问题是在组件style部分使用public文件夹下的静态资源： 

- 如果需要使用图片等作为背景图片等，尽量使用内联方式使用吧，像在模板中使用一样。 
- 如果需要引入样式文件，则在index.html中使用插值方式引入吧。 

关于静态资源的问题，vue-cli的推荐是尽量**将资源作为你的模块依赖图的一部分导入（即放到assets中，使用相对路径引用），**避免该问题的同时也带来其它好处： 


**四、history模式部署** <br />默认情况下，Vue项目使用的是hash路由模式，就是URL中会包含一个#号的这种形式。#号以及之后的内容是路由地址的hash部分。正常情况下，当浏览器地址栏地址改变，浏览器会重新加载页面，而如果是hash部分修改的话，则不会，这就是前端路由的原理，允许根据不同的路由页面局部更新而不刷新整个页面。H5新增了history的pushState接口，也允许前端操作改变路由地址但是不触发页面刷新，history模式即利用这一接口来实现。因此使用history模式可以去掉路由中的#号。 <br />**1. 项目配置** <br />在vue-router路由选项中配置mode选项和base选项，mode配置为'history'；如果部署到非域名根目录，还需要配置base选项为前文配置的publicPath值（注意：此情况下，publicPath必须使用绝对路径/test的配置形式，而不能用相对路径./） 

Vue项目打包发布问题汇总 <br />**2. nginx配置** <br />对于history模式，假设项目部署到域名下的/test目录，访问http://xxx/test/about的时候，服务器会去找/test指向的目录下的about子目录或文件，很显然因为是单页面应用，并不会存在a这个目录或者文件，就会导致404错误： 

Vue项目部署后页面找不到 <br />我们要配置nginx让这种情况下，服务器能够返回单页应用的index.html，然后剩下的路由解析的事情就交给前端来完成即可。 

history模式nginx配置 <br />这句配置的意思就是，拿到一个地址，先根据地址尝试找对应文件，找不到再试探地址对应的文件夹，再找不到就返回/test/index.html。再次打开刚才的about地址，刷新页面也不会404啦： 

Vue项目打包发布问题汇总 <br />**3. history模式部署到非域名根路径下** <br />非域名根目录下部署，首先肯定要配置publicPath。需要注意的点前面其实已经提过了，就是这种情况下不能使用相对路径./或者空串配置publicPath。为什么呢？原因是它会导致router-link等的表现错乱，使用测试项目[2]分别使用两种配置打包发布，审查元素就能看出区别。在页面上有两个router-link，Home和About： 

Vue项目打包发布问题汇总 <br />两种配置打包后的结果如下。 

1. publicPath配置为./或者空串： 

1. publicPath配置为/test： 

publicPath配置为相对路径的router-link打包后地址变成了相对根域名下地址，很明显是错误的，所以非域名根路径部署应该将publicPath配置为完整的前缀路径。 <br />**五、结语** <br />关于Vue项目发布的相关问题就先总结这么多，几乎在每一步都踩过坑才有所体会，有问题欢迎各位同学一起探讨。写博客很累，不过收获也很多，还是要坚持；有时候别人转载自己的原创文章也不标明出处，竟然将写文章日期改得比原创还早，有点心累。本文中使用到的图片都加了个自己的水印，是前端实现的，原理也很简单，之后写一篇简短的文章分享一下。 <br />**参考资料** <br />[1] <br />Vue项目打包发布: [https://github.com/Lushenggang/vue-publish-test](https://github.com/Lushenggang/vue-publish-test) <br />[2] <br />Vue项目打包部署测试项目地址: [https://github.com/Lushenggang/vue-publish-test](https://github.com/Lushenggang/vue-publish-test) 

来自 <[https://blog.csdn.net/lgno2/article/details/111148770](https://blog.csdn.net/lgno2/article/details/111148770)>  

