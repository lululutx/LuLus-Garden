
### 父传子

父传子可以吧参数拼进url中，子页面解析获取 

```js
  //获取url里面的参数 
  const getQueryVariable = (variable: String) => { 
    var query = window.location.search.substring(1); 
    var vars = query.split("&"); 
    for (var i = 0; i < vars.length; i++) { 
      var pair = vars[i].split("="); 
      if (pair[0] === variable) { 
        return pair[1]; 
      } 
    } 
    return ""; 
  }; 
```


### 子传父

子传父用window.parent.PostMessage方法，并且在父页面监听 

```js
        window.parent.postMessage( 
          //参数往里塞,建议加个type,在父页面进行判断分类.因为父页面是全监听,如果不加判断容易监听到别的业务 
          { 
            phone: num, 
            password: values, 
          }, 
          //第二个参数,dev模式下可以忽略,但是发布后会出错,所以随便加个参数 
          "*" 
        ); 
 
```

```js
  mounted() { 
    //接受子iframe传来的数据 
    window.onmessage = (e) => { 
      this.listen(e.data); 
    }; 
  }, 
 
 
 
    listen(data) { 
      if (data.type == "???") { 
         
      } else { 
        return; 
      } 
    }, 
 
```