# Go 学习笔记

## Go 的特性

- 内置并发编程支持
    - 使用携程(goroutine)作为基本的计算单元, 轻松地创建协程
    - 使用通道(channel)来实现协程间的同步和通信
- 内置映射(map)和切片(slice)类型
- 支持多态(polymorphism)
- 使用接口(interface)来实现裝盒(value boxing)和反射(reflection)
- 支持指针
- 支持函数闭包(closure)
- 支持方法
- 支持延迟函数调用(defer)
- 支持类型内嵌(type embedding)
- 支持类型推断(type deduction or type inference)
- 内存安全
- 自动垃圾回收
- 良好的代码跨平台性

## GO 的编译器

- gc
- gccgo 

## 书写顺序建议

```go
package main

import (
    "log"
)

const PI = 3.14

var name = "go"

type Int int

type Struct struct {
    
}

type Interface interface {
    
}

func main() {
    // to do
}
```

## 内置类型/函数

### 内置类型

#### 基本类型

```go
// 布尔类型
bool
// 数值类型
int8, int16, int32(rune), int64
uint8(byte), uint16, uint32, uint64
float32, float64
complex64, complex128
// 字符串类型
string
```

##### 整型字面量

```go
0xF // 十六进制(必须以 0x 或 0X 开头)
0o17 // 八进制(必须以 0 0o 或 0O 开头)
0b1 // 二进制(必须以 0b 或 0B 开头)
```

##### 浮点型字面量

```go
1.23
01.23 // == 1.23
.23
1.
// xEn/xE+n 表示 x 乘以 10^n, xE-n 表示 x 除以 10^n
1.23e2 // == 123.0
123E2 // == 12300.0
123.E+2 // == 12300.0
// 十六进制 yPn表示 y 乘以 2^n, yP-n 表示 y 除以 2^n
0x1p-2 // == 0.25
0x2.p10 // == 2048.0
```

#### 组合类型

- 指针类型
- 结构体类型
- 函数类型
- 容器类型
    - 数组类型
    - 切片类型
    - 映射类型
- 通道类型
- 接口类型

### 内置函数

```go
append
new
cap
copy
len
make
delete
print println
panic
recover
real // 返回复数实部
imag // 返回复数虚部
close
```

### 内置接口

```go
type error interface {
    Error() string
}
```

## `init` 函数和 `main` 函数

### `init` 函数

1. `init` 函数用于在程序执行前做包的初始化的函数, 比如初始化包里的变量等
2. 每个包内可以有多个 `init` 函数
3. 包的源文件也可以拥有多个 `init` 函数
4. 同一个包中多个 `init` 函数的执行顺序 Go 语言没有明确的定义(说明)
5. 不同包的 `init` 函数按照包导入的依赖关系决定该初始化函数的执行顺序
6. `init` 函数不能被其他函数调用, 而是在 `main` 函数执行前自动被调用

### `main` 函数

### `init` 函数和 `main` 函数的异同

#### 相同点

1. 两个函数在定义时不能含有参数和返回值, 且由 Go 自动调用

#### 不同点

1. `init` 可以在不同包定义多个, `main` 只可以在 `main` 包中定义一个

### `init` 函数和 `main` 函数的执行顺序

1. 对同一个 Go 文件的 `init` 调用顺序是从上到下的

2. 对同一个 `package` 中不同文件是按文件名字符串比较"从小到大"顺序调用各文件中的 `init` 函数

3. 对于不同的 `package`, 如果不相互依赖的话, 按照 `main` 包中"先 import 的后调用"的顺序调用其包中的 `init`, 如果 `package` 存在依赖，则先调用最早被依赖的 `package` 中的 `init`, 最后调用 `main` 函数

## 命令

## 运算符

## 下划线

### 下划线在 import 中

1. 只调用包中的 `init` 函数

### 下划线在代码中

1. 忽略变量
2. 占位符

## 变量和常量

### 变量

### 常量

#### iota

#### 自动补全

## 基本类型

### 类型转换

```go
// T(表达式)
var s = []byte{'a', 'b'}
ss := string(s)
```
### 修改字符串

```go
// 需要先转换成 []rune 或 []byte, 处理完后再转换为 string
```

## 函数

### 特点

- 支持不定变参
- 支持多返回值
- 函数也是一种类型, 可以赋值给变量

---

- 不支持重载
- 不支持默认参数

## 类型

### 类型声明的方式

- 类型定义声明
- 类型别名声明

### 类型定义

