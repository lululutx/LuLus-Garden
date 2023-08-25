#Vue 

### 背景

通常，当我们需要从父组件向子组件传递数据时，我们使用 [props](https://v3.cn.vuejs.org/guide/component-props.html "props")。想象一下这样的结构：有一些深度嵌套的组件，而深层的子组件只需要父组件的部分内容。在这种情况下，如果仍然将 prop 沿着组件链逐级传递下去，可能会很麻烦。

对于这种情况，我们可以使用一对 `provide` 和 `inject`。无论组件层次结构有多深，父组件都可以作为其所有子组件的依赖提供者。这个特性有两个部分：父组件有一个 `provide` 选项来提供数据，子组件有一个 `inject` 选项来开始使用这些数据。

![Provide/inject scheme](https://img-blog.csdnimg.cn/img_convert/67909e79cefb8df84e22c0692f26bf9a.png)

### 使用

假设有一个组件A，A组件引入B组件（A为B的父组件） ，B组件引入C组件（B为C的父组件），即A为C的祖先组件，此时二者可以使用provide / inject进行通信。

举例

A.vue

```vue
<template>
  <div>
    <B></B>
  </div>
</template>
 
<script>
import B from "./B.vue";
export default {
  name: "A",
  components: {
    B
  },
};
</script>
```

B.vue

```vue
<template>
  <div>
    <C></C>
  </div>
</template>
 
<script>
import C from "./C.vue";
export default {
  name: "B",
  components: {
    C
  },
};
</script>
```

C.vue

```vue
<template>
  <div>
  </div>
</template>
 
<script>
export default {
  name: "C"
};
</script>
```

A与C使用provide / inject方式进行通信

A使用provide

```vue
<template>
  <div>
    <B></B>
  </div>
</template>
 
<script>
import B from "./B.vue";
export default {
  name: "A",
  components: {
    B
  },
  provide:{
    name:"leo"
  }
};
</script>
```

C使用inject

```vue
<template>
  <div>
    <span>{{name}}</span>
  </div>
</template>
 
<script>
export default {
  name: "C",
  inject:["name"]
};
</script>
```

此时，C已经拿到A中的对应的name。但是，我们可能希望：当A中的name是本身某个可变化的数据时，如下：

```vue
<template>
  <div>
    <B></B>
  </div>
</template>
 
<script>
import B from "./B.vue";
export default {
  name: "A",
  components: {
    B
  },
  provide:{
    name:this.name
  },
  data(){
    return {
      name:"leo"
    }
  },
  methods:{
    changeName(){
      this.name = "lion"
    }
  }
};
</script>
```

我们希望当name改变时（如触发changeName方法），对应的C中的name也要相应改变，但是使用以上方式时，C中的name并未随着改变，此时需要我们进一步处理，即处理响应性。

### 处理响应性 

在上面的例子中，如果我们更改了name，这个变化并不会反映在 inject 的 name property 中。这是因为默认情况下，`provide/inject` 绑定_并不是_响应式的。在vue3中，我们可以通过传递一个 `ref` property 或 `reactive` 对象给 `provide` 来改变这种行为（下面展开）。在我们的例子（vue2）中，如果我们想对祖先组件中的更改做出响应，我们需要将 provide 传值进行改变。

A使用provide，此时传入的应是一个响应式对象（如以下的obj）

```vue
<template>
  <div>
    <B></B>
  </div>
</template>
 
<script>
import B from "./B.vue";
export default {
  name: "A",
  components: {
    B
  },
  provide(){
    return {
      obj:this.obj      //传入一个响应式对象
    }
  },
  data(){
    return {
      obj:{
        name:"leo"
      }
    }
  },
  methods:{
    changeName(){
      this.obj.name = "lion"
    }
  }
};
</script>
```

C使用inject

```vue
<template>
  <div>
    <span>{{obj.name}}</span>
  </div>
</template>
 
<script>
export default {
  name: "C",
  inject:["obj"]    //接收响应式对象
};
</script>
```

此时A中的name改变，C中的值也会相应跟着变化。

以上为A向C传数据，如果C向A传数据（或者说C需要改变A中的数据），该如何做？

我们这里不让C直接改变A中的数据，而是将A改变数据的方法通过provide传给C，C执行该方法，触发改变A中的数据。

A使用provide传入一个方法

```vue
<template>
  <div>
    <span>{{obj.name}}</span>
    <B></B>
  </div>
</template>
 
<script>
import B from "./B.vue";
export default {
  name: "A",
  components: {
    B
  },
  provide(){
    return {
      changeVal:this.changeName      //传入一个方法
    }
  },
  data(){
    return {
      obj:{
        name:"leo"
      }
    }
  },
  methods:{
    changeName(val){          //C中触发该方法执行，此时变成"lion"
      this.obj.name = val
    }
  }
};
</script>
```

C使用inject

```vue
<template>
  <div>
    <span @click="changeName">点击改变A组件数据</span>
  </div>
</template>
 
<script>
export default {
  name: "C",
  inject:["changeVal"],    //接收一个方法
  methods:{
    changeName(){
      this.changeVal("lion")     //执行此方法，改变A中的数据
    }
  }
};
</script>
```

以上就是在vue2中对provide / inject的基本使用。

### vue3中使用

**Provide**

在 `setup()` 中使用 `provide` 时，我们首先从 `vue` 显式导入 `provide` 方法。这使我们能够调用 `provide` 来定义每个 property。

`provide` 函数允许你通过两个参数定义 property：

1. name (`<String>` 类型)
2. value

使用A组件，provide 的值可以按如下方式重构：

```vue
<template>
  <C />
</template>
 
<script>
import { provide } from 'vue'
import C from './C.vue'
 
export default {
  components: {
    C
  },
  setup() {
    provide('location', 'North Pole')
    provide('geolocation', {
      longitude: 90,
      latitude: 135
    })
  }
}
</script>
```

**Inject**

在 `setup()` 中使用 `inject` 时，也需要从 `vue` 显式导入。导入以后，我们就可以调用它来定义暴露给我们的组件方式。

`inject` 函数有两个参数：

1. 要 inject 的 property 的 name
2. 默认值 (**可选**)

使用C组件，可以使用以下代码对其进行重构：

```vue
<script>
import { inject } from 'vue'
 
export default {
  setup() {
    const userLocation = inject('location', 'The Universe')
    const userGeolocation = inject('geolocation')
 
    return {
      userLocation,
      userGeolocation
    }
  }
}
</script>
```

上面提到，在vue3中处理响应性的方式

为了增加 provide 值和 inject 值之间的响应性，我们可以在 provide 值时使用 [ref](https://v3.cn.vuejs.org/guide/reactivity-fundamentals.html#%E5%88%9B%E5%BB%BA%E7%8B%AC%E7%AB%8B%E7%9A%84%E5%93%8D%E5%BA%94%E5%BC%8F%E5%80%BC%E4%BD%9C%E4%B8%BA-refs "ref") 或 [reactive](https://v3.cn.vuejs.org/guide/reactivity-fundamentals.html#%E5%A3%B0%E6%98%8E%E5%93%8D%E5%BA%94%E5%BC%8F%E7%8A%B6%E6%80%81 "reactive")。

```vue
<template>
  <C />
</template>
 
<script>
import { provide, reactive, ref } from 'vue'
import C from './C.vue'
 
export default {
  components: {
    C
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })
 
    provide('location', location)
    provide('geolocation', geolocation)
  }
}
</script>
```

现在，如果这两个 property 中有任何更改，C组件也将自动更新！

同样的，需要在C中修改它的祖先组件的数据，需像vue2一样在provide传入一个方法

```vue
<template>
  <C />
</template>
 
<script>
import { provide, reactive, ref } from 'vue'
import C from './C.vue'
 
export default {
  components: {
    C
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })
 
    const updateLocation = () => {
      location.value = 'South Pole'
    }
 
    provide('location', location)
    provide('geolocation', geolocation)
    provide('updateLocation', updateLocation)     //传入一个方法
  }
}
</script>
```

C中使用inject

```vue
<script>
import { inject } from 'vue'
 
export default {
  setup() {
    const userLocation = inject('location', 'The Universe')
    const userGeolocation = inject('geolocation')
    const updateUserLocation = inject('updateLocation')
 
    return {
      userLocation,
      userGeolocation,
      updateUserLocation      //执行该方法，触发祖先组件方法执行，从而改变数据
    }
  }
}
</script>
```

最后，如果要确保通过 `provide` 传递的数据不会被 inject 的组件更改，我们建议对提供者的 property 使用 `readonly`。

```vue
<template>
  <C />
</template>
 
<script>
import { provide, reactive, readonly, ref } from 'vue'
import C from './C.vue'
 
export default {
  components: {
    C
  },
  setup() {
    const location = ref('North Pole')
    const geolocation = reactive({
      longitude: 90,
      latitude: 135
    })
 
    const updateLocation = () => {
      location.value = 'South Pole'
    }
    // 使用readonly，数据只读
    provide('location', readonly(location))
    provide('geolocation', readonly(geolocation))
    provide('updateLocation', updateLocation)
  }
}
</script>
```

**总结**：主要介绍了provide / inject 的基本使用以及在vue2、vue3中使用的区别。