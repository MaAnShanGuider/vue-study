### VUE的卡槽机制

slot标签是写在子组件的template里的。当html里自定义的标签里面没有任何其他的内容时(`<wocao></wocao>`)，就会渲染出slot的内容;若是有内容`<wocao><p>哈哈哈</p></wocao>`，就不会渲染slot。

代码示例:

```
	<div id='box'>
		<h1>我是父组件的标题</h1>
		<wocao>
			<p>这是一些初始内容</p>
			<p>这是更多的初始内容</p>
		</wocao>
	</div>
	var child = Vue.extend({
		template:`<div>
		<h2>我是子组件的标题</h2>
		<slot>
		只有在没有要分发的内容时才会显示。
		</slot>
		</div>`
	})


```

因为`<wocao></wocao>`里有两个p标签，所以模板里的slot不会被渲染出来。

另一个代码示例：

```
	<div id='box'>
		<h1>我是父组件的标题</h1>
		<wocao></wocao>
	</div>
	var child = Vue.extend({
		template:`<div>
		<h2>我是子组件的标题</h2>
		<slot>
		只有在没有要分发的内容时才会显示。
		</slot>
		</div>`
	})

```
这段代码中的slot会被渲染出来。

--------

上面是匿名卡槽，下面讲讲具名卡槽。

### 具名卡槽

> <slot> 元素可以用一个特殊的特性 name 来进一步配置如何分发内容。多个插槽可以有不同的名字。具名插槽将匹配内容片段中有对应 slot 特性的元素。

> 仍然可以有一个匿名插槽，它是默认插槽，作为找不到匹配的内容片段的备用插槽。如果没有默认插槽，这些找不到匹配的内容片段将被抛弃。

**具名卡槽的作用在于排列html结构内自定义标签的子元素的顺序。**

上面是官网的说明。我们自己做做代码示例。

```
		<div id='box'>
			<wocao>
			  	<p slot="left">我是左边</p>
			  	<p slot="center">我是中间</p>
			  	<p slot="right">我是右边</p>
			</wocao>
		</div>

		//------第一种排列
		var child = Vue.extend({
			template:`
				<div>
					<slot name='left'></slot>
					<slot name='center'></slot>
					<slot name='right'></slot>
				</div>
			`
		})


		//-----第二种排列
		var child = Vue.extend({
			template:`
				<div>
					<slot name='center'></slot>
					<slot name='left'></slot>
					<slot name='right'></slot>
				</div>
			`
		})
		
		//-----第三种排列
		var child = Vue.extend({
			template:`
				<div>
					<slot name='right'></slot>
					<slot name='center'></slot>
					<slot name='left'></slot>
				</div>
			`
		})
```

**比较这三次的渲染结果，可以发现，顺序是按照模板内的slot的顺序来排列的。**

而且，我们来看另一个例子：

```
	<div class="container">
	  <header>
	    <slot name="header"></slot>
	  </header>
	  <main>
	    <slot></slot>
	  </main>
	  <footer>
	    <slot name="footer"></slot>
	  </footer>
	</div>

	//-模板
	<app-layout>
	  <h1 slot="header">这里可能是一个页面标题</h1>
	  <p 我是两个p之一>主要内容的一个段落。</p>
	  <p 我是两个p之一>另一个主要段落。</p>
	  <p slot="footer">这里有一些联系信息</p>
	</app-layout>
```

最后会被渲染成下面这样：

```
	<div class="container">
	  <header>
	    <h1>这里可能是一个页面标题</h1>
	  </header>
	  <main>
	    <p>主要内容的一个段落。</p>
	    <p>另一个主要段落。</p>
	  </main>
	  <footer>
	    <p>这里有一些联系信息</p>
	  </footer>
	</div>
```

通过渲染出的结果可以得到以下结论：
 * 写在html结构里的，只是类似于资源一样的，用来被模板里的slot根据slot属性读取(调用)。而模板里的才是真正的结构。
 * 具名slot按照模板里的顺序渲染，html结构里没有声明slot属性的标签(比如两个p)，会被同一归到匿名slot里，然后顺序也会按照匿名slot来决定。
---------

### 作用域卡槽

作用域卡槽是可以用来传递数据的。

代码示例:

```
	<div id='box'>
		<wocao>
			<template slot-scope='diuni'>					
						<p>{{ diuni.hahaP }}</p>
			</template>
		
		</wocao>
	</div>

	var child = Vue.extend({
		template:`
			<div>
				<slot hahaP='我是子组件的信息'></slot>
			</div>
		`,
		props:['hahaP','xixiP']
	})
```

不同于普通的props属性赋值，作用域卡槽的props赋值是写在模板内而不是html结构。

分析下`<temptlate slot-scope='diuni'></temptlate>`
> `<template></template>`在父级中，具有特殊特性 slot-scope 的 <template> 元素必须存在，表示它是作用域插槽的模板。slot-scope 的值(就是diuni)将被用作一个临时变量名，此变量接收从子组件传递过来的 prop 对象，也就是等于子组件的props。

