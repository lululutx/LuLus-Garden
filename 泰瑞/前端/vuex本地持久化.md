# 前言
在使用vue开发项目的时候，我们经常会利用vuex来进行全局的状态管理从而达到实现数据全局共享的目的，但是使用vuex有一个缺点就是在[页面刷新](https://so.csdn.net/so/search?q=%E9%A1%B5%E9%9D%A2%E5%88%B7%E6%96%B0&spm=1001.2101.3001.7020)之后数据会消失从而使页面展示异常或者请求接口报错，比如用户在登录成功后，我们会从后台拿到用户的token，或者uid等信息，因为大部分的接口都需要用户的token传给后台来进行鉴权处理，因为用户刷新页面是不可控的因素，这个时候我们就要结合本地存储来保证数据的存储。
# 1. 手动利用HTML5的本地存储API
存储数据第一反应就是[前端开发](https://so.csdn.net/so/search?q=%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91&spm=1001.2101.3001.7020)常见的localStorage或sessionStorage，cookie等存储方式，具体的使用方式就是在在mutations定义的方法里对state需要存储的数据的状态操作的同时对存储也做对应的增删改查操作。
```
const state = () => {
  return {
    token: ''
  }
}
const actions = {
  async login ({ commit }, param) {
    // 在这里获取用户的登录信息，返回值包含token，uid等信息
    const userInfo = await userService.login(param)
    commit('setUser', userInfo)
    return userInfo
  }
}
const mutations = {
  setUser (state, data) {
    state.token = data.token
    // 在这里获取用户token, 同步存储数据
    localStorage.setItem('user-token', data.token)
  }
}
```
**这样state就会和存储一起存在并且与vuex同步，保证用户在刷新后数据不会消失，这样只适合简单数据的存储缓存。**
## 2.使用createPersistedState插件来实现数据存储
createPersistedState插件的本质还是使用同样的web开发常见的几种存储方式，其特点就是结合了存储方式,在入口统一配置就不需要每次都手动写存储方法。
## 使用方法
### 1.本地安装
```
npm install --save vuex-persistedstate
```
也可以直接引用打包好的js文件
```
<script src="https://unpkg.com/vuex-persistedstate/dist/vuex-persistedstate.js"></script>

```
**然后就可以使用 window.createPersistedState来获取到对象。推荐直接使用npm install 的方式下载到本地使用。**
### 2.在store入口index.js文件中引入并进行对应配置
```
import createPersistedState from "vuex-persistedstate";

const store = new Vuex.Store({
  // ...
  plugins: [createPersistedState()]
});

```
## Vuex模块的用法
可以在单独的文件中创建新的插件实例，但必须将其导入并添加到主Vuex文件中的plugins对象中。
```
/* module.js */
export const dataStore = {
  state: {
    data: []
  }
}

/* store.js */
import { dataStore } from './module'

const dataState = createPersistedState({
  paths: ['data']
})

export new Vuex.Store({
  modules: {
    dataStore
  },
  plugins: [dataState]
})

```
**createPersistedState插件默认使用storage:localStorage存储，默认的存储键名key是“vuex”,而且提供了option参数来修改默认配置和个性化存储**。
### option常用的配置项
| **参数** | **描述** |
| --- | --- |
| key | 存储数据的键名。（默认：vuex） |
| paths | 部分路径可部分保留状态的数组。如果没有给出路径，则将保留完整状态。如果给出一个空数组，则不会保留任何状态。必须使用点符号指定路径。如果使用模块，请包括模块名称（默认：[]） |
| reducer | 将根据给定路径调用以减少状态持久化的函数 |
| storage | 指定存储数据的方式。默认为localStorage ，也可以设置 sessionStorage |
| getState | 用来重新补充先前持久状态的功能，默认使用：storage定义获取的方式 |
| setState | 用以保持给定状态的函数。默认使用：storage定义的设置值方式 |
| filter | 一个将被调用以过滤setState最终将在存储中筛选过滤的函数。默认为() => true。 |

### 想要使用存储到sessionStorage，只需要修改storage的配置如下：
```
import createPersistedState from "vuex-persistedstate"
const store = new Vuex.Store({
  // ...
  plugins: [createPersistedState({
      storage: window.sessionStorage
  })]
})

```
### 想要使用存储到cookie的配置如下：
因为使用cookie来操作数据并不是和localStorage一样来使用getItem、setItem、removeItem方法来操作数据，所以我们要进行对应的方法配置如下：
```
mport { Store } from "vuex"
import createPersistedState from "vuex-persistedstate"
import * as Cookies from "js-cookie"
// 使用js-cookie来简化cookie操作
const store = new Store({
  // ...
  plugins: [
    createPersistedState({
      storage: {
        getItem: (key) => Cookies.get(key),
        setItem: (key, value) =>
          Cookies.set(key, value, { expires: 3, secure: true }),
        removeItem: (key) => Cookies.remove(key)
      }
    })
  ]
})

```
因为这里对storage进行了重写补充，所以如果需要使用本地存储但需要保护数据的内容，则可以对其进行加密。
```
import { Store } from "vuex";
import createPersistedState from "vuex-persistedstate";
import SecureLS from "secure-ls";
var ls = new SecureLS({ isCompression: false });

// secure-ls 通过高级别的加密和数据压缩来保护localStorage数据 https://github.com/softvar/secure-ls

const store = new Store({
  // ...
  plugins: [
    createPersistedState({
      storage: {
        getItem: (key) => ls.get(key),
        setItem: (key, value) => ls.set(key, value),
        removeItem: (key) => ls.remove(key)
      }
    })
  ]
})

```
### 缓存Vuex多个模块下的指定某个模块的state，通过修改path配置来实现
```
/* user-module */
export const user = {
  state: {
    token: '',
    role: ''
  }
}
/* profile-module */
export const profile = {
  state: {
    name: '',
    company: ''
  }
}
/* modules目录下的index.js */
import user from './user'
import profile from './profile'
export default {
  user,
  profile
}
/* store.js */
import modules from './modules'
let store = new Vuex.Store({
  modules,
  plugins: [
    createPersistedState({
      key: 'zdao',
      paths: ['user'] // 这里便只会缓存user下的state值
    })
  ]
})
```
### 缓存state下指定的部分数据,配置如下
```
const state = () => {
  return {
    token: '',
    uid: ''
  }
}

import createPersistedState from "vuex-persistedstate"
const store = new Vuex.Store({
  // ...
  plugins: [createPersistedState({
      storage: window.sessionStorage,
      reducer(val) {
          return {
          // 只储存state中的token，而不会缓存uid
          token: val.token
        }
     }
  })]
```
如果path和reducer同时存在则使用reducer, 忽悠paths属性
```
plugins: [createPersistedState({
      storage: window.sessionStorage,
      paths: ['user'],
      reducer(val) {
      console.log(val)  //   { user: {token: '123', uid: '456'}, profile: {name: 'zdao', company: '上海合合信息股份有限公司'}}
          return {
            company: val.profile.company
          }
       }
   })]

```
### 使用filter属性可以过滤掉不同的mutation提交造成不想要的数据更新缓存。
我们同时由setUserA和setUserB来更改token的状态，但是我们在触发setUserB时候的修改不要影响到所存储的token值，我们就可以设置filter来过滤mutation
```javascript
/* user-module */
const mutations = {
  setUserA (state, data) {
    state.token = data.token
  },
  setUserB (state, data) {
    state.token = data.token
  }
}
/* store.js */
plugins: [
    createPersistedState({
      key: 'zdao',
      paths: ['user'],
      filter: (mutation) => {
        console.log(mutation)
        /*  mutation
        {
          payload: { token: "5491CC8FBB36408C9R7167yY"}
          type: "user/setUserA"
        }
        */
        // 这个时候触发setUserB则不会触发影响缓存的值
        return mutation.type.indexOf('user/setUserA') !== -1
      }
    })
  ]
```

[https://blog.csdn.net/weixin_55042716/article/details/123107602](https://blog.csdn.net/weixin_55042716/article/details/123107602)
