# Sass/Scss 笔记

## webpack 相关

### 依赖

- sass/node-sass
- sass-loader

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
      sass: {
        // 注意 node 和 node-sass 版本
        // additionalData 键名需要注意 sass-loader 的版本, v10.1.0
        additionalData: `
          @import "@/common/sass/vars.scss";
          @import "@/common/sass/mixins.scss";
          @import "@/common/sass/common.scss";
        `
      }
    }
  }
}
```

## 变量

```scss
$foo: 14px;
```

## 函数

### mixin

```scss
// 声明
@mixin bar1 {
  font-size: $foo;
}
@function bar2($size: 1) {
  @return @size * 2px;
}
```

```scss
// 使用
.baz1 {
  @include bar1;
}
.baz2 {
  width: #{bar2(1)};
}

```

### extend

```scss
// base class
.foo-base {
  font-size: 10px;
}
.foo {
  @extend .foo-base;
}
```

## 常用工具方法

### 多行文本过长显示省略号

```scss
// mixin
@mixin ellipsis_multiLine($num) {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: $num;
  -webkit-box-orient: vertical;
}
// class
.singleLine {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
.doubleLine {
  @include ellipsis_multiLine(2);
}
.tripleLine {
  @include ellipsis_multiLine(3);
}
// more: https://zhuanlan.zhihu.com/p/34326190
```

### 引入背景图片

```scss
@mixin bg-image($url) {
  background-image: url('~@/static/images/#{$url}');
}
```