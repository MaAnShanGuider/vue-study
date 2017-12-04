### 列表过渡
列表过渡:当组件的模板中的通过v-for创建的html结构，就可以应用列表过渡。
也就是说：*必须是通过v-for创建。*

代码示例：
```
	<!DOCTYPE html>
	<html lang="en">
	<head>
	  <meta charset="UTF-8">
	  <title>Document</title>
	</head>
	<style>
	  .a{
	    overflow: hidden;
	  }
	  .box-child{
	    float: left;
	    width:100px;
	    height:100px;
	    font:700 50px/100px '';
	    text-align: center;
	    margin: 20px;
	  }
	  .xixi-enter,.xixi-leave-to{
	    transform:translateX(-20px);
	    opacity: 0;
	  }
	  .xixi-enter-active,.xixi-leave-active{
	    transition: all 2s;
	  }
	</style>
	<script src="../lib/vue.js"></script>
	<body>
	  <div id="box">
	    <input type="button" class="add" value="添加" @click='add()'>
	    <input type="button" class="delete" value='删除' @click='deletehaha()'>
	    <transition-group name='xixi' tag='div' class='a'>
	        <div v-for='value in item' :style='{background:change()}' :key='value' class='box-child'>{{value}}</div>
	    </transition-group>
	  </div>
	</body>
	<script>
	  var vm = new Vue({
	    el:'#box',
	    data:{
	      item:'苟利国家生死以岂因祸福避趋之',
	      item2:'苟利国家生死以岂因祸福避趋之'
	    },
	    methods:{
	      change(){
	        var a=Math.floor(Math.random()*256);
	        var b=Math.floor(Math.random()*256);
	        var c=Math.floor(Math.random()*256);
	        return `rgba(${a},${b},${c},1)`;
	      },
	      add(){
	        this.item += this.item2[Math.floor(Math.random()*this.item2.length)];
	      },
	      deletehaha(){
	        //----当前数组的长度
	        var num = Math.floor(Math.random()*this.item.length);

	        var str1 = this.item.slice(0,num);

	        var str2 = this.item.slice(num+1);
	        console.log(str1,str2);
	        this.item = str1 + str2;
	      }
	    }
	  })
	</script>
	</html>
```

大体上和普通的组件过渡没什么区别，只是在v-for的组件外面包裹了一层`<transition-group></transition-group>`标签，

而且可以的得出结论，就是css里写的是针对单独的列表元素，不是整体列表。
