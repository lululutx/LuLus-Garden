在适当的时候添加窗口抖动以重画echarts
```vue
      this.$nextTick(() => [
        window.dispatchEvent(new Event('resize'))//初始化echarts
      ])
```

```vue
<template>
  <div class="chart" ref="chart"></div>
</template>


<script>
export default {
  props: {
    autoResize: {
      type: Boolean,
      default: true,
    },
    optionIn: {
      type: Object,
      required: true
    },
    type: {
      type: String,
      default: "canvas",
    },
  },
  data() {
    return {
      chart: null,
      dataList: [],
    }
  },
  computed: {
    option() {
      return this.optionIn
    },
  },
  watch: {
    option: {
      deep: true,
      handler(newVal) {
        this.setOptions(newVal)
      },
    },
  },
  mounted() {
    // this.initData()

    this.initChart()
    if (this.autoResize) {
      window.addEventListener("resize", this.resizeHandler)
    }
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    if (this.autoResize) {
      window.removeEventListener("resize", this.resizeHandler)
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    resizeHandler() {
      this.chart && this.chart.resize()
    },
    initChart() {
      this.chart = this.$echarts.init(this.$refs.chart, "", {
        renderer: this.type,
      })
      this.chart.setOption(this.option)
      this.chart.on("click", this.handleClick)
    },
    handleClick(params) {
      this.$emit("click", params)
    },
    setOptions(option) {
      this.clearChart()
      this.resizeHandler()
      if (this.chart) {
        this.chart.setOption(option)
      }
    },
    refresh() {
      this.setOptions(this.option)
    },
    clearChart() {
      this.chart && this.chart.clear()
    },
    async initData(requsetFn = countnameDimension) {
      const res = await requsetFn()
      if (res.code == 200) {
        this.dataList = res.result
        console.log("requsetFn", res)
      }
    },
  },



}
</script>


<style scoped lang="less">
.chart {
  width: 100%;
  height: 100%;
}
</style>
```
