title: Open Weather App using React ( one )
date: 2016-03-09 22:54:37
tags:
---
### Step1 配置最基本的React Router

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

<!--more-->

> 之后一些Component的引入就省略掉了，看到要用Component的地方应该知道肯定要先引入才行的吧~ 必要的引入会提出来

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