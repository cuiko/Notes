# 漫谈 React

## 事件系统

### 合成事件层

#### 1. 绑定方式

在 JSX 中, 用**小写驼峰**来书写, 而 html 事件则用**全部小写**来表示

- 事件委派
- 自动绑定
  - bind
    - 双冒号绑定 onClick="::handleSomeThing"
  - 构造器内声明
  - 箭头函数

使用原生事件时(更新后和挂载后), 记得需要手动移除

对比 React 合成事件和 JS 原生事件

1. 事件传播与阻止传播(兼容性)
2. 事件类型(子集, 放心使用)
3. 事件绑定方式(简便)

- 原生绑定方式
  - 直接在 DOM 绑定
  ```html
  <input onchange="" />
  ```
  - 在 JS 中为元素绑定
  ```js
  el.onclick = function () {};
  ```
  - 通过事件监听绑定
  ```js
  el.addEventListener("click", () => {});
  ```
- React 合成事件绑定
  ```html
  <input onChange="" />
  ```

4. 事件对象(兼容性, W3C 标准和 IE 标准的差异)

### 处理表单

- input(onChange)
- radio(onChange)
- checkbox(onChange, array) 选中时加入数组, 取消时从数组中过滤出去
- select
  - 单行(onChange)
  - 多行(onChange, array)

### 处理样式

建议为组件的 props 都加上 className

#### classnames

#### CSS Modules

CSS 命名技巧(BEM)

e.g. dialog\_\_confirm-butotn--disabled

- Block 模块名
- Element 模块中的节点名 \_\_
- Modifier 节点的相关状态 --

### 组件间通讯

#### 父 -> 子

- props

#### 子 -> 父

- 回调函数
- 自定义事件机制

#### 跨组件

- context(爷孙跨级)
- emitter(发布订阅模式)

### 组件间抽象

- mixin

```js
const mixin = function (obj, mixins) {
  const newObj = obj;
  newObj.prototype = Object.create(obj.prototype);
  for (let prop in mixins) {
    if (mixins.hasOwnProperty(prop)) {
      newObj.prototype[prop] = mixins[prop];
    }
  }
  return newObj;
};
const BigMixin = {
  fly: () => {
    console.log("I can fly");
  },
};
const Big = function () {
  console.log("new big");
};
const FlyBig = mixin(Big, BigMixin);
const flyBig = new FlyBig(); // => 'new Big'
flyBig.fly(); // => 'I can fly'
```

### 高阶组件(HOC)

接受一个组件作为输入, 输出一个新的组件

高阶组件的实现方法

- 属性代理
  - 控制 props
  - 通过 refs 使用引用
  - 抽象 state
  - 使用其他元素包裹 WrappedComponent(样式, 布局等)
- 反向继承
  - 渲染劫持
  - 控制 state, props 等
- 组件命名(ReactComponent.displayName)
- 组件参数(再包一个函数)