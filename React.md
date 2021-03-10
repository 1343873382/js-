# React

## jsx语法展示

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';

class ParentCom extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            isActive: "true",
            date:new Date()
        }
    }
    render() {
        return ( <div>
           
        <button> {this.state.date.toLocaleDateString()}</button> 
           <ChildCom isActive = { this.state.isActive } > </ChildCom> 
            </div>

        )
    }
}
class ChildCom extends React.Component {
    constructor(props) {
        super(props);
        this.state = { data: true }
    }
    render() {
        return ( 
        <div >
            <div > { this.props.isActive} </div>


      </div>

        )
    }
}
ReactDOM.render(
  <ParentCom /> ,
    document.getElementById('root')
)
```

## 父组件传递给子组件值

代码是上面的代码

```
  <ChildCom isActive = { this.state.isActive } > </ChildCom> 
    <div > { this.props.isActive} </div>
```

以子标签属性的值的形式传递

## 子组件传递给父组件

子组件里调用函数（此时将子组件state里的值传过去）触发子标签里的父函数从而将传递的值放入父组件的state中去渲染

```
class Parent extends React.Component{
    constructor(props){
        super(props)
        this.state={
            ChildrenData:""
        }

    }
    setChildrenData=(data)=>{
        this.setState({
            ChildrenData:data
        })
      
    }
    render(){
        return(
            <div>
                <input />
                <div>{this.state.ChildrenData}</div>
                <Child setChildrenData={this.setChildrenData}/>
            </div>
        )
    }
}
class Child extends React.Component{
    constructor(props){
        super(props)
        this.state={
            msg:'我是大便超人'
        }
    }
    sendChildrenData=()=>{
        this.props.setChildrenData(this.state.msg)
    }
    render(){
        return(
        <div>
            <button onClick={this.sendChildrenData}>拉拉</button>
        </div>
        )
    } 
}
ReactDom.render(
    <Parent/>,
    document.getElementById('root')
)
```



## 要在标签里的大括号内显示的值必须是字符串形式

## 状态提升

两个自组件一个变化另一个也要相应发生变化，说明值存放于父组件内，所以状态提升，具体例子

```
function toCelsius(fahrenheit) {
    return (fahrenheit - 32) * 5 / 9;
  }
  
  function toFahrenheit(celsius) {
    return (celsius * 9 / 5) + 32;
  }
  function tryConvert(temperature, convert) {
    const input = parseFloat(temperature);
    if (Number.isNaN(input)) {
      return '';
    }
    const output = convert(input);
    const rounded = Math.round(output * 1000) / 1000;
    return rounded.toString();
  }
class TemperaturneInput extends React.Component{
    constructor(props){
        super(props)
        this.state={}

    }
    handleChange=(e)=>{
        this.props.onTemperatureChange(e.target.value)
    }
    render(){
        return(
<div>
    <input type="text" onChange={this.handleChange} value={this.props.temperature}/>
</div>
        )
    }
}
class Calculator extends React.Component{
    constructor(props){
        super(props)
        this.state={
            temperature:'',
            scale:''
        }

    }

    handleCelsiusChange=(temperature)=> {
        this.setState({scale: 'c', temperature});
      }
    
      handleFahrenheitChange=(temperature)=> {
        this.setState({scale: 'f', temperature});
      }
    render(){
        const scale=this.state.scale;
        const temperature=this.state.temperature;
        const celsius= scale==='f'?tryConvert(temperature,toCelsius):temperature;
        const fahrenheit=scale==='c'?tryConvert(temperature,toFahrenheit):temperature;
        return(
<div>
    <TemperaturneInput 
      scale='c'
      temperature={celsius}
      onTemperatureChange={this.handleCelsiusChange}
    ></TemperaturneInput>
    <TemperaturneInput
    scale='f'
    temperature={fahrenheit}
    onTemperatureChange={this.handleFahrenheitChange}
    ></TemperaturneInput>
</div>
        )
    }
}
ReactDom.render(
    <Calculator/>,
    document.getElementById('root')
)
```

## 路由

```
 import{HashRouter,Router,Link, Route} from 'react-router-dom'
 <HashRouter>
        <div  style={{height:'100%'}} >
           <Layout className="layout" style={{height:'100%'}}>
    <Header>
      <div className="logo" />
      <Menu theme="dark" mode="horizontal" defaultSelectedKeys={['2']}>
        <Menu.Item key="1"><Link to="/about">首页</Link>  </Menu.Item>
        <Menu.Item key="2"><Link to="/home">电影</Link></Menu.Item>
        <Menu.Item key="3"><Link to="/movie">关于</Link></Menu.Item>
      </Menu>
    </Header>
    <Content style={{ backgroundColor:'#fff'}}>
      <Route path="/about" component={AboutContainer}></Route>
      <Route path="/home"  component={HomeContainer}></Route>
      <Route path="/movie"  component={MovieContainer}></Route>
     
    </Content>
    <Footer style={{ textAlign: 'center' }}>Ant Design ©2018 Created by Ant UED</Footer>
  </Layout>
      </div>
      </HashRouter>
