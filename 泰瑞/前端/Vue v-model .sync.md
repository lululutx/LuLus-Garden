#Vue 


## 前言

在平时开发过程中，**v-model** 使用频率很高。一般来说，**v-model** 可用于在表单及元素上创建**双向数据绑定**，其本质是**语法糖**。但提到 **语法糖**，这里就不得不提另一个与 **v-model** 有着相似功能的 双向数据绑定 语法糖，那就是 .**sync** 修饰符。

## 正文

### 一、v-model

#### **1、作用**

使用过 **vue 框架**的朋友应该对这个指令不会感到陌生，**v-model** 可用于对 `<input>`、`<textarea>`及 `<select>` 等元素上的数据进行双向绑定的。

一个简单的例子：

```vue
<template>
 <div>
   <input v-model="value" type="text" />
   <span>用户输入：{{value}}</span>
 </div>
</template>
 
<script>
export default {
 data() {
   return {
     value: ''
   }
 }
}
</script>
```

当我们在输入框里面输入了某个值的时候，下面展示部分可以直接获取到我们的输入值，而不需要操作 dom 元素进行获取。

#### 2、本质

v-model 本质上是一个**语法糖**。

习惯写法：

```html
<input v-model="val" type="text" />
```

完整写法：

```html
<input :value="val"  @input="val = $event.target.value" />
```

也可以将 @input 后面写成一个函数，然后在 methods 中进行赋值操作。

要理解这行代码，首先你要知道 input 元素本身有个 input 事件，这是 HTML5 新增加的，类似于 change 事件，每当输入框发生内容变化，就会触发 input 事件，把最新的 value 值传递给 val ，达到双向数据绑定的效果 。

**我们仔细观察语法糖和原始语法那两行代码，可以得出一个结论：**

在给 input 元素添加 v-model 属性时，默认会把 val 作为元素的属性，然后把 input 事件作为实时传递 value 的触发事件。

**注意:**  **不是所有能进行双向绑定的元素都是 input 事件。**

#### 3、特殊用法

一般情况，我们使用 v-model 主要是用于数据的双向绑定，方便获取到用户的输入值。但在某些特殊情况下，我们也可以将 v-model 用于父子组件之间数据的双向绑定。这里需要用到父子传值的相关知识：

```vue
<template>
  <div>
    <son v-model="num"></son>
  </div>
</template>
 
<script>
import son from '@/test/son.vue'   //引入子组件
export default {
  components: {
    son    //注册子组件
  },
  data() {
    return {
      num: 100
    }
  }
}
</script>
```

先定义了一个 father 组件和一个 son 组件，并且将 son 组件引入到 father 组件中。给 son 组件绑定了 v-model 进行传值。此时，我们需要到 son 组件中接收这个值并且使用（通过props）：

```vue
<template>
  <div>
    <span>我是son组件里面接收到的值: {{value}}</span>
  </div>
</template>
 
<script>
export default {
  props: {
    value: {
      type: Number,
      default: 0
    }
  }
}
</script>
```

**注意:**  **son 组件接收的值必须是 value，写成其他名字将无法使用！！！**

一般情况下的父向子传值，子组件中是不能直接修改的，如下我们在子组件中直接修改这个值，会报错：

```vue
<template>
  <div>
    我是son组件里面接收到的值: {{value}}
    <button @click="value+=1">点我value+1</button>  
  </div>
</template>
```

当我们需要修改这个值时，需将其从父组件中的传入的值进行修改（单向数据流）。

**这就需要在父组件中的子组件标签上定义一个自定义的事件，通过在子组件中使用 $emit 的方法将值传入父组件，在父组件中获取并更新，然后再传入子组件。**

但是在本例中我们不能使用自定义的事件，因为我们用的是 v-model 进行传值，所以我们只能使用input 事件进行修改。

在父组件中的子组件标签中定义一个 @input 方法，再设置一个形参 val 接收子组件的传值：

```vue
<template>
  <div>
    {{num}}
    <son v-model="num" @input="val => num = val" />
  </div>
</template>
```

子组件中使用 $emit 方法，调用父组件中的 input 事件，并且进行传值：

```vue
<template>
  <div>
    我是son组件里面接收到的值: {{value}}
    <button @click="$emit('input',value+1)">点我value+1</button>
  </div>
</template>
```

所以该例子整个数据流向为：

父组件（初始值）-> 子组件（初始值）-> 子组件传值（通过$emit("input",newVal)）-> 父组件获取并更新值（通过@input）-> 子组件（更新值）

### 二、.sync

#### 1、作用

相比较于 v-model 来说，.sync 便简单得多。

**.sync 修饰符可以实现子组件与父组件的双向绑定，并且可以实现子组件同步修改父组件的值。**

```vue
<son
  :a="num" @update:a="val => num = val"
  :b="num2" @update:b="val => num2 = val">
</son> 
```

#### 2、本质

正常父传子：

```html
<son :a="num" :b="num2"></son>
```

使用 .sync 之后的父传子：

```html
<son :a.sync="num" .b.sync="num2"></son> 
```

其等价于：

```vue
<son
  :a="num" @update:a="val => num = val"
  :b="num2" @update:b="val => num2 = val">
</son> 
// 相当于多了一个事件监听，事件名是 update:a，回调函数中，会把接收到的值赋值给属性绑定的数据项中。
```

**注意：这里的传值与接收与正常的父向子传值没有区别，唯一的区别在于往回传值的时候 $emit 所调用的事件名必须是 `update:属性名，`事件名写错不会报错，但是也不会有任何的改变。**

```javascript
this.$emit("update:a",newVal)
```

## 总结

### 一、v-model 与 .sync 的区别

#### 1、相同点

都是语法糖，都可以实现父子组件中的数据的双向通信。

#### 2、区别点

使用格式：

v-model="num"，:num.sync="num"

传值方式：

v-model：@input 、$emit("input")  
:prop.sync：@update:prop 、$emit("update:prop")

**注意：v-model** **只能有一个；.sync 可以有多个。**