```vue
test.html 
<!DOCTYPE html> 
<html lang="en"> 
  <head> 
    <meta charset="UTF-8" /> 
    <title>Title</title> 
    <style> 
      html, 
      body { 
        margin: 0; 
        padding: 0; 
        width: 100%; 
        height: 100%; 
        background: #191945; 
      } 
      .container { 
        width: 100%; 
        height: 100%; 
      } 
      .maptalks-msgBox { 
        background: transparent !important; 
        border: none !important; 
      } 
      .map-popover { 
        width: 100%; 
        height: 100px; 
        background: transparent; 
      } 
      .map-popover .mark-info { 
        position: relative; 
        display: inline-block; 
        width: 100%; 
        height: 100px; 
        background: rgba(0, 0, 255, 0.4); 
        border-radius: 4px; 
        border: 1px solid #00fff0; 
      } 
      .waver-span { 
        display: inline-block; 
        width: 3px; 
        height: 10px; 
        background-color: #00fff0; 
      } 
      .waver-box { 
        position: absolute; 
        right: 0; 
        top: 0; 
        width: 30px; 
        height: 12px; 
      } 
      .waver-spanr { 
        transform: skew(25deg); 
      } 
      .waver-span1 { 
        animation: shake1 1s infinite; 
      } 
      .waver-span2 { 
        animation: shake2 1s infinite; 
      } 
      .waver-span3 { 
        animation: shake3 1s infinite; 
      } 
      .waver-span4 { 
        animation: shake4 1s infinite; 
      } 
      @keyframes shake1 { 
        0% { 
          opacity: 1; 
        } 
        25% { 
          opacity: 0; 
        } 
        100% { 
          opacity: 1; 
        } 
      } 
      @keyframes shake2 { 
        0% { 
          opacity: 1; 
        } 
        50% { 
          opacity: 0; 
        } 
        100% { 
          opacity: 1; 
        } 
      } 
      @keyframes shake3 { 
        0% { 
          opacity: 1; 
        } 
        75% { 
          opacity: 0; 
        } 
        100% { 
          opacity: 1; 
        } 
      } 
      @keyframes shake4 { 
        0% { 
          opacity: 1; 
        } 
        100% { 
          opacity: 0; 
        } 
      } 
    </style> 
    <link 
      rel="stylesheet" 
      href="https://cdn.jsdelivr.net/npm/maptalks/dist/maptalks.css" 
    /> 
    <script 
      type="text/javascript" 
      src="https://cdn.jsdelivr.net/npm/maptalks/dist/maptalks.min.js" 
    ></script> 
    <script 
      type="text/javascript" 
      src="https://unpkg.com/maptalks.markercluster/dist/maptalks.markercluster.min.js" 
    ></script> 
    <script 
      type="text/javascript" 
      src="https://unpkg.com/maptalks.animatemarker/dist/maptalks.animatemarker.min.js" 
    ></script> 
    <script src="js/all_month.js"></script> 
    <script src="js/addressPoints.js"></script> 
  </head> 
  <body> 
    <div id="map" class="container"></div> 
    <script> 
      let map = new maptalks.Map("map", { 
        center: [-0.113049, 51.498568], 
        zoom: 14, 
        fpsOnInteracting: 0, // 解决marker不连续问题,关闭交互帧数限制,（默认25帧）限制 
        baseLayer: new maptalks.TileLayer("base", { 
          urlTemplate: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", 
          subdomains: ["a", "b", "c"], 
          attribution: 
            '&copy; <a href="http://www.osm.org" target="_blank">OpenStreetMap</a> contributors', 
          cssFilter: "sepia(100%) invert(90%)" 
        }) 
      }); 
      // mark点击弹框 
      function onClick(e) { 
        e.target.setInfoWindow({ 
          content: 
            '<div class="map-popover">' + 
            '<div class="mark-info">' + 
            '<div class="waver-box">' + 
            '        <span class="waver-span waver-spanr waver-span1"></span>' + 
            '        <span class="waver-span waver-spanr waver-span2"></span>' + 
            '        <span class="waver-span waver-spanr waver-span3"></span>' + 
            '        <span class="waver-span waver-spanr waver-span4"></span>' + 
            "    </div>" + 
            "</div>" + 
            "</div>", 
          width: 300, 
          minHeight: 100, 
          dy: 5, 
          autoPan: true, 
          custom: false, 
          autoOpenOn: "click", //set to null if not to open when clicking on marker 
          autoCloseOn: "click" 
        }); 
      } 
      /*maptalks的Mark的变形动画-----------------开始*/ 
      //图片标注+点击弹框 
      let markers = []; 
      for (let i = 0; i < addressPoints.length; i++) { 
        let a = addressPoints[i]; 
        markers.push( 
          new maptalks.Marker([a[0], a[1]], { 
            symbol: [ 
              { 
                markerType: "ellipse", 
                markerWidth: 20, 
                markerHeight: 20, 
                markerFill: "rgb(64, 158, 255)", 
                markerFillOpacity: 0.7, 
                markerLineColor: "#73b8ff", 
                markerLineWidth: 3 
              }, 
              { 
                markerType: "ellipse", 
                markerWidth: 10, 
                markerHeight: 10, 
                markerFill: "#006ddd", 
                markerFillOpacity: 0.7, 
                markerLineWidth: 0 
              } 
            ] 
          }).on("mousedown", onClick) 
        ); 
      } 
      
      // 聚合效果 
      let clusterLayer = new maptalks.ClusterLayer("cluster", markers, { 
        noClusterWithOneMarker: false, 
        maxClusterZoom: 16, 
        //"count" is an internal variable: marker count in the cluster. 
        symbol: { 
          markerType: "ellipse", 
          markerFill: { 
            property: "count", 
            type: "interval", 
            stops: [ 
              [0, "rgb(135, 196, 240)"], 
              [9, "#1bbc9b"], 
              [99, "rgb(216, 115, 149)"] 
            ] 
          }, 
          markerFillOpacity: 0.7, 
          markerLineOpacity: 1, 
          markerLineWidth: 3, 
          markerLineColor: "#fff", 
          markerWidth: { 
            property: "count", 
            type: "interval", 
            stops: [ 
              [0, 40], 
              [9, 60], 
              [99, 80] 
            ] 
          }, 
          markerHeight: { 
            property: "count", 
            type: "interval", 
            stops: [ 
              [0, 40], 
              [9, 60], 
              [99, 80] 
            ] 
          } 
        }, 
        drawClusterText: true, 
        geometryEvents: true, 
        single: true 
      }); 
      map.addLayer(clusterLayer); 
      // 执行mark的变形动画 
      setInterval(() => { 
        replay(); 
      }, 1500); 
      function replay() { 
        animate(); 
        setTimeout(() => { 
          reset(); 
        }, 600); 
      } 
      // mark的变形动画 
      function animate() { 
        markers.map(item => { 
          item.animate( 
            { 
              symbol: [ 
                { 
                  markerWidth: 40, 
                  markerHeight: 40, 
                  markerFillOpacity: 0.2, 
                  markerLineWidth: 1 
                }, 
                { 
                  markerWidth: 20, 
                  markerHeight: 20, 
                  markerFillOpacity: 0.4 
                } 
              ] 
            }, 
            { 
              duration: 600 
            } 
          ); 
        }); 
      } 
      // mark的变形动画 
      function reset() { 
        markers.map(item => { 
          item.animate( 
            { 
              symbol: [ 
                { 
                  markerWidth: 20, 
                  markerHeight: 20, 
                  markerFillOpacity: 0.7, 
                  markerLineWidth: 3 
                }, 
                { 
                  markerWidth: 10, 
                  markerHeight: 10, 
                  markerFillOpacity: 0.7 
                } 
              ] 
            }, 
            { 
              duration: 600 
            } 
          ); 
        }); 
      } 
      /*maptalks的Mark的变形动画-----------------结束*/ 
      /*maptalks.animatemarker插件------------------开始*/ 
      function getGradient(colors) { 
        return { 
          type: "radial", 
          colorStops: [ 
            [0.7, "rgba(" + colors.join() + ", 0.5)"], 
            [0.3, "rgba(" + colors.join() + ", 1)"], 
            [0.2, "rgba(" + colors.join() + ", 1)"], 
            [0.0, "rgba(" + colors.join() + ", 0)"] 
          ] 
        }; 
      } 
      //JSON序列化 - GeoJSON转化为Geometry 
      let geometries = maptalks.GeoJSON.toGeometry(earthquakes); 
      // 添加mark点击弹框操作 
      geometries.map(item => { 
        item.on("mousedown", onClick); 
      }); 
      let layer = new maptalks.AnimateMarkerLayer("animatemarker", geometries, { 
        animation: "scale,fade", 
        randomAnimation: true, 
        geometryEvents: true 
      }) 
        .setStyle([ 
          { 
            filter: ["<=", "mag", 2], 
            symbol: { 
              markerType: "ellipse", 
              markerLineWidth: 0, 
              markerFill: getGradient([135, 196, 240]), 
              markerFillOpacity: 0.8, 
              markerWidth: 50, 
              markerHeight: 50 
            } 
          }, 
          { 
            filter: ["<=", "mag", 5], 
            symbol: { 
              markerType: "ellipse", 
              markerLineWidth: 0, 
              markerFill: getGradient([255, 255, 0]), 
              markerFillOpacity: 0.8, 
              markerWidth: 50, 
              markerHeight: 50 
            } 
          }, 
          { 
            filter: [">", "mag", 5], 
            symbol: { 
              markerType: "ellipse", 
              markerLineWidth: 0, 
              markerFill: getGradient([216, 115, 149]), 
              markerFillOpacity: 0.8, 
              markerWidth: 50, 
              markerHeight: 50 
            } 
          } 
        ]) 
        .addTo(map); 
      /*maptalks.animatemarker插件------------------结束*/ 
      // 地图飞行到指定位置的效果 
      changeView(); 
      function changeView() { 
        map.animateTo( 
          { 
            center: [-0.113049, 51.498568], 
            zoom: 13, 
            pitch: 45, 
            bearing: 360 
          }, 
          { 
            duration: 3000 
          } 
        ); 
      } 
    </script> 
  </body> 
</html> 

```
```vue
js/all_month.js 
var earthquakes = { 
    "type": "FeatureCollection", "metadata": { "generated": 1493627919000, "url": "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson", "title": "USGS All Earthquakes, Past Month", "status": 200, "api": "1.5.7", "count": 9468 }, "features": [{ "type": "Feature", "properties": { "mag": 2.5, "place": "39km ESE of Sutton-Alpine, Alaska", "time": 1493625197031, "updated": 1493626024970, "tz": -540, "url": "https://earthquake.usgs.gov/earthquakes/eventpage/ak15851488", "detail": "https://earthquake.usgs.gov/earthquakes/feed/v1.0/detail/ak15851488.geojson", "felt": null, "cdi": null, "mmi": null, "alert": null, "status": "automatic", "tsunami": 0, "sig": 96, "net": "ak", "code": "15851488", "ids": ",ak15851488,", "sources": ",ak,", "types": ",geoserve,origin,", "nst": null, "dmin": null, "rms": 0.96, "gap": null, "magType": "ml", "type": "earthquake", "title": "M 2.5 - 39km ESE of Sutton-Alpine, Alaska" }, "geometry": { "type": "Point", "coordinates": [-0.12204900000004636, 51.498568000000006] }, "id": "ak15851488" }, 
    { "type": "Feature", "properties": { "mag": 0.31, "place": "10km NNE of Cabazon, California", "time": 1493624583650, "updated": 1493624807584, "tz": -480, "url": "https://earthquake.usgs.gov/earthquakes/eventpage/ci37637279", "detail": "https://earthquake.usgs.gov/earthquakes/feed/v1.0/detail/ci37637279.geojson", "felt": null, "cdi": null, "mmi": null, "alert": null, "status": "automatic", "tsunami": 0, "sig": 1, "net": "ci", "code": "37637279", "ids": ",ci37637279,", "sources": ",ci,", "types": ",geoserve,nearby-cities,origin,phase-data,scitech-link,", "nst": 12, "dmin": 0.09068, "rms": 0.3, "gap": 175, "magType": "ml", "type": "earthquake", "title": "M 0.3 - 10km NNE of Cabazon, California" }, "geometry": { "type": "Point", "coordinates": [-0.123049, 51.528568] }, "id": "ci37637279" }, 
    { "type": "Feature", "properties": { "mag": 1.14, "place": "17km N of Yucca Valley, California", "time": 1493624526120, "updated": 1493624745760, "tz": -480, "url": "https://earthquake.usgs.gov/earthquakes/eventpage/ci37637271", "detail": "https://earthquake.usgs.gov/earthquakes/feed/v1.0/detail/ci37637271.geojson", "felt": null, "cdi": null, "mmi": null, "alert": null, "status": "automatic", "tsunami": 0, "sig": 20, "net": "ci", "code": "37637271", "ids": ",ci37637271,", "sources": ",ci,", "types": ",geoserve,nearby-cities,origin,phase-data,scitech-link,", "nst": 31, "dmin": 0.05453, "rms": 0.26, "gap": 86, "magType": "ml", "type": "earthquake", "title": "M 1.1 - 17km N of Yucca Valley, California" }, "geometry": { "type": "Point", "coordinates": [-0.113049, 51.498568] }, "id": "ci37637271" }, 
    ], "bbox": [-179.9434, -65.0051, -3.43, 179.9803, 84.9861, 638.62] 
} 

```
