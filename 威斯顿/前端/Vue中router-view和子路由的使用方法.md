```vue
// 首先在/router/index.js配置子路由 
 
{ 
      path: '/Home', 
      name: 'Home', 
      component: Home, 
      meta: { requireAuth: false }, 
      children: [ 
        { 
          path: '/homeIndex', 
          name: 'homeIndex', 
          component: homeIndex 
        }, 
        { 
          path: '/aboutUs', 
          name: 'aboutUs', 
          component: aboutUs 
        }, 
      ] 
    }, 
 
 
 

// 父路由Home.vue文件中，增加router-view块，写法如下 

 
<template> 
  <div id="home"> 
    <homeHeader></homeHeader> 
    <router-view  /> 
    <commFooter></commFooter> 
  </div> 
</template> 
// 切换路由，有很多方法，可以用router-link，也可以用element组件 
  <div class="tab"> 
     <router-link to="/content1">content1</router-link> 
     <router-link to="/content2">content2</router-link> 
     <router-link to="/content3">content3</router-link> 
     <router-link to="/content4">content4</router-link> 
  </div> 


  this.$router.push()//可以传入参数
```
