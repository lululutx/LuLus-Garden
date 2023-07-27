```vue
 
<template> 
  <div 
    style=" 
          width: 100vw; 
          height: 50vh; 
          padding: 30px 0; 
          background: #000f0f; 
          display: flex; 
          justify-content: center; 
          align-items: center; 
        " 
  > 
    <!-- 加个盒子，防止抖动影响其他视图 --> 
    <div style="width: 200px; height: 300px"> 
      <!-- 抖动控件 --> 
      <div class="item"></div> 
    </div> 
  </div> 
</template> 
<script> 
export default { 
  name: "CanvasDraw", 
  props: {}, 
  data() { 
    return {}; 
  }, 
  mounted() {}, 
  methods: {} 
}; 
</script> 
<style scoped> 
.item { 
  width: 100px; 
  height: 150px; 
  background: #fff; 
  border-radius: 5px; 
} 
.item { 
  animation-delay: 0s; 
  animation-name: shock; 
  animation-duration: 1s; 
  animation-iteration-count: infinite; 
  animation-direction: normal; 
  animation-timing-function: linear; 
} 
@keyframes shock { 
  0% { 
    margin-left: 0px; 
    margin-right: 0px; 
    margin-top: 0px; 
  } 
  50% { 
    margin-left: 0px; 
    margin-right: 0px; 
    margin-top: 20px; 
  } 
  100% { 
    margin-left: 0px; 
    margin-right: 0px; 
    margin-top: 0px; 
  } 
} 
</style> 

```
