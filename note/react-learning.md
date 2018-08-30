# react 16.3 极客时间

- 传统web UI 开发 应用程序状态分散难以追踪和维护
- react 始终整体刷新页面

## flux 架构

- 单向数据流
- 是 react 解决关于数据模型的思路
- 不需要关心细节就可以把 store 连接到 view 上
- redux 是 fulx 架构的衍生项目（状态管理框架）

## 以组件方式考虑 UI 的构建

## 创建一个简单的组件

- 受控组件： 表单元素状态由使用者维护
- 非受控组件： 表单元素状态由 DOM 自身维护

需要考虑：
1. 创建静态 UI
2. 考虑组件的状态组成
3. 考虑组件 

## 何时创建组件

- 每个组件只做一件事
- 如果组件变得复杂，那么应该拆分成小组件；可以提高性能，组件过大所有状态都要刷新。

## 数据状态管理： DRY 原则

1. 能计算得到的状态就不要单独存储
2. 组件尽量无状态，所需数据通过 props 获取（可以使组件性能更优，并且提高可重用性 

## JSX： 语法糖

- 在 js 中直接写 html 标记。
- jsx 的本质：动态创建组件的语法糖 

```js
const name ="stanny";
const element = <h1>Hello,{name}</h1>;// 这里实际返回的是 html 结点
//等同于：
const name = "stanny";
const element = React.createElement(
    'h1',
    null,
    'Hello',
    name
);
```

- 延展属性

```js
const props = {firstName:"Stanely",lastName:"Stanny"};
const greeting = <Greeting {...props} />;
```

## 组件命名约定

1. React 认为小写的 tag 是原生 DOM 节点， 如'div'
2. 自定义的组件以大写字母开头
3. 属性语法？

## react 生命周期

可以分成三个阶段

1. 创建时
2. 更新时
3. 卸载时

1. render 阶段
2. pre-commit 阶段（更新 dom 结点称之为 commit）： 在此节点可以读取 DOM
3. commit 阶段： 

## Virtual DOM 

是 jsx 的运行基础

虚拟 DOM 的两个假设： 理解 DOM diff 算法（算法复杂度 优化到了 O（n））
1. 组件的 DOM 结构是相对稳定的
2. 类型相同的兄弟节点可以被唯一标识 （key 属性，提高性能）

DOM diff
- 节点跨层移动（几率小）删除父节点的所有子节点重新创建，不必考虑某个子节点是否会在其他地方用到。

## 组件复用的另外两种形式

设计模式，使用react 组件新的方法模式。
1. 高阶组件（一般不会有自己的 ui 呈现，只是负责为它自己封装的组件提供额外的功能支持，比如提供数据，  解决组件多层传递资源问题） 
2. 函数作为子组件

## context   

> Context 通过组件树提供了一个传递数据的方法，从而避免了在每一个层级手动的传递 props 属性。

- 根节点 提供上下文数据 provider
- 子节点共享这些数据
- provider（是根节点）  value 设置的值通过 consumer 子节点的函数参数传入
- 不要仅仅为了避免在几个层级下的组件传递 props 而使用 context，它是被用于在多个层级的多个组件需要访问相同数据的情景。

```js
const ThemeContext = React.createContext('light');

class App extends React.Component {
    render(){
        return (
            <ThemeContext.Provider value= "dark">
                <ThemeButton>
            </ThemeContext>
        )
    } 
}

function ThemeButton(prop){
    return (
        <ThemeContext.Consumer>
            {
                theme => <Button {...props} theme={theme}></Button>
            }
        </ThemeContext.Consumer>
    )
}

```

## 脚手架

三种脚手架工具：
1. Create React App，
2. Codesandbox：在线代码编辑器，支持react、vue 以及 angular 等项目。在线脚手架
3. Rekit：基于 create-react-app

### 为什么需要脚手架

web 开发日趋复杂
1. react 管理 UI
2. Redux 状态管理
3. React/Router
4. Babel 代码转换兼容
5. eslint 代码审查

脚手架工具提前定义好典型的项目需要的类库配置。

create-react-app （facebook 推出）
- 适合学习react 或者开发小型项目时使用，内置的工具是最小化
- 整合了babel、webpack、 eslint...

create-react-app + (Redux + React Router + less/scss + feature oriented architecture + dedicated ide) = Rekit

## codesandbox 

- webpack 打包工具运行在浏览器端
- 让你不再关注业务之外端配置
- 不需搭建本地环境

## 为什么需要打包

1. 编译 es6 语法特性，编译 jsx
2. 整合资源，例如图片， less/sass
3. 优化代码体积

- webpack： 各种loader => 每种资源打包后形成可以部署的资源
- 有了脚手架工具，不需要关注太多细节，就可以得到打包结果。

## 打包注意事项

1. 设置 nodejs 环境变量为 production（开发环境和生产环境不一样）  `scripts/build.js`
2. 禁用开发时专用代码，比如logger
3. 设置应用根路径： `package.json`  "homepage" 属性指明应用会被部署在哪里

- `npm run build` 通常会 生成一个 build 目录，用这个文件夹在生产环境中 部署。

---

