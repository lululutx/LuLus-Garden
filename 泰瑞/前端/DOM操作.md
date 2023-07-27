### js里面dom元素操作不支持隐式迭代（js操作dom不能直接操作一堆，只能单个操作）
```html
<!DOCTYPE html>
<html>
  <head lang="en">
    <meta charset="UTF-8">
    <title></title>
  </head>
  <body>
    <button class="btn" id="butinfo" name="an">按钮</button>
    <button class="btn" id="butlist" name="an">按钮</button>
    <div></div>
    <script>
      /*
     * 2个固定获取（dom）
     * querySelector   获取之后返回的对象为单个对象
     * querySelectorAll  返回的是多个对象   集合类型NodeList
     *    返回的是一个集合  这个集合至少有一个子集
     *
     *静态获取  指的是获取页面现有的元素，通过代码添加元素 获取不到
     * */
      var jt=document.querySelector(".btn");//方法的参数为元素选择器
      var jtall=document.querySelectorAll(".btn");
      console.log(jt);
      console.log(jtall);
      console.log(jtall[0]);
      console.log(jtall[1]);

      /*
     * 2个特殊获取
     * 指的是特定元素获取   body  html
     * 这两个返回的是单个对象
     * */
      var html =document.documentElement;
      console.log(html);
      var body =document.body;
      console.log(body);

      /*
     *  4个动态获取方案
     *  方法参数为字符串
     *  根据id  class  tagName  Name
     * */
      var id = document.getElementById("butinfo");
      console.log(id);
      /*id 属于特殊获取方案，不用获取可以直接使用*/
      console.log(butinfo);

      var Class = document.getElementsByClassName("but");
      console.log(Class);
      console.log(Class[0]);
      console.log(Class[1]);

      var tagname = document.getElementsByTagName("button");
      console.log(tagname);
      console.log(tagname[0]);
      console.log(tagname[1]);

      var name1 = document.getElementsByName("an");
      console.log(name1);
      console.log(name1[0]);
      console.log(name1[1]);

      /*先获取---再添加---再输出   动态获取*/
      var create=document.getElementsByClassName("btn");//动态获取
      /* var query=document.getElementsByClassName(".btn");//静态获取*/
      var list = document.createElement("div");//创建一个标签  div没name属性
      list.className="btn";
      document.body.appendChild(list);//添加进去
      console.log(create);//在输出
    </script>
  </body>
</html>

```
### 操作dom的属性和方法
```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        .btn{
            background-color: deeppink;
        }
    </style>
</head>
<body>
<button class="btn list" name="but" id="bttninfo"
        style="width: 100px;height: 30px;color: blue">按钮</button>
<script>
    var btnlist = document.getElementsByClassName("btn")[0];
    /*
     * dom属性操作
     * 属性获取或者设置
     * 原生js  操作样式  只能操作元素的行内样式
     * */
    //操作类名称
    console.log(btnlist);
    console.log(btnlist.className);
    console.log(btnlist.name);
    console.log(btnlist.id);
    console.log(btnlist.style);
    console.log(btnlist.style.width);
/*设置属性*/
    btnlist.className="btn pink";
    console.log(btnlist.classList);//返回元素的class 列表
    console.log(btnlist.classList[0]);
    console.log(btnlist.classList[1]);
    btnlist.classList.add("date");//类添加
    console.log(btnlist.classList.contains("btn"));//检测元素是否具有某个类  true false
    console.log(btnlist.classList.contains("time"));
    console.log(btnlist.classList.length);//输出元素类几个
    btnlist.classList.remove("date");//移除类
    btnlist.classList.toggle("date");//toggle两个参数 参数1是类名称  参数2是boolean值  true 添加 false 移除  可以不写

    //style的设置   原生js可以单独设置元素的行内样式
    btnlist.style.width="200px";
    btnlist.style.backgroundPosition="1px 2px";
    btnlist.style.border="1px solid red";

    //设置自定义属性
    btnlist.setAttribute("cls-tag","自定义");
    //获取自定义属性
    console.log(btnlist.getAttribute("cls-tag"));

    /*获取dom元素的非行内样式
    * 只能获取非行内样式   不能设置   read-only  只读不写
    * */
    console.log(window.getComputedStyle(btnlist).backgroundColor);
</script>
</body>
</html>

```
### dom子父节点操作
```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>dom子父节点操作</title>
</head>
<body>
<!--节点操作-->
<button>按钮1</button>
<button>按钮2</button>
<button id="btn">按钮3</button><button>按钮4</button>
<button>按钮5</button>
<button>按钮6</button>
<!--获取子元素子节点-->
<ul id="menu">
    <li>蔬菜</li>
    <li id="liinfo">水果</li>
</ul>
<input type="text" id="userName"/>

<h1>1234</h1>
<script>
    /*dom元素的节点操作*/
    var btn = document.querySelector("#btn");
    console.log(btn);

    /*
     * 获取当前元素的同胞兄弟元素
     * nextElementSibling  下一个元素
     * previousElementSibling 前一个元素
     * nextSibling   获取的是下一个节点
     * previousSibling  获取的是上一个节点
     * （节点包含：符号，文本，元素）
    * */
    console.log(btn.nextElementSibling);
    console.log(btn.previousElementSibling);
    console.log(btn.nextSibling);
    console.log(btn.previousSibling);

    /*
    * 获取元素的子元素或字节点
    * childElementCount  获取子元素的个数
    * children  获取的是子元素的集合
    * childNodes  获取的是子节点的集合
    * */
    console.log(menu.childElementCount);
    console.log(menu.children);
    console.log(menu.childNodes);

    //对子元素进行遍历
    for(var i = 0;i<menu.children.length;i++){
        console.log(menu.children[i]);
    }
    console.log(menu.childNodes);

    //对子节点进行遍历,子节点包含  文本、符号、元素  如何做区分？
    for(var i = 0;i<menu.children.length;i++){
        if(menu.childNodes[i].nodeName.toLowerCase()=="li"){
            console.log(menu.childNodes[i].childNodes[0].nodeValue);
        }
    }

    //元素的父元素 parentElement
    console.log(liinfo.parentElement);
    //元素的父节点
    console.log(liinfo.parentNode);

    /*
     * 获取子节点最后一个和第一个
     * firstChild  获取元素里面的第一个子节点
     * firstElementChild  获取第一个子元素
     * lastChild   获取元素的最后一个子节点
     * lastElementChild  获取最后一个元素
     * */
    console.log(menu.firstChild);
    console.log(menu.firstElementChild);
    console.log(menu.lastChild);
    console.log(menu.lastElementChild);

    /* 动态创建dom元素* */
    var ipt = document.createElement("input");
    ipt.type = "button";
    ipt.value = "按钮";
    console.log(ipt);
    /*  追加到页面元素
     * appendChild  子元素追加方法
     * 追加到当前元素的内容之后
     * */
    document.body.appendChild(ipt);

    //insertBefore   将插入的节点  添加已有的节点之前
    document.body.insertBefore(ipt, userName);
    //insertAfter 原生js不存在  实现insertAfter的功能  插入到一个节点之后
    Node.prototype.insertAfter = function (newchild, refchild) {
        console.log(this);
        this.insertBefore(newchild, refchild.nextElementSibling);
    }
    document.body.insertAfter(ipt, userName);

    //克隆节点
    //cloneNode  赋值节点   参数true  克隆子节点
    document.body.appendChild(ipt.cloneNode());

    //dom元素节点移除
    document.getElementsByTagName("h1")[0].remove();
</script>
</body>
</html>

```
