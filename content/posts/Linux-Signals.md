+++
title = 'Linux 信号'
date = 2019-01-02T22:03:20+08:00
+++
在 Linux 中，信号是一种进程间通信的机制，用于通知进程发生了特定事件。
<!--more-->
需要注意的是，进程 A 发送一个信号 S 给进程 B 这一看似简单的操作，中间是有一个容易被忽略但很重要的角色参与的，它就是操作系统。

# SIGINT (Interrupt) 信号
- 编号：2
- 描述：中断信号，通常用于请求进程终止，但程序可以自由设置该信号的响应逻辑。
- 触发方式：
    - 终端里按下 Ctrl+C 组合键，终端进程会发送 SIGINT 信号给终端前台运行的进程
    - `kill -2 pid` 或者 `kill -SIGINT pid`

# SIGQUIT (Quit) 信号：
- 编号：3
- 描述：类似于 SIGINT，通常用于请求进程终止，区别在于它会生成一个 core 转储（core dump），程序可以自由设置该信号的响应逻辑。
- 触发方式：
    - 终端里按下 Ctrl+\ 组合键，终端进程会发送 SIGQUIT 信号给终端前台运行的进程
    - `kill -3 pid` 或者 `kill -SIGQUIT pid`

# SIGKILL (Kill) 信号
- 编号：9
- 描述：用于强制终止进程，程序无法自由设置该信号的响应逻辑，可能会导致程序数据一致性问题。
- 触发方式：
    - `kill -9 pid` 或者 `kill -SIGKILL pid`

# SIGTERM (Terminate) 信号
- 编号：15
- 描述：用于请求进程正常终止，通常程序会捕获该信号并在终止前执行清理工作，但也以自由设置该信号的响应逻辑。
- 触发方式：
    - 操作系统在关机时，会发送 SIGTERM 给所有进程，请求它们正常退出
    - `kill -3 pid` 或者 `kill pid`（注意：这里不需要明确指定 `-SIGTERM`，因为 `-SIGTERM` 是 kill 命令的默认值）

# SIGCONT (Continue) 信号
- 编号：18
- 描述：对应于 SIGSTOP，用于恢复被暂停的进程的执行，程序无法自由设置该信号的响应逻辑。
- 触发方式：
    - `kill -18 pid` 或者 `kill -SIGCONT pid`

# SIGSTOP (Stop) 信号
- 编号：19
- 描述：用于暂停进程的执行，进程将停止在当前状态，并不再执行任何指令，程序无法自由设置该信号的响应逻辑。
- 触发方式：
    - `kill -19 pid` 或者 `kill -SIGSTOP pid`

# SIGTSTP (Terminal Stop) 信号
- 编号：20
- 描述：用于请求将进程挂起（停止），将进程放到后台运行，程序可以自由设置该信号的响应逻辑。
- 触发方式：
    - 用户在终端中按下 Ctrl+Z 组合键，终端进程会发送 SIGTSTP 信号给终端前台运行的进程
    - `kill -20 pid` 或者 `kill -SIGTSTP pid`
- 相关操作：
    - 进程被挂起后，使用 fg 命令可以将其恢复到前台继续运行
    - 进程被挂起后，使用 bg 命令可以令其在后台运行

# SIGTTIN (Terminal Input) 信号
- 编号：21
- 描述：后台进程尝试从终端读取输入时，操作系统会向该进程发送 SIGTTIN 信号，程序可以自由设置该信号的响应逻辑。
- 触发方式：
    - `kill -21 pid` 或者 `kill -SIGTTIN pid`

# SIGTTOU (Terminal Output) 信号
- 编号：22
- 描述：后台进程尝试向终端写入输出时，操作系统会向该进程发送 SIGTTOU 信号，程序可以自由设置该信号的响应逻辑。
- 触发方式：
    - `kill -22 pid` 或者 `kill -SIGTTOU pid`

# SIGWINCH (Window Change) 信号
- 编号：28
- 描述：用于通知进程当前终端窗口的尺寸发生了变化，程序可以自由设置该信号的响应逻辑。
- 触发方式：
    - 用户调整终端窗口大小时，操作系统向终端前台运行的进程发送 SIGWINCH 信号
    - `kill -28 pid` 或者 `kill -SIGWINCH pid`

