#Vue 

from [Vue 插槽（slot）详细介绍（对比版本变化，避免踩坑）](https://blog.csdn.net/qq_41809113/article/details/121640035)


---

# 前言

Vue 中的插槽（slot）在项目中用的也是比较多的，今天就来介绍一下插槽的基本使用以及 Vue 版本更新之后的插槽用法变化。

# 正文

## 插槽是什么？

插槽就是子组件中的提供给父组件使用的一个[占位符](https://so.csdn.net/so/search?q=%E5%8D%A0%E4%BD%8D%E7%AC%A6&spm=1001.2101.3001.7020)，用<slot></slot> 表示，父组件可以在这个占位符中填充任何模板代码，如 HTML、组件等，填充的内容会替换子组件的<slot></slot>标签。简单理解就是子组件中留下个“坑”，父组件可以使用指定内容来补“坑”。以下举例子帮助理解。

## 怎么使用插槽？

### 基本用法

现在，有两个组件，A 与 B，分别如下：

A.vue

```vue
<template>
  <div>
    <p>我是A组件</p>
  </div>
</template>
 
<script>
export default {
  name:'A',
  data(){
    return {
 
    }
  }
}
</script>
```

B.vue

```vue
<template>
  <div>
    <p>我是B组件</p>
  </div>
</template>
 
<script>
export default {
  name:'B',
  data(){
    return {
 
    }
  }
}
</script>
```

将 B 组件引入 A 组件里面（此时 B 为 A 的子组件）

```vue
<template>
  <div>
    <p>我是A组件</p>
    <B><B/>
  </div>
</template>
 
<script>
import B from './B.vue'    //引入B组件
export default {
  name:'A',
  components:{      //注册B组件
    B
  },
  data(){
    return {
 
    }
  }
}
</script>
```

页面效果如下：

![](https://img-blog.csdnimg.cn/1bd06df062c54f7d8ba4fefd40035cb2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

准备工作完毕，现在，在 B 组件里面使用插槽（slot）

```vue
<template>
  <div>
    <p>我是B组件</p>
    <slot></slot>     //插槽的使用方式
  </div>
</template>
 
<script>
export default {
  name:'B',
  data(){
    return {
 
    }
  }
}
</script>
```

此时页面并无变化（最开始的情况），当然，B 组件中使用了插槽 slot 之后，相当于留下了一个“坑”，占了个位置。  那么如何验证其存在了呢？

此时，修改 A 组件里面的代码

```vue
<template>
  <div>
    <p>我是A组件</p>
    <B>  
      验证插槽是否生效      //用B组件标签包裹内容，验证slot
    </B>
  </div>
</template>
 
<script>
import B from './B.vue'
export default {
  name:'A',
  components:{
    B
  },
  data(){
    return {
 
    }
  }
}
</script>
```

此时页面的效果如下：

![](https://img-blog.csdnimg.cn/eb96cc317ef442d18f436cb78f6aa1ab.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)页面中多出了在 A 中用 B 包裹的内容。没错，这就是插槽的基本使用，是不是很简单？

Vue 实现了一套内容分发的 API，这套 API 的设计灵感源自  [Web Components 规范草案](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md "Web Components 规范草案")，将  `<slot>`  元素作为承载分发内容的出口。

如上面的例子，当组件渲染的时候，`<slot></slot>`  将会被替换为“验证插槽是否生效”（即指定内容）。插槽内可以包含任何模板代码，包括 HTML：

```vue
<template>
  <div class="main">
    <p>我是A组件</p>
    <B>
      <span style="color:red">验证插槽是否生效</span> //内容为html
    </B>
  </div>
</template>
```

页面效果如下：

![](https://img-blog.csdnimg.cn/ec23ee96b6274eeda453e03798c10b85.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)插槽内也可以放其他组件，如此时再新建一个组件 C：

```vue
<template>
  <div>
    <p>我是C组件</p>
  </div>
</template>
 
<script>
export default {
  name:'C',
  data(){
    return {
 
    }
  }
}
</script>
```

在 A 组件中，将 B 组件包裹的内容换成 C 组件：

```vue
<template>
  <div class="main">
    <p>我是A组件</p>
    <B>
      <!-- <span style="color:red">验证插槽是否生效</span> -->
      <C />         //插入C组件
    </B>
  </div>
</template>
 
<script>
import B from './B.vue'
import C from './C.vue'   //引入C组件
export default {
  name:'A',
  components:{
    B,
    C      //注册C组件
  },
  data(){
    return {
 
    }
  }
}
</script>
```

页面效果如下：

![](https://img-blog.csdnimg.cn/a52d3431777140249931671073a005cb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)此时检查页面元素，我们会发现，在原本 B 组件中<slot></slot>的位置，替换成了 C 组件：

```vue
//B.vue
<template>
  <div>
    <p>我是B组件</p>
    <slot></slot>
  </div>
</template>
 
//观察页面元素,<slot></slot>被替换成C组件
```

![](https://img-blog.csdnimg.cn/893809431c4e418db0818b5458efb59a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

也印证了开篇对插槽作用的解释，即使用<slot></slot>的组件指定的位置留一个坑，如果在外部，使用其组件包裹某内容（可以是任何模板代码，也可以是 HTML，还可以是组件），则该内容就会被分发到<slot></slot>处（一个有趣的说法就是把“坑”补上），渲染出来。当然，也可以不放任何内容，不影响组件渲染，就好比最开始的情况。

注意：如果 B 组件的  `template`  中**没有**包含一个  `<slot>`  元素，即不使用插槽，则该组件起始标签和结束标签之间的任何内容都会被抛弃。例如：

```vue
//B.vue
<template>
  <div>
    <p>我是B组件</p>
    <!-- <slot></slot> -->   //不使用插槽
  </div>
</template>
```

```vue
//A.vue
<template>
  <div>
    <p>我是A组件</p>
    <B>
      <!-- <span style="color:red">验证插槽是否生效</span> -->
      <C />
    </B>
  </div>
</template>
 
//此时在<B></B>包裹的内容都会被抛弃
```

页面效果如下，在 B 组件中插入的 C 组件被抛弃了，因为 B 组件中没使用插槽：

![](https://img-blog.csdnimg.cn/d6ad73d6a9d244a1ac37ce43c663ab20.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

### 后备（默认）内容

有时为一个插槽设置具体的后备 (也就是默认的) 内容是很有用的，它只会在没有提供内容的时候被渲染。例如在 B 组件中：

```vue
<template>
  <div>
    <slot></slot>
  </div>
</template>
```

我们可能希望这个 B 组件内绝大多数情况下都渲染文本“我是 B 组件”。为了将“我是 B 组件”作为后备内容，我们可以将它放在  `<slot>`  标签内：

```vue
<template>
  <div>
    <slot>
      <p>我是B组件</p>
    </slot>
  </div>
</template>
```

现在当我在一个父级组件中使用 B 组件并且不提供任何插槽内容时：

```vue
<B></B>
```

后备内容“我是 B 组件”将会被渲染：

![](https://img-blog.csdnimg.cn/30f069e48534470fa3ba9fa4fe0f1b08.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_18,color_FFFFFF,t_70,g_se,x_16)但是如果我们提供内容：

```vue
<B>
  <p>我是插槽内容</p>
</B>
```

则这个提供的内容将会被渲染从而取代后备内容：

![](https://img-blog.csdnimg.cn/c70c089571674b40b8f13f0cd68dfdfe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_18,color_FFFFFF,t_70,g_se,x_16)

### 具名插槽

所谓具名插槽，顾名思义就是起了名字的插槽。有时我们需要多个插槽，例如当我们想使用某种通用模板：

```vue
<template>
  <div>
    <header>
      <!-- 我们希望把页头放这里 -->
    </header>
    <main>
      <!-- 我们希望把主要内容放这里 -->
    </main>
    <footer>
      <!-- 我们希望把页脚放这里 -->
    </footer>
  </div>
</template>
```

对于这样的情况，`<slot>`  元素有一个特殊的 attribute：`name`。这个 attribute 可以用来定义额外的插槽：

```vue
//B.vue
<template>
  <div>
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>
```

一个不带  `name`  的  `<slot>`  出口会带有隐含的名字“default”。

在向具名插槽提供内容的时候，我们可以在一个  `<template>`  元素上使用  `slot`  指令，并以  `slot`  的参数的形式提供其名称（当然也可以直接放在标签中，如
`<div slot="header"></div>`
）：

```vue
<template>
  <div>
    <p>我是A组件</p> 
    <B>
      <template slot="header">
        <p>我是header部分</p>
      </template> 
      
      <p>我是main（默认插槽）部分</p> 
      
      <template slot="footer">
        <p>我是footer部分</p>
      </template>
    </B>
  </div>
</template>
```

现在  `<template>`  元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有`slot`  的  `<template>`  中的内容都会被视为默认插槽的内容。

页面效果如下：

![](https://img-blog.csdnimg.cn/a481e32bd1854eaeab83e9111b9420c3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)

观察页面元素，内容被放入相应名字的插槽中：

![](https://img-blog.csdnimg.cn/4b1451bac9754fc3aca475535db7ff81.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)Tips：说到这里就不得不提一下，这种方式在项目中比较常用，可以当成一个复用（通用）模板组件。如多个组件的布局使用相似模板，只是具体内容不同，那么我们可以使用这种插槽方式封装成一个通用组件，在其他组件使用的时候只需要传对应的内容到对应名字的插槽即可，不需要将该模板在每个组件重新写一遍，减少代码冗余，大大提高开发效率。

### 作用域插槽

有时让插槽内容能够访问子组件中才有的数据是很有用的。现在，假设 B 组件：

```vue
<template>
  <div>
    <p>我是B组件</p>
    <slot>{{obj.firstName}}</slot>
  </div>
</template>
 
<script>
export default {
  name:'B',
  data(){
    return {
      obj:{
        firstName:'leo',
        lastName:'lion'
      }
    }
  }
}
</script>
```

我们可能想换掉备用内容，用“lion”来显示。如下，在 A 组件：

```vue
<template>
  <div>
    <p>我是A组件</p>
    <B>
      {{obj.lastName}}
    </B>
  </div>
</template>
```

然而上述代码不会正常工作，因为只有 B 组件可以访问到 obj，而我们提供的内容是在父级渲染的，即在父级作用域中。页面并无变化：

![](https://img-blog.csdnimg.cn/082ddb40b0564b6397fdb3d5e8997460.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)为了让  obj 在父级的插槽内容中可用，我们可以将 obj 作为  `<slot>`  元素的一个 attribute 绑定上去：

```vue
<template>
  <div>
    <p>我是B组件</p>
    <slot :obj="obj">{{obj.firstName}}</slot>
  </div>
</template>
```

绑定在  `<slot>`  元素上的 attribute 被称为**插槽 prop**。现在在父级作用域中，我们可以使用带值的  `slot-scope`  来定义我们提供的插槽 prop 的名字：

```vue
<template>
  <div class="main">
    <p>我是A组件</p>
    <B>
      <template slot-scope="data">
        {{data.obj.lastName}}
      </template>
    </B>
  </div>
</template>
```

在这个例子中，我们选择将包含所有插槽 prop 的对象命名为 data，但你也可以使用任意你喜欢的名字。此时页面效果如下：

![](https://img-blog.csdnimg.cn/f221449f2c3046aaa6d046c3c653abf1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YmN56uv5LiN6YeK5Y23bGVv,size_20,color_FFFFFF,t_70,g_se,x_16)如果你有使用过 ElementUI 里面的表格 el-table，当改变某一列展示的字段时，我们经常使用：

```vue
<el-table-column>
  <template slot-scope="scope">
    <span>{{scope.row.xxx}}</span>
  </template>
</el-table-column>
```

### 插槽版本变化

`v-slot`  指令自 Vue 2.6.0 起被引入，提供更好的支持  `slot`  和  `slot-scope` attribute 的 API 替代方案。`v-slot`  完整的由来参见这份  [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md "RFC")。在接下来所有的 2.x 版本中  `slot`  和  `slot-scope` attribute 仍会被支持，但已经被官方废弃且不会出现在 Vue 3 中。也就是说，在 vue2 版本中，我们仍可以使用 slot 跟 slot-scope，但是在 vue3 中就只能使用 v-slot 了。

原来的带有 slot 的具名插槽

```vue
//B.vue
<template>
  <div>
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>
```

写法变化，使用 v-slot

```vue
<template>
  <div>
    <p>我是A组件</p> 
    <B>
      <template v-slot:header>
        <p>我是header部分</p>
      </template> 
      
      <p>我是main（默认插槽）部分</p> 
      
      <template v-slot:footer>
        <p>我是footer部分</p>
      </template>
    </B>
  </div>
</template>
```

原来的作用域插槽

```vue
<template>
  <div class="main">
    <p>我是A组件</p>
    <B>
      <template slot-scope="data">
        {{data.obj.lastName}}
      </template>
    </B>
  </div>
</template>
```

写法变化，使用 v-slot

```vue
<template>
  <div class="main">
    <p>我是A组件</p>
    <B>
      <template v-slot="data">
        {{data.obj.lastName}}
      </template>
    </B>
  </div>
</template>
```

# 总结

在 2.6.0 中，为具名插槽和作用域插槽引入了一个新的统一的语法 (即  `v-slot`  指令)。它取代了  `slot`  和  `slot-scope`  这两个目前已被废弃但未被移除且仍在[文档中](https://cn.vuejs.org/v2/guide/components-slots.html#%E5%BA%9F%E5%BC%83%E4%BA%86%E7%9A%84%E8%AF%AD%E6%B3%95 "文档中")的 attribute。新语法的由来可查阅这份  [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md "RFC")。注意 slot 版本变化，vue2 中仍可以使用 slot 与 slot-scope，但是 vue3 只能使用 v-slot 了，切记，避免踩坑。
