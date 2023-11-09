#Vue 

### 用法

keep-alive 是  `Vue`  的内置组件，当它包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和 transition 相似，keep-alive 是一个抽象组件：它自身不会渲染成一个 DOM 元素，也不会出现在父组件链中。当组件在 `<keep-alive>` 内被切换，它的 `activated` 和 `deactivated` 这两个 `生命周期` 钩子函数将会被对应执行。

> 在 2.2.0 及其更高版本中，`activated` 和 `deactivated` 将会在 `<keep-alive>` 树内的所有嵌套组件中触发。

### 作用

在组件切换过程中将状态保留在缓存中，防止重复渲染DOM，减少加载时间及性能消耗，优化用户体验。（主要用于保留组件状态或避免重新渲染）

### Props

- **`include`** - 字符串或正则表达式。只有名称匹配的组件会被缓存。
- **`exclude`** - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。
- **`max`** - 数字。最多可以缓存多少组件实例。

### 生命周期钩子

**activated**

在 keep-alive 组件激活时调用（该 钩子函数 在服务器端渲染期间不被调用）。

**deactivated**

在 keep-alive 组件失活时调用（该钩子在服务器端渲染期间不被调用）。

被包含在 keep-alive 中的组件，会多出两个生命周期的钩子：activated 与 deactivated，使用 keep-alive 会将数据保留在缓存中，如果要在每次进入页面的时候获取最新的数据，需要在activated 阶段获取数据（因为被包裹的组件只有在首次渲染才会去执行created、mounted等钩子），承担原来在created、mounted等钩子获取数据的任务。

**注意：** 只有组件被 keep-alive 包裹时，这两个生命周期函数才会被调用，如果作为正常组件使用，是不会被调用的，以及在 2.1.0 版本之后，使用 exclude 排除之后，就算被包裹在 keep-alive 中，这两个钩子函数依然不会被调用！另外，在服务端渲染时，此钩子函数也不会被调用。

### 示例

**缓存（所有）路由组件**

App.vue（未使用keep-alive）

```vue
<template>
  <div id="app">
    <div id="nav">
      <div class="main-content">
        <router-link to="/home" active-class="isActive">Home</router-link>
        <span>|</span>
        <router-link to="/about" active-class="isActive">About</router-link>
        <div class="router-view-content">
          <!-- 使用 <router-view> 渲染路径匹配到的视图组件 -->
          <router-view />
        </div>
      </div>
    </div>
  </div>
</template>
 
<script>
export default {
  name: "App"
};
</script>
```

路由组件1：Home.vue

```vue
<template>
  <div class="home">
    <p>This is a Home page</p>
    <el-input v-model="inputText" placeholder="请输入" style="width:50%"></el-input>
  </div>
</template>
 
<script>
export default {
  name: "Home",
  data() {
    return {
      inputText: "",
    };
  },
};
</script>
```

路由组件2：About.vue

```vue
<template>
  <div class="about">
    <p>This is an About page</p>
    <el-select v-model="value" placeholder="请选择" style="width: 50%">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      >
      </el-option>
    </el-select>
  </div>
</template>
 
<script>
export default {
  name: "About",
  data() {
    return {
      value: "",
      options: [
        {
          value: "zhangsan",
          label: "张三",
        },
        {
          value: "lisi",
          label: "李四",
        },
      ],
    };
  },
};
</script>
```

在未使用keep-alive情况下，在Home页面输入框输入内容

![](https://img-blog.csdnimg.cn/45e455af90a9404689ee4226e657c669.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

然后切换到About页面，在选择框选择内容

![](https://img-blog.csdnimg.cn/ab117d671be3458084d4327ddb6d953b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

之后进行一次来回切换，此时切换回Home页面，原先输入框的内容消失；相同的，切换回About页面，选择框的内容也消失，其实是因为这两个组件被销毁之后重新渲染了。

加上keep-alive，重复上面的操作，发现保留了组件状态（不销毁组件），避免了重新渲染。

![](https://img-blog.csdnimg.cn/37a47874d0dd41109eae6f94ba882102.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

**根据条件缓存页面**

如App.vue，使用**include**，此时只缓存Home页面的状态

```vue
<template>
  <div id="app">
    <div id="nav">
      <div class="main-content">
        <router-link to="/home" active-class="isActive">Home</router-link>
        <span>|</span>
        <router-link to="/about" active-class="isActive">About</router-link>
        <div class="router-view-content">
          <!-- 使用 <keep-alive> 按条件缓存路由组件，使用include -->
          <keep-alive include="Home">
            <!-- 使用 <router-view> 渲染路径匹配到的视图组件 -->
            <router-view />
          </keep-alive>
        </div>
      </div>
    </div>
  </div>
</template>
 
<script>
export default {
  name: "App"
};
</script>
```

或者使用**exclude**，如下，表示缓存除了Home之外其他页面的状态

```vue
<template>
  <div id="app">
    <div id="nav">
      <div class="main-content">
        <router-link to="/home" active-class="isActive">Home</router-link>
        <span>|</span>
        <router-link to="/about" active-class="isActive">About</router-link>
        <div class="router-view-content">
          <!-- 使用 <keep-alive> 按条件缓存路由组件，使用exclude -->
          <keep-alive exclude="Home">
            <!-- 使用 <router-view> 渲染路径匹配到的视图组件 -->
            <router-view />
          </keep-alive>
        </div>
      </div>
    </div>
  </div>
</template>
 
<script>
export default {
  name: "App"
};
</script>
```

**`include`** 和 **`exclude`** 允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示。匹配时，首先检查组件自身的 `name` 选项，如果 `name` 选项不可用，则匹配它的局部注册名称 (父组件 `components` 选项的键值)。匿名组件不能被匹配。

除了以上方式，另外一种对**路由组件部分缓存**

修改路由配置

![](https://img-blog.csdnimg.cn/78a65af322ea49909ce0170229237a95.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

修改App.vue

```vue
<template>
  <div id="app">
    <div id="nav">
      <div class="main-content">
        <router-link to="/home" active-class="isActive">Home</router-link>
        <span>|</span>
        <router-link to="/about" active-class="isActive">About</router-link>
        <div class="router-view-content">
          <keep-alive>
            <!-- keepAlive为true，则使用 <keep-alive> 进行缓存 -->
            <router-view v-if="$route.meta.keepAlive"></router-view>
          </keep-alive>
 
          <!-- keepAlive为false，则不缓存 -->
          <router-view v-if="!$route.meta.keepAlive"></router-view>
        </div>
      </div>
    </div>
  </div>
</template>
 
<script>
export default {
  name: "App",
};
</script>
```

通过 $route.meta.keepAlive（即配置中的keepAlive）决定那些路由组件需要被缓存。

### 其他

```vue
<!-- 基本 -->
<keep-alive>
  <component :is="view"></component>
</keep-alive>
 
<!-- 多个条件判断的子组件 -->
<keep-alive>
  <comp-a v-if="a > 1"></comp-a>
  <comp-b v-else></comp-b>
</keep-alive>
 
<!-- 和 `<transition>` 一起使用 -->
<transition>
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>
</transition>
```

**注意：**`<keep-alive>` 是用在其一个直属的子组件被开关的情形。如果你在其中有 `v-for` 则不会工作。如果有上述的多个条件性的子元素，`<keep-alive>` 要求同时只有一个子元素被渲染。

以上就是**`keep-alive`**的基本用法，它包裹的组件（即被缓存的组件），再次进入时执行**activated**钩子，离开时执行**deactivated**钩子，可以利用此进行一些操作，如请求数据，解除监听等。