- 一个新定义的类型和它的源类型为两个不同的类型
- 在两个不同的类型定义中的定义的两个类型肯定为两个不同的类型
- 一个新定义的类型和它的源类型的底层类型一致并且它们的值可以相互显式转换
- 类型定义可以出现在函数体内

### 定义类型和非定义类型

```go
type A []string // 定义类型
type B = A // 定义类型
type C = []string // 非定义类型
```

### 类型别名声明

```go
type (
    Name = string
    Age = int
)
type table = map[string]int
type Table = map[Name]Age
```

### 底层类型(与源类型不同)

#### 如何溯源一个声明的类型的底层类型

```go
type (
    MyInt int
    Age int
)

type (
    IntSlice []int
    MyIntSlice []MyInt
    AgeSlice []Age
)

type Ages AgeSlice
```

在溯源过程中, 当遇到一个内置类型或者非定义类型时, 溯源结束

MyInt → int

Age → MyInt → int

IntSlice → []int

MyIntSlice → []MyInt → ~~[]int~~

AgeSlice → []Age → ~~[]MyInt~~ → ~~[]int~~

Ages → AgeSlice → []Age → ~~[]MyInt~~ → ~~[]int~~

### 值

### 值部

### 值尺寸

### 容器类型

- 映射类型的键值(key)类型必须是可比较的
- 容器类型的元素类型必须为同一个类型

### 通道类型的方向

### 可比较类型和不可比较类型

不可比较类型:

- 切片类型
- 映射类型
- 函数类型
- 任何包含有不可比较类型的字段的结构体和任何元素类型为不可比较类型的数组类型

其他类型为可比较类型

## 指针

### 内存地址

### 值的地址

### 什么是指针?

指针是 Go 的一种类型分类, 一个指针可以存储一个内存地址.

### 指针类型和值

一个非定义指针类型的字面形式为 *T, T 为任意类型, 类型 T 为 指针类型 *T 的基类型

```go
*int // int
**int // *int

type Ptr *int // int
type PP *Ptr // Ptr
```

### 如何获取一个指针值?

1. new(T) -> *T
2. &t -> *T

```go
func main() {
    var p1 = new(int)
    var p2 = new(int)
    fmt.Println(p1 == p2)
    fmt.Println(&p1 == &p2)
    fmt.Println(*p1 == *p2)
    fmt.Println(p1, p2)
    fmt.Println(&p1, &p2)
    fmt.Println(*p1, *p2)
}
```

### 指针(地址)解引用

使用前置解引用操作符*来访问存储在一个指针所表示的地址处的值(即此指针所引用着的值)

### 我们为什么需要指针?

所有赋值(包括函数调用传参)过程都是一个值赋值过程

## 结构体

### 组合字面量

T{ ... }

### 结构体字段的可寻址性

如果一个结构体是可寻址的, 则它的字段也是可寻址的; 反之, 一个不可寻址的结构的字段也是不可寻址的. 不可寻址的字段的值是不可更改的. 所有的组合字面量都是不可寻址的.

```go
type Book struct {
    Pages int
}

var book = Book{}
p := &book.Pages
*p = 100
fmt.Println(book) // {123}

// 因为 Book{} 不可寻址, 所以 Book{}.Pages 也不可寻址
// Book{}.Pages = 123
// p = &Book{}.Pages // <=> p = &(Book{}.Pages)
```

属性选择操作符优先级高于取地址操作符

### 组合字面量不可寻址但可被取地址

```go
type Book struct {
    Pages int
}
p := &Book{100} // <=> tmp := Book{100}; p := &tmp
p.Pages = 200
```

### 关于结构体值的类型转换

S0 的值不能被转换为其他四个类型中的一个, 原因是它与另外四个类型的对应字段名不相同

```go
package main

type S0 struct {
    y int "foo"
    x bool
}

type S1 = struct {
    x int "foo"
    y bool
}

type S2 = struct {
    x int "bar"
    y bool
}

type S3 S2
type S4 S3

var v0, v1, v2, v3, v4 = S0{}, S1{}, S2{}, S3{}, S4{}
_ = v0
v1 = S1(v2)
v2 = S2(v1)
v1 = S1(v3)
v3 = S3(v1)
v1 = S1(v4)
v4 = S4(v1)
v2 = v3
v3 = v2 // 这两个转换可以是隐式的
v2 = v4
v4 = v2 // 这两个转换也可以是隐式的
v3 = S3(v4)
v4 = S4(v3)
```

### 匿名结构体类型可以使用在结构体字段声明中

