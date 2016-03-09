title: Open Weather App using React (One)
date: 2016-03-09 18:04:46
tags:
---
这是一个利用 Open Weather 的 [API](http://openweathermap.org/api) 使用 React 写的一个天气查询App。

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
var HtmlWebpackPlugin = require('html-webpack-plugin')
var HTMLWebpackPluginConfig = new HtmlWebpackPlugin({
  template: __dirname + '/app/index.html',  //模板文件
  filename: 'index.html',  //文件名
  inject: 'body'  //script标签插入到body标签里
})

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

### Step2 配置最基本的React Router

在根目录创建一个app目录，里面存放我们开发时用的文件，转到生产环境后会build文件到dist目录，这个之后再讲。


> 注意：webpack-dev-server中有一个命令是`--content-base dist/`，意思是启动服务器时会把dist目录作为根目录，开发时用到的公共文件将放在这里。

app/目录结构：
```
app/
    components/
    config/
    containers/
    helpers/
    index.html
    index.js
dist/
package.json
```

** index.html **
```html
<div id="app"></div>
```
因为上面webpack里配置了`html-webpack-plugin``插件，所以启动服务器后会制动生成一个带有bundle后的js文件的html文件。

** index.js **
```javascript
ReactDOM.render(routes, document.getElementById('app'))
```
把用`react-router`组织的组件render到`#app`上。

在config文件夹里面新建一个`routes.js`文件

> 之后一些Component的引入就省略掉了，看到游泳Component的地方应该知道肯定要先引入才行的吧~ 必要的引入会提出来

```
import { Router, Route, browserHistory } from 'react-router'

var routes = (
  <Router history={browserHistory}>  //为了得到干净的url
    <Route path='/' component={Main}>
      //设置一个url为'/'时的路由
      //渲染其子路由时，Main组件会一直存在，SPA嘛~
      <IndexRoute component={HomeContainer}></IndexRoute>
      //路由为'/'时渲染HomeComponent组件
    </Route>
  </Router>
);

module.exports = routes;
```
在containers文件夹里新建一个Main.js文件：
```
import { Link } from 'react-router'

export default class Main extends React.Component{
  render() {
    return (
      <div>
      //每个React Component都必须有且只有一个根元素！！很容易出错的地方
        <div>
          <h1>
            <Link to='/' style={styles.link}>Clever Weather</Link>
            //Link组件是react-router里定义的
            //to属性是定义的路由path，链接要用这个组件，用普通的a标签不可以
          </h1>
        </div>
        {this.props.children}
        //这是子路由起作用的关键，Main组件之外的子组件将渲染到这里。
        //作用应该和Angular里的ui-view差不多吧~
      </div>
    )
  }
}
```

然后新建一个HomeContainer.js文件：
```
export default class HomeContainer extends React.Component{
    render() {
        return <h1>Hello World!</h1>
    }
}
```
到这里在终端里输入`npm start`启动我们的webpack-dev-server就可以看到`Hello World！`了~

** open http://localhost:8080 **