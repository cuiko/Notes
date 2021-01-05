一、安装
---
1. 安装 [JDK](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)(64位)
2. 安装 IDE
  [eclipse](https://www.eclipse.org/downloads/) 或 [idea](https://www.jetbrains.com/idea/download/#section=windows)
3. 设置环境变量
系统变量里新建 **JAVA_HOME**，指向 C:\Program Files\Java\jdk-14
在 **PATH**，添加 **%JAVA_HOME%\bin**
4. 测试

打开 cmd
```bash
java -version
```

java：这个可执行程序其实就是JVM，运行Java程序，就是启动JVM，然后让JVM执行指定的编译后的代码；

javac：这是Java的编译器，它用于把Java源码文件（以.java后缀结尾）编译为Java字节码文件（以.class后缀结尾）；

jar：用于把一组.class文件打包成一个.jar文件，便于发布；

javadoc：用于从Java源码中自动提取注释并生成文档；

jdb：Java调试器，用于开发阶段的运行调试。

二、基本
---
1. 名词解释

JDK: Java Development Kit 包括 JRE 外，还提供编译器，调试器等开发工具

JRE: Java Runtime Environment Java 字节码的虚拟机

2. Hello, world
```java
// 类名首字母大写，e.g HelloWorld/VRMode 等
public class Hello {
  // 入口函数(main)必须长这样，方法首字母小写 e.g helloWorld/vrMode
  // public/private 公有/私有函数/类
  // static 静态
  // void 没有返回值
  // main 函数名
  // String[] 字符串数组类型的参数
  // args 参数名
  public static void mian(String[] args) {
    System.out.println('Hello, world!');
  }
}
```

3. 编译运行
```
// 编译成 .class 的文件
javac Hello.java
// 运行 .class
java Hello
```

4. IDE 设置

Eclipse 菜单 -> Window -> Preferences

打开行号： General -> Editors -> Text Editors 勾上 Show line numbers

缩进由 Tab 换成 空格： General -> Editors -> Text Editors 勾上 Insert spaces for tabs

设置字符编码格式： General -> Workspace 将 Text file encoding 设置成 UTF-8

设置换行符： General -> Workspace 将 Next text file line delimiter 设置成 Unix

5. 数据类型

1). 基本类型

整数类型：byte，short，int，long

浮点类型：float，double

字符类型：char

布尔类型：boolean

字节占用

byte：1
short：2
int：4
long：8
float：4
double：8
char：2

2). 引用类型

占位符	说明

%d	格式化输出整数

%x	格式化输出十六进制整数

%f	格式化输出浮点数

%e	格式化输出科学计数法表示的浮点数

%s	格式化字符串


字符串方法

.equals() 字符串比较 return boolean
.equalsIgnoreCase() 字符串比较(忽略大小写) return boolean
.trim() 去除首尾空格，空格包括 \n \t \r 不改变原字符串 return string
.strip() 去除首尾空格，还包括中文的空格字符 \u3000 也会删除 return string
.isEmpty() 判断是否为空字符串 return boolean
.isBlank() 判断是否是空格 return boolean
.replacae() 字符串替换 return string
.indexOf() / .lastIndexOf() 返回正序/倒序参数字符串的位置 return number
.startsWith() / .endsWith() 字符串首/尾部是否是参数字符串 return boolean


