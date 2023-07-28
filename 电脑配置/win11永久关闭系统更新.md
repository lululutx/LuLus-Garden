**方法一、**<br />1、首先点击开始，在开始菜单中找到“设置”;<br />
![[Pasted image 20230729001133.png]]
2、进入设置页面，点击左边的“windows更新”选项;
![[Pasted image 20230729001146.png]]<br />3、进入后在windows更新页面，点击“暂停更新”，选择暂停的时间，就可以暂时关闭win11的自动更新了。<br />![[Pasted image 20230729001223.png]]**方法二、** 
1、在键盘上按下**win+r**调出运行窗口;
2、在运行窗口中输入**services.msc**按下回车键确认即可打开“服务”;
3、在服务右侧下拉找到“windows update”，双击打开;
4、接着将“启动类型”改为“禁用”，再点击“停止”，最后点击确定即可。
![[Pasted image 20230729001412.png]]

**方法三、**
1、如果使用的是win11专业版或者是更高版本，则可以使用组策略来禁用自动更新。首先点击开始按钮，输入gpedit.msc，点击选择最上方的结果；
![[Pasted image 20230729001433.png]]
2、本地组策略编辑器打开时，转到以下路径：**ComputerConfiguration》AdministrativeTemplates》WindowsComponents》WindowsUpdate》Manageenduserexperience**
![[Pasted image 20230729001512.png]]3、点击配置自动更新策略；
![[Pasted image 20230729001541.png]]4、点击已禁用选项以永久关闭Windows11的自动更新；
5、最后点击屏幕底部的确定即可。![[Pasted image 20230729001559.png]]
