![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662467227272-0a323fff-aa62-47a7-90a5-881794e45795.png#clientId=u83349ffd-abb1-4&from=paste&id=u67f7c42e&originHeight=837&originWidth=1209&originalType=url&ratio=1&rotation=0&showTitle=false&size=115543&status=done&style=none&taskId=ub75e48f4-61a1-409d-ba43-f488ec0aacc&title=)
```vue
 
<template> 
  <div id="developHistory"> 
    <div class="centerPart5Tit"> 
      <div class="titleTop"> 
        <div class="titleTop_Left">发展历程</div> 
        <div class="titleTop_Right">关于我们 >发展历程</div> 
      </div> 
      <div class="titleBottom"></div> 
    </div> 
    <div class="centerMain"> 
      <div class="timeLine"> 
        <el-timeline> 
          <el-timeline-item 
            v-for="(item, index) in timeList" 
            :key="index" 
            :class="index % 2 != 0 ? 'el-timeline-item-live-out' : ''" 
            :timestamp="item.timestamp" 
            placement="top" 
            color="#079B9E" 
            size="large" 
          > 
            <el-card :class="index % 2 != 0 ? 'content' : 'content1'"> 
              <div class="contentTitleRow"> 
                <div class="colorDiv"></div> 
                <div class="contentTitle">{{ item.contentTitle }}</div> 
              </div> 
              <div class="dividerLine"></div> 
              <div class="contentDetail"> 
                {{ item.detail }} 
              </div> 
            </el-card> 
          </el-timeline-item> 
        </el-timeline> 
      </div> 
    </div> 
  </div> 
</template> 
<script> 
export default { 
  name: "developHistory", 
  data() { 
    return { 
      timeList: [ 
        { 
          timestamp: "2025", 
          contentTitle: "远景规划", 
          detail: 
            "这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字...", 
        }, 
        { 
          timestamp: "2021", 
          contentTitle: "建设统一密码服务平台", 
          detail: 
            "这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字...", 
        }, 
        { 
          timestamp: "2018", 
          contentTitle: "国家电网商用密码管理中心", 
          detail: 
            "这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字...", 
        }, 
        { 
          timestamp: "2017", 
          contentTitle: "《电力行业国产密码应用规划》", 
          detail: 
            "这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字...", 
        }, 
        { 
          timestamp: "2011", 
          contentTitle: "CA建设", 
          detail: 
            "这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字这是一段描述文字...", 
        }, 
      ], 
    }; 
  }, 
  methods: {}, 
}; 
</script> 
<style scoped lang="scss" type="text/css"> 
#developHistory { 
  width: 100%; 
  display: flex; 
  flex-direction: column; 
  background: url("../assets/img/submitBack.png") 100% 100%; 
} 
.centerPart5Tit { 
  height: 52px; 
  line-height: 52px; 
  width: 1160px; 
  text-align: center; 
  border-bottom: 1px solid #dddddd; 
  margin: 40px auto 29px auto; 
  position: relative; 
  .titleTop { 
    display: flex; 
    flex-direction: row; 
    justify-content: space-between; 
    .titleTop_Left { 
      font-family: Source Han Sans CN; 
      font-style: normal; 
      font-weight: normal; 
      font-size: 26px; 
      line-height: 26px; 
      color: #333333; 
    } 
    .titleTop_Right { 
      font-family: Source Han Sans CN; 
      font-style: normal; 
      font-weight: normal; 
      font-size: 14px; 
      line-height: 26px; 
      color: #666666; 
    } 
  } 
  .titleBottom { 
    position: absolute; 
    width: 106px; 
    height: 4px; 
    background: #004098; 
    bottom: 0; 
  } 
} 
.centerMain { 
  display: flex; 
  flex-direction: column; 
  justify-content: center; 
  width: 1200px; 
  margin-bottom: 30px; 
  align-self: center; 
  .timeLine { 
    padding-top: 100px; 
    .content { 
      margin-top: 20px; 
      background: #f2f5f8; 
      display: flex; 
      flex-direction: column; 
      align-items: flex-start; 
      width: 530px; 
    } 
    .content:before { 
      content: ""; 
      width: 0; 
      height: 0; 
      border: 10px solid transparent; /*调整小三角形的大小*/ 
      border-bottom-color: #f2f5f8; /*改变小三角的颜色（最好就是和父级div的背景颜色相同）;bottom是用于调整小三角的方向*/ 
      position: absolute; 
      right: -25px; /*调整小三角形的位置*/ 
      top: 29px; /*调整小三角形的位置*/ 
    } 
    .content1 { 
      margin-top: 20px; 
      background: #f2f5f8; 
      display: flex; 
      flex-direction: column; 
      align-items: flex-start; 
      width: 530px; 
    } 
    .content1:before { 
      content: ""; 
      width: 0; 
      height: 0; 
      border: 10px solid transparent; /*调整小三角形的大小*/ 
      border-bottom-color: #f2f5f8; /*改变小三角的颜色（最好就是和父级div的背景颜色相同）;bottom是用于调整小三角的方向*/ 
      position: absolute; 
      left: 50px; /*调整小三角形的位置*/ 
      top: 20px; /*调整小三角形的位置*/ 
    } 
    .contentTitleRow { 
      display: flex; 
      flex-direction: row; 
      align-items: center; 
      .colorDiv { 
        width: 5px; 
        height: 23px; 
        background: #005f61; 
      } 
      .contentTitle { 
        margin-left: 20px; 
        font-size: 24px; 
        line-height: 24px; 
        align-items: center; 
        color: #005f61; 
      } 
    } 
    .dividerLine { 
      align-self: center; 
      width: 490px; 
      border-top: solid 1px #e5e5e5; 
      margin-top: 20px; 
    } 
    .contentDetail { 
      margin-top: 20px; 
      font-size: 16px; 
      line-height: 30px; 
      letter-spacing: 0.035em; 
      color: #525356; 
      text-align: left; 
    } 
    >>> .el-timeline { 
      margin-left: 600px; 
      width: 500px; 
    } 
    >>> .el-timeline-item__tail { 
      border-left: 2px solid #079b9e; 
    } 
    >>> .el-timeline-item__timestamp.is-top { 
      width: 10px; 
      font-size: 24px; 
      line-height: 15px; 
      color: #005f61; 
    } 
    >>> .el-timeline-item-live-out { 
      .el-timeline-item__wrapper { 
        left: -570px; 
        display: flex; 
        flex-direction: column; 
        .el-timeline-item__timestamp.is-top { 
          align-self: flex-end; 
          margin-right: 0px; 
        } 
      } 
    } 
    >>> .el-timeline-item { 
      margin-top: -100px; 
    } 
  } 
} 
</style> 

```
