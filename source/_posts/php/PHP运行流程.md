---
title: PHP运行流程
date: 2020-11-03 14:42:59
tags: runtime
categories: php
---
> 命名空间用一句话说，就是：把 类、函数、变量 等放到逻辑子文件夹中去，以避免命名冲突。
PSR-4 命名空间规范约定了 PHP 类的命名空间应该和实际在文件系统中的位置一致，而现实中绝大多数 PHP 框架为了方便都采纳了这条规范，最明显的就是 Laravel 4 到 5 的转变。在这种情况下，我发现不少新手又迷茫了，错误地理解了我在上文中的阐述的“路径”的概念。基于此我要简单讲述一下 PHP 运行的基本流程，我相信看完你们就不会再有上面的误解了。
### PHP 运行流程
在一个典型的 Apache +  mod_php 架构的 PHP 运行环境中，一个 PHP 网站是这样运行的：
```
     1.    Apache 收到用户的 HTTP 请求
     2.    这个请求是以 .php 结尾或者是一个不存在的路径（.htaccess 会将其转发到 index.php）
     3.    Apache 的 mod_php 会启动一个新的 PHP 进程（PHP 解释器），读取 HTTP 请求的 URL 中的那个 .php 文件或者 index.php
     4.    被读取进 PHP 解释器的字符串被按照 PHP 的语法进行解析。为了方便理解，我们将这些经过解析的字符串所生成的 context（上下文）命名为 Matrix
     5.    然后 PHP 解释器会根据从 Matrix 中解析出的特定 PHP 语句（如 require）载入其他 PHP 文件，并将其内容以字符串的形式加入 Matrix
     6.    最终 Matrix 变成一个数万行代码的巨型上下文（为了便于理解可以想象成巨长的代码文件字符串），PHP 解释器会按照 PHP 语法执行 Matrix，进行数据库连接、网络请求、文件读写等操作
     7.    每一次的 echo 都会被写入到输出缓冲区，最终这个巨长的代码字符串被执行完毕，PHP 进程退出内存
     8.    缓冲区中就是要发给用户的 HTTP response，其实就是一堆字符串，只不过它遵守 HTML 规范，可以被浏览器解析。这一堆字符串被 Apache 发送回用户的浏览器，浏览器渲染，用户看到内容
```