# Redux

## Redux是什么？

Redux是JavaScript`状态容器`,提供`可预测化的状态管理`。Redux除了和React一起用，还支持其他界面库。

> 状态容器是什么？就是提供一个容器给你存储各种状态量。比如我们在React组件中会使用State保存这个组件的状态量，但这样如果两个组件想要传递数据就需要通过一些手段来解决。
>
> 如果是父组件需要传递数据（状态）给子组件，那么通过props就可以实现。
>
> 如果子组件要传递数据给父组件，那么通过callback就可以实现。
>
> 但是如果两个兄弟组件要传递数据，我们一般会把这个状态提升到它们的父组件中，然后通过父组件来传递这个状态。
>
> 那么如果两个组件的关系相差很远，那么我们就需要频繁地进行状态提升，这样非常麻烦而且不利于维护。
>
> 于是，我们可以把这个状态存储到一个全局能访问的地方，并通过一些中间操作实现可预测的功能，这样我们就能在状态被改变后重新去渲染组件。这个也就是Redux做的事情。

## 核心概念

当使用普通对象来描述应用的state时，todo应用的state可能长这样：

```js
{
  todos: [
    {
    	text: 'Eat food',
    	completed: true
  	}, 
    {
    	text: 'Exercise',
    	completed: false
  	}
  ],
  visibilityFilter: 'SHOW_COMPLETED'
}
```

这个对象没有setter方法，因此其他的代码不能随意修改它。想要更新state中的数据，你需要发起一个action。`Action是一个普通JavaScript对象，用来描述发生了什么`。示例如下：

```js
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
```

强制使用action来描述所有变化带来的好处是可以清楚地知道应用中发生了什么。最终，为了把action和state串起来，开发了一些函数，这就是reducer。`reducer只是一个接收state和action，并返回新的state的函数。`当然对于大的应用来说，不太可能只写一个这样的函数，而会拆成很多小函数来分别管理state的一部分。

```js
function visibilityFilter(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_VISIBILITY_FILTER') {
    return action.filter;
  } else {
    return state;
  }
}

function todos(state = [], action) {
  switch (action.type) {
  case 'ADD_TODO':
    return state.concat([{ text: action.text, completed: false }]);
  case 'TOGGLE_TODO':
    return state.map((todo, index) =>
      action.index === index ?
        { text: todo.text, completed: !todo.completed } :
        todo
   )
  default:
    return state;
  }
}
```

再开发一个 reducer 调用这两个 reducer，进而来管理整个应用的 state：

```js
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  };
}
```

这差不多是Redux思想的全部。但Redux里有一些工具来简化这种模式，但是最主要的就是如何根据这些action对象来更新state。

## 三大原则

### `单一数据源`

整个应用的state被存储在一颗object tree中，并且这个object tree只存在于唯一一个store中。

```js
console.log(store.getState())

/* 输出
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
*／
```

### `State是只读的`

唯一改变state的方法就是触发action，action是一个用于描述发生事件的普通对象。

```js
store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
})
```

### `使用纯函数（pure functions）来执行修改`

使用reducer描述action如何改变state tree，reducer只是一些纯函数，它接收先前的state和action，并返回新的state。

随着应用的变大，我们通常会把它拆成多个小的reducers，分别独立地操作state tree的不同部分，甚至可以编写可复用的reducer来处理一些通用任务，如分页器。

```js
//比如我有很多地方用了这个todo的数据展示，这个时候我通过redux改变后就可以一起性重新渲染全部的数据展示。
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      //分页数据
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true
          })
        }
        return todo
      })
    default:
      return state
  }
}

import { combineReducers, createStore } from 'redux'
let reducer = combineReducers({ visibilityFilter, todos })
let store = createStore(reducer)
```

## 生态系统

redux的生态系统非常丰富，如react-redux是用于与react整合的，redux-thunk和redux-saga是中间件等等。具体可以查看[redux的生态系统](https://www.redux.org.cn/docs/introduction/Ecosystem.html)。

## Action

Action是把数据从应用传到store的有效载荷。它是store数据的唯一来源。一般来说我们会说通过store.dispatch()将action传到store。

#### Action创建函数

Action创建函数就是生成action的方法。“action”和“action创建函数”这个两个概念很容器混在一起，要注意区别。

在Redux中的action创建函数只是简单地返回一个action：

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

这是为了使action更容易被移植和测试。

#### 发起dispatch的方法

1. `把action创建函数的结果传给dispatch()方法`

   ```js
   dispatch(addTodo(text))
   ```

2. `创建一个被绑定的action创建函数来自动dispatch,然后直接调用它们`

   ```js
   const boundAddTodo = text => dispatch(addTodo(text))
   const boundCompleteTodo = index => dispatch(completeTodo(index))
   
   boundAddTodo(text);
   boundCompleteTodo(index);
   ```

> store里可以直接通过store.dispatch()调用dispatch方法。但多数情况会使用react-redux提供的connect()帮助器来调用。
>
> bindActionCreators()可以自动把多个aciton创建函数绑定到dispatch()方法上。

## Reducer

Reducers指定了应用状态的变化如何响应actions并发送到store

## Store

#### store有5个职责：

- 维持应用的 state；
- 提供 [`getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 方法获取 state；
- 提供 [`dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 方法更新 state；
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 注册监听器;
- 通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 返回的函数注销监听器。

一个Redux应用只有一个单一的store。当需要拆分数据处理逻辑时，应该使用reducer组合，而不是创建多个store。

#### 根据reducer来创建store

```js
//1.将多个reducer合并
const todoApp = combineReducers({
  potato: potatoReducer,
  tomato: tomatoReducer
})

//2.通过createStore()创建store
let store = createStore(todoApp, window.STATE_FROM_SERVER)
```

> createStore()的第二个参数是可选的，用于设置state初始状态。这对开发同构应用时非常有用，服务器redux应用的state结构可以与客户端保持一致，那么客户端可以将从网络接收到的服务端state直接用于本地数据初始化。

#### 发起Actions

```js
import {
  addTodo,
  toggleTodo,
  setVisibilityFilter,
  VisibilityFilters
} from './actions'

// 打印初始状态
console.log(store.getState())

// 每次 state 更新时，打印日志
// 注意 subscribe() 返回一个函数用来注销监听器
const unsubscribe = store.subscribe(() =>
  console.log(store.getState())
)

// 发起一系列 action
store.dispatch(addTodo('Learn about actions'))
store.dispatch(addTodo('Learn about reducers'))
store.dispatch(addTodo('Learn about store'))
store.dispatch(toggleTodo(0))
store.dispatch(toggleTodo(1))
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

// 停止监听 state 更新
unsubscribe();
```

> 注意subscribe的使用

## 数据流



## 异步Action



## 异步数据流



## Middleware



