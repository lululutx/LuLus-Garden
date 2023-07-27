```vue
 
      let userSearch = document.getElementsByClassName("user_search")[0]; 
      let mainBody = document.getElementsByClassName("main_body")[0]; 
      let elCalendarHeader = document.getElementsByClassName( 
        "el-calendar__header" 
      )[0]; 
       
          //将user_search的父节点由mainBody改为elCalendarHeader 
      mainBody.removeChild(userSearch); 
      elCalendarHeader.appendChild(userSearch); 
 
 
 
 
 
//getElementsByClassName 可以换成  getElementById 这样结果就不是数组，就不用加[0]了 

```
