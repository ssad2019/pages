# REST API 设计规范
### 一、REST 简介

REST（Representational State Transfer，表现层状态转化）是 [Roy Thomas Fielding](https://link.jianshu.com?t=https://en.wikipedia.org/wiki/Roy_Fielding) 在2000年他的博士论文[《Architectural Styles and the Design of Network-based Software Architectures》](https://link.jianshu.com?t=http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)中提出的一个描述互联系统架构风格的名词。让我们先去理解Representational State Transfer这个词组到底是什么意思？Web 本质上由各种各样的资源组成，资源由 URI 唯一标识。浏览器（或者任何其它类似于浏览器的应用程序）将展示出该资源的一种表现方式，或者一种表现状态。如果用户在该页面中定向到指向其它资源的链接，则将访问该资源，并表现出它的状态。"表现层"（Representation）其实指的是"资源"（Resources）的"表现层"，这意味着客户端应用程序随着每个资源表现状态的不同而发生状态转移，也即所谓 REST。

综合上面的解释，我们总结一下什么是RESTful架构：

> （1）每一个URI代表一种资源；
> 
> （2）客户端和服务器之间，传递这种资源的某种表现层；
> 
> （3）客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。

* * *

### 二、HTTP动词

对于资源的具体操作类型，由HTTP动词表示。REST设计中最常见的一种设计错误，就是URI中包含动词。因为"资源"表示一种实体，所以应该是名词，URI不应该有动词，动词应该放在HTTP协议中。

举例来说，某个URI是/show/book/1，其中show是动词，这个URI就设计错了，正确的写法应该是/book/1，然后用GET方法表示show。

> GET /transaction HTTP/1.1
> 
> Host: 127.0.0.1/book/1

安全性和幂等性的一些介绍：

> **安全性**：不会改变资源状态，可以理解为只读的；
> 
> **幂等性**：执行1次和执行N次，对资源状态改变的效果是等价的。幂等在HTTP/1.1中定义如下：
> 
> Methods can also have the property of "idempotence" in that (aside from error or expiration issues) the side-effects of N &gt; 0 identical requests is the same as for a single request. 如今鲜有人在撰写REST API时，简单说来就是一个操作符合幂等性，那么相同的数据和参数下，执行一次或多次产生的效果（副作用）是一样的。

常用的HTTP动词有下面五个（括号里是对应的SQL命令）。

> GET（SELECT）：从服务器取出资源（一项或多项），此操作是安全的，幂等的。
> 
> POST（CREATE）：在服务器新建一个资源。
> 
> PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源），此操作是幂等的。
> 
> PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性），此操作是幂等的。
> 
> DELETE（DELETE）：从服务器删除资源，此操作是幂等的。

还有两个不常用的HTTP动词。
> HEAD：获取资源的元数据。
> 
> OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

下面是一些例子。
> GET /books：列出所有书籍信息
> 
> POST /books：新建一个图书馆
> 
> GET /book/id：根据唯一标识获取某个指定图书的信息
> 
> PUT /book/id：更新某个指定图书的信息（并返回该图书的全部信息）
> 
> PATCH /book/id：更新某个指定图书的信息（并返回该图书的全部信息）
> 
> DELETE /book/id：删除某本图书

* * *

### 三、状态码

服务器向用户返回的状态码和提示信息。作为 API 的设计者，正确的将 API 执行结果和失败原因用清晰简洁的方式传达给客户程序是十分关键的一步。 我们确实可以在 HTTP 的相应内容中描述是否成功，如果出错是因为什么。因此，HTTP 响应代码可以保证客户端在第一时间用最高效的方式获知 API 运行结果，并采取相应动作。 下面列出了比较常用的响应代码。

#### 请求成功

> 200 **OK**: 请求执行成功并返回相应数据，如GET成功
> 
> 201 **Created**: 对象创建成功并返回相应资源数据，如POST成功；创建完成后响应头中应该携带头标Location，指向新建资源的地址
> 
> 202 **Accepted**: 接受请求，但无法立即完成创建行为，比如其中涉及到一个需要花费若干小时才能完成的任务。返回的实体中应该包含当前状态的信息，以及指向处理状态监视器或状态预测的指针，以便客户端能够获取最新状态。
> 
> 204 **No Content**: 请求执行成功，不返回相应资源数据，如PATCH，DELETE成功

#### 重定向
> 重定向的新地址都需要在响应头Location中返回
> 
> 301 **Moved Permanently**: 被请求的资源已永久移动到新位置
> 
> 302 **Found**: 请求的资源现在临时从不同的 URI 响应请求

#### 条件请求
> 304 **Not Modified**: 资源自从上次请求后没有再次发生变化，主要使用场景在于实现[数据缓存](https://link.jianshu.com?t=https://github.com/bolasblack/http-api-guide#user-content-%E6%95%B0%E6%8D%AE%E7%BC%93%E5%AD%98)
> 
> 409 **Conflict**: 请求操作和资源的当前状态存在冲突。主要使用场景在于实现[并发控制](https://link.jianshu.com?t=https://github.com/bolasblack/http-api-guide#user-content-%E5%B9%B6%E5%8F%91%E6%8E%A7%E5%88%B6)

#### 客户端错误
> 400 **Bad Request**: 请求体包含语法错误
> 
> 401 **Unauthorized**: 需要验证用户身份，如果服务器就算是身份验证后也不允许客户访问资源，应该响应403 Forbidden。
> 
> 403 **Forbidden**: 服务器拒绝执行
> 
> 404 **Not Found**: 找不到目标资源
> 
> 405 **Method Not Allowed**: 不允许执行目标方法，响应中应该带有Allow头，内容为对该资源有效的 HTTP 方法
> 
> 406 **Not Acceptable**: 服务器不支持客户端请求的内容格式，但响应里会包含服务端能够给出的格式的数据，并在Content-Type中声明格式名称
> 
> 410 **Gone**: 被请求的资源已被删除，只有在确定了这种情况是永久性的时候才可以使用，否则建议使用404 Not Found
> 
> 413 **Payload Too Large**:POST或者PUT请求的消息实体过大
> 
> 415 **Unsupported Media Type**: 服务器不支持请求中提交的数据的格式

#### 服务端错误
> 500 **Internal Server Error**: 服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。
> 
> 501 **Not Implemented**: 服务器不支持当前请求所需要的某个功能。
> 
> 502 **Bad Gateway**: 作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。
> 
> 503 **Service Unavailable**: 由于临时的服务器维护或者过载，服务器当前无法处理请求。这个状况是临时的，并且将在一段时间以后恢复。如果能够预计延迟时间，那么响应中可以包含一个Retry-After头用以标明这个延迟时间（内容可以为数字，单位为秒；或者是一个[HTTP 协议指定的时间格式](https://link.jianshu.com?t=http://tools.ietf.org/html/rfc2616#section-3.3)）。如果没有给出这个Retry-After信息，那么客户端应当以处理 500 响应的方式处理它。
> 
> 501与405的区别是：405是表示服务端不允许客户端这么做，501是表示客户端或许可以这么做，但服务端还没有实现这个功能

* * *

### 四、Hypermedia API

RESTful API最好做到Hypermedia（超媒体），即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。比如，当用户向api.example.com的根目录发出请求，会得到这样一个文档。
> {"link": {
> 
> "rel":   "collection https://www.example.com/books",
> 
> "href":  "https://api.example.com/books",
> 
> "title": "List of books",
> 
> "type":  "application/vnd.yourformat+json"
> 
> }}

上面代码表示，文档中有一个link属性，用户读取这个属性就知道下一步该调用什么API了。rel表示这个API与当前网址的关系（collection关系，并给出该collection的网址），href表示API的路径，title表示API的标题，type表示返回类型。Hypermedia API的设计被称为HATEOAS。Github的API就是这种设计，访问api.github.com会得到一个所有可用API的网址列表。
> {
> 
> "current_user_url": "https://api.github.com/user",
> 
> "authorizations_url": "https://api.github.com/authorizations",
> 
> // ...
> 
> }

从上面可以看到，如果想获取当前用户的信息，应该去访问api.github.com/user，然后就得到了下面结果。
> {
> 
> "message": "Requires authentication",
> 
> "documentation_url": "https://developer.github.com/v3"
> 
> }

上面代码表示，服务器给出了提示信息，以及文档的网址。

* * *

### 五、URI规范
> 1、不用大写，不用中文；
> 
> 2、用中杠-不用下杠_；
> 
> 3、参数列表要encode；
> 
> 4、URI中的名词表示资源集合，使用复数形式。

避免URI层级过深，/在url中表达层级，用于按实体关联关系进行对象导航，一般根据id导航。过深的导航容易导致url膨胀，不易维护，如GET /zoos/1/areas/3/animals/4，尽量使用查询参数代替路径中的实体导航，如GET /animals?zoo=1&amp;area=3；参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。下面是一些常见的参数。
> ?limit=10：指定返回记录的数量
> 
> ?offset=10：指定返回记录的开始位置。
> 
> ?page=2&amp;per_page=100：指定第几页，以及每页的记录数。
> 
> ?sortby=name&amp;order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
> 
> ?animal_type_id=1：指定筛选条件

URI表示资源的两种方式：资源集合、单个资源。

#### 资源集合：
> /zoos                         //所有动物园
> 
> /zoos/1/animals        //id为1的动物园中的所有动物

单个资源：
> /zoos/1                //id为1的动物园
> 
> /zoos/1;2;3          //id为1，2，3的动物园

### 六、其它事项

#### 域名相关

应该尽量将API部署在专用域名之下。

> https://api.example.com

如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下,但不建议您这么做。
> https://www.example.com/api/

#### 版本相关

另一个设计误区，就是在URI中加入版本号：

> http://www.example.com/app/1.0/foo
> 
> http://www.example.com/app/2.0/foo

因为不同的版本，可以理解成同一种资源的不同表现形式，所以应该采用同一个URI。版本号可以在HTTP请求头信息的Accept字段中进行区分，缺点是将版本号放在HTTP头信息中，但不如放入URL方便和直观（参见[Versioning REST Services](https://link.jianshu.com?t=http://www.informit.com/articles/article.aspx?p=1566460)）：
> Accept: vnd.example-com.foo+json; version=1.0
> 
> Accept: vnd.example-com.foo+json; version=2.0

#### 错误处理

如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可。

> {error:"Invalid API key"}