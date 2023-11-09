#Vue 

from [下班前几分钟，我学会了如何使用 Vuex](https://blog.csdn.net/qq_41809113/article/details/123118315)

# 正文

## 一、基本概念

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式 **存储管理** 应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

## 二、项目场景

如果你的项目里有很多页面（组件/视图），页面之间存在多级的嵌套关系，此时，如果这些页面都需要共享一个状态的时候，此时就会产生以下两个问题：

- 多个视图依赖同一个状态
    
- 来自不同视图的行为需要变更同一个状态


解决方案（初版）：

- 对于第一个问题，假如是多级嵌套关系，你可以使用父子组件传参进行解决，虽有些麻烦，但好在可以解决；对于兄弟组件或者关系更复杂组件之间，虽然可以通过各种各样的办法解决，可实在很不优雅，而且等项目做大了，代码量愈发巨大，实在令人心烦。
    
- 对于第二个问题，你可以通过父子组件直接引用，或者通过事件来变更或者同步状态的多份拷贝，但是这种模式很脆弱，使得代码难以维护，而且同样会让代码量剧增。


思路：

- 把各个组件都需要依赖的同一个状态抽取出来，全局统一管理。
    
- 在这种模式下，任何组件都可以直接访问到这个状态，或者当状态发生改变时，所有的组件都相应更新。


此时，Vuex 就诞生了。 这就是它背后的基本思想，借鉴了 Flux、Redux。与其他模式不同的是，Vuex 是专门为 Vue 设计的状态管理库，以利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新。

## 三、如何使用

### 1、安装

进入项目，键入以下命令：

```
npm install vuex --save
```

### 2、State 初始值

在 src 路径下创建 store 文件夹，并在该文件夹下新增 index.js 文件：

```js
import Vue from 'vue';
import Vuex from 'vuex';
 
Vue.use(Vuex);
 
const store = new Vuex.Store({
    state: {
        // 定义一个name，以供全局使用
        name: 'leo'
    }
});
 
export default store;
```

修改 main.js： 

```javascript
import Vue from 'vue';
import App from './App';
import store from './store';   // 引入我们前面导出的store对象
 
Vue.config.productionTip = false;
 
new Vue({
    el: '#app',
    store,   // 把store对象添加到vue实例上
    components: { App },
    template: '<App/>'
});
```

最后修改 App.vue：

```vue
<template>
    <div></div>
</template>
 
<script>
    export default {
        mounted() {
            // 使用this.$store.state.xxx可以直接访问到仓库中的状态
            console.log(this.$store.state.name);   // 'leo'
        }
    }
</script>
```

建议一：this.$store.state.xxx 放在计算属性中，这样可以让你的代码看起来更优雅一些。 

```javascript
export default {
    mounted() {
        console.log(this.getName);   // 'leo'
    },
    computed:{
        getName() {
            return this.$store.state.name;
        }
    }
}
```

建议二：每次都写 this.$store.state.xxx 让你感到厌烦，造成代码冗余，可以使用 mapState。

```vue
<script>
import { mapState } from 'vuex';   // 从vuex中导入mapState
export default {
    mounted() {
        console.log(this.name);   // 'leo'
    },
    computed: {
        ...mapState(['name'])   // 经过解构后，自动就添加到了计算属性中，此时就可以直接像访问计算属性一样访问它
    }
}
</script>
```

你甚至可以给它取别名：

```javascript
...mapState({ userName: 'name' })   // 赋别名的话，这里接收对象，而不是数组
```

### 3、Getters 修饰值

设想一个场景，你已经将 store 中的 name 展示到页面上了，而且是很多页面都展示了，此时客户要求在每个 name 前都加上“hello”。

最容易想到的办法就是去每个用到 name 的页面一个一个加上“hello”，显然这种方式并不友好，原因如下：

- 第一，假如你在A、B、C三个页面都用到了 name，那么你要在这A、B、C三个页面都修改一遍，多个页面你就要加很多遍这个方法，造成代码冗余，很不好。
    
- 第二，假如下次客户让你把“hello”改成“greet”的时候，你又得把三个页面（甚至更多）都改一遍，这时候你是拿石头砸自己的脚。


吸取上面的教训，我们有一个新的思路：直接在 store 中对 name 进行一些操作或者加工，从源头解决问题！这时候，Getter 闪亮登场！ 

首先，在 store 对象中增加 getters 属性：

```javascript
const store = new Vuex.Store({
    state: {
        name: 'leo'
    },
    // 在store对象中增加getters属性
    getters: {
        getMessage(state) {   // 获取修饰后的name，第一个参数state为必要参数，必须写在形参上
            return `hello ${state.name}`;
        }
    }
});
```

在组件中使用：

```javascript
export default {
    mounted() {
        console.log(this.$store.state.name);
        console.log(this.$store.getters.getMessage);   // 注意不是$store.state了，而是$store.getters
    }
}
```

建议使用 mapGetters：

```javascript
<script>
import { mapState, mapGetters } from 'vuex';
export default {
    mounted() {
        console.log(this.name);   // 'leo'
        console.log(this.getMessage);   // 'hello leo' 
    },
    computed: {
        ...mapState(['name']),
        ...mapGetters(['getMessage'])
    },
}
</script>
```

你也可以给它取别名： 

```javascript
...mapGetters({ greetMsg: 'getMessage' })   // 赋别名的话，这里接收对象，而不是数组
```

### 4、Mutations 修改值

说到修改值，有的人可能会想到这样写：

```javascript
this.$store.state.xxx = XXX;
```

首先，这是一种错误的写法。为什么说是错误的写法？因为这个 store 仓库比较奇怪，你可以随便拿，但是你不能随便改，举个例子：

假如你打开朋友圈，看到你的好友发了条动态，但是动态里有个错别字，你要怎么办呢？你可以帮他改掉吗？当然不可以！我们只能通知他本人去修改，因为是别人的朋友圈，你是无权操作的，只有他自己才能操作。同理，在 Vuex 中，我们不能直接修改仓库里的值，必须用 Vuex 自带的方法去修改。这个时候，Mutation 闪亮登场！

首先，修改 store/index.js：

```javascript
const store = new Vuex.Store({
    state: {
        name: 'leo',
        number: 0,
    },
    mutations: {   // 增加nutations属性
        setNumber(state) {   // 增加一个mutations的方法，方法的作用是让number从0变成5，state是必须默认参数
            state.number = 5;
        }
    }
});
```

然后，修改 App.vue：

```vue
<script>
export default {
    mounted() {
        console.log(this.$store.state.number);   // 旧值，0
        this.$store.commit('setNumber');
        console.log(this.$store.state.number);   // 新值，5
    },
}
</script>
```

如果我们想传动态参数怎么办？

首先，修改 store/index.js：

```javascript
const store = new Vuex.Store({
    state: {
        name: 'leo',
        number: 0,
    },
    mutations: {
        setNumber(state, num) {   // 增加一个带参数的mutations方法
            state.number = num;
        },
    },
});
```

相应地，修改 App.vue：

```vue
<script>
export default {
    mounted() {
        console.log(this.$store.state.number);   // 旧值，0
        this.$store.commit('setNumber', 666);
        console.log(this.$store.state.number);   // 新值，666
    },
}
</script>
```

上面的这种传参的方式虽然可以达到目的，但是并不推荐。建议传递一个对象，看起来更美观，对象的名字你可以随意命名，一般命名为 payload：

```javascript
mutations: {
    setNumber(state, payload) {   // 增加一个带参数的mutations方法，并且官方建议payload为一个对象
        state.number = payload.number;
    },
},
```

相应地，修改 App.vue：

```vue
<script>
export default {
    mounted() {
        console.log(this.$store.state.number);   // 旧值，0
        this.$store.commit('setNumber', { number: 666 });  // 调用的时候需要传递一个对象
        console.log(this.$store.state.number);   // 新值，666
    },
}
</script>
```

建议使用 mapMutations：

```vue
<script>
import { mapMutations } from 'vuex';
export default {
    mounted() {
        this.setNumber({ number: 666 });
    },
    methods: {   // 注意，mapMutations是解构到methods里面的，而不是计算属性了
        ...mapMutations(['setNumber'])
    }
}
</script>
```

你也可以给它取别名： 

```javascript
methods:{
    ...mapMutations({ changeNumber: 'setNumber' })   // 赋别名的话，这里接收对象，而不是数组
}
```

**注意：Mutations 里面的函数必须是同步操作，不能包含异步操作。**

Mutations 只能进行同步操作，所以，我们马上开始下一节，看看使用 Actions 进行异步操作的时候应该注意什么！

### 5、Actions 异步修改值

Vuex 不希望你将异步操作放在 Mutations 中，所以就给你设置了一个区域，让你进行异步操作，这就是 Actions。

首先，修改 store/index.js：

```javascript
const store = new Vuex.Store({
    state: {
        name: 'leo',
        number: 0,
    },
    mutations: {
        setNumber(state, payload) {
            state.number = payload.number;
        },
    },
    actions: {   // 增加actions属性
        setNum(content) {   // 增加setNum方法，默认第一个参数是content，其值是复制的一份store
            return new Promise(resolve => {  // 模拟异步操作，1秒后修改number为666
                setTimeout(() => {
                    content.commit('setNumber', { number: 666 });
                    resolve();
                }, 1000);
            });
        }
    }
});
```

然后，修改 App.vue：

```javascript
async mounted() {
    console.log(this.$store.state.number);   // 旧值，0
    await this.$store.dispatch('setNum');   // 等待异步操作完成
    console.log(this.$store.state.number);   // 新值，666
}
```

其实 action 就是去提交 mutation 的。异步操作都在 action 中消化了，最后再去提交 mutation。 

当然，你也可以进行动态传参。

首先，修改 store/index.js：

```javascript
actions: {
    setNum(content, payload) {   // 增加payload参数
        return new Promise(resolve => {
            setTimeout(() => {
                content.commit('setNumber', { number: payload.number });
                resolve();
            }, 1000);
        });
    },
}
```

相应地，修改 App.vue：

```javascript
async mounted() {
    console.log(this.$store.state.number);   // 旧值，0
    await this.$store.dispatch('setNum', { number: 888 });
    console.log(this.$store.state.number);   // 新值，888
}
```

建议使用 mapActions：

```vue
<script>
import { mapActions } from 'vuex';
export default {
    methods: {
        ...mapActions(['setNum']),   // 解构到methods中
    },
    async mounted() {
        await this.setNum({ number: 888 });  // 直接调用即可
    }
}
</script>
```

你也可以给它取别名：

```javascript
...mapActions({ changeNumber: 'setNum' })   // 赋别名的话，这里接收对象，而不是数组
```

> tips：在 actions 里面，方法的形参可以直接将 commit 解构出来，方便后续操作。 

```javascript
actions: {
    setNum({ commit }) {   // 直接将content结构掉，解构出commit，下面就可以直接调用了
        return new Promise(resolve => {
            setTimeout(() => {
                commit('xxx');  // 直接调用
                resolve();
            }, 1000);
        });
    },
},
```

## 四、总结

下班前这几分钟，你知道了如何使用 Vuex。你会安装它，配置它，读取 state 的值，甚至修饰读（Getters），然后你会修改里面的值（Mutations），如果有异步操作并且需要修改 state，那你就要使用 Actions。

一张神奇的图。

![](https://img-blog.csdnimg.cn/e9efea9c026b4570ba66a9786c4aec29.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

## 五、建议

### 何时使用 Vuex ？

这个问题因人而异，如果你不需要开发大型的单页应用，此时你完全没有必要使用 Vuex，比如你的页面就两三个，使用 Vuex 后增加的文件比你现在的页面还要多，完全没这个必要。

假如你的项目达到了中大型应用的规模，此时你很可能会考虑如何更好地在组件外部对状态进行统一管理，甚至会分模块（Vuex 中的 module，不展开），它将会是一个不错的选择。