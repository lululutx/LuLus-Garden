**箭头函数和function的区别** 1、写法不同 let foo=()=>{console.log('foo');} function foo(){console.log('foo');} 2、this的指向不一样 使用function定义的函数，this的指向随着调用环境的变化而变化的，而箭头函数中的this指向是固定不变的，一直指向的是定义函数的环境。 

function的this指向function的调用者，箭头函数没有自己的this，this指向函数定义时所处的上下文。 

来自 <[https://blog.csdn.net/weixin_39820527/article/details/118650588](https://blog.csdn.net/weixin_39820527/article/details/118650588)>  


3、构造函数 //使用function方法定义构造函数 function Person(name,age){this.name =name;this.age =age;} var lenhart =newPerson(lenhart,25); console.log(lenhart);//{name: 'lenhart', age: 25} //尝试使用箭头函数 var Person=(name,age)=>{this.name =name;this.age =age;}; var lenhart =newPerson('lenhart',25); //Uncaught TypeError: Person is not a constructor 4、变量提升 由于js的内存机制，函数声明的时候存在变量提升， foo()// foofunctionfoo(){console.log('foo')} fn()//Uncaught TypeError: fn is not a functionletfn=()=>{console.log('fn');} 

来自 <[https://www.jianshu.com/p/3a0921edd4b7](https://www.jianshu.com/p/3a0921edd4b7)>  

