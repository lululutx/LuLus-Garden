![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662467696472-0951ca67-9962-43fd-9e68-a6f6a96cc6e8.png#clientId=udcae6687-e56f-4&from=paste&id=u8ecfadd2&originHeight=567&originWidth=628&originalType=url&ratio=1&rotation=0&showTitle=false&size=6899&status=done&style=none&taskId=u243d2b9e-10bd-45a1-89a0-959e27d071a&title=)
```vue
 
<template> 
  <div id="personalCenter"> 
    <div class="centerPart5Tit"> 
      <div class="titleTop"> 
        <div class="titleTop_Left">个人中心</div> 
        <div class="titleTop_Right"> 
          <el-breadcrumb separator-class="el-icon-arrow-right"> 
            <el-breadcrumb-item to="/indexHome">首页</el-breadcrumb-item> 
            <el-breadcrumb-item>个人中心</el-breadcrumb-item> 
          </el-breadcrumb> 
        </div> 
      </div> 
      <div class="titleBottom"></div> 
    </div> 
    <div class="centerMain"> 
      <div class="mainLeft"> 
        <Anchor :bounds="50"> 
          <div class="myInfo"></div> 
          <AnchorLink href="#myInformation" title="我的信息" /> 
          <AnchorLink href="#enterpriseInformation" title="企业资料" /> 
          <AnchorLink href="#accountSecurity" title="账号安全" /> 
        </Anchor> 
      </div> 
      <div class="mainRight"> 
        <div class="part" id="myInformation"> 
          <div class="partTitleRow"> 
            <div class="partTitle">我的信息</div> 
            <div class="partEdit">编辑</div> 
          </div> 
        </div> 
        <div class="part" id="enterpriseInformation" style="margin-top: 24px"> 
          <div class="partTitleRow"> 
            <div class="partTitle">企业资料</div> 
            <div class="partEdit">编辑</div> 
          </div> 
        </div> 
        <div class="part" id="accountSecurity" style="margin-top: 24px"> 
          <div class="partTitleRow"> 
            <div class="partTitle">账号设置</div> 
            <div class="partEdit">编辑</div> 
          </div> 
        </div> 
      </div> 
    </div> 
  </div> 
</template> 
<script> 
export default { 
  name: "personalCenter", 
  data() { 
    return {}; 
  }, 
  watch: {}, 
  created() {}, 
  mounted() { 
    this.$nextTick(() => { 
      document.getElementById("personalCenter").scrollIntoView(); 
    }); 
  }, 
  methods: {}, 
}; 
</script> 
<!-- Add "scoped" attribute to limit CSS to this component only --> 
<style lang="scss" scoped> 
#personalCenter { 
  width: 100%; 
  min-height: 100%; 
  box-sizing: border-box; 
  display: flex; 
  flex-direction: column; 
  align-items: center; 
  position: relative; 
  background-color: #e5e5e5; 
} 
.centerPart5Tit { 
  height: 52px; 
  line-height: 52px; 
  width: 1160px; 
  text-align: center; 
  border-bottom: 1px solid #dddddd; 
  margin: 100px auto 29px auto; 
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
      display: flex; 
      flex-direction: column; 
      justify-content: center; 
      >>> .el-breadcrumb__inner { 
        color: #666666; 
        font-weight: 400; 
      } 
    } 
  } 
  .titleBottom { 
    position: absolute; 
    width: 106px; 
    height: 4px; 
    background: #005f61; 
    bottom: 0; 
  } 
} 
.centerMain { 
  display: flex; 
  flex-direction: row; 
  width: 1200px; 
  height: 1500px; 
  margin-bottom: 30px; 
  .mainLeft { 
    width: 270px; 
    margin-right: 24px; 
    position: relative; 
  } 
  .myInfo { 
    height: 205px; 
    width: 100%; 
    background: #ffffff; 
    border: solid 1px red; 
  } 
  >>> .ivu-anchor-link { 
    height: 70px; 
    text-align: center; 
    padding: 0px; 
    background: #ffffff; 
    display: flex; 
    flex-direction: column; 
    justify-content: center; 
    a:hover { 
      color: #005f61; 
    } 
  } 
  >>> .ivu-anchor-link-active { 
    background-color: #f0f0f0; 
  } 
  >>> .ivu-anchor-link-active::before { 
    content: ""; 
    left: 0; 
    height: 70px; 
    width: 3px; 
    margin-left: 3px; 
    position: absolute; 
    background-color: #005f61; 
  } 
  .mainRight { 
    width: 100%; 
    height: 100%; 
    display: flex; 
    flex-direction: column; 
    .part { 
      background: #ffffff; 
      border: 1px solid #f0f0f0; 
      border-radius: 2px; 
      height: 500px; 
      display: flex; 
      flex-direction: column; 
      .partTitleRow { 
        height: 56px; 
        border-bottom: 1px solid #f0f0f0; 
        display: flex; 
        flex-direction: row; 
        justify-content: space-between; 
        align-items: center; 
        .partTitle { 
          font-size: 16px; 
          color: rgba(0, 0, 0, 0.85); 
          margin-left: 24px; 
        } 
        .partEdit { 
          font-size: 14px; 
          color: #079b9e; 
          margin-right: 24px; 
          cursor: pointer; 
        } 
      } 
    } 
  } 
} 
</style> 

```
