+++
title = '计算机语言层次结构：乔姆斯基谱系（Chomsky Hierarchy）'
date = 2014-12-23T20:13:50+08:00
+++
计算机科学中的乔姆斯基谱系是一种用于分类形式语言的体系结构，由语言学家和计算机科学家诺姆·乔姆斯基在 20 世纪中期提出。该谱系将形式语言划分为四个主要层次，每个层次对应一类文法，从而为理解计算机语言的层次结构提供了基本框架。
<!--more-->
# 递归可枚举语言
递归可枚举语言的理论性质使其在计算理论、形式语言理论和理论计算机科学研究中起到关键作用。它是图灵机等强大计算模型的等价描述。

- 文法类型：递归可枚举文法（Recursively Enumerable Grammar）
- 自动机模型：图灵机（Turing Machine）

# 上下文有关语言
上下文有关语言的理论性质使其在形式语言理论、自然语言处理等领域发挥着重要作用。这一层次的语言能更好地描述自然语言中的语法结构。

- 文法类型：上下文有关文法（Context Sensitive Grammar）
- 自动机模型：线性有界非确定性图灵机（Linear-bounded non-deterministic Turing Machine）

# 上下文无关语言
上下文无关语言在编程语言设计、编译原理等领域有着广泛的应用。许多编程语言的语法可以用上下文无关文法描述。

- 文法类型：上下文无关文法（Context Free Grammar）
- 自动机模型：非确定性下推自动机（Non-deterministic Pushdown Automation）

# 正则语言
正则语言常用于描述简单的模式匹配，例如正则表达式。在计算机科学中，正则语言有着广泛的应用，尤其是在字符串匹配和搜索方面。

- 文法类型：正则文法（Regular Grammar）
- 自动机模型：有限状态自动机（Finite State Automation）

# 参考
1. [https://en.wikipedia.org/wiki/Chomsky_hierarchy](https://en.wikipedia.org/wiki/Chomsky_hierarchy)
