### vue-awesome-swiper插件

第一步，安装:

```
	npm install vue-awesome-swiper --save

	//还得安装依赖
	npm install css-loader
	npm install less-loader
	这两个。
```

第二步，在main.js引入

```
import Vue from 'vue'
import VueAwesomeSwiper from 'vue-awesome-swiper'
Vue.use(VueAwesomeSwiper)
```

然后就可以在组件中通过:

`import { swiper, swiperSlide } from 'vue-awesome-swiper'`

一个标准的小案例:

```
		
		<template>
		  <div class="index">

		    <swiper :options="swiperOption" class="swiper-box" ref="mySwiper">
		      <swiper-slide class="swiper-item">
		        <!-- <img src="" alt="" class="w100"> -->
		        11111111
		      </swiper-slide>
		      <swiper-slide class="swiper-item">
		        222222222
		        <!-- <img src="" alt="" class="w100"> -->
		      </swiper-slide>
		    </swiper>

		  </div>
		</template>

		<script>
		  import Vue from 'vue'
		  import { swiper, swiperSlide } from 'vue-awesome-swiper'
		  require('swiper/dist/css/swiper.css')    //注意这里

		  export default {
		    name: 'index',
		    components: {
		      swiper,
		      swiperSlide
		    },
		    data() {
		      return {
		        swiperOption: {
		          notNextTick: true,
		          loop:true,
		          initialSlide:0,
		          autoplay: 2000,
		          direction : 'horizontal',
		          grabCursor : true,
		          onSlideChangeEnd: swiper => {}
		        }
		      }
		    },
		    computed: {
		      swiper() {
		        return this.$refs.mySwiper.swiper
		      }
		    },
		    mounted() {
		      // this.swiper.slideTo(1000, 10000, false);
		    }
		  }
		</script>
```

这里面有几个注意的点:
1. `<swiper></swiper>`标签里有个`:options=swiperOption`属性，通过读取组件的swiperOption属性来设置轮播的各种式样，这是必须的。
2. `require('swiper/dist/css/swiper.css')`,这一步！是重中之重！！！必须要写！！！
3. 第三点简单了，就是将通过vue-awesome-swiper引入的两个属性绑定组件的标签。