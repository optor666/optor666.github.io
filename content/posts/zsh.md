+++
title = 'Zsh 笔记'
date = 2024-01-04T19:05:35+08:00
+++
Zsh（Z Shell）是一款强大的命令行解释器，具有高度可定制性和扩展性。它提供了先进的命令补全、强大的通配符和历史记录管理功能，使用户能够更高效地与命令行交互。
<!--more-->
Zsh 在用户友好性和功能丰富性方面相较于其他 Shell（如 Bash）有一些优势，被广泛用于Unix和类Unix系统。
# 常用命令
## setopt
`setopt` 是在 Zsh 中用于设置选项的命令。它可以用来启用或禁用各种选项，以更改 shell 的行为。以下是一些常用的 `setopt` 选项以及它们的作用：

1. **`setopt`：** 列出当前已启用的选项。
   ```zsh
   setopt
   ```

2. **`setopt +o <option>`：** 启用指定的选项。
   ```zsh
   setopt +o extendedglob
   ```

3. **`setopt -o <option>`：** 禁用指定的选项。
   ```zsh
   setopt -o nocaseglob
   ```

4. **`setopt <option>`：** 启用指定的选项。
   ```zsh
   setopt histignoredups
   ```

5. **`setopt no<option>`：** 禁用指定的选项。
   ```zsh
   setopt nohistexpand
   ```

以下是一些常用的 `setopt` 选项：

- **`extendedglob`：** 启用更强大的通配符扩展功能。
- **`histignoredups`：** 在历史记录中忽略重复的命令。
- **`nocaseglob`：** 在文件名匹配时不区分大小写。
- **`noglob`：** 禁用通配符展开，即 `*` 和 `?` 不再被解释为通配符。
- **`histverify`：** 在按下 Enter 键之前，将历史记录中的命令放入命令行供编辑。
- **`autocd`：** 如果输入的不是命令而是一个目录，自动切换到该目录。

# 参考
1. [https://ohmyz.sh/](https://ohmyz.sh/)
