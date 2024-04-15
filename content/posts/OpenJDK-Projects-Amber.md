+++
title = 'OpenJDK Project Amber'
date = 2023-12-15T20:28:59+08:00
+++
Amber 项目的目标是探索和孵化较小的、面向生产环境的 Java 语言特性。这些特性已经作为候选 JEP 被接受。
<!--more-->
Amber 项目相关的特性在成为 Java 官方平台的一部分之前，要通过两轮的预览测试。对于一个特性，每轮预览测试和最终的标准化都有单独的提案。

下文分别描述了 Amber 项目相关的每个特性。
1. [JEP 430: 模板字符串](https://openjdk.org/jeps/430)

# JEP 430: 模板字符串
## STR 模板处理器
### 示例代码
``` Java
import java.util.Date;

public class Main {
    private static class Req {
        private Date date;
        private String ip;
        private String path;

        public Req(Date date, String ip, String path) {
            this.date = date;
            this.ip = ip;
            this.path = path;
        }

        public String getPath() {
            return path;
        }
    }

    public static void main(String[] args) {
        // 1. Embedded expressions can be strings.
        String firstName = "Bill";
        String lastName = "Duck";

        String fullName = STR."\{firstName} \{lastName}";
        System.out.println(fullName);

        // 2. Embedded expressions can perform arithmetic
        int x = 10, y = 20;
        String s = STR."\{x} + \{y} = \{x + y}";
        System.out.println(s);

        // 3. Embedded expressions can invoke methods and access fields
        Req req = new Req(new Date(), "192.168.1.1", "/index.html");
        String t = STR."Access \{req.getPath()} at \{req.date} from \{req.ip}";
        System.out.println(t);
    }
}
```
### 运行结果
``` Shell
[optor666.github.io]$javac --enable-preview -source 21 Main.java
Note: Main.java uses preview features of Java SE 21.
Note: Recompile with -Xlint:preview for details.
[optor666.github.io]$java --enable-preview Main
Bill Duck
10 + 20 = 30
Access /index.html at Fri Dec 15 11:47:42 CST 2023 from 192.168.1.1
[optor666.github.io]$
```

# 参考
1. https://openjdk.org/projects/amber/
2. https://blogs.oracle.com/java/post/the-arrival-of-java-21
