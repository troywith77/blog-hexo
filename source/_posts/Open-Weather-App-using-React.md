title: Open Weather App using React
date: 2016-03-09 18:04:46
tags:
---
这是一个利用 Open Weather 的 [API](http://openweathermap.org/api) 使用 React 写的一个天气查询App。[Github Repo](https://github.com/troywith77/react-weather)

虽然挺简单的，但是做教程写的话，感觉也得分好几篇了。。（好像还没到可以写教程的水平(•̀ロ•́)و✧ ~~ 就尽量记录清楚一点吧~）

PS: 写这个React app的时候一会用ES5，一会用ES6也是醉了 | ｀Д´|

<!--more-->

### Step0 配置npm、webpack以及babel

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

### Step2 添加查询输入框和按钮

> 以下引入的CSS都不会写出来，反正很简单~(๑╹∀╹๑)萌

新建一个** GetCityContainer.js ** 文件
```
var GetCityContainer = React.createClass({
  contextTypes: {
    router: React.PropTypes.object.isRequired  //用来跳转路由
  },
  getDefaultProps() {
    return {
      direction: 'column'  //设置默认的direction prop为"column"
    }
  },
  getInitialState() {
    return {
      city: ''  //设置state
    }
  },
  handleSubmitCity() {

  },
  handleUpdateCity(e) {
    this.setState({city: e.target.value})
    //子组件里的Input 触发onChange事件时最终会触发这个事件
    //子组件向夫组件通信通过props来完成
  },
  render() {
    return (
      <GetCity
      onSubmitCity={this.handleSubmitCity}
      onUpdateCity={this.handleUpdateCity}
      city={this.state.city}  //像子组件传递一个prop
      direction={this.props.direction}  //传递prop
      />
    )
  }
})
```

<!--more-->

在components文件夹里新建一个** GetCity **文件
```
class Input extends React.Component{
  constructor(props) {
    super(props);  //必要
  }
  render() {
    return (
      <input
      className='form-control'
      placeholder='St. George, Utah'
      type='text'
      value={this.props.city}
      onChange={this.props.onUpdateCity}  //通过props向父组件通信
      />
    )
  }
}

class Button extends React.Component{
  constructor(props) {
    super(props);
  }
  render() {
    return (
      <button
      type='button'
      style={{margin: 10}}
      className='btn btn-success'
      onClick={this.props.onSubmitCity}  //点击按钮时触发父组件的事件
      >
      {this.props.children}  //使用该组件式里面的内容会渲染到这
      </button>
    )
  }
}

function getStyles(props) {
  //根据传入的props返回不同的css
  //在这里是改变flex-direction，因为会有不同使用情况
  return {
    display: 'flex',
    flexDirection: props.direction || 'column',
    justifyContent: 'center',
    alignItems: 'center',
    maxWidth: 300,
    alignSelf: 'right'
  }
}

class GetCity extends React.Component{
  constructor(props) {
    super(props);
  }
  render() {
    return (
      <div style={getStyles(this.props)}>  //这里应用了上面返回的css对象
        <Input
        city={this.props.city}
        onUpdateCity={this.props.onUpdateCity}
        //接收来自Input组件的事件继续向父组件传递
        />
        <Button
        onSubmitCity={this.props.onSubmitCity}>
        Get Weather
        </Button>
      </div>
    )
  }
}
```

在Home组件里引入GetCityContainer组件
```
render() {
    return (
      <div>
        <h1>Enter a City Name</h1>
        <GetCityContainer />
      </div>
    )
  }
```
在Main组件里引入GetCityContainer组件并且传入一个prop为 `direction='row'`
```
<GetCityContainer direction='row' />
```

现在首页的UI就完成了，但是还没有任何功能(⊙o⊙)哦！现在页面应该长这样~ (点击看大图~)

![post](/blog/images/Open-Weather-App-using-Reac/home.png)

### Step3 添加新的天气预报页面的路由
首先在routes.js里添加一个新的路由
```
<Router history={browserHistory}>
    <Route path='/' component={Main}>
      <IndexRoute component={HomeContainer}></IndexRoute>
      <Route path='forecast/:city' component={ForecastContainer}></Route>
      //这个path的意思是可以进入比如'/forecast/shanghai'这样的url里
    </Route>
  </Router>
```
在 ** GetCityContainer **里更新一下方法
```
handleSubmitCity() {
    this.context.router.push('/forecast/' + this.state.city)
    //点击按钮时会使用context的push方法跳转到其他路由
}
```

然后把open weather的api写到** helpers/api.js ** 中，具体可查看[源代码](https://github.com/troywith77/react-weather/blob/master/app/helpers/api.js)

新建一个** ForecasrContainer.js **

<!--more-->

```
import {getCurrentWeather, getForcast} from '../helpers/api'
//引入API

var ForecastContainer = React.createClass({
  contextTypes: {
    router: React.PropTypes.object.isRequired  //使用context进行路由跳转
  },
  getInitialState: () => {
    return {
      isLoading: true,  //设置一个loading状态，初始为true
      forecastData: {}  //设置一个state存放预报数据
    }
  },
  componentDidMount() {
    this.makeRequest(this.props.params.city)
    //组件加载完成后执行函数，将路由中的city参数传入进去
  },
  componentWillReceiveProps(nextProps) {
    this.makeRequest(nextProps.params.city)
    //当组件即将要接收到新的props参数前发生的事件
    //在这里是当点击Header中的按钮时会重新发送一次请求，获取新的城市的数据
  },
  makeRequest(city) {  //Promise从API获取五日天气预报数据
    getForcast(city)
      .then(function (forecastData) {
        this.setState({
          isLoading: false,  //获取数据之后设置loading状态为false
          forecastData: forecastData  //将返回的数据传给这个state
        });
      }.bind(this));  //记得要bind一下，把函数内this的指向改成本组件
  },
  render() {
    return (
      <div>
        <Forecast
        city={this.props.params.city}
        isLoading={this.state.isLoading}
        forecastData={this.state.forecastData} />  //向子组件传入数据
      </div>
    )
  }
})

ForecastContainer.contextTypes = {
  router: React.PropTypes.object.isRequired  //类型绑定
}

module.exports = ForecastContainer
```


在components文件夹里新建一个**Forecast**组件
```
import DayItem from './DayItem'
/* 更小的一个组件，显示具体天气的图标，以及一些简要信息，
   因为会在多处用到，所以提取成一个组件，React也提倡具体的细分组件 */

/* 下面这种写法好像叫stateless 什么什么，意思是这种组件没有state，
   给它传入什么数据它就只会单纯的显示什么出来，不会向上对父组件
   发送任何数据，用定义函数的方法来定义 */

function ForecastUI (props) {
  return (
    <div>
      <h1>{props.city}</h1>  //
      <div>
      //这里的props.data.list是接下来五天天气预报数据的一个数组，
      //使用map方法然后返回组件（或者是HTML）可以将这个数组里的数据呈现出来
        {props.data.list.map(function(item) {
          return (
            <DayItem
            key={item.id}
            //每个数组map时或者使用iterator时，都要添加一个key属性
            //但是我这里是照官网的教程说的，添加到Component上而不是HTML，
            //但是这里仍然会报警告，don't know why(｡•ˇ‸ˇ•｡)
            day={item} />
          )
        })}
      </div>
    </div>
  )
}

export default class Forecast extends React.Component{
  render() {
    if(this.props.isLoading) {
    //由传入的isLoading判断显示哪一个组件
      return (<h1> Loading </h1>)
    } else {
      return (
        <ForecastUI
        city={this.props.city}  //继续向下传递数据
        data={this.props.forecastData}
        handleClick={this.props.handleClick} />
      )
    }

  }
}
```
新建** DayItem **组件
```
import {convertTemp, convertDate} from '../helpers/utils'
//API返回的温度是绝对温度，需要转换
//返回的时间是UnixTimesMpa， 需要转换

//数据具体怎么渲染要看API借口噢~

function DayItem(props) {
  var date = convertDate(props.day.dt);  //转换后格式化的时间
  var icon = props.day.weather[0].icon;
  //不同的天气有不同的代码，通过这个代码引入不同的文件
  //文件名和icon代码一样，方便引入
  return (
    <div onClick={props.handleClick}>
      <img src={'/blog/images/weather-icons/' + icon + '.svg'} />
      <h2>{date}</h2>
    </div>
  )
}

module.exports = DayItem;
```
现在App的预报界面应该是这样的（这里没有写CSS的操作哦）

![post](/blog/images/Open-Weather-App-using-Reac/forecast.png)

到现在我们已经完成了一个具有简单查询功能的天气APP啦，接下来就再添加一个详情页面，使上面的每一天的预报都可以点击然后进入详情页面的路由~

### Step4 添加详情路由，跳转到该路由时传入数据
首先打开** routes.js **，在主路由下添加一行
```
<Route path='detail/:city' component={DetailContainer}></Route>
```
然后新建一个** DetailContainer.js ** 和 ** Detail.js **文件
```
//DetailContainer.js

class DetailContainer extends React.Component{
  render() {
    return (
      <Detail
      city={this.props.routeParams.city} />  //获取路由上的city参数
    )
  }
}
```

<!--more-->

```
//Detail.js

export default class Detaul extends React.Component{
  constructor(props) {
    super(props)
  }
  render() {
    return (
      <div>
           //暂时没有东西，因为还没有把数据从上一个预报页面传过来
      </div>
    )
  }
}

```

打开** DayItem.js **
```
在根元素上添加onClick={props.handleClick}
//注意这里用的是stateless functional component，所以不是this.props
```
然后打开** Forecast.js **
```
在ForecastUI里map函数返回DayItem组件时，在DayItem组件上添加一个
handleClick={props.handleClick.bind(null, item)}
//这里把item，也就是每一天的天气预报数据传到父组件里

在Forecast组件中同样添加handleClick={this.props.handleClick}，继续传递事件与数据。
```
再打开** ForecastContainer.js **
```
//在return的Forecast组件上添加一个属性，handleClick = {this.handleClick}
//用来处理下面穿上来的数据的函数就是这个ForecastContainer组件的handleClick函数

//添加handleClick事件
handleClick(weather) {  //这里的weather就是之前传上来的item
    this.context.router.push({
    //之前我们用过context来改变路由，是直接传入string的
    //这里是传入一个对象，state属性可以传递数据
        pathname: '/detail/' + this.props.routeParams.city,  //路由地址
        state: {
            weather: weather
        }
    })
}
```
这样的话我们就可以把详细的每一天的数据传到详情路由页面的，下面看看我们要怎么在详情界面获取到这些数据并渲染到页面上~

打开** DetailContainer.js **
```
return (
      <Detail
      //显而易见，就是这么简单
      weather={this.props.location.state.weather}

      city={this.props.routeParams.city} />  //获取路由上的city参数
    )
```
打开** Detail.js **
```
import {convertTemp} from '../helpers/utils'  //转换绝对温度

return (
      <div>
        <DayItem day={this.props.weather} />
        //下面的数据全部都是从刚刚的state.weather中获取到的
        //具体的信息可以查看API解释
        <div style={styles.container}>
          <p>City: {this.props.city}</p>
          <p>{this.props.weather.weather[0].description}</p>
          <p>min Temp: {convertTemp(this.props.weather.temp.min)}degree</p>
          <p>max Temp: {convertTemp(this.props.weather.temp.max)}degree</p>
          <p>humidity: {this.props.weather.humidity}</p>
        </div>
      </div>
    )
```
Finally，这个简单的App就基本完成了！！！( ﾟ▽ﾟ)/

这个详情页的样子是这样~

![post](/blog/images/Open-Weather-App-using-Reac/detail.png)

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
//所以我们可以使用/blog/images/...的路径来获取我们的icon图标等等

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

![post](/blog/images/Open-Weather-App-using-Reac/end.png)

