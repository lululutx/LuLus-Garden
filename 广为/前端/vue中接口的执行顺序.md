同步和异步：用同步的思维来解决异步的问题当前接口调用需要等到接口返回值以后渲染页面 

名词解释  **async** 

async的用法，它作为关键字放到函数前面，用于表示函数是个异步函数，因为async就是异步的意思，异步函数也就以为着该函数的执行不会阻碍后面代码的执行，async函数返回的是一个promise对象。 

**await** 

**await的含义为等待。意思就是代码需要等待await后面的函数运行完并且有了返回结果只后，才继续执行下面的代码。这正是同步的效果** 

**理解好了什么是同步 异步后我们来看，用代码怎么实现** **vue页面主要代码如下** 

[https://www.jianshu.com/p/ab767311589d](https://www.jianshu.com/p/ab767311589d) 
```vue
 
    async init() { 
      //同步获取用户信息 
      const userInfo = await this.getUserInfo(); 
      this.getUserRole(userInfo.code); 
    }, 
    getUserInfo() { 
      return new Promise((resolve) => { 
        qryUserInfo().then((res) => { 
          if (res.code === 0) { 
            resolve(res.data); 
          } 
        }); 
      }); 
    }, 
    getUserRole() { 
      qryUserRole().then((res) => { 
        if (res.code === 0) { 
          this.roleInfo = res.data; 
        } 
      }); 
    }, 

```




