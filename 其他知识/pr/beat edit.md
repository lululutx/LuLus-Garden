链接：https://pan.baidu.com/s/1ZssO8VMbTCjPnuUQ4aHyvA 提取码：ivan

本文分为：1 使用教程，2 安装教程，3 安装包下载地址



1 使用教程

使用方法其实很简单，找到插件位置，如图1。

图1：插件位置然后打开插件，如图2：

图2：设置插件相关参数设置完参数，试听鼓点，然后倒入到序列里，如图3所示：

图3：鼓点倒入序列图4 看生成鼓点位置

图4：序列上鼓点位置然后接下来要将图片或者视频插入你的鼓点之间，可不是手动，选中你的素材，点击自动匹配按钮，如图5所示：

图5： 自动匹配然后弹出图6所示的框框：

图6： 设置匹配序列然后就匹配到你的序列中了，如图7所示：

图7：匹配序列基本上就这了，很简单！


2 安装教程

-1 下载完成后，将对应的BeatEditPremiere文件夹拷贝到一下目录：

Win: C:\Program Files (x86)\Common Files\Adobe\CEP\extensions\

Mac : /Library（中文叫资源库）/Application Support/Adobe/CEP/extensions

-2 运行文件

Win 运行一下Add Keys.reg

Mac 运行一下install-as-admin

-3 重启2020版pr即可使用





3 安装包下载地址

链接：[https://pan.baidu.com/s/1s9E0j_LkY3jtr-ddKEjTzA](https://pan.baidu.com/s/1s9E0j_LkY3jtr-ddKEjTzA) 

提取码：ivan

如果对可爱好学的你有帮助，请动动小手，一键三连，up也在学习pr相关技巧，争取把所掌握的，都通俗易懂的分享给大家！

 作者：bili_ivanyang [https://www.bilibili.com/read/cv14760596](https://www.bilibili.com/read/cv14760596) 出处：bilibili



遇到启动Beat Edit无反应的各位同行可通过如下步骤解决：

1. 快捷键【Win+R】输入regedit回车打开注册表编辑器
2. 定位到如下地址【计算机\HKEY_CURRENT_USER\Software\Adobe\】（可复制黏贴）
3. 在这一级目录中分别有几个CSXS.x目录（x代表不同数字）
4. 依次点击每个CSXS.x目录，在右侧窗口中右键点击新建，字符串项
5. 重命名新建的项名为【PlayerDebugMode】，并双击打开该键值，将数值数据改为1，编辑完成后退出注册表编辑器
6. 重启Pr，此时Beat Edit应当可以正常打开



