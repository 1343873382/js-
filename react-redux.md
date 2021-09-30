# react-redux

ui组件外要包一个容器组件，由容器组件控制store进行容器状态改变

## store

利用combineReducers方法将不同的reducer合并成一个大的，形成一个对象，想要获取的状态都集合在里面

然后用createStore方法

```
import { createStore, combineReducers } from "redux"
import countReducer from "./reducer/count"
import personReducer from "./reducer/person"
const allReducer = combineReducers({
    he: countReducer,
    rens: personReducer
})
export default createStore(allReducer)
```

## reducer

控制状态的方法

```
const initState = [{ id: 1, name: "tom", age: 19 }]
  export default function personReducer(preState = initState, action) {
      let { type, data } = action
      switch (type) {
          case "jiaren":
              return [data, ...preState]
          default:
              return preState
      }
  }
```

## action

设置每个reducer的一些增减方法

```
export const Increment = data => ({ type: "increment", data })
export const Decrementn = data => ({ type: "decrement", data })
```

## 容器组件

### 精简版

```
import React, { Component } from 'react'
import { connect } from 'react-redux'
import {createIncrementAction,createDecrementAction}from "../../redux/action/count"
class countUI extends Component {
    increment=()=>{
        const {select1:{value}}=this.refs
      this.props.jia(value*1)
       
    }
decrement=()=>{     
    const {select1:{value}}=this.refs
    this.props.jian(value*1)
  }
incrementIfOdd=()=>{
    const {select1:{value}}=this.refs
     if(this.props.count%2===1){
        this.props.jia(value*1)
     }
    
}
incrementAsync=()=>{
    const {select1:{value}}=this.refs
       setTimeout(()=>{  this.props.jia(value*1)},500)
       
}
    render() {
        return (
            <div>
                <h1>当前求和:{this.props.count}</h1>
                <select ref="select1">
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                </select> &nbsp;
                <button onClick={this.increment}>+</button>&nbsp;
                <button onClick={this.decrement}>-</button>&nbsp;
                <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
                <button onClick={this.incrementAsync}>异步加</button>
            </div>
        )
    }
}
export default connect(
    // 状态
    state=>({count:state.he}),
    //操作
    {jia:createIncrementAction,
    jian:createDecrementAction
    }
)(countUI)
```

### 原始版

```
primport CountUI from "../../components/count/count"
import {connect} from "react-redux"
import {createIncrementAction,createDecrementAction}from "../../redux/action/count"
function mapStateToprops(state){
    return {count:state}
}
function mapDispatchToProps(dispatch){
    return {
        jia:number=>dispatch(createIncrementAction(number)),
        jian:number=>dispatch(createDecrementAction(number))
    }
}
export default connect(mapStateToprops,mapDispatchToProps)(CountUI)
```

## provider

```
此处用provider是为了让app的后代组件全能接收到store，不用自己一个个去匹配
ReactDOM.render( < Provider store = { store } > < App / > < /Provider>, document.getElementById("root"))
```



# 懒加载路由

```
import {lazy,Suspense}from "react"
const home=lazy(()=>import("./home"))
//suspense里面放路由组件 fallback里放当网速慢时加载的组件
<Suspense fallback={<h1></h1>}>
<Route></Route>
</Suspense>
```

# Hooks

## React.useState()

```
//初始值0在底层只会在刚开始时执行一次
const [cont,setCount]=React.useState(0)

add=()=>{setCount(count+1)}
```



## React.useEffect()

```
如果没有空数组则代表只在创建时发生一次函数
如果没有数组，则在任何时候都会发生
如果数组里有值，则只有那个值改变时才会发生
里面function表示要发生的函数，return表示组件销毁时发生的
React.useEffect(()=>{
function
return
},[])
```



## React.useRef()

```
const myRef=React.useRef()
<input ref={myRef}></input>
```

