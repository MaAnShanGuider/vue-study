### VUE的组件系统

**全局组件的注册**

```
	Vue.component('my-component',{
		template:'<div>我是组件</div>'
		})
```

然后在html里写：

```		<div id='box'>
			<my-component></my-component>
		</div>
```

最后会渲染为：
```
		<div id='box'>
			<div>我是组件</div>
		</div>
```

-----------

**局部注册**

```
		<div id="box">
			<haha></haha>
			<xixi></xixi>
		</div>
		var child1 = {
			template:'<div>我是刘德华</div>'
		};
		var child2 = {
			template:'<div>我才是刘德华</div>'
		};
		var vm = new Vue({
			el:'#box',
			data:{
				msg:'张学友'
			},
			components:{
				//xixi,haha只能在父组件中使用
				'haha':child1,
				'xixi':child2
			}
		})
```
结论：vue对象有个components属性，属性值为对象。这个对象的属性名就是html结构里的标签名，属性值就是实际的标签。


### 全局注册的注意点：

```
	<table id="box">
		<wocao></wocao>
	</table>
	Vue.component('wocao',{
		template:'<tr><td>11111</td></tr><tr><td>11111</td></tr>'
	})
```
自定义组件 `<wocao> `会被当作无效的内容，因此会导致错误的渲染结果。
有些特殊的元素是不能用这样的写法去注册全局组件。，像 `<ul>、<ol>、<table>、<select> `这样的元素里允许包含的元素有限制，而另一些像 `<option>` 这样的元素只能出现在某些特定元素的内部。

解决的办法是：给tr加is属性,而且table外面必须包裹一层标签，这个标签的id值用作构建vue对象。

```	
	<!DOCTYPE html>
	<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <title>Document</title>
	</head>
	<script src="../lib/vue.js"></script>
	<body>
	    <div id="box">
	        <table>
	            <tr is="wocao"></tr>
	        </table>
	    </div>
	</body>
	<script>
	    Vue.component('wocao',{template:'<tr><td>我操你妈</td></tr>'});
	    var vm = new Vue({
	        el:'#box'
	    });
	</script>
	</html>
```

--------

### Vue.extend与Vue.component的差异

**Vue.extend**
extend是一个组件构造器(还没注册),你给它参数,他给你一个组件,然后这个组件挂载在Vue.component上，或者局部组件里用作components的属性。

用法一：
```
	
	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<script src="../lib/vue.js"></script>
	<body>

		<div id="box">
			<wocao></wocao>
		</div>
	</body>
	<script>
		var Geshen = Vue.extend({
			template:'<div>aaaaaaa</div>'
		});	
		new Geshen().$mount('wocao');	//$mount()里面的参数就是html结构里的wocao标签
	</script>
	</html>

```
**可以不声明vue挂载环境。**
用法二:
```

	<!DOCTYPE html>
	<html>
	    <body>
	        <div id="app">
	            3. #app是Vue实例挂载的元素，应该在挂载元素范围内使用组件
	            <my-component></my-component>
	        </div>
	    </body>
	    <script src="../lib/vue.js"></script>
	    <script>
	    
	        // 1.创建一个组件构造器
	        var myComponent = Vue.extend({
	            template: '<div>This is my first component!</div>'
	        })
	        
	        // 2.注册组件，并指定组件的标签，组件的HTML标签为<my-component>
	        Vue.component('my-component', myComponent)
	        
	        new Vue({
	            el: '#app'
	        });
	        
	    </script>
	</html>
```
**必须声明vue挂载环境，而且必须在环境里面。**

用法三:

```
	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<script src="../lib/vue.js"></script>
	<body>
		<div id="box">
			<wocao></wocao>
		</div>
	</body>
	<script>
		var Geshen = Vue.extend({
			template:'<div>aaaaaaa</div>'
		});	
		var vm = new Vue({
			el:'#box',
			components:{
				wocao:Geshen
			}
		})
	</script>
	</html>
```