![image.png](https://cdn.nlark.com/yuque/0/2022/png/26798000/1662465953866-a7d4dffb-745c-49b9-b131-399521efa31c.png#clientId=u4cf938d4-1e8f-4&from=paste&id=ue755ba9e&originHeight=283&originWidth=509&originalType=url&ratio=1&rotation=0&showTitle=false&size=20800&status=done&style=none&taskId=u0dd6806c-7254-479b-9b6c-28055c93e5e&title=)
```vue
 
            <div ref="table1" style="height: 100%; position: relative"> 
              <el-table 
                :data="testData" 
                stripe 
                :show-header="false" 
                :cell-style="{ 
                  border: '1px #129BFF solid', 
                  fontSize: '10px', 
                  fontWeight: '500', 
                }" 
                :height="table1Height" 
                :row-style="{ height: table1RowHeight + 'px' }" 
                :span-method="objectSpanMethod" 
              > 
                <el-table-column prop="param1" label="参数1" align="center"> 
                  <template v-slot="scope"> 
                    <div class="tableSlot"> 
                      <img :src="scope.row.img" alt="" width="20" height="20" /> 
                      <div>{{ scope.row.param1 }}</div> 
                    </div> 
                  </template> 
                </el-table-column> 
                <el-table-column prop="param2" label="参数2" min-width="130px"> 
                </el-table-column> 
                <el-table-column prop="value1" label="数值" align="right"> 
                </el-table-column> 
              </el-table> 
            </div> 
 
 
 
 
      testData: [ 
        { 
          param1: "厂站", 
          param2: "厂站终端数量", 
          value1: "", 
          img: require("../../../static/assets/img/changzhan.png"), 
        }, 
        { 
          param1: "厂站", 
          param2: "测量点数", 
          value1: "", 
          //  img: require('../../../static/assets/img/logo-mini.png') , 
        }, 
        { 
          param1: "专变用户", 
          param2: "负控终端数量", 
          value1: "", 
          img: require("../../../static/assets/img/zuanbian.png"), 
        }, 
        { 
          param1: "专变用户", 
          param2: "测量点数", 
          value1: "", 
          // img: require('../../../static/assets/img/logo-mini.png') , 
        }, 
        { 
          param1: "低压用户", 
          param2: "集中器数量", 
          value1: "", 
          img: require("../../../static/assets/img/diya.png"), 
        }, 
        { 
          param1: "低压用户", 
          param2: "低压测量点数", 
          value1: "", 
          // img: require('../../../static/assets/img/logo-mini.png') , 
        }, 
        { 
          param1: "合计", 
          param2: "终端数量", 
          value1: "", 
          img: require("../../../static/assets/img/diya.png"), 
        }, 
        { 
          param1: "合计", 
          param2: "测量点数", 
          value1: "", 
          // img: require('../../../static/assets/img/logo-mini.png') , 
        }, 
      ], 

```
