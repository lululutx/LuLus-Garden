![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662463138493-b5e9afc9-15fd-4656-8e9f-08cdc8763c11.png#averageHue=%23252e31&clientId=ueabe4968-6303-4&from=paste&id=ubc811f2e&originHeight=325&originWidth=737&originalType=url&ratio=1&rotation=0&showTitle=false&size=47512&status=done&style=none&taskId=u6c128205-d440-480b-90ec-3658bb47231&title=)
```vue

  for (var key in data) { 
    var a = []; 
    for (var i = 0; i < data[key].length; i++) { 
      if (data[key][i].replyId) { 
        data[key][i].replyId = data[key][i].replyId.c.join(""); 
        a.push(data[key][i]); 
      } 
    } 
    if (a.length > 0) { 
      b.push(a); 
    } 
  } 

```
