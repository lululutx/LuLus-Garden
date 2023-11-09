很多时候，我们希望前端的程序时放在二级目录下的，比如 http://www.xxx.com/app/ (app就是二级)

前端vue需要注意的是，静态资源文件的路径和路由的路径，两个默认都是针对／目录配置的，需要修改。

具体修改方法如下所示：

1.[nginx](https://so.csdn.net/so/search?q=nginx&spm=1001.2101.3001.7020)配置文件：
```
  location /app {

            alias   /usr/local/etc/nginx/html/ruoyi;

            try_files $uri $uri/ /app/index.html; （注意重定向路径要带上/app/）
            #index  index.html;

        }
```



2.vue.config.js配置publicPath属性

```
module.exports = {
  publicPath: process.env.VUE_APP_PATH,
}
```

3.vuerouter配置base属性

```
const vueRouter = new Router({
  base: process.env.VUE_APP_PATH,
})
```