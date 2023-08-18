# 用户账户限制例如时间限制会阻止你登录（用户账户限制）
1、原因：可能的原因包括不允许空密码，登录时间限制，或强制的策略限制。
2、”的情况，这是因为电脑没有开机密码，而windows的默认策略是空白密码的账号只允许从控制台登录，也就是本地登录。
3、所以我们要想允许空密码共享，就需要更改默认的策略。
4、具体方法如下:按WIN+R，调出运行框。
5、WIN就是键盘上有个windows图标的那个键；
2、在运行框输入 gpedit.msc ，并回车；
3、在弹出的窗口，展开左侧目录树至安全选项，如图。
6、（计算机配置-Windows设置-安全设置-本地策略-安全选项）；4、在右侧策略处找到“账户:使用空白密码的本地账户只允许进行控制台登录”，此策略默认是已启用；
5、双击打开“账户:使用空白密码的本地账户只允许进行控制台登录”，将其改为“已禁用”，并确定，生效。
7、再访问共享就正常了。



---------------------------------------------



**下面就介绍几种在Windows 10家庭版中找回组策略编辑器的方法：**<br />**方法1：批处理文件**<br />在Windows 10家庭版中有三种可能的方法来安装组策略编辑器，但是使用批处理文件是最简单的一个方法，因为它对用户简化了操作过程，对于一个新计算机用户来说都足够简单。<br />1.打开记事本程序。<br />2.将以下代码输入到记事本中：<br />@echo off<br />@echo "这个批处理文件将在Windows 10家庭版上启用组策略编辑器."<br />pushd "%~dp0"<br />dir /b %SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientExtensions-Package~3*.mum >List.txt<br />dir /b %SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~3*.mum >>List.txt<br />for /f %%i in ('findstr /i . List.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"<br />pause<br />3.单击文件--另存为，保存这个文件，“保存类型”选择“所有文件”，文件名命名为gpedit-enabler.bat。

![[Pasted image 20230729001023.png]]
4.右键单击gpedit-enabler.bat，然后单击“以管理员身份运行”。<br />5.完成后，将看到文本滚动并关闭Windows。如果看到错误740，说明你忘记了以管理员身份运行这个批处理文件。
