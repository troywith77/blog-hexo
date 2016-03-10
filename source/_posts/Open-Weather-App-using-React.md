title: Open Weather App using React (One)
date: 2016-03-09 18:04:46
tags:
---
这是一个利用 Open Weather 的 [API](http://openweathermap.org/api) 使用 React 写的一个天气查询App。[Github Repo](https://github.com/troywith77/react-weather)

虽然挺简单的，但是做教程写的话，感觉也得分好几篇了。。（好像还没到可以写教程的水平(•̀ロ•́)و✧ ~~ 就尽量记录清楚一点吧~）

PS: 写这个React app的时候一会用ES5，一会用ES6也是醉了 | ｀Д´|

### Step1 配置npm、webpack以及babel
`package.json`里面的部分关键配置，包括启动脚本以及依赖
```javascript
"scripts": {
    "start": "webpack-dev-server --inline --content-base dist/ --history-api-fallback",
    //使用webpack-dev-server时的命令
    "start:prod": "webpack -p && node server.js"
    //生产环境的命令
  }

---

"dependencies": {
    "axios": "^0.9.1",   // making http request
    "babel-preset-es2015": "^6.6.0",
    "babel-preset-react": "^6.5.0",
    "react": "^0.14.7",
    "react-dom": "^0.14.7",
    "react-router": "^2.0.0"
  },
  "devDependencies": {
    "babel-core": "^6.5.2",
    "babel-loader": "^6.2.4",
    "babel-preset-react": "^6.5.0",
    "html-webpack-plugin": "^2.9.0",
    "webpack": "^1.12.14",
    "webpack-dev-server": "^1.14.1"
  }
```
这里是`webpack.config.js`的配置
```javascript
/*
var HtmlWebpackPlugin = require('html-webpack-plugin')
var HTMLWebpackPluginConfig = new HtmlWebpackPlugin({
  template: __dirname + '/app/index.html',  //模板文件
  filename: 'index.html',  //文件名
  inject: 'body'  //script标签插入到body标签里
})
*/
/*
    之前用了html-webpack-plugin这个插件自动生成dist里面的index.html，
    但是由于使用browserHistory需要引用script标签时用绝对引用，这个插件引入
    的不是绝对引用，所以暂时这里自己在dist目录下新建一个index.html，
    引入inde_bundle.js文件
*/

module.exports = {
  entry: [
    './app/index.js'   //设置入口文件，webpack将从这里开始打包依赖的代码
  ],
  output: {
    path: __dirname + '/dist',  //顾名思义，输出文件的目录
    filename: "index_bundle.js"  //输出的文件名
  },
  module: {
    loaders: [  //对文件进行操作用到的（工具？）不知道怎么描述(￣ε(#￣) Σ
      {
        test: /\.js$/,  //正则匹配以js结尾的文件
        exclude: /node_modules/,  //不匹配这个文件夹里文件
        loader: "babel-loader",  //使用这个loader
        query: {
          presets: ['react', 'es2015']  //对这个loader的配置，先用es2015转换一下再用react的语法转换
        }
      }
    ]
  },
  plugins: [HTMLWebpackPluginConfig]  //使用这个插件可以自动生成带有script标签的html文件，配置在顶上
}
```