```go
var aBook = struct {
    author struct {
        firstName, lastName string
        gender bool
    }
    title string
    pages int
}{
    author: struct {
        firstName, lastName string
        gender bool
    }{
        firstName: "Mark",
        lastNmae: "Twain",
    },
    title: "The Million",
    pages: 10,
}
```

## 值部

Go 中有的值在内存中只占据一个内存块, 也有的值在内存中占据多个内存块

一个 Go 值分布在不同内存块上的部分为此值的各个值部, 一个分布在多个内存块上的值含有
一个直接值部和若干个被此直接值部应用着的间接值部

### Go 类型的两大类别

| 每个值在内存中只分布在一个内存块上的类型                     | 每个值在内存中会分布在多个内存块上的类型                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 单直接值部                                                   | 直接值部 -> 底层间接值部                                     |
| 布尔类型<br />各个数值类型<br />指针类型<br />非类型安全指针类型<br />结构体类型<br />数组类型 | 切片类型<br />映射类型<br />通道类型<br />函数类型<br />接口类型<br />字符串类型 |

如果一个值 x(直接或间接地) 引用着另一个值 y, 并且值 y(直接或间接地) 引用着第三个值 z,
则我们可以说值 x 间接地引用值 z

指针包裹类型: 一个含有 (直接或者间接) 指针字段的结构体类型

指针持有者类型: 一个含有 (直接或者间接) 指针的类型

指针类型和指针包裹类型都属于指针持有者类型

元素类型为指针持有者类型的数组类型也属于指针持有者类型

### Go 中的两种指针类型

### 第二个分类中的类型的(可能的)内部实现结构定义

#### 映射 通道 函数类型的内部定义

```go
// 映射类型
type _map *hashtableImpl // 目前, 官方标准编译器是使用哈希表来实现映射的

// 通道类型
type _channel *channelImpl

// 函数类型
type _function *functionImpl
```

这三个种类的值的直接部分在内部是一个指针, 每个值的直接部分引用着它具体实现的底层间接部分

#### 切片类型的内部定义

```go
type _slice struct {
    elements unsafe.Pointer // 引用着底层的元素
    len int // 元素个数
    cap int // 切片容量
}
```

一个切片类型在内部可以被看作是一个指针包裹类型, 每个非零切片值包含着一个底层间接
部分来存储此切片的元素, 一个切片值的底层元素序列(间接部分)被此切片值的 elements 
字段所引用

#### 字符串类型的内部结构

```go
type _string struct {
    elements *byte // 引用着底层的 byte 元素
    len int // 字符串长度
}
```

字符串类型在内部也可以看做是一个指针包裹类型, 每个非零字符串值含有一个指针
字段 elements, 这个指针字段引用着此字符串值的底层字节元素序列

#### 接口类型的内部定义

### 在赋值中, 底层间接值部将不会被复制

在 Go 中, 每个赋值操作(包括函数调用传参)都是一个值的浅复制过程(假设源值和
目标值的类型相同).
换句话说, 在一个赋值操作中, 只有源值的直接部分被复制给了目标值. 如果源值含有间接部分
, 则在此赋值操作完成之后, 目标值和源值的直接部分将引用着相同的间接部分. 换句话说,
两个值将共享底层的间接值部

因为一个间接值部可能不专属于任何一个值, 所以在使用 `unsafe.Sizeof` 函数计算一个
值的尺寸的时候, 此值的间接部分所占的内存空间未被计算在内

## 数组 切片 映射

每个容器(值)用来表示和存储一个元素序列或集合, 一个容器中的所有元素类型都
是相同的

键值类型必须是可比较类型, 数组和切片的键值类型均为内置类型 int

每个容器都有长度属性, 用来表明此容器中当前存储了多少个元素. 一个数组或切片的键值
(元素索引)范围为 [0, 容器长度), 一个映射类型的键值为该键值类型的任意值

一个数组或者切片的所有元素紧挨着存放在一块内存中. 一个数组中所有元素均存放在此数组值
的直接部分, 一个切片中的所有元素均存放在此切片值的间接部分

一个映射中的所有元素也均存放在一块连续的内存中, 但映射中的元素并不一定紧挨着存放,
一个映射中所有元素的键值也存放在此映射值(的间接部分)中

### 非定义容器类型的字面表达式

- 数组类型: `[N]T`
- 切片类型: `[]T`
- 映射类型: `map[K]T`

T 可为任意类型, 指定容器类型中的值的类型只能是该类型
N 必须为非负整数, 指定数组的长度
K 必须为一个可比较类型, 指定映射类型的键值类型

### 容器类型字面量的表达式

