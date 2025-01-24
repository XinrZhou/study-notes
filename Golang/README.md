# Golang

## Go基础

### 主要特性

#### 可见性

1. 声明在函数内部，类似 private  
2. 声明在函数外部，对当前包可见，类似 protected  
3. 声明在函数外部且首字母大写，所有包可见，类似 public  

#### 声明方式

变量：var，常量：const，函数：func，类型：type  

声明顺序：package -> import -> type -> var -> const -> func

#### 构建及编译

src：源代码文件，pkg：包文件，bin：相关 bin 文件

### 内置类型和函数

#### 内置类型

1. 数值类型：int, int8, int16, int32, int64, uint, uint8, uint16, uint32, uint64, bool, float32, float64, complex64, complex128, string, array（固定长度的数组）
2. 引用类型：slice（序列数组）、map（映射）、chan（管道）

### init 和 main 函数

#### init 函数
> 用于 package 初始化

1. 每个包可以有多个 init 函数
2. init 函数不能被其他函数调用，而是在 main 函数执行之前，自动被调用

#### main 函数
> Go 语言默认入口函数

``` go 
func main() {
    // 函数体
}
```

1. init 和 main 在调用时不能有任何的参数和返回值，且由 Go 程序自动调用
2. init 可以用于任意包且可以有多个，main 只能用于 main 包且只能有一个

#### 函数执行顺序

- 同一个 go 文件：init 从上到下
- 同一个 package 文件：按文件名字符串比较“从小到大”顺序调用 init
- 不同 package 文件：不存在相互依赖，按照 main 包中的 import 顺序；存在相互依赖，先调用最早被依赖的 package 中的 init，最后调用 main 函数

### 下划线

#### 在 import 中

``import _ 包路径``引用该包，仅仅是为了调用 init 函数，无法通过包名来调用包中的其他函数

#### 在代码中

占位符，代表忽略某个变量，例如：``n, _ := f.Read(buf)``

### 变量和常量

#### 变量
 
变量声明以 ``var`` 关键字开头，go 语言中支持批量声明，例如  

``` go
  var (
    a, b string
    c int
    d bool
  )
```  

go 语言在声明变量的时候，会自动对变量对应的内存区域进行初始化操作，初始化成其对应类型的默认值  

##### 短变量

在函数内部，可以使用 ``:=`` 方式声明并初始化变量

``` go 
package main

import (
    "fmt"
)

var m = 100

func main() {
    n := 10
    fmt.Println(n)
}
```

##### 匿名变量

在使用多重赋值时，可以如果想要忽略某个值，可以使用匿名变量，用 _ 表示。例如 ``_, y := foo()``  

说明：匿名变量不占用命名空间，也不会分配内存，又称哑元变量

#### 常量

常量声明以 ``const`` 关键字开头，``const`` 声明多个常量时，如果省略了值则表示和上一行的值相同，例如：

``` go
const (
    a = 100
    b
    c
)
```

##### iota

在常量声明中，可以使用 ``iota`` 关键字，表示当前常量的值是上一个常量的值加 1。在 ``const`` 关键词出现时，将被重置为 0。``const`` 中每新增一行常量声明将使 ``iota`` 计数一次（``iota``可理解为 ``const`` 语句块中的行索引）

``` go
const (
    n1 = iota // 0
    n2        // 1
    n3        // 2
)
```
