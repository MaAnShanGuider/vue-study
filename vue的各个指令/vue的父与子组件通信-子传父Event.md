### 子组件传数据给父组件

每个 Vue 实例都实现了事件接口，即：

  * 使用 $on(eventName) 监听事件
  * 使用 $emit(eventName) 触发事件

另外，父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件。

代码示例：

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
			<div>{{fatherCount}}</div>
			<wocao @fatherclick='fatherMethod()'></wocao>
			<wocao @fatherclick='fatherMethod()'></wocao>
		</div>
	</body>
	<script>
		var btn = Vue.extend({
			template:'<button @click="childMethod()">{{childCount}}</button>',
			data(){
				return {
					childCount:0
				}
			},
			methods:{
				childMethod(){
					this.$emit('fatherclick')
					this.childCount++;
				}
			}
		});

		var vm = new Vue({
			el:'#box',
			data:{
				fatherCount:0
			},
			components:{
				wocao:btn
			},
			methods:{
				fatherMethod(){
					console.log('触发了父组件的点击事件.')
					this.fatherCount++;
				}
			}
		})
	</script>
	</html>

```

上面这段代码最重要的两个点：

 * 第一个点：

	<wocao @fatherclick='fatherMethod()'></wocao>
	<wocao @fatherclick='fatherMethod()'></wocao>

 * 第二个点：

	var btn = Vue.extend({
		template:'<button @click="childMethod()">{{childCount}}</button>',
		data(){
			return {
				childCount:0
			}
		},
		methods:{
			childMethod(){
				this.$emit('fatherclick')
				this.childCount++;
			}
		}
	});

在第一个点里，我们可以看见`wocao`标签绑定了`fatherclick`事件，这个`fatherclick`事件是在父组件里注册(写在html的结构里面)的，而不是由组件本身定义的(写在定义模板的template的里面)。

然后在第二个点里，我们看见template里写了`@click`点击事件，点击会触发组件的`childMethod()`方法，然后这个方法执行，里面的`this.$emit('fatherclick')`由触发，又会调用父组件的`fatherMethod()`方法，接着父组件执行这个方法。

我们来理一理思路:

渲染后的子组件被点击，触发了子组件定义的`click`事件-->执行`childMethod()`-->执行触发了`this.$emit('fatherclick')`,触发了fatherclick事件-->执行`fatherMethod()`


这个顺序不能乱，结构也不能乱。重点在于：
	* 点击事件是写在模板里的。
	* 自定义事件是写在html结构里的。
	* 点击事件触发子组件的方法，导致触发自定义事件，然后又触发了父组件的方法。

------------

**实际上，写在html结构里的事件调用的函数，最好只写函数名，后面不要再加括号。**

----------

**子组件传信息给父组件:**

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
				<wocao @fatherevent='fatherMethod'></wocao>
			</div>
		</body>
		<script>
			var child = Vue.extend({
				template:'<div @click="childMethod()">我是子组件</div>',
				methods:{
					childMethod(){
						console.log('子组件方法触发。');
						//---传数据给父组件
						this.$emit('fatherevent','我是来自子组件的信息');
					}
				}
			})

			var vm = new Vue({
				el:'#box',
				methods:{
					fatherMethod(msg){
						console.log('我是父组件，这是我接收的信息：'+msg);
					}
				}
				,
				components:{
					wocao:child
				}
			})
		</script>
		</html>
```

**在事件中：$event可以代替原生的event对象。**

代码示例:

将上面的child改写就成了。

```
		var child = Vue.extend({
			template:'<input type="text" :value="childMsg" @input="childMethod($event.target.value)"/>',
			data(){
				return {
					childMsg:'我是子元素的输入'
				}
			},
			methods:{
				childMethod(a){
					console.log('子组件方法触发。');
					//---传数据给父组件
					this.$emit('fatherevent',a);
				}
			}
		})

```

### 子组件是无法更改自己的data和props。

代码示例:

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
				<wocao @fatherevent='fatherMethod' :childmsg='fatherMsg'></wocao>
			</div>
		</body>
		<script>
			var child = Vue.extend({
				template:'<input type="text" :value="childmsg" @input="childMethod($event.target.value)"/>',
					props:['childmsg']
				,
				methods:{
					childMethod(a){
						
						console.log(this.childmsg);//a段代码
						//---传数据给父组件
						this.$emit('fatherevent',a);
					}
				}
			})

			var vm = new Vue({
				el:'#box',
				data:{
					fatherMsg:'我是你爸爸'
				},
				methods:{
					fatherMethod(msg){
						console.log('我是父组件，这是我接收的信息：'+msg);
					}
				}
				,
				components:{
					wocao:child
				}
			})
		</script>
		</html>

```

对于a段代码，控制台只会输入:我是你爸爸