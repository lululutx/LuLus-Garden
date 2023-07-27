Map <br />Map是一组键值对结构，具有很快的查找速度。 <br />Map的定义。 <br />//空map设置key-value <br />var map = new Map() <br />map.set("xiaohu",23); <br />map.set("xiaoming",35) <br />//或者构造传参key-value <br />var map = new Map([["xiaohu",23],["xiaoming",35]]); 




Map中的方法。 <br />var map = new Map(); //空map <br />map.set("xiaohu",43); //添加新的key-value <br />map.has("xiaohu");  //是否存在key"xiaohu" 返回Boolean值，true <br />map.get("xiaohu");  //返回对应value, 43, <br />map.delete("xiaohu")  //删除key "xiaohu" <br />map.get("xiaohu");  //undefined 


对一个key重复设值，后面的值会将前面的覆盖。 <br />var map = new Map(); <br />map.set('xioahu',66); <br />map.set('xiaohu',23); <br />map.get("xiaohu") //返回23 

Set <br />Set和Map类似，但set只存储key，且key不重复。 <br />Set的创建。 <br />var s1 = new Set(); // 空Set <br />s1.add(1); <br />s1.add(2); <br />s1.add(3); <br />var s2 = new Set([1, 2, 3]); // 含1, 2, 3 


插入重复的值，set会将重复的值进行过滤。 <br />var s = new Set([1, 2, 3]); <br />s.add(3); <br />s; // Set{1,2,3} <br />s.delete(3); <br />s; // Set([1,2]); 


Map及Set的遍历 <br />Array可以采用下标进行循环遍历，Map和Set就无法使用下标。为了统一集合类型，ES6标准引入了iterable类型，Array、Map、Set都属于iterable类型。 <br />具有iterable类型的集合可以通过新的for ... of循环来遍历。 <br />var a = ['A', 'B', 'C']; <br />var s = new Set(['A', 'B', 'C']); <br />var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]); <br />for (var x of a) { // 遍历Array <br />alert(x); <br />} <br />for (var x of s) { // 遍历Set <br />alert(x); <br />} <br />for (var x of m) { // 遍历Map <br />alert(x[0] + '=' + x[1]); <br />} 


更好的遍历：forEach <br />forEach是iterable内置的方法，它接收一个函数，每次迭代就自动回调该函数。 <br />var a = ['A', 'B', 'C']; <br />a.forEach(function (element, index, array) { <br />// element: 指向当前元素的值 <br />// index: 指向当前索引 <br />// array: 指向Array对象本身 <br />alert(element); <br />}); 

Set与Array类似，但Set没有索引，因此回调函数的前两个参数都是元素本身： <br />var s = new Set(['A', 'B', 'C']); <br />s.forEach(function (element, sameElement, set) { <br />alert(element); <br />}); 


Map的回调函数参数依次为value、key和map本身： <br />var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]); <br />m.forEach(function (value, key, map) { <br />alert(value); <br />}); 




















