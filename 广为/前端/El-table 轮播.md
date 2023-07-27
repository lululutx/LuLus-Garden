```vue
<template> 
  <div> 
    <el-table 
      :data="data" 
      border 
      style="width: 100%" 
      align="center" 
      size="mini" 
      id="dbM" 
    > 
      <el-table-column prop="type1" label="事项名称"> </el-table-column> 
      <el-table-column prop="type2" label="状态"> </el-table-column> 
      <el-table-column prop="type3" label="状态"> </el-table-column> 
      <el-table-column prop="type4" label="状态"> </el-table-column> 
       
    </el-table> 
  </div> 
</template> 
<script> 
export default { 
  data() { 
    return { 
      data: [ 
        { 
          type1: "人工鱼礁规模", 
          type2: "万空方", 
          type3: "1234", 
          type4: "123", 
        }, 
        { 
          type1: "构建礁", 
          type2: "具体尺寸", 
          type3: "2222", 
          type4: "222", 
        }, 
        { 
          type1: "石块礁", 
          type2: "立方", 
          type3: "3333", 
          type4: "333", 
        }, 
        { 
          type1: "海藻场或海草床建设", 
          type2: "公顷", 
          type3: "4444", 
          type4: "444", 
        }, 
        { 
          type1: "观测网建设", 
          type2: "具体建设", 
          type3: "5555", 
          type4: "555", 
        }, 
      ], 
    }; 
  }, 
  created() { 
    // this.getData(); 
    var isScroll = true; 
    this.$nextTick(() => { 
      let div = document.getElementsByClassName("el-table__body-wrapper")[0]; 
      div.style.height = "110px"; 
      // this.addEvent(div, "mouseenter", function() { 
      //   isScroll = false; 
      // }); 
      // this.addEvent(div, "mouseleave", function() { 
      //   isScroll = true; 
      // }); 
      let t = document.getElementsByClassName("el-table__body")[0]; 
      setInterval(() => { 
        if (isScroll) { 
          var data = this.data[0]; 
          setTimeout(() => { 
            this.data.push(data); 
            t.style.transition = "all .5s"; 
            t.style.marginTop = "-35px"; 
          }, 500); 
          setTimeout(() => { 
            this.data.splice(0, 1); 
            t.style.transition = "all 0s ease 0s"; 
            t.style.marginTop = "0"; 
          }, 1000); 
        } 
      }, 2500); 
    }); 
  }, 
}; 
</script> 
<style></style> 

```
