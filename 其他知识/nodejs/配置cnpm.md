方法1、临时使用 <br />npm --registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org/) install [依赖的名称] <br />方法2、持久使用（慎用） <br />注：这种方法不建议使用，因为使用这种方式会造成之后都要通过淘宝镜像来获取依赖包，如果是公司内部发布到npm的依赖包，会出现下载失败的情况 <br />npm config set registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org/) <br />检查是否配置成功 <br />npm config get registry <br />方法3、安装cnpm（推荐） <br />推荐这种方式是因为既不会影响npm命令，又不用每次都写淘宝地址进行依赖包的安装 <br />npm install -g cnpm --registry=https://registry.npm.taobao.org 

来自 <[https://zhuanlan.zhihu.com/p/120159632](https://zhuanlan.zhihu.com/p/120159632)>  

