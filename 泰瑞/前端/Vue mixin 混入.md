#Vue 

from [Vue 混入（mixin）详细介绍（可复用性、全局混入）](https://blog.csdn.net/qq_41809113/article/details/121912330)
## 基础

混入（mixin）提供了一种非常灵活的方式，来分发 [Vue](https://so.csdn.net/so/search?q=Vue&spm=1001.2101.3001.7020) 组件中的可复用功能。一个混入对象可以包含任意组件选项（如data、methods、mounted等等）。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。 

### 基本使用

1、定义一个混入（mixin）

![](https://img-blog.csdnimg.cn/391d4f2d536e4b0cab78dbd56b262cd4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

```js
let mixin = {
  created() {
    console.log("我是mixin里面的created!")
  },
  methods: {
    hello() {
      console.log("hello from mixin!")
    }
  }
}
 
export default mixin
```

2、在组件（Home.vue）中使用

![](https://img-blog.csdnimg.cn/af21c7a4069e4c478cba30821f9f04bb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

```vue
<template>
  <div class="home">
    <span>This is a Home page</span>
  </div>
</template>
 
<script>
import myMixins from "../mixins";   //导入混入（mixin）
export default {
  name: 'Home',
  mixins: [myMixins]     //使用混入（mixin）
}
</script>
```

注意观察Home组件里面，并没有任何的生命周期钩子或者方法，但是打开页面之后查看控制台，却打印了内容，这就是混入对象中的created钩子中的console.log()。

![](https://img-blog.csdnimg.cn/e687dcd133644950a84d0baad3acf48f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

现在，在Home组件加上mounted生命周期钩子

![](https://img-blog.csdnimg.cn/be0d9326b1e14a3a82780cadce1e8840.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

```vue
<template>
  <div class="home">
    <span>This is a Home page</span>
  </div>
</template>
 
<script>
import myMixins from "../mixins";
export default {
  name: 'Home',
  mixins: [myMixins],
  mounted(){         
    this.hello()     //在该组件中并没用定义hello方法，使用的是混入中的hello
  }
}
</script>
```

刷新页面，查看控制台打印信息

![](https://img-blog.csdnimg.cn/f65aced468c14ee7ae71ce12fc45f8ea.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)非但没报错， 还打印出了混入对象中定义的hello方法，即执行了混入对象中的mounted生命周期钩子，也验证了当前组件中合并了混入对象中定义的选项和方法。这就是混入的基本使用。

## 选项合并

当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。

如下，我们在Home.vue中定义与混入对象中同名的选项

mixin.js

![](https://img-blog.csdnimg.cn/60c397b967db4af492bae774be901e76.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

Home.vue 

![](https://img-blog.csdnimg.cn/10107f8eabf849739acbbb51dc2e5b1d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

刷新页面，查看控制台打印信息

![](https://img-blog.csdnimg.cn/3aa62990db7449a8875a84b73a2789f9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

此时，同名的created生命周期钩子进行了合并，合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。而methods中的hello方法因为冲突，在合并时保留组件中的hello，即优先当前组件的选项，因此打印的是“hello from Home!”。

值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

## 可复用性

开篇提过，混入对象可成为一个可复用功能，我们在另外的组件中引入已定义的混入对象，可实现同样的逻辑与功能。

如在另外一个组件About.vue使用该混入对象

![](https://img-blog.csdnimg.cn/ce15a00df5ab4f91bf8f9ec7314fdd82.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

```vue
<template>
  <div class="about">
    <span>This is an About page</span>
  </div>
</template>
 
<script>
import myMixins from "../mixins";
export default {
  name: "About",
  mixins: [myMixins]
};
</script>
```

刷新页面，进行切换，查看控制台打印信息

![](https://img-blog.csdnimg.cn/4ae4fa83bf1f41e18ad4b528359df367.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

![](https://img-blog.csdnimg.cn/cafb6d6b49d84989bcf305b4e7a0e9c5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

混入对象中的console.log均被执行了，实现了逻辑或功能的复用。

那么有个问题，如果很多个组件同时使用同一个混入对象，这时候都要重复一个步骤，就是先导入混入对象，然后再使用，类似如下

```vue
<script>
import myMixins from "../mixins";
export default {
  mixins: [myMixins]
};
</script>
```

这样未免有点繁琐，且代码冗余，此时我们可以使用全局混入来优化这个问题。

## 全局混入

混入也可以进行全局注册。使用时格外小心！一旦使用全局混入，它将影响**每一个**之后创建的 Vue 实例。使用恰当时，这可以用来为自定义选项注入处理逻辑。

在main.js中通过Vue.mixin()引入混入对象即可全局使用（作用于该Vue实例下的所有组件）

![](https://img-blog.csdnimg.cn/34a834298b6f4bbc8aa257ce019d097d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

```dart
import mixin from './mixins';Vue.mixin(mixin)
```

此时我们将Home.vue、About.vue中使用混入对象的代码注释

![](https://img-blog.csdnimg.cn/18528d0f2d2e495abbd1550d75b28303.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

![](https://img-blog.csdnimg.cn/ab09c706dcfe4dc7ad04932a930cda85.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

刷新页面，进行切换，查看控制台打印信息

![](https://img-blog.csdnimg.cn/b3dbe4ae0c7a4c1598145b26ab6df255.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

效果与在每个组件中单独引入混入对象相同，这便是全局混入。

> 请谨慎使用全局混入，因为它会影响每个单独创建的 Vue 实例 (包括第三方组件)。大多数情况下，只应当应用于自定义选项，就像上面示例一样。推荐将其作为[插件](https://cn.vuejs.org/v2/guide/plugins.html "插件")发布，以避免重复应用混入。