### 1.安装库 npm i json-bigint

```undefined
npm i json-bigint
```

### 2.json-bigint 会把超出 JS 安全整数范围的数字转为一个 BigNumber 类型的对象，对象数据是它内部的一个算法处理之后的，我们要做的就是在使用的时候转为字符串来使用。在axios 中引用

```jsx
import axios from 'axios'

import jsonBig from 'json-bigint'

var json = '{ "value" : 9223372036854775807, "v2": 123 }'

console.log(jsonBig.parse(json))

const request = axios.create({
  baseURL: '你接口的基础路径', // 接口基础路径

  // transformResponse 允许自定义原始的响应数据（字符串）
  transformResponse: [function (data) {
    try {
      // 如果转换成功则返回转换的数据结果
      return jsonBig.parse(data)
    } catch (err) {
      // 如果转换失败，则包装为统一数据格式并返回
      return {
        data
      }
    }
  }]
})

export default request
```

### 3,完美解决Number大的问题

  
  
作者：生活不能没有前端  
链接：https://www.jianshu.com/p/9268bc8d461c  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。