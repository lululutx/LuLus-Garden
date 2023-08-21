**FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed - JavaScript heap out of memory （JavaScript堆内存不足）**

1，安装依赖npm install cross-env increase-memory-limit

2，在package.json 里的 script 里进行配置LIMIT是你想分配的内存大小，这里的8192单位是M也就是8G，大小可根据情况而定。"scripts": {"limit": "cross-env LIMIT=8192 increase-memory-limit"},

3，执行一次 npm run limit ，然后重新启动项目即可。但是这时候，重新启动会报错，如图：'"node --max-old-space-size=8192"' 不是内部或外部命令，也不是可运行的程序或批处理文件。在项目的 node_modules/.bin 文件下找到所以的 *.cmd 文件，"%_prog%" 去掉 双引号 %_prog%可在 node_modules 同级下，写一个fix-memory-limit.config.js文件进行批次处理。
```javascript
//运行项目前通过node执行此脚本 (此脚本与 node modules 目录同级)
const fs = require('fs')
const path = require('path')
const wfPath = path.resolve(__dirname, './node_modules/.bin')


fs.readdir(wfPath, (err, files) => {
  if (err) {
    console.log(err)
  } else {
    if (files.length != 0) {
      files.forEach(item => {
        if (item.split('.')[1] === 'cmd') {
          replaceStr(`${wfPath}/${item}`, /"%_prog%"/, "%_prog%")
        }
      })
    }
  }
})


function replaceStr(filePath, sourceRegx, targetStr) {
  fs.readFile(filePath, (err, data) => {
    if (err) {
      console.log(err)
    } else {
      let str = data.toString()
      str = str.replace(sourceRegx, targetStr)
      fs.writeFile(filePath, str, err => {
        console.log(err)
      })
    }
  })
}
```
然后修改 package.json里的 script里的语句先处理内存溢出问题，然后再执行js，进行替换&& 运算符，（相继执行，只有前一个执行成功才会执行下一个）
```javascript
 "scripts": {

    "limit": "cross-env LIMIT=8192 increase-memory-limit && node fix-memory-limit.config.js"

  },

```
