#Cesium

在基于 vue 框架+Cesium产品做三维应用开发的项目中，比较常见的问题是性能卡顿、帧率低。经过排查，发现普遍是由以下问题引起的，现总结出引起相关问题的原因及解决办法，以供参考：

## 1. 任意 Cesium 对象放入到 store、data、computed 中后，会引起系统越用越卡。

vue 响应式框架对页面数据的渲染非常友好，会把 data 内所有对象的属性都转换成 get,set 进行监听。 但是将 Cesium 的任意对象直接挂载到 vue 的 data 对象上时，Cesium 的对象被双向绑定后，会造成无法自动释放、整个页面的渲染效率，降低帧率，越用越卡，特别在有加载 3dtiles 大体量模型场景时，影响更加明显。

> 正确做法: 将 map、viewer、graphicLayer 等对象作为不要放在 store、data、computed 中，避免 vue 劫持 map。

![image](http://mars3d.cn/dev/img/guide/issue-vue-data.jpg)

### vue3 中可以使用`markRaw`来标识不进行双向绑定

markRaw 作用：标记一个对象，使其永远不会再成为响应式对象。

![image](http://mars3d.cn/dev/img/guide/issue-vue3-markRaw.jpg)

## 2. 用完的对象之后要及时销毁，防止出现功能在界面上关闭但对象还驻留在内存中的情况。

目前 Cesium 的类有 destroy 方法，在 vue 组件关闭时及时销毁使用完的对象。

> 正确做法: 在 vue 组件的 destroy(vue2)、onUnmounted(vue3)等钩子方法中销毁使用完的对象。

```js
onUnmounted(() => {  
  if(this.tilesetPlanClip){  
    this.map.removeThing(this.tilesetPlanClip, true); //移除并销毁  
    this.tilesetPlanClip = null  
  }  
​  
  if(this.graphicLayer){  
    this.graphicLayer.destroy(); //销毁  
    this.graphicLayer = null  
  }  
});
```

