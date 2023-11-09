#Vue 

## 一、Render 的资料简介

**Render 函数是 Vue2.x 新增的一个函数、主要用来提升节点的性能，它是基于 JavaScript 计算。使用 Render 函数将 Template 里面的节点解析成虚拟的 Dom 。**

> Vue 推荐在绝大多数情况下使用模板来创建你的 HTML。然而在一些场景中，你真的需要 JavaScript  
> 的完全编程的能力。这时你可以用渲染函数，它比模板更接近编译器。

简单的说，在 Vue 中我们使用模板 HTML 语法组建页面的，使用 Render 函数我们可以用 Js 语言来构建 DOM。  
因为 Vue 是虚拟 DOM，所以在拿到 Template 模板时也要转译成 VNode 的函数，而用 Render 函数构建 DOM，Vue 就免去了转译的过程。

## 二、与 Render 的初次相遇

你第一次邂逅它的时候，它可能是这样的：

- IView

```javascript
render:(h, params)=>{
    return h('div', {style:{width:'100px',height:'100px',background:'#ccc'}}, '地方')
}
```

- Element

``` css
<el-table-column :render-header="setHeader">
</el-table-column>

setHeader (h) {
 return h('span', [
    h('span', { style: 'line-height: 40px;' }, '备注'),
      h('el-button', {
        props: { type: 'primary', size: 'medium', disabled: this.isDisable || !this.tableData.length },
        on: { click: this.save }
      }, '保存当前页')
    ])
  ])
},
```

或者这样的：

```javascript
renderContent (createElement, { node, data, store }) {
    return createElement('span', [
        // 显示树的节点信息
        createElement('span', node.label)
        // ......
    ])
}
```

那它的真身到底是什么样的呢？这还要从它的身世说起。

### 2.1、节点、树

在深入渲染函数之前，了解一些浏览器的工作原理是很重要的。以下面这段 HTML 为例：

```vue
<div>
  <h1>My title</h1>
  Some text content
  <!-- TODO: Add tagline -->
</div>
```

当浏览器读到这些代码时，它会建立一个DOM 节点树来保持追踪所有内容，如同你会画一张家谱树来追踪家庭成员的发展一样。

上述 HTML 对应的 DOM 节点树如下图所示：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/18856e6b5c61490d83560ea5a8aa87c8.png#pic_center)  
每个元素都是一个节点。每段文字也是一个节点。甚至注释也都是节点。一个节点就是页面的一个部分。就像家谱树一样，每个节点都可以有孩子节点 (也就是说每个部分可以包含其它的一些部分)。

高效地更新所有这些节点会是比较困难的，不过所幸你不必手动完成这个工作。你只需要告诉 Vue 你希望页面上的 HTML 是什么，这可以是在一个模板里：

```vue
<h1>{{ blogTitle }}</h1>
```

或者一个渲染函数里：

```javascript
render: function (createElement) {
  return createElement('h1', this.blogTitle)
}
```

在这两种情况下，Vue 都会自动保持页面的更新，即便 blogTitle 发生了改变。

### 2.2、虚拟 DOM

Vue 通过建立一个虚拟 DOM 来追踪自己要如何改变真实 DOM。请仔细看这行代码：

```javascript
return createElement('h1', this.blogTitle)
```

`createElement`到底会返回什么呢？其实不是一个实际的 DOM 元素。它更准确的名字可能是 `createNodeDescription`，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，包括及其子节点的描述信息。我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为“VNode”。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。

注：当使用 `render函数` 描述虚拟 DOM 时，vue 提供一个函数，这个函数是就构建虚拟 DOM 所需要的工具。官网上给他起了个名字叫 createElement。还有约定的简写叫 h，将 h 作为 createElement 的别名是 Vue 生态系统中的一个通用惯例，实际上也是 JSX 所要求的。

> 有点意思~ 其实它就是 createElement，接下来让我们来走近一点点，来深入的了解它吧~

## 三、与 Render 的约会

### 3.1 createElement 参数

> createElement（TagName，Option，Content）接受三个参数  
> `createElement(" 定义的元素 "，{ 元素的性质 }，" 元素的内容"/[元素的内容])`

- 官方文档

```javascript
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签名、组件选项对象，或者
  // resolve 了上述任何一种的一个 async 函数。必填项。
  'div',

  // {Object}
  // 一个与模板中属性对应的数据对象。可选。
  {
    // (详情见下一节-3.2 深入数据对象)
  },

  // {String | Array}
  // 子级虚拟节点 (VNodes)，由 `createElement()` 构建而成，
  // 也可以使用字符串来生成“文本虚拟节点”。可选。
  [
    '先写一些文字',
    createElement('h1', '一则头条'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
```

