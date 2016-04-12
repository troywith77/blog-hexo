title: Open Weather App using React ( four )
date: 2016-03-10 10:12:25
tags:
---
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

![post](/images/Open-Weather-App-using-Reac/detail.png)