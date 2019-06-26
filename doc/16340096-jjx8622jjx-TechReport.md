# 初探PHP

## PHP基本描述
---
PHP（全称：PHP：Hypertext Preprocessor，即“PHP：超文本预处理器”）是一种开源的通用计算机脚本语言，尤其适用于网络开发并可嵌入HTML中使用。PHP的语法借鉴吸收C语言、Java和Perl等流行计算机语言的特点，易于一般程序员学习。PHP的主要目标是允许网络开发人员快速编写动态页面，但PHP也被用于其他很多领域。

## PHP的四大特性
---
1. PHP 独特的语法混合了 C、Java、Perl 以及 PHP 自创新的语法。
2. PHP可以比CGI或者Perl更快速的执行动态网页——动态页面方面，与其他的编程语言相比，PHP是将程序嵌入到HTML文档中去执行，执行效率比完全生成htmL标记的CGI要高许多；PHP具有非常强大的功能，所有的CGI的功能PHP都能实现。
3. PHP支持几乎所有流行的数据库以及操作系统。
4. 最重要的是PHP可以用C、C++进行程序的扩展！

## PHP的八大优势
---
1. 开放源代码，所有的PHP源代码事实上都可以得到。
2. 免费性，php和其它技术相比，PHP本身免费且是开源代码。
3. 快捷性，程序开发快，运行快，技术本身学习快。嵌入于HTML：因为PHP可以被嵌入于HTML语言，它相对于其他语言。编辑简单，实用性强，更适合初学者。
4. 跨平台性强，由于PHP是运行在服务器端的脚本，可以运行在UNIX、LINUX、WINDOWS、Mac OS下。
5. 效率高PHP消耗相当少的系统资源。
6. 图像处理，用PHP动态创建图像,PHP图像处理默认使用GD2。且也可以配置为使用image magick进行图像处理。
7. 面向对象，在php4,php5 中，面向对象方面都有了很大的改进，php完全可以用来开发大型商业程序。
8. 专业专注，PHP支持脚本语言为主，同为类C语言。

## PHP内嵌函数构建数据库
---
1. 初始化数据库连接配置，与其他语言类似，PHP同样支持嵌入式mysql操作，通过`mysqli()`函数绑定对应地址对应名字的库。
```
function initConnection()
{
    $mysql = new mysqli(DB_HOST, DB_USER, DB_PASS, DB_NAME);
    if ($mysql->connect_error) die($mysql->connect_error);
    return $mysql;
}
```
2. 基于底层的C语言结构，PHP同样支持在`query()`语句传入SQL语言进行建表。
```
 $mysql->query('CREATE TABLE IF NOT EXISTS user (
        id INTEGER AUTO_INCREMENT PRIMARY KEY,
        username VARCHAR(32) UNIQUE NOT NULL,
        passwd VARCHAR(255) NOT NULL,
        nickname VARCHAR(255) DEFAULT \'\',
        description VARCHAR(255) DEFAULT \'\',
        avatar VARCHAR(255) DEFAULT \'\'
    ) DEFAULT CHARSET = utf8');
```
3. 绑定查询结果，PHP将内嵌SQL语句的查询操作封装成固定格式的对象，通过调用对象的`bind_param()`成员函数来传入查询变量，之后保存查询结果并设置指示位指向结果集中的第一条信息，最后`bind_result()`用于将对象中指示位指向的当前条目结果按序与变量绑定，及通过调用`fetch()`函数将指示位移动到指向结果集中的下一条信息。
```
$mysql = initConnection();
    $stmt = $mysql->prepare("SELECT passwd FROM user WHERE username = ?");
    $stmt->bind_param("s", $user);
    $stmt->execute();
    $stmt->store_result();

    if ($stmt->num_rows <= 0) return false;

    $stmt->bind_result($passwd_hash);
    $stmt->fetch();

    $stmt->close();
    $mysql->close();

    return $passwd_hash;
```

## 部分小知识点
---
* PHP中`isset()`函数用于判定某个变量是否被设置，无法检验变量内容是否为空，而`empty()`函数则用于判定某个变量中的内容是否为空。
* PHP中`empty()`因为对于变量判定的要求是不为0的其余符号，因此当我们传入0参数时，`empty(0)`会判定为真。
* PHP中对于数组的格式没用特别严格的要求，任何的对象或基本类型都可以放入到同一个数组中变成一个集合，并且支持键值格式的存入，在结构上十分类似于json，这也为我们编码或解码json格式文件的时候提供了巨大的便利。