+++
title = '2024 阅读 Redis 3.0 源代码：数据结构'
date = 2024-04-02T19:45:58+08:00
+++
2024 年，Redis 已经到了 7.x 的版本了，但是我参考了书籍《Redis 设计与实现》，所以决定使用 3.0 版本的源代码。
<!--more-->
Redis 内部使用 C 语言从零实现了许多经典的数据结构，支撑了 Redis 内部组件实现和上层类型系统的实现。

其中，SDS、Linked List、Dict 的实现都非常简单朴实，跟教科书上描述的区别不大。但 Skip List、Int Set、Zip List 却稍微有点复杂。

# Skip List
相较于 Linked List、Dict 而言，Skip List 在数据结构入门级的书上并不常见，但还是有相关书籍和论文讲解描述它的。

# Int Set
Int Set 是用于保存不同长度整数值集合的数据结构。
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240409150626.png" title="Int Set 整体结构" >}}
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240409150728.png" title="Int Set 整体结构" >}}

# Zip List
Zip List 是 Redis 作者为了兼容多种使用场景并且尽可能地节省内存而自创的数据结构。
## Zip List 整体结构
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240407171236.png" title="Zip List 整体结构" >}}

## Zip List 节点结构
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240407171557.png" title="Zip List 节点结构" >}}

### previous_entry_length 编码
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240407171739.png" title="previous_entry_length 编码" >}}

### encoding 编码
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240407174509.png" title="encoding 编码" >}}
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240407174610.png" title="encoding 编码" >}}
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240407174714.png" title="encoding 编码" >}}

