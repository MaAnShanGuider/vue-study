### vue的表单绑定

你可以用 v-model 指令在表单控件元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。

也就是说:*表单操作时双向的，会同时影响vue对象的data。*

代码示例:

		<div id="box">
			<input type="text" v-model='msg'>
		</div>
		var vm = new Vue({
			el:'#box',
			data:{
				msg:'我是张学友'
				}
			})

浏览器会显示输入框里有我是张学友，这个是我我们随便在输入框里改写成什么东西，然后再在控制台输入:vm.msg

**那么，控制台会输出我们刚才输入的内容**

-------

#### 多行文本textarea

代码示例(注:*这是错误的代码示例*):

		<div id="box">
			<textarea name="" id="" cols="30" rows="10">{{msg}}</textarea>
		</div>
		var vm = new Vue({
			el:'#box',
			data:{
				msg:'我是张学友'
				}
			})

我们如果直接写在标签外面，那么我们改变内容，再打印vm.msg,那么仍旧会输出:'我是张学友'。

正确的应该是下面的：

		<div id="box">
			<textarea name="" id="" cols="30" rows="10" v-model='msg'></textarea>
		</div>
		var vm = new Vue({
			el:'#box',
			data:{
				msg:'我是张学友'
				}
			})

这样写的话，改变内容，再打印vm.msg就会输出我们的输入的内容。

---------------

#### 复选框

1. **单个复选框**

单个复选框里也得分情况,
 * input元素设置了value：

```	
 	<div id="box">		
		<input type="checkbox" value="我操你妈" v-model='msg'>	
		<p>{{msg}}</p>	
	</div>
	var vm = new Vue({
		el:'#box',
		data:{
			msg:''
		}
	})

```
那么，点击按钮，按钮被选中时：p会显示'我操你妈';按钮没被选中时：p不显示。
 
 * input元素未设置value
```
	<div id="box">
		<input type="checkbox" v-model='msg'>
		<p>{{msg}}</p>
	</div>
	var vm = new Vue({
		el:'#box',
		data:{
			msg:''
		}
		})

```
按钮被选中时：p会显示'true';按钮没被选中时：p显示'false'。

2. **多个复选框**
多个复选框绑定同一个数组,而且必须为数组，是字符串或者对象都会报错。

多个单选框时，单选框的input设置v-model='msg',而且value必须设置，而这个msg是一个数组，。
```
		<div id="box">
			<label for="s1"><input type="checkbox" value='张学友' v-model='msg' name="s1">张学友</label>
			<label for="s2"><input type="checkbox" value='刘德华' v-model='msg' name="s2">刘德华</label>
			<label for="32"><input type="checkbox" value='周星驰' v-model='msg' name="32">周星驰</label>
			<p>{{msg}}</p>
		</div>
			var vm = new Vue({
				el:'#box',
				data:{
					msg:[]
				}
			})
```
点击按钮，p里的[],会显示出已经被点击的input元素的value。

----------

#### 单选按钮

```
		<div id="box">
			<label for="s1"><input type="radio" value='张学友' v-model='msg' name="s1">张学友</label>
			<label for="s2"><input type="radio" value='刘德华' v-model='msg' name="s2">刘德华</label>
			<label for="32"><input type="radio" value='周星驰' v-model='msg' name="32">周星驰</label>
			<p>{{msg}}</p>
		</div>
		var vm = new Vue({
			el:'#box',
			data:{
				msg:{}
			}
		})

```
点击哪个按钮，p就会显示谁。

不管msg原本是字符串，还是数组，还是对象，在被点击时，都会转为为字符串，值为input的value。

#### 选择列表

```
	
		<div id="box">
			<select name="ok"  v-model='msg'>
				<option disabled value="">请选择</option>
				<option value="张学友">张学友</option>
				<option value="刘德华">刘德华</option>
				<option value="周星驰">周星驰</option>
			</select>
			<p>{{msg}}</p>
		</div>
		var vm = new Vue({
			el:'#box',
			data:{
				msg:[]
			}
		})
```



