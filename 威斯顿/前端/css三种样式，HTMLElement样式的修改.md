# <br />
```vue

 //先说HTMLElement样式的修改，建议使用穿透式 
      let tempContent = ` 
        <div 
           style="margin-top:15px; 
           font-size:12px; 
           color:#000; 
           background-color:#fff; 
           padding:10px; 
           width:200px; 
           display:flex; 
           flex-direction:column; 
           box-shadow: 0 0 5px rgba(77, 108, 108, 0.5); 
           " 
           class="_map_info_class" 
        > 
           <span style="white-space:nowrap;text-align:left">${name}</span> 
           <span style="margin-top:5px;text-align:left">${where}</span> 
        </div>`; 
 
 
>>> ._map_info_class { 
} 
>>> ._map_info_class:before { 
  content: ""; 
  width: 0; 
  height: 0; 
  border: 10px solid transparent; /*调整小三角形的大小*/ 
  border-top-color: #fff; /*改变小三角的颜色（最好就是和父级div的背景颜色相同）;bottom是用于调整小三角的方向*/ 
  position: absolute; 
  left: 90px; /*调整小三角形的位置*/ 
  bottom: -20px; /*调整小三角形的位置*/ 
} 

```
[CSS中的内部样式、内联样式、外部样式。（优先级）](https://www.cnblogs.com/wwdahaoren/p/12307574.html) 

![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662467372834-d6230527-47e9-4ce3-87af-45c056410abe.png#clientId=ua159844b-986b-4&from=paste&id=uab732a28&originHeight=571&originWidth=752&originalType=url&ratio=1&rotation=0&showTitle=false&size=49146&status=done&style=none&taskId=u9fe51089-1382-4489-bb00-757c8f5780e&title=)


![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662467372826-095adeea-3d8e-4d86-a797-5e39456c27a1.png#clientId=ua159844b-986b-4&from=paste&id=uf23eeed7&originHeight=152&originWidth=514&originalType=url&ratio=1&rotation=0&showTitle=false&size=6919&status=done&style=none&taskId=ucd919eb1-2fde-4ec7-8a1d-843c57771d5&title=)

**优先级：**内部样式和外部样式优先级相同，那么后写入的优先级高一些。 

来自 <[https://www.cnblogs.com/wwdahaoren/p/12307574.html](https://www.cnblogs.com/wwdahaoren/p/12307574.html)>  
