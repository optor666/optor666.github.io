+++
title = '2024 阅读 Redis 3.0 源代码：准备调试环境'
date = 2024-03-15T19:45:58+08:00
+++
2024 年，Redis 已经到了 7.x 的版本了，但是我参考了书籍《Redis 设计与实现》，所以决定使用 3.0 版本的源代码。
<!--more-->
我日常开发使用的是 Apple M2 芯片的苹果笔记本，但是在编译 3.0 分支代码时遇到了一些涉及芯片架构层面的不兼容问题，毕竟 3.0 分支最后一次代码提交时间是 2017 年，当时还没有 Apple M 系列的芯片吧？

Apple M2 芯片属于 ARM 架构，我仅仅学习过一点 X86 芯片架构的知识，对 ARM 架构知之甚少，所以在折腾半小时仍不能解决不兼容问题后，选择开一台使用 X86 芯片的腾讯云云服务器来编译调试代码。

# 云服务器
## 系统信息
```
Ubuntu 22.04 LTS

Linux VM-16-12-ubuntu 5.15.0-88-generic #98-Ubuntu SMP Mon Oct 2 15:18:56 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
```

## 编译
Redis 信奉简单的哲学，编译非常简单，直接在源代码根目录执行 `make` 命令即可。但是想要调试的话，需要使用命令 `make CFLAGS="-g -O0"`，在编译的时候生成调试信息。

## 调试
习惯了使用功能完善的 IDE 做开发，在终端中使用 gdb 调试感觉挺不适应的，所以接下来在本地使用 IDE 进行远程访问和调试源代码。

# 本地 macOS
## CLion
平时主要是使用 JetBrains 全家桶，所以首先选择了 CLion。但是，使用之后发现 CLion 需要远程机器至少 4G 内存，而我的云服务器的配置是 2C2G，硬着头皮试了一下，结果直接把云服务器卡死了半天，最终也没成功，只能放弃 CLion 了。

## VSCode
VSCode 需要先安装 Remote-SSH 扩展，然后连接云服务器，编译、调试流畅的一批，VSCode 万岁！

### VSCode 配置
VSCode 编译、调试源代码需要两个配置文件配置编译命令和要调试的对象，它们是：

1. launch.json
``` Json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "C Debug",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/src/redis-server",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "make build"
        }
    ]
}
```

2. tasks.json
```Json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "make build",
            "type": "shell",
            "command": "make",
            "args": [
                "CFLAGS=\"-g -O0\""
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```
