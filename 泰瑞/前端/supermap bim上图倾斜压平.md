```javascript
//先改变状态
FlattenCongif[BIMName].show = !FlattenCongif[BIMName].show
//遍历列表
Object.keys(FlattenCongif).forEach((key)=>{
  let config = FlattenCongif[key]
  //如果勾选
  if(config.show){
    //压平begin
    let positions= getFlattenRegionPositon(config.name)
    if(positions) {
      map.map3d.viewer.scene.layers._layers.values.forEach(function (layer) {
        let hasName = pitchLayernameAtDisableFlattion(layer.name)
        console.log('是否存在禁用名单里', hasName)
        if (layer &&
            layer.addFlattenRegion && !hasName) {
          // 不在禁用压平图层配置内
          layer.addFlattenRegion({
            position: positions,
            name: config.name
          });
        }

      })
    }
    //压平end
    //bim上图begin
    let promise = Global.viewer.scene.open('http://{s}/geoesb/proxy/' + config.dataId + '/' + (loginUserObj.userKey || '886e60bb7e014f22a707de23e6f6505d') + '/rest/realspace', undefined, {
      // subdomains: ['192.168.141.15:8081', '192.168.141.15:8082', '192.168.141.15:8083'],
      subdomains: Ip3dServerArr,
      autoSetView: false
    });
    Cesium.when(promise, function (layers) {
      let queryUrl = 'http://192.168.141.15:8081/geoesb/proxy/3930fb96b496454a8c9ef0d526bccffb/886e60bb7e014f22a707de23e6f6505d'
      layers.forEach(function (layer) {
        let dataSourceName = layer._name.split("@")[1];
        let dataSetName = layer._name.split("@")[0];
        layer.setQueryParameter({
          url: queryUrl,
          dataSourceName: dataSourceName,
          dataSetName: dataSetName,
          dataType:'BIM',
          dataName:config.name
        });
      })
    });
    //bim上图end
  }else if(config.show == false){
    map.map3d.viewer.scene.layers._layers.values.forEach(layer => {
      //没勾选的取消压平
      layer &&
        layer.removeFlattenRegion &&
        layer.removeFlattenRegion(config.name); // 根据resourceId 作为压平面的唯一标识
      //bim下图
      if(layer.queryParameter && layer.queryParameter.dataType == 'BIM' && layer.queryParameter.dataName == config.name){
        //不加计时器，下图不全
        setTimeout(()=>{
          Global.viewer.scene.layers.remove(layer.name, true);
        }
                   , 50 )
      }
    });
  }
})
```
