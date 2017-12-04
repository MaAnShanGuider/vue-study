### 在vue项目中配置sass

1. 安装模块
```
	npm install --save-dev node-sass
	npm install --save-dev sass-loader
```

2.在`webpack.base.config.js`里加上:

```
	{
	      test: /\.scss$/,
	      loaders: ["style", "css", "sass"]
	    }
```





### 项目中引入iconfont

在阿里的iconfont下载好图标包，然后引入他们的css就可以了。

引入方式一:

  在vue组件的script中:

```
	import './iconfont.css'               //iconfont.css文件的地址
```

引入方式二；

  在vue组件的style标签中引入

```
	<style src='./iconfont.css'></style>   //iconfont.css文件的地址
```