# TypeScript

## 数组

- type[]

- 嵌套 type\[\]\[\]
- 泛型 Array<type>
- 泛型嵌套 Array<Array<type>>

## 枚举

## 接口

## 断言

- 值 as 类型

  - ```ts
    if (typeof (animal as Fish).swim === 'function') {
        // to do sth
    }
    ```