title: Open Weather App using React ( five )
date: 2016-03-10 10:12:29
tags:
---
### Step5 配置生产环境服务器

** 最终Demo位置，** [看这里~](http://115.159.95.239:8080/)

其实在step4的时候我们的app已经完成了，但是webpack-dev-server并不是一个production server，要把我们的app部署到真正的服务器上的话，我们需要一个production server，which is to say，配置一个Node服务器。

but，我只是简单的配置一下，毕竟我对node也不熟啊( ͡° ͜ʖ ͡°)

`npm install --save express` 安装一下express

然后我们到根目录下创建一个** server.js **文件

<!-- more -->

```
var express = require( 'express' );
var path = require( 'path' );

var app = express()  //创建一个express实例

app.use(express.static(path.join(__dirname, 'dist')))
//使用dist目录作为静态文件根目录
//所以我们可以使用/images/...的路径来获取我们的icon图标等等

app.get('*', function(req, res) {
  res.sendFile(path.join(__dirname, 'dist', 'index.html'))
  //这个是express的路由配置，但是我们是使用react router来配置路由的
  //所以我们这里就不管请求什么页面，都返回我们的index.html
  //然后让react router来跳转路由
})

var PORT = process.env.port || 8080
//设置服务器端口，默认8080

app.listen(PORT, function() {
  console.log('App is running on port ' + PORT)
})

```
设置完成之后添加一条`npm script`，然而我在step1的时候已经写进去了(*・_・)ノ⌒*
```
{
    "scripts": {
        "start:prod": "webpack -p && node server.js"
        //先webpack打包压缩一下，然后启动node服务器
    }
}
```
Biu~ 一个简单的生产环境服务器就配置完成了。

在终端使用`npm run start:prod`命令就可以启动服务器了。

到这里，这个简单的教程应该就全部结束了，把这些东西写下来也是够累的。(●´∀｀●)

[Github Repo](https://github.com/troywith77/react-weather)

![post](/images/Open-Weather-App-using-Reac/end.png)