# redux： js 状态管理框架 @dan_abramov
 
 基于 flux ，单向数据流；可以脱离于 react 独立运行，也可以于其他框架结合

1. react 组件（React 的一大创新，就是将组件看成是一个状态机，一开始有一个初始状态，然后用户互动，导致状态变化，从而触发重新渲染 UI ）： state->DOM
2. redux 把 state 移到组件之外，全局只有一个唯一的 store， tree 结构和组件有映射关系；管理全局状态。
3. redux 让组件通信更加容易：react 父子组件通过props 和暴露事件，同级组件通信只能通过父组件中转，有了 store ，所有的组件都和 store 进行通信，无须多余的中转。
4. redux 的三个特性：
    - store：所有的 view 依赖于一个 store 
    - action：可预测性： 产生一个新的 state 一定是由某个 action 触发的
    - reducer： 纯函数 更新 store （产生新的 store）

## Store

```js
const store = createStore(reducer)
```

三个方法：
1. getState() 得到当前数据
2. dispatch（action）
3. subscribe（listener） 监听 store 的变化

## action

- 异步 action 一般用来发送 ajax 请求
- 中间件可以截获特殊的 action 

## reducer

一个action 发送出来， 所有的reducer 都能接收到，根据 action.type 来决定执行哪一部分的reducer

1. combineReducers  
```js
const store = createStore(
    combineReducers({
        todos,
        counter
    })
);
```
2. bindActionCreators

## redux 异步请求

- 新加了一个 middleware
- 异步 action 把同步 action 的 dispatch 分成了不同的阶段，其实只是 几个同步 action 的组合，

## redux 中间件

- 截获某种特殊的 action,(对于 ajax 请求这种类型 用 redux thunk 来截获，判断这个action是不是一个promise，如果是一个函数就会执行它)
- 中间件截获特殊的 action； 处理完发出结果 action 
- 中间件一个典型的应用场景就 redux-logger  把所有的 action 打印到控制台，方便调试。

## 如何在 redux 中组织 action 和 reducer

- 标准形式 redux action（redux 入门教程）：所有的 action 放在一个文件， 所有的 reducer 放在一个文件？
- 新的方式： 单个 action 和 reducer 放在同一个文件（然后也会有一个总的 action 和 reducer 文件）


## 不可变数据

- redux的运行机制： store 每一个数据都是不可变数据，不可直接修改 
- 如果要修改就通过复制产生一个新对象 然后去包含你修改的值
- 新的 state 和旧的 state 是两个不同的对象 
- 为什么需要它？ 性能优化，当判断是否刷新组件时，不需要遍历每个值判断有没有变化 ；只需要判断是否指向之前的对象，不是则刷新组件；这样可以带来一个性能优化。
- 在调试的时候可以对比前后两个状态，便于追踪，查看diff
- 易于推测

## 如何操作不可变数据

- es6 {...} Object.assign
- immutability-helper  引入一个类库 语法结构看起来更清晰，只在需要的时候使用
- immer  就像操作原生对象结构一样去操作，对比来说 性能稍差

## react-router 

- 用来实现前端的路由
- 路由的概念：切换不同的 url 切换不同的内容页面
- 基于路由不仅仅是实现页面的切换，还有进一步到资源组织
- 当路由发生变化的时候，只是路由对应返回的组件容器刷新，而不是整个页面都刷新
- react-router 的实现
```jsx
<Router>
  <div>
    <ul id="menu">
      <li><Link to="/home">Home</Link></li>
      <li><Link to="/hello">Hello</Link></li>
      <li><Link to="/about">About</Link></li>
      <li><Link to="/contact">Contact</Link></li>
    </ul>

    <div id="page-container">
      <Route path="/home" component={Home}/>
      <Route path="/hello" component={Hello}/>
      <Route path="/about" component={About}/>
      <Route path="/contact" component={contact}/>
    </div>
  </div>
</Router>  

```
- 对于传统的后端路由，react-router 的新特性是：1声明式定义 2动态路由

## 三种路由实现方式

1. URL 路径 
2. hash 路由 （兼容低版本浏览器） 
3. 内存路由: 内存路由并不会反应到 url 上

```js
import { HashRouter as Router} from "react-router-dom";

import { BrowserRouter as Router} from "react-router-dom";

import {MemoryRouter} from 'react-router';
```
 
## 基于路由配置进行资源组织

1. 实现业务逻辑到松耦合：把页面相关的东西组织在一起
2. 易于扩展，重构和维护：每个路由对应一个组件
3. 路由层面实现 Lazy load

## react-router API

掌握这些就可以自己开发基于路由的应用
1. <Link> 普通链接 不会触发浏览器刷新
2. <Route> 路径匹配时 显示对应的组件  （多匹配）
3. <NavLink> 匹配时增加一个 class
4. <Prompt> 满足条件时提示用户是否离开当前页面
5. <Redirect> 根据条件 重定向当前页面，例如登录
6. <Switch>  只显示第一个匹配的路由 与Route搭配使用

## 如何通过url传递参数以及嵌套路由的使用

1. this.props.match.params.id  匹配路由参数
2. 页面状态尽量通过 url 参数定义
3. 潜逃路由：组件的嵌套 与路由的嵌套匹配
