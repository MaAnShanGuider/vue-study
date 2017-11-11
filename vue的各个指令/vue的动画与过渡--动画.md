### vue的动画

CSS 动画用法同 CSS 过渡，区别是在动画中 v-enter 类名在节点插入 DOM 后不会立即删除，而是在 animationend 事件触发时删除。

代码示例:

```
	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<style>
	*{margin:0;padding: 0;}
	.child{
		height:100px;width:100px;background: red;position: absolute;top:100px;
	}

	.xixi-enter-active {
		animation: xixi-in .5s;
	}
	.xixi-leave-active {
		animation: xixi-in .5s reverse;
	}
	@keyframes xixi-in {
		0% {
			left:-100px;
		}
		100% {
			left:0px;
		}
	}

	</style>
	<script src="../lib/vue.js"></script>
	<body>
		<div id="box">
			<button @click='isShow = !isShow'>我要变化</button>
			<transition name="xixi">
				<div v-if='isShow' class='child'>aaaaaaaaaa</div>
			</transition>
		</div>
	</body>
	<script>
		var vm = new Vue({
			el:'#box',
			data:{
				isShow:false
			}
		})
	</script>
	</html>


```

### 自定义过渡动画的类名：

我们可以通过以下特性来自定义过渡类名：

* enter-class
* enter-active-class
* enter-to-class (2.1.8+)
* leave-class
* leave-active-class
* leave-to-class (2.1.8+)

他们的优先级高于普通的类名，注意，仅仅是类名。优先级最高的还是id。这样就可以别的css动画库来代替自己写的。

代码示例:

```
	<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">
	<div id="example-3">
	  <button @click="show = !show">
	    Toggle render
	  </button>
	  <transition
	    name="custom-classes-transition"
	    enter-active-class="animated tada"
	    leave-active-class="animated bounceOutRight"
	  >
	    <p v-if="show">hello</p>
	  </transition>
	</div>

	new Vue({
	  el: '#example-3',
	  data: {
	    show: true
	  }
	})
```

### 子组件的动画与过渡

在切换的时候会分别执行自己的进入过渡，离开过渡。

```
		<!DOCTYPE html>
		<html lang="en">
		<head>
			<meta charset="UTF-8">
			<title>Document</title>
			<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">
		</head>
		<style>
		.component-fade-enter-active, .component-fade-leave-active {
		  transition: opacity .3s ease;
		}
		.component-fade-enter, .component-fade-leave-to
		/* .component-fade-leave-active for below version 2.1.8 */ {
		  opacity: 0;
		}
		</style>
		<script src="../lib/vue.js"></script>
		<body>


			<div id="box">
				<button @click='view= view == "v-a"?"v-b":"v-a"'>超级变变变</button>
					<transition name="component-fade" mode="out-in" enter-active-class='animated fadeInLeft' leave-active-class='animated fadeOutLeft'>
						<component v-bind:is="view"></component>
					</transition>

			</div>
		</body>
		<script>
		var vm = new Vue({
			  el: '#box',
			  data: {
			    view: 'v-a'
			  },
			  methonds:{

			  },
			  components: {
			    'v-a': {
			      template: '<div>Component A</div>'
			    },
			    'v-b': {
			      template: '<div>Component B</div>'
			    }
			  }
			})
		</script>
		</html>

```

`<transition></transition>`标签有个mode属性：
同时生效的进入和离开的过渡不能满足所有要求，所以 Vue 提供了 过渡模式

  * in-out：新元素先进行过渡，完成之后当前元素过渡离开。

  * out-in：当前元素先进行过渡，完成之后新元素过渡进入。
