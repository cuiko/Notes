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

## 关键字/高阶类型

- never
- void
- readonly
- ?
- -?
- +?
- &
- infer

  待推断的类型

  ```ts
  type ParamType<T> = T extends (param: infer P) => any ? P : T
  // 如果 T 能够赋值给 (param: infer P) => any 则返回 P 否则返回 T
  type ParamType<T> = T extends [(param: infer P) => any] ? P : T
  ```

- !
- in

  遍历枚举类型

- keyof

  可以获取某种类型的所有键, 并返回联合类型

  ```ts
  interface Person {
    name: string
    age: number
    location: string
  }

  type K1 = keyof Person // name | age | location
  type K2 = keyof Person[] // number | length | push | ...
  type K3 = keyof { [x: string]: Person } // string | number
  ```

- typeof

  ```ts
  type A = 'a'
  type a = typeof A // 'a'
  ```

- as
- Partial<T>

  将传入的属性变为可选项

  ```ts
  // lib.es5.d.ts:1441
  type Partial<T> = {
    [P in keyof T]?: T[p]
  }
  interface User {
    name: string
  }
  type optional = Partial<User>
  // ↓
  type optional = {
    name?: string | undefined
  }
  ```

- Required<T>

  将传入的属性变为必选项

  ```ts
  type Required<T> = {
    [p in typeof T]-?: T[P]
  }
  ```

- Mutable<T>

  将 T 所有属性的 readonly 移除

  ```ts
  type Mutable<T> = {
    -readonly [P in keyof T]: T[P]
  }
  ```

- Readonly<T>

  将传入的属性变为只读属性

  ```ts
  type Readonly<T> = {
    readonly [P in keyof T]: T[P]
  }
  ```

- Record<K, T>

  将 K 中所有的属性的值转换为类型 T

  ```ts
  type Record<K extends keyof any, T> = {
    [P in K]: T
  }
  ```

- Pick<T, K>

  从 T 中取出一系列 K 的属性

  ```ts
  type Pick<T, K extends keyof T> = {
    [P in K]: T[P]
  }
  ```

- 条件类型

  ```ts
  // T extends U ? X : Y
  type TypeName<T> =
    T extends string ? 'string' :
    T extends number ? 'number' :
    T extends boolean ? 'boolean' :
    T extends undefined ? 'undefined' :
    T extends Function ? 'function' :
    'object'
  ```

- Exclude<T, U>

  从 T 中排除 U

  ```ts
  type Exclude<T, U> = T extends U ? never : T
  type Foo = Exclude<1 | 2, 1 | 3> // -> 2
  ```

- Extract<T, U>

  从 T 中提取出 U

  ```ts
  type Extract<T, U> = T extends U ? T : never
  type Foo = Extract<'a' | 'b' | 'c', 'a' | 'c'> // 'a' | 'c'
  ```

- Omit

  忽略对象某些属性功能

  ```ts
  type Omit<T, K> = Pick<T, Exclude<keyof T, K>>
  type Foo = Omit<{name: string, age: number}, 'name'> // -> { age: number }
  ```

- ReturnType

  获取函数的返回类型

  ```ts
  type ReturnType<T> = T extends (
    ...args: any[]
  ) => infer R
    ? R
    : any

  function foo(x: number): Array<number> {
    return [x]
  }
  type fn = RetrnType<typeof foo> // Array<number>
  ```

- AxiosReturnType(拓展)

  封装 axios 返回

  ```ts
  import { AxiosPromise } from 'axios

  type AxiosReturnType<T> = T extends (... args: any[]) => AxiosPromise<infer R> ? R : any

  type Resp = AxiosRetrnType<Api> // 泛型任意
  ```

- NonNullable<T>

  排除 null 和 undefined

  ```ts
  type NoNullable<T> = T extends null | undefined ? never : T
  ```

- Parameters<T>

  返回函数参数类型

  ```ts
  type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never
  ```

- ConstructorParameters<T>

  返回构造函数的参数类型

  ```ts
  type ConstructorParameters<T extends new (...args: any) => any> = T extends new (...args: infer P) => any ? P : never
  ```

- InstanceType<T>

  获取构造函数的返回类型

  ```ts
  type InstanceType<T extends new (...args: any) => any> = T extends new (...args: any) => infer R ? R : never
  
  ```
  
- PropType<T, K>(拓展)

  返回 T 中 K 的类型

  ```ts
  PropType<T, K> = K extends keyof T ? T[K] : never
  ```

## FAQ

1. null 和 undefined 区别?

  ```js
  console.log(null == undefined) // true 两者默认都转换成了 false
  console.log(typeof undefined) // undefined
  console.log(typeof null) // object
  console.log(null === undefined) // false
  ```

  - null 表示没有对象, 即该出不应该有值
  - undefined 表示缺少值, 即该出应该有值, 但是没有定义

2. any unknown never null undefined void