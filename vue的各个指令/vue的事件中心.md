### 事件中心

在一些小的项目中，用不到vuex来完成组件之间的通信，可以使用bus。

下面是一个最简单的例子:
	在第二个输入框输入，第一个输入框也会跟着改变。
	但是第一个输入框输入，第二个是不会变化的。
	因为bus.$emit只是写在了第二个输入框的组件中。

```
		<!DOCTYPE html>
		<html lang="en">
		<head>
		  <meta charset="UTF-8">
		  <title>Document</title>
		</head>
		<script src='../lib/vue.js'></script>
		<body>
		  <div id="box">
		     <my-input></my-input>
		     <my-result></my-result>
		  </div>
		</body>
		<script>
		  //-----------中心处理
		var bus = new Vue();
		Vue.component('my-input',{
		  template:`<input type="text" v-model='msg' />`,
		  data(){
		    return {
		      msg:''
		    }
		  },
		  //--------在创建之前给bus绑定事件！！！
		  //所有的事件绑定都应该在created
		  created(){
		    bus.$on('haha',(res)=>{
		      // console.log('')
		       this.msg = res;
		    })
		  },

		})

		Vue.component('my-result',{
		  template:`<input type="text" @input='change($event.target.value)' v-model='msg' />`,
		  data(){
		    return {
		      msg:''
		    }
		  },
		  methods:{
		    change(data){
		      bus.$emit('haha',data);
		      console.log('触发事件');
		    }
		  }
		})

		var vm = new Vue({
		  el:'#box'
		})
		</script>
		</html>
```


### 重点一：我们要区别方法与事件
事件：
```	
		//------绑定事件
		bus.$on(eventName,callBack)

		//------触发事件
		bus.$emit(eventName,arguments)

		//有个要注意的点就是，触发谁的事件，那么触发时就写谁。不能用其他。
```
* eventName:事件名。
* callBack:触发事件后，执行的回调函数。
* arguments:传给回调函数的参数。

方法:
```
		var vm = new Vue({

			//----方法的声明
			methods:{
				change(aa){
					doSomething;
				}
			}
			})

			//----调用方法：调用方法有很多种形式
			在vm内部：this.change(aa);
			在vm外部：vm.change(aa);
			在vm的模板中：直接写change(aa);
```

好，知道了上面的区别以后，再写中心事件后，是有个基本套路。就是再bus内部的created时绑定事件，事件的回调函数用bus的方法。

流程： 组件触发事件`this.$emit(eventName,arg)`----->`bus.$on(eventName,callBack)`------>callback就是bus本身的方法。
看下面的例子：


```
	<!DOCTYPE html>
	<html lang="en">
	<head>
	  <meta charset="UTF-8">
	  <title>Document</title>
	</head>
	<script src='../lib/vue.js'></script>
	<body>
	  <div id="box">
	     <my-input></my-input>
	     <my-result></my-result>
	  </div>
	</body>
	<script>
	  //-----------中心处理
	var bus = new Vue({
	  data(){
	    return {
	      myArr:['张学友','刘德华','梁朝伟']
	    }
	  },
	  created(){
	    this.$on('addMsg',this.addMsg);
	    this.$on('deleteMsg',this.deleteMsg);
	  },
	  //-----给中心定义事件
	  methods:{
	    addMsg(res){
	      this.myArr.push(res);
	      console.log('添加');
	    },
	    deleteMsg(res){
	      this.myArr.splice(res,1);
	      console.log('删除');
	    }

	  }
	})

	var myInput = Vue.extend({
	  template:'<div><input type="text" v-model="inputMsg"/><button @click="haha">添加列表</button></div>',
	  data(){
	    return {
	      inputMsg:''
	    }
	  },
	  methods:{
	    haha(){
	      bus.$emit('addMsg',this.inputMsg);
	    }
	  }

	})
	var myResult = Vue.extend({
	  template:`<ul><li v-for='(value,index) in aa'>{{value}} <button @click='deleteMsg(index)'>删除</button></li></ul>`,
	  data(){
	    return {
	      aa:bus.myArr
	    }
	  },
	  computed:{
	    change(){
	      console.log('bus.myArr发生变化');
	      return bus.myArr;
	    }
	  },
	  methods:{
	    deleteMsg(index){
	      bus.$emit('deleteMsg',index);
	    }
	  }
	})
	var vm = new Vue({
	  el:'#box',
	  components:{
	    'my-input':myInput,
	    'my-result':myResult
	  }
	})
	</script>
	</html>

```

其实可以观察出，上面已经有了vuex的影子了。把bus换成store会觉得更像，哈哈。

bus就像是一个中转站，我们可以把数据，操作放在这上面，然后别的组件通过触发bus的事件，来传信息给bus，然后直接bus.$emit(事件名，argu)