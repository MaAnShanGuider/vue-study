### 哈哈，老子搞出来了。
**重点在于那个衔接点并不是随意的！！！而是基于原链接的！！！**

先不多说，来看看案例。
假设我们要请求这么一个网址：
	`http://api.aixifan.com/searches/channel?sort=5&pageNo=1&pageSize=20&parentChannelId=59`

看见那个网址后面的`/searches`没有???这个就是我说的重点！！！因为待会儿衔接点只能是它。

我们在主目录的`config`文件下，修改`index.js`文件。

修改过程：找到 `proxyTable:{}`;

改为:
```
	proxyTable: {
	         '/searches': {
	          target: 'http://api.aixifan.com',
	          host: 'api.aixifan.com',
	          changeOrigin:true,
	          // pathRewrite: {
	          //     '^/v4/api': '/v4/api'
	          //   }
	      }
	}
```

看，这个`/searches`就是关键。

然后我们在任意一个vue文件的script下，添加:
```
	var req = new Request('/searches/channel?sort=5&pageNo=1&pageSize=20&parentChannelId=59',
	    {
	     method:'get',
	     headers:{
	    "deviceType":"2"
	  }

	  });
	fetch(req).then(res=>res.json()).then((aa)=>{
	  // fetch(req).then(res=>res.json()).then(res=>console.log(res)
	  console.log(aa);
	});
	```

这里面的的扩展点是fetch，这里不细讲。待会儿自己查fetch的api。