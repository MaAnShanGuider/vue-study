### 项目中对vue-router的一些使用经验

  首先，我们要知道一个知识点:Vue.use(new Router())之后，每个组件就都有两个方法:

  * this.$router
  * this.$route

这两个是有区别的。

#### this.$router
  
  this.$router可以进行函数式导航，他的方法:
 * this.$router.push()
 * this.$router.go()
 * this.$router.replace ()

函数式导航:`this.$router.push({path:'/index',params:{id:'Alice'},query:{blog:'title'}})`

#### this.$route

  this.$route是用来获取当前地址的信息的。

  * this.$route.query:获取？后面的键值对

   例子：this.$route.query.blog   //--title

  * this.$route.params:获取动态路由的参数
   
   例子: this.$route.params.id   //--Alice

------------

#### 设置激活的router-link式样；

全局激活式样:

>在路由的inex.js文件中做如下操作:

```
	var vueRouterObj = new VueRouter({

	linkActiveClass: 'active', //将激活的路由添加一个mui-active类名称
	routes: [
	    { path: '/', redirect: '/Home' },
	    { path: '/Home', component: home, name: '主页' },
	    {
	    path: 'news'  // 列表
	    component: news,
	    children: [
	        {
	            path: '', // 列表目录
	            name: 'newList', 
	            component: newsList
	        },
	        {
	            path: 'newDetail', //列表详情
	            name: 'newDetail',
	            component: newDetail
	        }
	    ]
	}
	]
	});
```
局部激活式样:
>在组件的router-link标签添加active-class属性。

```
    <template>
	<div id="myfooter">
		<router-link tag='div'  :to="{name:'shop'}"  active-class="wocao"><i class='iconfont icon-shop'></i><span>商城</span></router-link>
		<router-link tag='div'  to='/hot'  active-class="wocao"><i class='iconfont icon-text'></i><span>头条</span></router-link>
		<router-link tag='div'  to='/club'  active-class="wocao"><i class='iconfont icon-friend'></i><span>社区</span></router-link>
		<router-link tag='div'  to='/cart'  active-class="wocao"><i class='iconfont icon-cart'></i><span>购物车</span></router-link>
		<router-link tag='div'  to='/mine'  active-class="wocao"><i class='iconfont icon-people'></i><span>我的</span></router-link>
	</div>
</template>
```
#### <router-link></router-link>

链接用的是`<router-link></router-link>`标签，

属性介绍:

  to:跳转到该地址

  可以直接写地址，也可以写成对象格式.

  直接写法:

`<router-link to='/index/game?id=33'>...</router-link>`
  
  对象写法；

`<router-link :to='{path:"index",}'></router-link>`

重点在于对象写法，只有对象写法才能自己传参。

**只有v-bind:to才能使用对象写法**

**只有v-bind:to才能使用对象写法**

**只有v-bind:to才能使用对象写法**

----------

所以我们来看看:to是怎么传参的。

#### 传参

我们现在路由组件里定义好路由规则:
```
	import shop from '@/components/shop'

	new Router({
		routes:[
			{
				path:'/index', //---定义路径
				name:'shop',   //---定义路径name
				component:shop //---路径渲染组件
			}
		]
		})
```

```
	//------例子1 
	//------- :to="{name:'index'}",这个'index'是已经在路由组件new Router({})已经定义好的，匹配到这个name，就会跳到这个name所在的path目标地址。
	//----会跳转到 '/index'
	<router-link :to="{name:'shop'}"></router-link>

	//------例子2
	//------- :to="{path:'/index'}",这个path属性，就是我们将要跳转的目标地址
	<router-link :to="{path:'/index'}"></router-link>

```

好了，如果name和path属性一起写呢???我们试试

```
	//-----例子3
	<router-link :to="{name:'shop',path:'/hot'}"></router-link>
```

最后可以发现，点击这个按钮会还是会跳到`/index`,这说明:**name的优先级比path高，会覆盖path。**

传参:

传参有个前提，就是`new Router()`中也要做相应的调整，所以我们修改一下前面的`new Router()`,修改为:

```
	new Router({
		routes:[
			{
				path:'index/:id',
				name:'shop',
				component:shop
			}
		]
		})
```

那么跳转链接修改为:
```
	//----例子4
	<router-link :to="{path:'/index',params:{id:'Alice'},query:{'blog':'title'}}"></router-link>
```

那么点击这个按钮会跳转到；
	
**/index/Alice?blog=title**

就是这样，通过params传参的前提，是要在`new Router()`里设置动态路由。

#### 读参

* 通过params传进来的 : `this.$route.params.id`,这个id就是前面设置的动态路由
* 通过query传进来的 : `this.$route.query.blog`

-------------

#### 路由的嵌套:

关于这点，我们只要知道点击父组件的路由，只是会将子路由匹配到的组件渲染进父组件的`<router-view></router-view>`。

#### "exact" 属 性

开启router-link的严格模式

<router-link :to="/" exact>home</router-link>

上面这个标签如果不加exact属性，则会在vue2.leenty.com/article页面下也会被匹配到，
这却不是我们的本意,在加了这个属性后就会正确的匹配到vue2.leenty.com下