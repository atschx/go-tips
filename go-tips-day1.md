# Go Tips Day1

> Go学习过程中的一些参考，入门级学习路径，也作为回顾知识点。

## Environment

GO 的开发环境相对比较简单。

1. 下载安装GO（Mac 推荐 `brew install go`)

2. 配置两个环境变量

   | 变量   | 描述                               |
   | ------ | ---------------------------------- |
   | GOROOT | Go 安装目录                        |
   | GOPATH | 可以是工作区，Mac 默认为`$HOME/go` |

## Packages

包，可称为 `namespace` 的东西。

知识点：

- 包名小写

- 导包使用`import`关键字

- 批量导包，可使用`()`一次性导入

  ```go
  import (
   "fmt"
   "math/rand"
  )
  ```

- **导入的包必须使用**，

  - 不使用会直接编译失败（特例）
  - there are some cases (which we'll cover later in this book) where you will want to load a package, but not use it directly. This can be accomplished by appending an **underscore** before the package name in the import statement. Here is what this would look like:

  ```
  import (
   "database/sql"
   _ "github.com/go-sql-driver/mysql"
  )
  ```

- 只有一个main包，main包所在的地方才可以执行 `go instal/build`等命令

## Varibales

变量的声明简单的形如 

```Go
var s string
var s1,s2,s3 string
var s string = "albert test"
var s1,s2,s3 string = "first-string", "second-string", "third-string"
```

mix data types with the following syntax:

```go
var s,i,f = "mystring",12,14.53
```

开下眼，尤其骚气

A popular way to declare and initialize multiple variables at once is as follows：

```go
var (
   s = "mystring"
   i = 12
   f = 14.53
)
```

函数内声明和初始化变量时，可简写为

```go
s := "albert"
i := 12
f := 14.53
```

不要太开心

Go's standard data types:

| **Data type(s)**                                 | **Description**                                              |
| ------------------------------------------------ | ------------------------------------------------------------ |
| bool                                             | A Boolean (either true or false).                            |
| string                                           | A string is a collection of byte and can hold any characters. Strings are read only (immutable), so whenever you need to add or remove characters from a string, you are in effect creating a new string. |
| int, int8, int16, int32, and int64               | Signed integer types. They represent non-decimal numbers that can be either positive or negative. As you can probably tell from the type names, you can explicitly specify the number of bits that it can allow. If you go with the int type, it will pick the number of bits that correspond to your environment. For most modern CPU architectures, it will pick 64 bits, unless you are working with a smaller CPU or older environment. For smaller CPUs or older environments, the choice becomes 32 bits. |
| uint, uint8, uint16, uint32, uint64, and uintptr | Unsigned integer types. They represent non-decimal numbers, which can only be positive. Except for the signage, they are similar to their signed brethren. The uintptrtype is an unsigned integer type that is large enough to hold a memory address. |
| byte                                             | An alias for uint8, it holds 8 bits, which basically represents a byte of memory. |
| rune                                             | An alias for int32, it is typically used to represent a Unicode character. |
| float32and float64                               | Simply decimal numbers. For smaller decimal numbers, use the float32 type, as it only allows 32 bits of data. For larger decimal numbers, use the float64 type, as it only allows 64 bits of data. |
| complex64and complex128                          | Complex numbers. Those data types are useful for programs where serious math is needed. The first type, complex64, is a complex number where the real part is a 32-bit float, and the imaginary part is a 32-bit float. The second type, complex128, is a complex number where the real part is a 64-bit float, while the imaginary part is a 64-bit float. |

一些默认值需要了解下

| **Type(s)**   | **Zero value** |
| ------------- | -------------- |
| Numeric types | 0              |
| Boolean types | false          |
| String type   | ""             |
| Pointers      | nil            |

## Pointers

指针，程序的灵魂。

A **pointer** is a language type that represents the memory locations of your values. Pointers in Go are used everywhere, and that's because they give the programmer a lot of power over the code. For example, having access to your value in memory allows you to change the original value from different parts of your code without the need to copy your value around.

声明变量时，数据类型前加个`*` 就创建了一个指针（其值为nil）。nil 等同于 Java 中的null。

```
var iptr *int
```

使用`&`符号获取一个变量的内存地址

```go
var x int = 5
var xptr = &x // The & operand here means that we want the address of x.
y := *xptr //retrieve the value that it points to,This operation is called de-referencing, 
*xptr = 4
```

y 拷贝了x内存中的值 5，`*xptr = 4` 将x改为了4。

## Functions

函数和闭包

- main 函数是程序的入口，从 C 开始就是如此。

- 简化入参数数据类型

  ```GO
  func add(a,b int) int{
  	return a+b
  }
  ```

- 多值返回

- 一等公民，可以直接当参数传递

## Closures

闭包，仁者见仁。高级编程技巧

## data structures

> 数据结构，游走的基础

1. Array `var myarray [3]int`
2. Slice `var mySlice []int`
3. Map `var myMap map[int]string`
4. Struct&Method
5. Interface

## Conditional statements

- if
- switch

## Loops

```go
for i:=1;i<=10;i++{
//do something with i
}

// 遍历列表
myslice := []string{"one","two","three","four"}
for i,item := range myslice{
//do something with i and item
}

/*
when you obtain the item from the for..range statement, you only obtain a copy of the item, which means that you won't be able to change the original item that lives in the slice should the need arise. However, when you obtain the index, this gives you the power to change the item inside the slice. 
*/

// 遍历列表，并修改
myslice := []string{"one","two","three","four"}
for i := range myslice {
  myslice[i] = "other"
}
fmt.Println(myslice)
//output is: other other other other

// 这个暂且理解为 while
for i>5{
//do something
}
```

## Panics, recovers, and defers

When you invoke panicin your code, your program is interrupted, and a panic message is returned. If a panic gets triggered and you don't capture it in time, your program will stop execution and will exit, so be very careful when you use a panic. 

## defers

The defer statement basically pushes a function call to a list, and the list of saved calls is executed after the surrounding function returns. Defer is most commonly used to clean up resources, like closing a file handler, for example.

Panic -> defer -> recover()