### webpack打包时，css文件和图片会报错。解决的办法是：

1. 在`./build/utils.js`文件中找到:

```
	   if (options.extract) {
      return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader',
        
      })
    } else {
      return ['vue-style-loader'].concat(loaders)
    }

```

添加`publickPath:'../../'`进去,

```
	if (options.extract) {
	  return ExtractTextPlugin.extract({
	    use: loaders,
	    fallback: 'vue-style-loader',
	    publicPath:'../../'
	  })
	} else {
	  return ['vue-style-loader'].concat(loaders)
	}

```

2. 在`./config/index.js`文件中，找到:

```
	build: {
	  index: path.resolve(__dirname, '../dist/index.html'),
	  assetsRoot: path.resolve(__dirname, '../dist'),
	  assetsSubDirectory: 'static',
	  assetsPublicPath: '/',
	  productionSourceMap: true,
	  productionGzip: false,
	  productionGzipExtensions: ['js', 'css'],
	  bundleAnalyzerReport: process.env.npm_config_report
	}
```

将`assetsPublicPath`的值改为`./`,即:`assetsPublicPath: './'`。

### webpack打包时，iconfont的css失效的问题的解决办法。

我们知道iconfont是i标签，我们要是自己给这个i设置font,在开发环境并没有什么问题。但是打包后就会i的font属性并不会传到before，所以我们要自己给每个i标签的before伪元素单独设置font属性。