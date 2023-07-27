```vue
<!-- Maximum call stack size exceeded错误描述 
 
其实就是超出最大调用堆栈大小，查了很长时间，总结解决方法如下几种： 
 
1、最常见的原因就是：递归函数出错 
检查递归函数是否具有停止调用的判断条件，解决后，就不会有堆栈溢出了。 
 
2、路由拦截出错 
问题代码如下：想实现的是，路由拦截，不允许乱跳转页面  -->
 
router.beforeEach((to, from, next) => { 
  if (to.path === '/login') next() 
  const tokenStr = window.sessionStorage.getItem('token') 
  if (!tokenStr) return next('/login') 
  next() 
}) 

解决如下： 
 
router.beforeEach((to, from, next) => { 
  if (to.path === '/login') next() 
  const tokenStr = window.sessionStorage.getItem('token') 
  // 增加判断条件 
  if (!tokenStr && to.path !== '/login') return next('/login') 
  next() 
}) 

3、如果没有递归模块，查看路由拦截器重定向错误 
如：想法是访问不存在的页面，跳转到404页面 
 
{ path: '*', redirect: '/404' } 

错误描述：页面 /404 在路由里面没有配置，故引发出错 
解决：使用路由时，先配置注册这个页面 
———————————————— 
版权声明：本文为CSDN博主「lvan-ah」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。 
原文链接：https://blog.csdn.net/weixin_43848802/article/details/106310630 

```
