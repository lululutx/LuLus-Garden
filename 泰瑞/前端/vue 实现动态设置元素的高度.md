```vue
<div class="container" :style="{height: scrollerHeight}">
</div>

computed: {
    // 滚动区高度 
    scrollerHeight: function() {
      return (window.innerHeight - 50) + 'px'; //自定义高度需求
    }
  }
```
