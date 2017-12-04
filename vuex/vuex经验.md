### vuex的一些经验


#### 一: 引入:

store.js:

```
	import Vue from 'vue'
	import Vuex from 'vuex'

	const store = {
		state,
		mutations,
		getters,
		actions
	}

	export default new Vuex.store(store);
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
重点二: store的生成是`new Vuex.store()`,
重点三:在store.js里写:`Vue.use(Vuex)`,在main.js里将store.js导入并且放到new Vue对象中。

vuex 是一个插件，当你加载了这个插件，并且 使用 Vue.use(Vuex) 的时候，
Vuex 执行了类似下面的代码，所以我们可以在组件里面直接使用 $store

Vue.prototype.$store = ...

$event 是 vue 提供的传递实践的参数。