### 3.2 深入数据对象

```javascript
{
  // 与 `v-bind:class` 的 API 相同，
  // 接受一个字符串、对象或字符串和对象组成的数组
  'class': {
    foo: true,
    bar: false
  },
  // 与 `v-bind:style` 的 API 相同，
  // 接受一个字符串、对象，或对象组成的数组
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // 普通的 HTML 特性
  attrs: {
    id: 'foo'
  },
  // 组件 prop
  props: {
    myProp: 'bar'
  },
  // DOM 属性
  domProps: {
    innerHTML: 'baz'
  },
  // 事件监听器在 `on` 属性内，
  // 但不再支持如 `v-on:keyup.enter` 这样的修饰器。
  // 需要在处理函数中手动检查 keyCode。
  on: {
    click: this.clickHandler
  },
  // 仅用于组件，用于监听原生事件，而不是组件内部使用
  // `vm.$emit` 触发的事件。
  nativeOn: {
    click: this.nativeClickHandler
  },
  // 自定义指令。注意，你无法对 `binding` 中的 `oldValue`
  // 赋值，因为 Vue 已经自动为你进行了同步。
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    },
    //设置隐藏，display属性
    {
          name: 'show',
          value: false
     }
  ],
  // 作用域插槽的格式为
  // { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
  // 如果组件是其它组件的子组件，需为插槽指定名称
  slot: 'name-of-slot',
  // 其它特殊顶层属性
  key: 'myKey',
  ref: 'myRef',
  // 如果你在渲染函数中给多个元素都应用了相同的 ref 名，
  // 那么 `$refs.myRef` 会变成一个数组。
  refInFor: true
}
```

### 3.3 举个小栗子

```javascript
render:(h) => {
  return h('div',{
　　　//给div绑定value属性
     props: {
         value:''
     },
　　　//给div绑定样式
　　　style:{
　　　　　width:'30px'
　　　},　
　　　//给div绑定点击事件　　
     on: {
         click: () => {
            console.log('点击事件')
         }
     },
  })
}
```

### 3.4 约束

> 它也是有小脾气的~ 要记得这个约束哟~

- VNode 必须唯一  
    组件树中的所有 VNode 必须是唯一的。这意味着，下面的渲染函数是不合法的：

```javascript
render: function (createElement) {
  var myParagraphVNode = createElement('p', 'hi')
  return createElement('div', [
    // 错误 - 重复的 VNode
    myParagraphVNode, myParagraphVNode
  ])
}
```

如果你真的需要重复很多次的元素/组件，你可以使用工厂函数来实现。例如，下面这渲染函数用完全合法的方式渲染了 20 个相同的段落：

```javascript
render: function (createElement) {
  return createElement('div',
    Array.apply(null, { length: 20 }).map(function () {
      return createElement('p', 'hi')
    })
  )
}
```

> 上面的都是基础，大家要记牢哟，下面介绍一些它的特性

## 四、Render 的小个性

### 4.1 v-if 和 v-for

只要在原生的 JavaScript 中可以轻松完成的操作，Vue 的渲染函数就不会提供专有的替代方法。比如，在模板中使用的 `v-if` 和 `v-for`：

```javascript
<ul v-if="items.length">
  <li v-for="item in items">{{ item.name }}</li>
</ul>
<p v-else>No items found.</p>
```

这些都可以在渲染函数中用 JavaScript 的 `if/else` 和 `map` 来重写：

```javascript
props: ['items'],
render: function (createElement) {
  if (this.items.length) {
    return createElement('ul', this.items.map(function (item) {
      return createElement('li', item.name)
    }))
  } else {
    return createElement('p', 'No items found.')
  }
}
```

### 4.2 v-model

渲染函数中没有与 v-model 的直接对应——你必须自己实现相应的逻辑：

```javascript
props: ['value'],
render: function (createElement) {
  var self = this
  return createElement('input', {
    domProps: {
      value: self.value
    },
    on: {
      input: function (event) {
        self.$emit('input', event.target.value)
      }
    }
  })
}
```

这就是深入底层的代价，但与 v-model 相比，这可以让你更好地控制交互细节。

### 4.3 事件 & 按键修饰符

对于 `.passive`、`.capture` 和 `.once` 这些事件修饰符, Vue 提供了相应的前缀可以用于 `on`：

|修饰符|前缀|
|---|---|
|`.passive`|`&`|
|`.capture`|`!`|
|`.once`|`~`|
|`.capture.once`或`.once.capture`|`~!`|

例如:

