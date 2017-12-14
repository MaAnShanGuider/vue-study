### vuex的一些经验


#### 一: 引入:

store.js:

```
	import Vue from 'vue'
	import Vuex from 'vuex'

	Vue.use(Vuex);

	const store = {
		state,
		mutations,
		getters,
		actions
	}

	export default new Vuex.Store(store);
```

main.js

```
	import Vue from 'vue'
	import App from './App'
	import router from './router'
	import store from './store/store.js'

	Vue.config.productionTip = false
	new Vue({
	  el: '#app',
	  router,
	  store,
	  template: '<App/>',
	  components: { App }
	})
	
```

重点一:`import Vue from 'vue'` 以及`import Vuex from 'vuex'`,from后面的vue与vuex是全都是小写!!!
重点二: store的生成是`new Vuex.Store()`,
重点三:在store.js里写:`Vue.use(Vuex)`,在main.js里将store.js导入并且放到new Vue对象中。

vuex 是一个插件，当你加载了这个插件，并且 使用 Vue.use(Vuex) 的时候，
Vuex 执行了类似下面的代码，所以我们可以在组件里面直接使用 $store

Vue.prototype.$store = ...

$event 是 vue 提供的传递实践的参数。

---------------

### 二: actions与mutations的第一个参数的区别:

组件通过`$store.dispatch('actionFunc',str)`,发送str到vuex的actions，然后在它的actionFunc中，第一个参数是子state，第二个参数是我们传过来的数据。

```
	actions:{
		actionFunc(context,str){
			console.log(context);//----这是我们的这个子store
			console.log(this);   //-----这是我们的根store

			//-------将数据提交给mutations
			context.commit('mutationFunc',str);//-----传数据过去
		}
	}
```


```
	mutations:{
		mutationFunc(context,str){
			console.log(context);//---仅仅是子store的state
			console.log(this);   //------这也是我们的根store

			context.isShow = str; //--将接收到的数据赋给state
		}
	}
	
```

-----------

### 将vuex赋值给组件的data

**注意:**这是很重要的，如果state是传一个基本类型的值，(传值)，那么会satae改变，子组件的data并不会改变，也就不会重新渲染。

但是，如果是传一个对象，或者数组之类的符合类型，(传引用),那么state改变，子组件的data也会改变，同时，子组件的data的改变也会造成state改变。

这个和redux的子reducer返回的子state是一样的，如果是原来的，那么不会改变，只有返回新值才会重新渲染组件。

只有vuex的getters改变，才会直接使组件重新渲染

-----------------

### 关于getters

**getters**相当于vuex的计算属性，当它的依赖对象发生变化时，getters就会触发。

getters可以通过`store.getters`读取，

##### mapGetters函数

mapGetters函数可以将getters映射到局部计算属性。

```	
	import { mapGetters } from 'vuex'

	export default{
		...
		computed:{
			//--使用对象展开运算符将getters混入到局部的计算属性当中
			...mapGetters(['myGetters1','myGetters2'])
		}
		...
	}

```

如果要将Getters取另外一个名字可以使用对象形式参数:

```
	mapGetters({childComputed1:'myGetters1',childComputed2:'myGetters2'})
```