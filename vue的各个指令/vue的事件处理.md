### vue的事件处理

在vue的事件处理中怎么访问原生自带的event对象?

答:可以用特殊变量 $event 把它传入方法。

		<div id="box">
			<div @click='tiao($event)'>{{datalist.Alice}}</div>
		</div>
		var vm = new Vue({
			el:'#box',
			data:{
				datalist:{'Alice':'我是爱丽丝','Bob':'我是鲍勃','Cindy':'我是辛迪'}
				},
			methods:{
				tiao(event){
					console.log(event.target);
					//--会打印出：<div>我是爱丽丝</div>
				}
			}	
			})

-------

#### 事件修饰符

在事件处理程序中调用 event.preventDefault() 或 event.stopPropagation() 是非常常见的需求。尽管我们可以在 methods 中轻松实现这点，但更好的方式是：methods 只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 v-on 提供了事件修饰符。通过由点 (.) 表示的指令后缀来调用修饰符。

* .stop
* .prevent
* .capture
* .self
* .once

	<!-- 阻止单击事件冒泡 -->
	<a v-on:click.stop="doThis"></a>

	/..
	<!-- 提交事件不再重载页面 -->
	<form v-on:submit.prevent="onSubmit"></form>

	/..
	<!-- 修饰符可以串联 -->
	<a v-on:click.stop.prevent="doThat"></a>

	/..
	<!-- 只有修饰符 -->
	<form v-on:submit.prevent></form>

	/..
	<!-- 添加事件侦听器时使用事件捕获模式 -->
	<div v-on:click.capture="doThis">...</div>

	/..
	<!-- 只当事件在该元素本身 (比如不是子元素) 触发时触发回调 -->
	<div v-on:click.self="doThat">...</div>

	/..
	<!-- 点击事件将只会触发一次 -->
	<a v-on:click.once="doThis"></a>

### v-on:是可以绑定多个函数的

 eg:

```
	<div v=on:click='funC1();funC2();'></div>
 ```

就像上面的一样，和普通js代码里调用就行。