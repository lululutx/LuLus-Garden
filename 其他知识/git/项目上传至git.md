1. git仓库为空，项目文件夹不为空的情况 
   1. 在项目目录cmd 
   2. git init 
   3. git remote add origin [http://xx.com/xx/xx.git](http://xx.com/xx/xx.git) 
   4. git pull origin master 
   5. git add . 
   6. git commit -m "注释" 
   7. git push origin master 



**方法2、本地初始化一个仓库，设置远程仓库地址后再做push** 和方法1的差别，在于先创建仓库。 $ git init  $ git remote add origin [https://gitee.com/用户个性地址/HelloGitee.git](https://gitee.com/%E7%94%A8%E6%88%B7%E4%B8%AA%E6%80%A7%E5%9C%B0%E5%9D%80/HelloGitee.git) 这样就完成了版本的一次初始化。 接下去，进入你已经初始化好的或者克隆仓库的目录,然后执行： $ git pull origin master  修改/添加文件，否则与原文件相比就没有变动。 $ git add .$ git commit -m "第一次提交"$ git push origin master  然后如果需要账号密码的话就输入账号密码，这样就完成了一次提交。此时，你可以在你的个人面板、仓库主页查看到你的提交记录。 在新建仓库时，如果在码云平台仓库上已经存在 readme 或其他文件，在提交时可能会存在冲突，这时用户需要选择的是保留线上的文件或者舍弃线上的文件，如果您舍弃线上的文件，则在推送时选择强制推送，强制推送需要执行下面的命令(默认不推荐该行为)： $ git push origin master -f  如果您选择保留线上的 readme 文件,则需要先执行： $ git pull origin master 

作者：记忆哪里去了 链接：https://www.jianshu.com/p/19996dce9b41 来源：简书 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。 
