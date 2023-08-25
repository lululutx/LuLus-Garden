#vue

vue.extend()方法其实是vue的一个构造器，继承自vue

Component.vue
```vue
<template>
	<div>
		{{obj.name}}
	</div>
</template>

<script>
export default{
	data(){
		return{
			obj:{}
		}
	}
}
</script>
```

Page.vue
```vue
<template>
	<div id="dialog">
	</div>
</template>

<script>
import Component from "./Component";
let ComponentConstructor = Vue.extend(Component);
export default{
	moutend(){
		let comp = new ComponentConstructor({
			data: {
			  name: 'compName',
			},
		});
		comp..$mount('#dialog')//挂载
	}
}
</script>
```