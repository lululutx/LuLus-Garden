```vue
 
 
import elementResizeDetectorMaker from "element-resize-detector"; 
 
 
observer: {}, 
 
    //监听重画图表 
    watchSize1() { 
      let contentMap = document.getElementById("watchwindow");//如果有俩id，尽量别一样，就算不是一个vue文件 
      this.observer = new ResizeObserver((entries) => { 
        this.lineChart1.resize(); 
        this.lineChart2.resize(); 
        this.lineChart3.resize(); 
        this.lineChart4.resize(); 
      }); 
      this.observer.observe(contentMap); 
    }, 
    //监听重画图表 
    watchSize() { 
      let erd = elementResizeDetectorMaker(); 
      erd.listenTo(document.getElementById("watchwindow"), (element) => { 
        this.$nextTick(() => { 
          //使echarts尺寸重置 
          this.$echarts.init(document.getElementById("lineChart1")).resize(); 
          this.$echarts.init(document.getElementById("lineChart2")).resize(); 
          this.$echarts.init(document.getElementById("lineChart3")).resize(); 
          this.$echarts.init(document.getElementById("lineChart4")).resize(); 
          window.onresize = () => { 
            this.lineChart1.resize(); 
            this.lineChart2.resize(); 
            this.lineChart3.resize(); 
            this.lineChart4.resize(); 
            this.$forceUpdate(); 
          }; 
        }); 
      }); 
    }, 

```
```vue
 
  mounted() { 
    this.watchSize(); 
  }, 
  methods: { 
    watchSize() { 
      let contentMap = document.getElementById("login"); 
      this.observer = new ResizeObserver((entries) => { 
        console.info("大小改变了,宽：",document.getElementById("login").offsetWidth,"高",document.getElementById("login").offsetHeight); 
      }); 
      this.observer.observe(contentMap); 
    }, 
  }, 

```
