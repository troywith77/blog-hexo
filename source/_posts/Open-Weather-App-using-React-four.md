title: Open Weather App using React ( four )
date: 2016-03-10 10:12:20
tags:
---
### Step4 添加新的天气预报页面的路由
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
      <img src={'/images/weather-icons/' + icon + '.svg'} />
      <h2>{date}</h2>
    </div>
  )
}

module.exports = DayItem;
```
现在App的预报界面应该是这样的（这里没有写CSS的操作哦）

![post](/images/Open-Weather-App-using-Reac/forecast.png)

到现在我们已经完成了一个具有简单查询功能的天气APP啦，接下来就再添加一个详情页面，使上面的每一天的预报都可以点击然后进入详情页面的路由~