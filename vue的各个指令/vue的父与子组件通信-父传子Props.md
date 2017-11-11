### 父与子组件之间的通信

在 Vue 中，父子组件的关系可以总结为 prop 向下传递，事件向上传递。父组件通过 prop 给子组件下发数据，子组件通过事件给父组件发送消息。看看它们是怎么工作的。


```	
	  -----→Pass Props-----→
	  ↑					   ↓
	Parent         		Children
	  ↑					   ↓
	  ←-----Emit Events-----

```

### Props(父传子)

先看一段代码:
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
				<wocao msg='我操你妈' msg2='dddd'></wocao>
			</div>
		</body>
		<script>
			var Geshen = Vue.extend({
				template:'<div v-bind:class="msg2">{{msg}}</div>',
				props:['msg','msg2']
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

最后会渲染成:

```
	<div id="box">
		<div class="dddd">我操你妈</div>
	</div>
```

也就是msg,msg2都被渲染成了写在标签内部的msg,msg2的值。
而props里是数组，成员为设置自定义的属性值。

#### 动态Props

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
			<input type="text" v-model='msg'><br />
			<wocao v-bind:msg='msg' msg2='dddd'></wocao>
			<!-- 可以得出结论：
			v-bind:msg 这个msg是Geshen的组件里的msg，
			而等于号后面的msg则由于v-bind的原因，是父组件的data的msg属性。
			也就是说：父子组件的data不共享！！！
		
			但是父组件可以通过v-bind的方式将自己的data绑定到子组件上。


			 -->
		</div>
	</body>
	<script>
		var Geshen = Vue.extend({
			template:'<div :class="msg2">{{msg}}</div>',
			props:['msg','msg2']
		});
		var vm = new Vue({
			el:'#box',
			data:{
				msg:'哈哈'
			},
			components:{
				wocao:Geshen
			}
		})
	</script>
	</html>
```

可以看见，子div的文本是跟随着输入框而变化的。

**流程：输入框变化-->父组件的msg变化-->html结构里`<wocao>`等于号后面的msg变化-->Geshen的template中{{msg}}的值变化(注意，不是msg本身变化，而是他的值。他的值就是上一步的msg)-->Geshen的data的msg变化-->渲染的结果变化**

这一整串流程是单向的,也就是说，父组件data的变化可以改变子组件的props值，但是子组件的值的变化并不会影响父组件的data。*看下面代码*

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
				<label for='first'>第一个</label><input type="text" v-model='msg' name='first'><br />
			<label for='second'>第二个</label><wocao v-bind:msg='msg' msg2='dddd' name='second'></wocao>
			</div>
		</body>
		<script>
			var Geshen = Vue.extend({
				template:'<input type="text" :value="msg" />',
				props:['msg','msg2']
			});	

			var vm = new Vue({
				el:'#box',
				data:{
					msg:'哈哈'
				},
				components:{
					wocao:Geshen
				}
			})
		</script>
		</html>
```

可以观察出，当改变第一个输入框的内容时，第二个输入框的内容也会变化。*但是，改变第二个输入框的内容并不会导致第一个输入框的内容的变化。*这就说明，只能父组件的data改变子组件的data，而不能反过来。

**原则上，我们尽量要求通过父组件的data来改变子组件的prop，而不是直接改动子组件的prop**

方法有两种。

**方法一**
```
		template:'<div>{{counter}}</div>'
		props: ['initialCounter'],
		data: function () {
		  return { counter: this.initialCounter }
		}
```

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
			<wocao v-bind:msg='fatherMsg' msg2='xixihaha'></wocao>
		</div>
	</body>
	<script>
		var Geshen = Vue.extend({
			template:'<div :class="msg2">{{childMsg}}</div>',
			props:['msg','msg2'],
			data(){
				return 	{
						
					childMsg:this.msg				
				}
			}
		});	
		var vm = new Vue({
			el:'#box',
			data:{
				fatherMsg:'我操你妈'
			},
			components:{
				wocao:Geshen
			}
		})
	</script>
	</html>




```

**方法二**

```
		template:'<div>{{normalizedSize}}</div>'
		props: ['size'],
		computed: {
		  normalizedSize: function () {
		    return this.size.trim().toLowerCase()
		  }
		}
```

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
			<wocao v-bind:msg='fatherMsg' msg2='xixihaha'></wocao>
		</div>
	</body>
	<script>
		var Geshen = Vue.extend({
			template:'<div :class="msg2">{{change}}</div>',
			props:['msg','msg2'],
			computed:{
				change(){
					return this.msg
				}
			}		
		});	
		var vm = new Vue({
			el:'#box',
			data:{
				fatherMsg:'我操你妈'
			},
			components:{
				wocao:Geshen
			}
		})
	</script>
	</html>

```

-------

### Props验证

这东西对我用处不大，就不写了。

-------

### Props的合并

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
				<!-----这个class由父组件决定 -->
				<wocao :class='haha'></wocao>
			</div>
		</body>
		<script>
			var Geshen = Vue.extend({
				//----这个class是模板本身已经设定好的。
				template:'<div class="xixi"></div>',
				props:['msg']
			});	
			var vm = new Vue({
				el:'#box',
				data:{
					haha:'haha'
				},
				components:{
					wocao:Geshen
				}
			})
		</script>
		</html>

```

看上面，子组件与父组件都对子组件的class有设置，但是并不会覆盖，而是合并在一起。会渲染出：

	<div class='xixi haha'></div>

另外一种方式：
		
	<div id="box">
		<wocao :class='haha' xixi='wocaoni'></wocao>
	</div>
	var Geshen = Vue.extend({
		template:'<div :class="xixi"></div>',
		props:['xixi']
	});	
	var vm = new Vue({
		el:'#box',
		data:{
			haha:'haha'
		},
		components:{
			wocao:Geshen
		}
	})

一个class由子组件的props决定，另一个class由父组件决定。结果也是会合并在一起。

	<div class='haha wocaoni'></div>