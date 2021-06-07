# 初入 React 世界

React: 用于构建用户**界面**的 JavaScript 库

## 两种架构

> MVC: M(model) V(view) C(controller)

> MVVM: M(model) V(view) VM(view model)

### DOM 更新方式

- 传统 DOM 更新
- Virtual DOM 更新
  - 事件合成
  - 创建和更新虚拟元素
    - DOM 元素
    - 组件元素

## JSX

语法约束
- 定义标签时, 只允许被一个标签包裹
- 标签一定要闭合

## React 元素类型

- DOM 元素
- 组件元素

## React 元素属性

- boolean 属性 `checked`
- 展开属性 `{ ... }`
- 自定义 html 属性 `data-xxx`

例外
- class => className
- for => htmlFor

## React 组件

数据单向向下流动(props)

### 1. 组成
- 属性(props)
- 状态(state)
- 生命周期

Web Components 与 React 组件的实现方式差异
- React 自定义组件元素是库自己构建的, 与 W 规范并不通用
- React 渲染过程包含了模板(JSX)的概念
- React 组件的实现均在方法与类中, 做到了互相隔离, 但不包括样式
- React 引用方式遵循了 ES6 module 标准

### 2. 构建方式
- React.createClass
- ES6 classes
- 无状态函数

React 的所有组件都继承自顶层类 React.Component, 声明了 props,
context, refs 等, 并在原型上定义了 setState, forceUpdate 方法

无状态组件不像其他两种方法在调用时会创建新实例, 它创建时始终保持了
一个实例, 避免了不必要的检查和内存分配, 做到了内部优化

### 3. 数据流

#### 3.1 props

由父组件传入的数据, 本身是不可以改变的

#### 3.2 state

组件内部创建的数据

setState 是一个异步方法

开发视角(tabs)
- 内部更新
- 外部更新(回调)

组件拆分案例

- case 1
```html
<Tabs classPrefix={'tabs'} defaultActiveIndex={0}>
  <TabNav>
    <TabHead>Tab 1</TabHead>
    <TabHead>Tab 2</TabHead>
    <TabHead>Tab 3</TabHead>
  </TabNav>
  <TabContent>
    <TabPane>第一个 Tab 里的内容</TabPane>
    <TabPane>第二个 Tab 里的内容</TabPane>
    <TabPane>第三个 Tab 里的内容</TabPane>
  </TabContent>
</Tabs>
```

- case 2
```html
<Tabs classPrefix={'tabs'} defaultActiveIndex={0}>
  <TabPane key={0} tab={'Tab 1'}>第一个 Tab 里的内容</TabPane> 
  <TabPane key={1} tab={'Tab 2'}>第二个 Tab 里的内容</TabPane> 
  <TabPane key={2} tab={'Tab 3'}>第三个 Tab 里的内容</TabPane>
</Tabs>
```

#### 3.3 propTypes

### 4. 生命周期

- componentWillMount()
- componentDidMount()
- componentWillReceiveProps(nextProps)
- shouldComponentUpdate() => (boolean)
- componentWillUpdate(nextProps, nextState)
- componentDidUpdate()
- componentWillUnmount()

### ReactDOM

Api
- findDOMNode()
- unmountComponentAtNode()
- redner() => _renderSubtreeIntoContainer
(null, nextElement, container, callback)
- unstable_renderSubtreeIntoContainer() => 
_renderSubtreeIntoContainer
(parentComponent, nextElement, container, callback)
- unstable_batchedUpdates

### ref

指向组件被调用时所创建的实例