```go
// 数组
[4]bool{false, true, true, false}
[4]bool{0: false, 1: true, 2: true, 3: false}
[4]bool{1: true, true}
[4]bool{2: true, 1: true}
[...]bool{false, true, true, false}
[...]bool{3: false, 1: true, true}
// 切片类型
[]string{"break", "continue", "fallthrough"}
[]string{0: "break", 1: "continue", 2: "fallthrough"}
[]string{2: "fallthrough", 1: "continue", 0: "break"}
[]string{2: "fallthrough", 0: "break", "continue"}
// 映射类型
map[string]int{"C": 1,"Python": 2, "Go": 3}
```

### 容器类型零值的字面量表达式

一个数组类型的零值可以表示为 A{}, 当中的所有元素均为对应数组元素类型的零值, 
切片和映射类型的零值均用预声明的标识符 nil 表示.

在运行时, 即时一个数组变量在声明的时候未指定初始值, 它的元素所占的内存空间也已
经被开辟出来, 但是一个 nil 切片或者映射值的元素的内存空间尚未被开辟出来.

[]T{} 表示类型 []T 的一个空切片值, 它和 []T(nil) 是不等价的, 同样 map[K]T{} 和
map[K]T(nil) 也是不等价的.

试解释以下区别:

```go
var m1 map[int]int
var m2 = map[int]int{}
var s1 []int
var s2 = []int{}
fmt.Println(m1, s1) // map[] []
fmt.Println(m2, s2) // map[] []
fmt.Println(&m1, &m2) // &map[] &map[]
fmt.Println(&s1, &s2) // &[] &[]
fmt.Printf("m1: %p m2: %p s1: %p s2: %p\n", m1, m2, s1, s2) // m1: 0x0 m2: 0x001 s1: 0x0 s2: 0x002
fmt.Printf("m1: %#v m2: %#v s1: %#v s2: %#v\n", m1, m2, s1, s2) // m1: map[int]int(nil) m2: map[int]int{} s1: []int(nil) s2: []int{}
fmt.Println(m1 == nil) // true
fmt.Println(m2 == nil) // false
fmt.Println(s1 == nil) // true
fmt.Println(s2 == nil) // false
//fmt.Println(m1 == m2) // 报错
//fmt.Println(s1 == s2) // 报错
```

### 容器字面量是不可寻址的但可以被取地址

### 容器元素的可寻址性

- 可寻址的数组的元素也是可寻址的, 不可寻址的数组元素的元素也是不可寻址的
- 一个切片值的任何元素都是可寻址的, 即时此切片本身是不可寻址的
- 任何映射元素都是不可寻址的

----- 150 -----

## 内存

### 


---

## FAQ

### Why does Go not have (generic types)/execptions/assertions/(type inheritance)/(overloading of methods and operators)/("implements" declarations)/(variant types)/(untagged unions)/(covariant result types)/(implicit numberic conversions)/?

### How can i guarantee my type satisfies an interface?

### Why doesn't type T satisfy the Equal interface?

### Can I convert a []T to an []interface{}?

### Can I convert []T1 to []T2 if T1 and T2 have the same underlying type?

### Why is my nil error value not equal to nil?

### Why are map operations not defined to be atomic?

### How do i get denamic dispatch of methods?

### How do canstants work in Go?

### Why don't maps allow slice as keys?

### Why are maps, slice, and channels references while arrays are values?

### When are function parameters passed by value?

### When should I use a pointer to an interface?

### Should I define methods on values or pointers?

### What's the difference between new and make?

### What operations are atomic? What about mutexes?

### Why doesn't my program run faster with more CPUs?

### How can I control the number of CPUs?

### Why is there no goroutinue ID?

### Why do T and *T have different method sets?

### What happens with closures running as goroutines?

### Why does Go not have the ?: operator?(三元表达式)

### Where is my favorite helper function for testing?

---

### [fmt 包](https://golang.google.cn/pkg/fmt/)

### 哪些地址可以被取地址, 哪些地址不可用被取地址?

以下的值是不可用寻址的:

- 字符串的字节元素
- 映射元素
- 接口值的动态值?(类型断言的结果)
- 常量(包括有名常量和字面量)
- 声明的包级别函数?
- 方法(用做函数值)
- 中间结果值
    - 显式值转换
    - 函数调用
    - 各种操作, 不包含指针解引用操作, 但是包含:
        - 通道接收操作
        - 子字符串操作
        - 子切片操作
        - 加法/减法/乘法以及除法等等

以下的值是可寻址的, 因此可以被取代地址:

- 变量
- 可寻址的结构体字段
- 可寻址的数组的元素
- 任意切片的元素
- 指针解引用操作
