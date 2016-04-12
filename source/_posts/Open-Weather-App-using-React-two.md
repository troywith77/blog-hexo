title: Open Weather App using React ( two )
date: 2016-03-10 09:44:36
tags:
---
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

![post](/images/Open-Weather-App-using-Reac/home.png)

