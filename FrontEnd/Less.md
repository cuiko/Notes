# Less 笔记

## webpack 相关

### 依赖

- less
- less-loader

### 全局引入

#### vue-cli

```js
// vue.config.js
const path = require('path')

function resolve(dir) {
  return path.join(__dirname, dir)
}

module.exports = {
  chainWebpack: config => {
    config.resolve.alias.set('@', resolve('src'))
  },
  css: {
    loaderOptions: {
      less: {
        // additionalData 键名需要注意 less-loader 的版本, v7.2.1
        additionalData: `
          @import "@/common/less/vars.less";
          @import "@/common/less/mixins.less";
          @import "@/common/less/common.less";
        `
      }
    }
  }
}
```

## 变量

```less
@foo: 14px;
```

## 函数

### mixin

```less
// 声明
.bar1 {
  font-size: @foo;
}
.bar2(@size: 1) {
  @val: @size * 2px;
}
```

```less
// 使用
.baz1 {
  .bar1(); // 继承也是相同用法
}
.baz2 {
  width: .bar2(1)[@val];
}
```

### rgba

## 常用工具方法

### 多行文本过长显示省略号

```less
// mixin
.ellipsis_multiLine(@num) {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: @num;
  -webkit-box-orient: vertical;
}
// class
.singleLine {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
.doubleLine {
  .ellipsis_multiLine(2);
}
.tripleLine {
  .ellipsis_multiLine(3);
}
// more: https://zhuanlan.zhihu.com/p/34326190
```

### 引入背景图片

```less
.bg-image(@url) {
  background-image: url('~@/static/images/@{url}');
}
```