```

link是跳转的标签，route是显示的标签

### 如何获取hash地址

window.location.hash便是当前网页的hash值

### 路由规则

```
 <Route path="/movie/:type/:page" component={MovieList}></Route>
```

### 路由重定向

```
  <BrowserRouter>
       <Route path="/order" component={Order}></Route>
           <Route path="/home" component={Home}></Route>
           <Route path="/my" component={My}></Route>
           <Redirect from="/" to="/home"/>
           </BrowserRouter>
```

### this.props控制路由

```
  this.props.history.push('/home')
```



## 生命周期

```
componentDidMount() {  }高频率用到
```

window.location.hash.split('/')

## npm --save与--save-dev

--save会把依赖添加到packege.json文件中的dependencies,就是生产环境，而--save-dev会把以来添加到devDependencies下，就是开发环境中

## postcss依赖进行移动端适配

1.使用npm run eject 命令进行释放，让新生成的react项目存在config配置文件

2.安装postcss相关依赖

cnpm i postcss-aspect-ratio-mini postcss-px-to-viewport-opt postcss-write-svg postcss-preset-env postcss-viewport-units cssnano -S

3.配置config下的webpack.config.js文件

注意根据需求（设计图）改pxtowiew里的viewportwidth

```
// 引入postCss插件
const postcssAspectRatioMini = require('postcss-aspect-ratio-mini');
const postcssPxToViewport = require('postcss-px-to-viewport-opt');
const postcssWriteSvg = require('postcss-write-svg');
const postcssPresetEnv = require('postcss-preset-env');
const postcssViewportUnits = require('postcss-viewport-units');
const cssnano = require('cssnano');
// 引入postCss配置
postcssAspectRatioMini({}),
postcssPxToViewport({
  viewportWidth: 750, // (Number) The width of the viewport.
  viewportHeight: 1334, // (Number) The height of the viewport.
  unitPrecision: 3, // (Number) The decimal numbers to allow the REM units to grow to.
  viewportUnit: 'vw', // (String) Expected units.
  selectorBlackList: ['.ignore', '.hairlines', '.antd'], // (Array) The selectors to ignore and leave as px.
  minPixelValue: 1, // (Number) Set the minimum pixel value to replace.
  mediaQuery: false, // (Boolean) Allow px to be converted in media queries.
  exclude: /(\/|\\)(node_modules)(\/|\\)/
}),
postcssWriteSvg({
  utf8: false
}),
postcssPresetEnv({}),
postcssViewportUnits({}),
cssnano({
  "cssnano-preset-advanced": {
    zindex: false,
    autoprefixer: false
  },
})，
 
```

![](C:\Users\MECHREVO\Desktop\前端联系\学习文档\postcss1.jpg)

![](C:\Users\MECHREVO\Desktop\前端联系\学习文档\postcss2.jpg)

重新运行即可生效

## sass安装时总会出现版本问题，记得装node-sass@4.14.1版本

## react{}里的方法别加（）

## 在react更新state里的数据时可能数据更新了视图没有更新，forEach方法不靠谱得用map因为前者是浅拷贝，引用地址，react会认为仍然是之前的元素

具体的请看https://blog.csdn.net/qq_40259641/article/details/105275819