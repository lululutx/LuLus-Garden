```javascript
// vueToDom.js

import Vue from 'vue'
export function CreateDom(obj) {   // 创建一个构造函数
  const vm_obj = obj;   // 存储传入的参数对象
  function _getDomItem() {    // 定义一个转换dom方法
    const extendVM = Vue.extend(vm_obj);  // 创建一个vue构造器
    const vue_tmp = new extendVM();    // 实例化构造器
    const dom = vue_tmp.$mount().$el;  // 手动地挂载一个未挂载的实例,并获取跟元素
    return {dom,vue_tmp};//返回1.Dom元素对象;2.Vue实例对象
  }
  return {
    getDomItem: _getDomItem
  }
}


```

此处camera为自己写的vue组件
```javascript
this.newDom = new CreateDom({
  template: `<camera :id="${new Date().getTime()}" ref="CAMERA1"
    :cam-url="${item.url}" /> `,
  data() {
    return {};
  },
  created() {
    // 获取当前页面的数据
  },
  components: {
    camera,
  },
});

let contain = document.getElementById("cameraContain");
contain.appendChild(this.newDom.getDomItem().dom);
```
使用：
```javascript
this.newDom.getDomItem().dom
this.newDom.getDomItem().vue_tmp
```
