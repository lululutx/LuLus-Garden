Map Map是一组键值对结构，具有很快的查找速度。 Map的定义。 //空map设置key-value var map = new Map() map.set("xiaohu",23); map.set("xiaoming",35) //或者构造传参key-value var map = new Map([["xiaohu",23],["xiaoming",35]]); 




Map中的方法。 var map = new Map(); //空map map.set("xiaohu",43); //添加新的key-value map.has("xiaohu");  //是否存在key"xiaohu" 返回Boolean值，true map.get("xiaohu");  //返回对应value, 43, map.delete("xiaohu")  //删除key "xiaohu" map.get("xiaohu");  //undefined 


对一个key重复设值，后面的值会将前面的覆盖。 var map = new Map(); map.set('xioahu',66); map.set('xiaohu',23); map.get("xiaohu") //返回23 

Set Set和Map类似，但set只存储key，且key不重复。 Set的创建。 var s1 = new Set(); // 空Set s1.add(1); s1.add(2); s1.add(3); var s2 = new Set([1, 2, 3]); // 含1, 2, 3 


插入重复的值，set会将重复的值进行过滤。 var s = new Set([1, 2, 3]); s.add(3); s; // Set{1,2,3} s.delete(3); s; // Set([1,2]); 


Map及Set的遍历 Array可以采用下标进行循环遍历，Map和Set就无法使用下标。为了统一集合类型，ES6标准引入了iterable类型，Array、Map、Set都属于iterable类型。 具有iterable类型的集合可以通过新的for ... of循环来遍历。 var a = ['A', 'B', 'C']; var s = new Set(['A', 'B', 'C']); var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]); for (var x of a) { // 遍历Array alert(x); } for (var x of s) { // 遍历Set alert(x); } for (var x of m) { // 遍历Map alert(x[0] + '=' + x[1]); } 


更好的遍历：forEach forEach是iterable内置的方法，它接收一个函数，每次迭代就自动回调该函数。 var a = ['A', 'B', 'C']; a.forEach(function (element, index, array) { // element: 指向当前元素的值 // index: 指向当前索引 // array: 指向Array对象本身 alert(element); }); 

Set与Array类似，但Set没有索引，因此回调函数的前两个参数都是元素本身： var s = new Set(['A', 'B', 'C']); s.forEach(function (element, sameElement, set) { alert(element); }); 


Map的回调函数参数依次为value、key和map本身： var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]); m.forEach(function (value, key, map) { alert(value); }); 




















