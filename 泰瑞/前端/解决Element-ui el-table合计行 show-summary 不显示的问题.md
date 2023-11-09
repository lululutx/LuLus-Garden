

有时候需要在table的底部有合计，这时候官方给的是在table里设置，show-summary就可以了

但是给table加了一个固定高度的话，就不显示了，打开控制台可以看到这个合计是存在的

那么需要做两步,合计即可出现



- el-table中加上ref属性

```html
<el-table ref="tableData" highlight-current-row :data="tableData" height="600" border style="width: 100%"
            show-summary :summary-method="getSummaries"></el-table>
```
- 在updated生命周期函数

```js
updated() {
	this.$nextTick(() => {
		this.$refs['tableData'].doLayout();
	})
},
```
- 另外合计的是经常要设置保留小数点位数，可以用下面的方法
```js
methods: {
	// 截取当前数据到小数点后两位    
	numFilter(value) {
		const realVal = parseFloat(value).toFixed(2);
		return realVal;
	},
}
```