```javascript
on: {
  '!click': this.doThisInCapturingMode,
  '~keyup': this.doThisOnce,
  '~!mouseover': this.doThisOnceInCapturingMode
}
```

对于所有其它的修饰符，私有前缀都不是必须的，因为你可以在事件处理函数中使用事件方法：

|修饰符|处理函数中的等价操作|
|---|---|
|`.stop`|`event.stopPropagation()`|
|`.prevent`|`event.preventDefault()`|
|`.self`|`if (event.target !== event.currentTarget) return`|
|按键：`.enter`, `.13`|`if (event.keyCode !== 13) return` (对于别的按键修饰符来说，可将 `13` 改为另一个按键码)|
|修饰键：`.ctrl`, `.alt`, `.shift`, `.meta`|`if (!event.ctrlKey) return` (将 `ctrlKey` 分别修改为 `altKey`、`shiftKey` 或者 `metaKey`)|

这里是一个使用所有修饰符的例子：

```javascript
on: {
  keyup: function (event) {
    // 如果触发事件的元素不是事件绑定的元素
    // 则返回
    if (event.target !== event.currentTarget) return
    // 如果按下去的不是 enter 键或者
    // 没有同时按下 shift 键
    // 则返回
    if (!event.shiftKey || event.keyCode !== 13) return
    // 阻止 事件冒泡
    event.stopPropagation()
    // 阻止该元素默认的 keyup 事件
    event.preventDefault()
    // ...
  }
}
```

### 4.4 插槽

你可以通过 `this.$slots` 访问静态插槽的内容，每个插槽都是一个 VNode 数组：

```javascript
render: function (createElement) {
  // `<div><slot></slot></div>`
  return createElement('div', this.$slots.default)
}
```

也可以通过 `this.$scopedSlots` 访问作用域插槽，每个作用域插槽都是一个返回若干 VNode 的函数：

```javascript
props: ['message'],
render: function (createElement) {
  // `<div><slot :text="message"></slot></div>`
  return createElement('div', [
    this.$scopedSlots.default({
      text: this.message
    })
  ])
}
```

如果要用渲染函数向子组件中传递作用域插槽，可以利用 VNode 数据对象中的 `scopedSlots` 字段：

```javascript
render: function (createElement) {
  return createElement('div', [
    createElement('child', {
      // 在数据对象中传递 `scopedSlots`
      // 格式为 { name: props => VNode | Array<VNode> }
      scopedSlots: {
        default: function (props) {
          return createElement('span', props.text)
        }
      }
    })
  ])
}
```

## 五、实战

**Element 中的 Tree**

```javascript
// 树节点的内容区的渲染回调
renderContent(h, { node, data, store }) {
    let aa = () => {
        console.log(data)
    }
    return  h('span', [
        h('span', {
            class: "custom-tree-node"
        }, [
            h('i', { class: "icon-folder" }), h('span', { props: { title: node.label }, class: "text ellipsis" }, node.label),
            h('el-popover', {
                props: {
                    placement: "bottom",
                    title: "",
                    width: "61",
                    popperClass: "option-group-popover",
                    trigger: "hover"
                }
            }, [
                h('ul', { class: "option-group" }, [
                    h('li', {
                        class: "pointer-text",
                        on: {
                            click: aa
                        }
                    }, '编辑'),
                    h('li', { class: "pointer-text" }, '删除'),
                    h('li', { class: "pointer-text" }, '添加')
                ]),
                h('i', { slot: "reference", class: "el-icon-more fr more-icon",
                    on: {
                        click: (e) => {
                            e.stopPropagation();
                        }
                    }
                })
            ])
        ])
    ])
},
```

## 六、扩展-[JSX](https://so.csdn.net/so/search?q=JSX&spm=1001.2101.3001.7020)

如果你写了很多 render 函数，可能会觉得下面这样的代码写起来很痛苦：

```javascript
createElement(
  'anchored-heading', {
    props: {
      level: 1
    }
  }, [
    createElement('span', 'Hello'),
    ' world!'
  ]
)
```

特别是对应的模板如此简单的情况下：

```javascript
<anchored-heading :level="1">
  <span>Hello</span> world!
</anchored-heading>
```

这就是为什么会有一个 Babel 插件，用于在 Vue 中使用 JSX 语法，它可以让我们回到更接近于模板的语法上。

```javascript
import AnchoredHeading from './AnchoredHeading.vue'

new Vue({
  el: '#demo',
  render: function (h) {
    return (
      <AnchoredHeading level={1}>
        <span>Hello</span> world!
      </AnchoredHeading>
    )
  }
})
```