+++
title = 'C 笔记'
date = 2013-01-03T21:27:36+08:00
+++
C 语言是一种通用的、面向过程的编程语言，被广泛应用于系统软件、嵌入式系统和高性能计算领域，因其效率高和灵活性强而备受程序员青睐。
<!--more-->
# 类型系统
## 布尔类型
早期版本的 C 语言中，并没有直接定义布尔类型，条件表达式中会将其他类型的值按特定规则转换为真值或假值。

在 C99 标准中，引入了 `bool` 关键字，用于支持布尔类型。
```
#include <stdbool.h>

bool isTrue = true;
bool isFalse = false;
```
### 真值 & 假值转换规则
- 整型值：0 表示假，非零值表示真

# 函数
## 静态函数
static 关键字修饰的函数为静态函数。静态函数属于文件作用域，仅作用于当前文件，所以多个文件可以定义同名静态函数。
```
static void hi(char *name) {
    printf("Hi, %s.\n", name);
}
```

# 变量
## 全局变量
定义在函数外的变量为全局变量。

## 局部变量
定义在函数内的变量是局部变量。

## 静态变量
static 关键字修饰的变量为静态变量，静态变量可以是全局变量，也可以是局部变量。

静态全局变量和全局变量的区别是：静态全局变量作用域为当前文件。

静态局部变量和局部变量的作用域都是函数级别，它们的区别是：静态局部变量的生存周期与程序的生存周期相同。
```
static int i;
```

## 外部变量
extern 关键字修饰的变量为外部变量，当前文件无需为该变量分配内存。外部变量只能是全局变量，不能是局部变量。
```
extern int i;
```

# 系统库
系统库函数文档可以使用如下方式查询：
1. [https://man7.org/linux/man-pages/dir_section_3.html](https://man7.org/linux/man-pages/dir_section_3.html)
2. man 命令，比如：man stdio 或 man fopen

## errno.h
### 常量
- errno

## fcntl.h
主要内容有：文件控制相关函数。

注意：这里使用的是文件描述符，而非 FILE 结构体。
### 函数
- open

## [stddef.h](https://man7.org/linux/man-pages/man0/stddef.h.0p.html)
### 常量
- NULL

## stdio.h
主要内容有：标准输入、输出函数。

注意：这里使用的是 FILE 结构体，而非文件描述符。

### 常量
- stdin（注意：这里使用的是 FILE 结构体，而非文件描述符。）

### 函数
- fileno
- fopen
- flockfile
- perror

## [stdlib.h](https://man7.org/linux/man-pages/man0/stdlib.h.0p.html)

### 函数
- exit

## string.h
主要内容有：字符串处理函数。
### 函数
- strcmp

## sys/stat.h
主要内容有：文件状态结构定义和文件状态相关函数。
### 常量
- struct stat

## sys/types.h
主要内容有：POSIX 系统类型定义。
### 常量
- ino_t

## unistd.h
主要内容有：POSIX 系统调用、符号常量。
### 常量
- STDIN_FILENO（注意：这里使用的是文件描述符，而非 FILE 结构体。）

### 函数
- read
- write

# 参考
1. The GNU C Library Reference Manual: [https://www.gnu.org/software/libc/manual/html_node/index.html](https://www.gnu.org/software/libc/manual/html_node/index.html)
