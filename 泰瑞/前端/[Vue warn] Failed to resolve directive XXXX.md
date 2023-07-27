vue自定义指令报错,得提前给他注册进去<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/26798000/1683680609471-d936fcd3-9a92-4a21-923c-46e4e96fab3f.png#averageHue=%23fee7e4&clientId=uea0edba7-5233-4&from=paste&height=543&id=u8db78211&originHeight=543&originWidth=443&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42117&status=done&style=none&taskId=ua50fe3b4-201f-4961-a1cb-e16e8500675&title=&width=443)

```vue
Vue.directive('test',{
  bind(el,binding,vnode){
    console.log(el);
    el.style.color = "red"
  }
})
new Vue({
  el: '#app',
  components: { App },
  template: '<App/>'
})
```
