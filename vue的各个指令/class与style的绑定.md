### class与style的绑定

#### 先看class

代码:

	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
		<style>
			.yellow{background: yellow;}
			.red{background: red;}
		</style>
		<script src="../lib/vue.js"></script>
	</head>
	<body>
		<div id="box">
			<p :class='isColor?"red":"yellow"'>{{his}}</p>
		</div>
	</body>
	<script>
		var vm = new Vue({
			el:'#box',
			data:{
				isColor:false,
				his:'天下合久必分，久分必合'
			}
		})
	</script>
	</html>

浏览器会显示p标签的背景为黄色。
通过前面，我们知道，部分指令里是可以直接运行js代码的。

上面的
		
		<p :class='isColor?"red":"yellow"'>{{his}}</p>

里面的isColor,就是vue对象的isColor属性，所以整个三目运算符就是根据isColor的布尔值进行运算。

来看另一种代码示例:

#### class绑定的对象语法

	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
		<style>
			.yellow{background: yellow;}
			.red{background: red;}
			.big{font:700 30px/50px '';}
		</style>
		<script src="../lib/vue.js"></script>
	</head>
	<body>
		<div id="box">
			<p v-bind:class='{yellow:isColor,big:true}'>{{his}}</p>
		</div>
	</body>
	<script>
		var vm = new Vue({
			el:'#box',
			data:{
				isColor:false,
				his:'天下合久必分，久分必合'
			}
		})
	</script>
	</html>

将指令里面的代码写成对象的形式，那么对象的属性值就是布尔值，属性名就是我们需要的class的类名。
但是这样写，也是很繁琐的。能不能进一步简化呢？当然可以，我们已经知道指令后面的值可以是对象，那么我们就把vue对象的属性作为这个对象。

	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
		<style>
			.yellow{background: yellow;}
			.red{background: red;}
			.big{font:700 30px/50px '';}
		</style>
		<script src="../lib/vue.js"></script>
	</head>
	<body>
		<div id="box">
			<p v-bind:class='ok'>{{his}}</p>
		</div>
	</body>
	<script>
		var vm = new Vue({
			el:'#box',
			data:{
				ok:{
					yellow:false,
					red:true,
					big:true
				}
				,
				his:'天下合久必分，久分必合'
			}
		})
	</script>
	</html>

可以观察出，ok作为vue对象的属性传入指令。而且这种形式绑定class是最常见的。

-------------

而且`v-bind:class`可以与普通class属性共存

	//----比如
	<p class='aa' v-bind:class='{bb:isBB,cc:isCC}'></p>
	data:{
		isBB:true,
		cc:false
	}
将会渲染为：`<p class='aa bb'></p>`


### class绑定的数组语法

我们可以将一个数组传入dom元素。

	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
		<style>
			.yellow{background: yellow;}
			.red{background: red;}
			.big{font:700 30px/50px '';}
		</style>
		<script src="../lib/vue.js"></script>
	</head>
	<body>
		<div id="box">
			<p v-bind:class='[ok.haha,"big"]'>{{his}}</p>
		</div>
	</body>
	<script>
		var vm = new Vue({
			el:'#box',
			data:{
				ok:{
					yellow:false,
					red:true,
					haha:'red',
					big:true
				}
				,
				his:'天下合久必分，久分必合'
			}
		})
	</script>
	</html>


我们可以的出结论：那就是标签内的数组的成员的值是vue对象的属性，成员的值必须是字符串。
而且，数组成员也可以直接写成字符串形式，也会被渲染出来。

----------

### 内联式样的绑定

具体的用法和class的绑定是差不多的。

区别只是在于，style的绑定是属性名和属性值都要出现。而符合这种格式的只有对象。

eg:`<p v-bind:style='{fontsize:"12px}",'></p>`

**对象写法**

		<div id="box">
			<p v-bind:style='styleBind'>{{his}}</p>
		</div>
		var vm = new Vue({
			el:'#box',
			data:{
				styleBind:{
					fontSize:'22px',
					backgroundColor:'red',
					textShadow:'0 0  5px black'
				}
				,
				his:'天下合久必分，久分必合'
			}
		})

**数组写法**

		<div id="box">
			<p v-bind:style='[styleBind1,styleBind2]'>{{his}}</p>
		</div>
		var vm = new Vue({
			el:'#box',
			data:{
				styleBind1:{
					fontSize:'22px'				
				},
				styleBind2:{
					backgroundColor:'red',
					textShadow:'0 0  5px black'
				}
				,
				his:'天下合久必分，久分必合'
			}
		})