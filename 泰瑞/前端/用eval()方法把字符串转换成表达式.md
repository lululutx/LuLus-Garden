
**`eval()`** 函数会将传入的字符串当做 JavaScript 代码进行执行。

```js
let a = 1;
let str = "a == 2"
console.log(str)//a == 2
console.log(eval(str))//false
if(str){
	//go there
}else{
	
}

if(eval(str)){

}else{
	//go there
}

```


```js
console.log(eval('2 + 2'));
// Expected output: 4

console.log(eval(new String('2 + 2')));
// Expected output: 2 + 2

console.log(eval('2 + 2') === eval('4'));
// Expected output: true

console.log(eval('2 + 2') === eval(new String('2 + 2')));
// Expected output: false

```