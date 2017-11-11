### v-if,v-show,v-for

**最重要的点，自然放在最前面**

...我语文不好，还是直接搬文档的吧。

**Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使 Vue 变得非常快之外，还有其它一些好处。**例如，如果你允许用户在不同的登录方式之间切换：

	<template v-if="loginType === 'username'">
	  <label>Username</label>
	  <input placeholder="Enter your username">
	</template>
	<template v-else>
	  <label>Email</label>
	  <input placeholder="Enter your email address">
	</template>

那么在上面的代码中切换 loginType 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，`<input>` 不会被替换掉——仅仅是替换了它的 placeholder。这样，是复用了原来的元素。

这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 key 属性即可：

	<template v-if="loginType === 'username'">
	  <label>Username</label>
	  <input placeholder="Enter your username" key="username-input">
	</template>
	<template v-else>
	  <label>Email</label>
	  <input placeholder="Enter your email address" key="email-input">
	</template>

现在，每次切换时，输入框都将被重新渲染。

注意，`<label>` 元素仍然会被高效地复用，因为它们没有添加 key 属性。

**v-if对于前面的呢补充**
v-if,v-show前面已经提到过了，区别在于，v-if是真正的切换标签元素，而v-show只是单纯的将标签元素的display变为none。
**条件渲染的一个特点**

这个特点就是可以使用一个`<template></template>`包裹住我们希望出现或者消失的代码块,而且在渲染的时候，`<template></template>`是不会被渲染出来的。

**eg**

	<template v-if="ok">
	  <h1>Title</h1>
	  <p>Paragraph 1</p>
	  <p>Paragraph 2</p>
	</template>

ok为真，将打印出这整段。ok为假，则反之。

---------

**而第二个重点就是我们的v-for**

### 数组的v-for
一个最简单的例子：

		<ul id="box">
			<li v-for='data in datalist'>{{data}}</li>
		</ul>
		var vm = new Vue({
			el:'#box',
			data:{
				datalist:['诸葛亮','司马懿','郭嘉']
			}
		});

好了，上面的看了就看了，重点也不是它。

		<li v-for='(data,index) in datalist'></li>

第二个参数是数组的序号，可选。

-----

### 对象的v-for

		<ul id="box">
			<li v-for='(value,keyName,index) in datalist'>我是第{{index}}个元素:{{value}},来自{{keyName}}</li>
		</ul>
		var vm = new Vue({
			el:'#box',
			data:{
				datalist:{'ALice':'我是爱丽丝','Bob':'我是鲍勃','Cindy':'我是辛迪'}
				}		
			})

**第一个参数value，是键值；第二个参数valueName，是键名；第三个参数index，是索引。**


### 最重要的一个点：key属性

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。

这个默认的模式是高效的，但是只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出。

为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。理想的 key 值是每项都有的且唯一的 id。
	
	<div v-for="item in items" :key="item.id">
	  <!-- 内容 -->
	</div>

建议尽可能在使用 v-for 时提供 key，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。

因为它是 Vue 识别节点的一个通用机制，key 并不与 v-for 特别关联，key 还具有其他用途，我们将在后面的指南中看到其他用途。

-------

### 数组更新检测

#### 变异方法

当使用一下数组方法时，会导致视图重新渲染。

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

你打开控制台，然后用前面例子的*诸葛亮*那段 数组调用变异方法：vm.datalist.push('称共') 。页面会刷新。
记住了，只有这些方法被调用才会触发视图的更新，也就是说除了这几个方法以外的对datalist的任何其他操作，都不会导致视图的重新渲染。像下面这些，当点击按钮时并不会重新渲染页面。
	
		<ul id="box">
			<button @click='change()'>我要超级变变变</button>
			<li v-for='data in datalist'>{{data.name}}</li>
		</ul>
		var vm = new Vue({
			el:'#box',
			data:{
				datalist:[{name:'Alice',age:22},{name:'Bob',age:24},{name:'Cindy',age:25}]
				},
			methods:{
				change(){
					var arr = [];
				for(var i in this.datalist){
					arr[i] = this.datalist[i];
				}
				this.datalist[0]=arr[1];
				this.datalist[1]=arr[2];
				this.datalist[2]=arr[0];
				}
			}		
			})

**但是可以使用this.$forceUptdate()强制更新视图。**
	
		var vm = new Vue({
			el:'#box',
			data:{
				datalist:[{name:'Alice',age:22},{name:'Bob',age:24},{name:'Cindy',age:25}]
			},
			methods:{
				change(){
					var arr = [];
					for(var i in this.datalist){
						arr[i] = this.datalist[i];
					}
					this.datalist[0]=arr[1];
					this.datalist[1]=arr[2];
					this.datalist[2]=arr[0];
					//-----加这个属性，是为了强制刷新v-for指令，不然的话，就算点击按钮，也不会列表页面也不会重新渲染。
					//這是 vue 內建方法，會強制更新當前組件（以及 Slot 裡面的組件，但不包含全部子組件 )
					this.$forceUpdate();
				}
			}
			
		})

**但是必须注意，在Alice那段，也就是datalist是个对象时，给在控制台给datalist添加属性并不会导致视图刷新。**
那么当datalist为对象时，怎么刷新呢？

**有两个方法:**

1. vm.$forceUpdate();
2. vm.datalist = Object.assign({},vm.datalist,{'David':'戴维'});

--------------

#### 显示过滤/排序结果

有时，我们想要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据。在这种情况下，可以创建返回过滤或排序数组的计算属性。

例如：

`<li v-for="n in evenNumbers">{{ n }}</li>`

	data: {
	  numbers: [ 1, 2, 3, 4, 5 ]
	},
	computed: {
	  evenNumbers: function () {
	    return this.numbers.filter(function (number) {
	      return number % 2 === 0
	    })
	  }
	}

在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 你可以使用一个 method 方法：

`<li v-for="n in even(numbers)">{{ n }}</li>`

	data: {
	  numbers: [ 1, 2, 3, 4, 5 ]
	},
	methods: {
	  even: function (numbers) {
	    return numbers.filter(function (number) {
	      return number % 2 === 0
	    })
	  }
	}

-------------

#### v-for与v-if连用

当它们处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。当你想为仅有的一些项渲染节点时，这种优先级的机制会十分有用，如下：

	<li v-for="todo in todos" v-if="!todo.isComplete">
	  {{ todo }}
	</li>

上面的代码只传递了未 complete 的 todos。

而如果你的目的是有条件地跳过循环的执行，那么可以将 v-if 置于外层元素 (或 `<template>`)上。如：

	<ul v-if="todos.length">
	  <li v-for="todo in todos">
	    {{ todo }}
	  </li>
	</ul>
	<p v-else>No todos left!</p>
