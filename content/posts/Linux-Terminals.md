+++
title = 'Linux 终端编程'
date = 2019-01-02T19:32:19+08:00
draft = true
+++
现代计算机有着漂亮、易用的图形用户界面，普通用户已经不需要再用终端操作计算机了。但是，对于程序员群体来说，终端依然扮演着不可或缺的角色。
<!--more-->
# 终端 & 伪终端 & 终端模拟器 & 终端驱动程序 & bash
1. 终端（Terminal），是计算机上的一个硬件设备，用于用户与操作系统进行文本或命令交互，现代计算机基本上已经不再提供终端硬件设备了，而是使用软件实现的终端模拟器。
2. 终端模拟器（Terminal Emulator），是一个图形界面（GUI）程序，用于模拟终端硬件设备。终端模拟器使用伪终端来处理用户界面的输入、输出。
3. 伪终端（Pseudo Terminal，PTY），是计算机上操作系统提供的一个虚拟设备，用于模拟真实终端的行为，本质上它只是文件描述符。

# 终端驱动程序
1. 在常规模式下，解释 CTRL+C 等特殊字符，将其解释为向前台进程组发送 SIGNAL
2. 在常规模式下，自动回显用户的输入

# 伪终端
A pseudoterminal is a virtual device that provides an IPC channel. On one end of the channel is a program that expects to be connected to a terminal device. On the other end is a program that drives the terminal-oriented program by using the channel to send it input and read its output.

## 使用伪终端的程序
1. terminal emulator
2. script(1) program
3. network login service program, such as ssh

# tty & pty


# 终端驱动
1. 不论是真实终端、还是伪终端，都需要驱动程序处理终端设备的输入、输出。
2. 终端驱动通常会操作两个队列：
    - 第一个队列，用于接收终端设备的输入，并将其传输给读取中的进程
    - 第二个队列，用于接收进程的输出，并将其发送给终端设备
3. 终端驱动通常提供两种模式：标准模式、非标准模式。

## 标准模式
1. 按行读取，且支持行编辑。
2. 上层软件执行 read 操作时，会阻塞，直到用户按下回车键。
3. 在标准模式下，终端驱动通常会解释一系列特殊字符，例如：interrupt character (Control-C), the end-of-file character (normally Control-D)。终端驱动会将这些特殊字符解释为对前台进程组的信号或者正在从终端读取输入的输入条件。

## 非标准模式
1. 实时读取。使用这种模式的程序有：vi、more、less
2. 在非标准模式下，终端驱动通常不会解释特殊字符。

# 控制终端的 Shell 命令
1. tty 查询终端名称
2. stty 查询终端设置、修改终端设置

# 终端编程相关 API
## 存储终端设置的结构体 termios
```C
struct termios {
    tcflag_t c_iflag; // Input flags
    tcflag_t c_oflag; // Output flags
    tcflag_t c_cflag; // Control flags, relating to hardware control of the terminal line
    tcflag_t c_lflag; // Local modes, controlling the user interface for terminal input
    cc_t c_cc[NCCS]; // Terminal special characters 
    cc_t c_line; // Line discipline (nonstandard)
    speed_t c_ispeed; // Input speed (nonstandard; unused)
    speed_t c_ospeed; // Output speed (nonstandard; unused)
}
```
1. 前四个 flag 字段用于控制终端行为。
2. 第五个数组字段用于存储终端特殊字符。
3. 后三个非标准字段不常用，可忽略。
## 查询终端设置、修改终端设置
``` C
#include <termios.h>

int tcgetattr(int fd, struct termios *termios_p);
int tcsetattr(int fd, int optional_actions, const struct termios *termios_p);
    // Both return 0 on success, or -1 on error.

/*
 * optional_actions 取值如下：
 *  TCSANOW: The change is carried out immediately.
 *  TCSADRAIN: The change takes effect after all currently queued output has been 
 *      transmitted to the terminal.
 *  TCSAFLUSH: The change takes effect as for TCSADRAIN, but, int addition, any 
 *      input that is still pending at the time the change takes effect is 
 *      discarded.
 */
```

# 关联章节
1. 64. Pseudoterminals P1419

# 参考程序
1. vi
2. more
3. less
4. telnet
5. curses library
6. ncurses library

# 参考书籍
1. Strang, J. 1986. Programming with Curses. O’Reilly, Sebastopol, California.
2. Strang, J., Mui, L., and O’Reilly, T. 1988. Termcap & Terminfo (3rd edition). O’Reilly, Sebastopol, California.
3. https://tldp.org/HOWTO/Serial-HOWTO.html
4. https://tldp.org/HOWTO/Text-Terminal-HOWTO.html
5. Serial Programming Guide for POSIX Operating Systems by Michael R. Sweet
6. Stevens, W.R. 1992. Advanced Programming in the UNIX Environment. Addison-Wesley, Reading, Massachusetts.
