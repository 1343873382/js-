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
import CountUI from "../../components/count/count"
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

