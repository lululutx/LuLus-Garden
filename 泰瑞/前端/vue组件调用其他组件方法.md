### from [https://blog.csdn.net/weixin_44867717/article/details/125657669](https://blog.csdn.net/weixin_44867717/article/details/125657669)
### Vue中，一个组件调用其他组件的方法（非父子组件）
**场景**——B页面（组件）想调用 A页面(组件)中的方法；但是两个页面（组件）毫无关联（刷新 A的数据）。
#### 方式一：引用式
##### 1、当前组件引入将要调用方法所属的组件
这里我的当前组件要调用OrderDetail这个组件的check()方法
```
import Detail from "./detail.vue";
```
该方法定义在OrderDetail的methods属性中
##### 2、当前组件通过该组件methods属性直接调用该方法
```
// 也可以调用 created、data等
Detail.methods.check();
```
#### 方式二：vuex

- 使用 **VueX 定义一个属性** ，然后在**A页面 定义一个计算属性**（computed） 再把 vuex 的属性返回给它，<br />再监听这个计算属性，发生变化就调用你要执行的方法。

1、src/store/index.js
```
//  Vuex   全局
state: { 
	tableStatus:false  
	 	}
 mutations:{  
	changeStatus(state,status) {  //  重复赋值
      state.tableStatus = status;
    },
}
```
2、被使用组件- A 页面（组件）
```
//  A 页面（组件）
computed: {  
  status() {  //  计算属性
    return this.$store.state.tableStatus; //  Vuex 中定义的属性
  }
},
watch:{
  status() {
    this.getTableList();  //   需要调用的方法
  }
},
```
3、使用触发页面-B 页面（组件）<br />然后就是在B 页面定义一个点击事件（举例），重新给 Vuex中的属性赋值就行了
```
//  B页面（组件）
closePage() {
    let status = this.$store.state.tableStatus;   // 重新赋值
    this.$store.commit("changeStatus", !status);
},
```
#### 方式三：使用事件总线eventBus定义全局事件
1、src/main.js
```
window.eventBus = new Vue();
```
2、触发页面-B组件/发布事件
```
window.eventBus.$emit('setFeaturesData', data)  // 带参数
window.eventBus.$emit('setFeaturesData')  // 不带参数
```
3、接收页面-A组件/订阅事件
```
window.eventBus.$on('setFeaturesData', (data)=>{ // 带参数
      this.hoveredFeatures = [data]
      this.onClick()
    })

  mounted() {
    this.getTableData()
    window.eventBus.$on('setFeaturesData', ()=>{ // 不带参数
      this.getTableData()
    })
  },

```
4、移除事件
```
window.eventBus.$off('setFeaturesData')
```

