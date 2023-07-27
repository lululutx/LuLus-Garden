![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662467769423-1c19520b-1cba-432e-85fa-2788b16704d3.png#averageHue=%23ecd5b2&clientId=u6aaacaea-a148-4&from=paste&id=u32010156&originHeight=932&originWidth=1920&originalType=url&ratio=1&rotation=0&showTitle=false&size=190333&status=done&style=none&taskId=u4bd30c90-25b3-45ef-82df-135043bd84b&title=)![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662467772521-00b1693f-f3c4-463e-b0be-5128ea9ee29c.png#averageHue=%23ecd5b2&clientId=u6aaacaea-a148-4&from=paste&id=u47a2c886&originHeight=932&originWidth=1920&originalType=url&ratio=1&rotation=0&showTitle=false&size=187203&status=done&style=none&taskId=uce8bde8f-8a58-4466-b1c0-7a0f3aece84&title=)<br />对滚动容器的父容器或者父父容器进行 <br />overflow:hidden;  

要对滚动的容器进行 <br />overflow-y: scroll; 

```vue
另一个问题 
 
经常我们会使用flex布局，但是flex布局常常会遇到一些不可思议的麻烦，下面介绍一下overflow遇到的麻烦 
  
我在工作中使用了左右两栏布局 
 

.container { 
    display: flex; 
} 
.left { 
    flex: 1; 
    overflow: auto; 
} 
.right { 
    width: 500px; 
} 
 

  
我的想法是左边宽度要自适应，而且需要滚动条，虽然这样设置了，但奇怪的事情发生，左边的内容并没有出现滚动条，通过查阅资料，可以通过如下办法解决 
 

.container { 
    display: flex; 
} 
.left { 
    flex: 1; 
    overflow: auto; 
    width: 0; 
} 
.right { 
    width: 500px; 
} 
 

  
样式需要多加一个 width: 0;  
 
来自 <https://www.cnblogs.com/wuxianqiang/p/9282040.html>  
 
 

```
