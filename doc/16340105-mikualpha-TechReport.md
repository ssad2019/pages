# 利用JsonWebToken对API进行无状态管理的分析

## 什么是JsonWebToken
根据维基百科的定义，JSON WEB Token（JWT），是一种基于JSON的、用于在网络上声明某种主张的令牌（token）。JWT通常由三部分组成: 头信息（header）, 消息体（payload）和签名（signature）。

头信息指定了该JWT使用的签名算法:

```
header = '{"alg":"HS256","typ":"JWT"}'
```
 
`HS256` 表示使用了 HMAC-SHA256 来生成签名。

消息体包含了JWT的意图：
```
payload = '{"loggedInAs":"admin","iat":1422779638}'//iat表示令牌生成的时间
``` 

未签名的令牌由`base64url`编码的头信息和消息体拼接而成（使用"."分隔），签名则通过私有的key计算而成：
```
key = 'secretkey'  
unsignedToken = encodeBase64(header) + '.' + encodeBase64(payload)  
signature = HMAC-SHA256(key, unsignedToken) 
```

最后在未签名的令牌尾部拼接上`base64url`编码的签名（同样使用"."分隔）就是JWT了：
```
token = encodeBase64(header) + '.' + encodeBase64(payload) + '.' + encodeBase64(signature) 
# token看起来像这样: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsb2dnZWRJbkFzIjoiYWRtaW4iLCJpYXQiOjE0MjI3Nzk2Mzh9.gzSraSYS8EXBxLN_oWnFSRgCzcmJmMjLiuyu5CSpyHI
```
    
JWT常常被用作保护服务端的资源（resource），客户端通常将JWT通过HTTP的`Authorization` header发送给服务端，服务端使用自己保存的key计算、验证签名以判断该JWT是否可信：
```
Authorization: Bearer eyJhbGci*...&lt;snip&gt;...*yu5CSpyHI
```

## JsonWebToken的具体优势
### 1.JsonWebToken的无状态特性易于水平扩展
在Cookie-Session方案中，Cookie内仅包含一个Session标识符，而用户信息、授权列表等都保存在服务端的Session中。

如果把Session中的认证信息都保存在JWT中，在服务端就没有Session存在的必要了。

此时，整个服务端都不需要对Session进行维护，所有的鉴权都通过JWT所属的算法来完成。当服务端水平扩展的时候，就不用处理Session复制（Session Replication）/ Session黏连（Sticky Session）或是引入外部Session存储了。

### 2.可防护CSRF攻击
跨站请求伪造[Cross-site request forgery](https://en.wikipedia.org/wiki/Cross-site_request_forgery)（简称CSRF）是一种典型的利用Cookie-Session漏洞的攻击，这里借用[spring-security](https://docs.spring.io/spring-security/site/docs/current/reference/html/csrf.html)的一个例子来解释CSRF：

假设你经常使用`bank.example.com`进行网上转账，在你提交转账请求时`bank.example.com`的前端代码会提交一个HTTP请求:
```
    POST /transfer HTTP/1.1
    Host: bank.example.com
    cookie: JsessionID=randomid; Domain=bank.example.com; Secure; HttpOnly
    Content-Type: application/x-www-form-urlencoded

    amount=100.00&amp;routingNumber=1234&amp;account=9876
```
如果图方便没有登出`bank.example.com`，随后又访问了一个恶意网站，该网站的HTML页面包含了这样一个表单：
```
    &lt;form action="https://bank.example.com/transfer" method="post"&gt;
        &lt;input type="hidden" name="amount" value="100.00"/&gt;
        &lt;input type="hidden" name="routingNumber" value="evilsRoutingNumber"/&gt;
        &lt;input type="hidden" name="account" value="evilsAccountNumber"/&gt;
        &lt;input type="submit" value="点击就送!"/&gt;
    &lt;/form&gt;
```
此时当你点了提交按钮时你已经向攻击者的账号转了100元。现实中的攻击可能更隐蔽，恶意网站的页面可能使用Javascript自动完成提交。尽管恶意网站没有办法盗取你的Session Cookie（从而假冒你的身份），但恶意网站向bank.example.com发起请求时，你的cookie会被自动发送过去。

因此，前端代码将JWT通过HTTP header发送给服务端（而不是通过cookie自动发送）可以有效防护CSRF。

## JsonWebToken的缺点
### 1.更多的空间占用
如果将原存在服务端session中的各类信息都放在JWT中保存在客户端，可能造成JWT占用的空间变大，需要考虑cookie的空间限制等因素，如果放在Local Storage，则可能受到XSS攻击。

### 2.更不安全
这里是特指将JWT保存在Local Storage中，然后使用Javascript取出后作为HTTP header发送给服务端的方案。在Local Storage中保存敏感信息并不安全，容易受到跨站脚本攻击，跨站脚本（Cross site script，简称XSS）是一种“HTML注入”，由于攻击的脚本多数时候是跨域的，所以称之为“跨域脚本”，这些脚本代码可以盗取cookie或是Local Storage中的数据。

并且，我们注销时可以做到的仅仅是清空客户端的Cookie，这样用户访问时就不会携带 JWT，服务端就认为用户需要重新登录。这是一个典型的假注销，对于用户表现出退出的行为，实际上这个时候携带对应的 JWT 依旧可以访问系统。 

### 3.无法作废已颁布的令牌
所有的认证信息都在JWT中，由于在服务端没有状态，即使你知道了某个JWT被盗取了，你也没有办法将其作废。在JWT过期之前（你绝对应该设置过期时间），你无能为力。

### 4.不易应对数据过期
与上一条类似，JWT有点类似缓存，由于无法作废已颁布的令牌，在其过期前，你只能忍受“过期”的数据。

### 5.无法应对单端登录问题
由于签名算法的限制问题，我们无从得知目前有多少个客户端正在登录某个账户，同样地，也无法将已上线的客户端强行退出。

## 应对JsonWebToken缺点的方法
### 1.尽可能完善其它地方的安全性
为了防止JWT的泄露，我们可以使用 https 加密应用，返回 JWT 给客户端时设置 httpOnly=true 并且使用 Cookie 而不是 Local Storage 存储 JWT，这样可以防止 XSS 攻击和 CSRF 攻击。

### 2.将用户密钥作为加密密钥的一部分
我们在JWT所用加密密钥中加入用户密码密文的全部或一部分，这样当用户修改密码时，用户密码的密文也随之改变(如果用SHA1等哈希算法)，从而导致已经颁发的JWT无法通过验证，加强了一定的安全性。

### 3. 刷新JWT
#### (1) 每次请求刷新 JWT
JWT 修改 payload 中的 exp 后整个 JWT 串就会发生改变，那…就让它变好了，每次请求都返回一个新的 JWT 给客户端。

但是这样太暴力了，不用我赘述这样做是多么的不优雅，以及带来的性能问题。但，至少这是最简单的解决方案。

#### (2) 只有快要过期的时候刷新 JWT 
一个上述方案的改造点是，只在最后的几分钟返回给客户端一个新的 JWT。

但这样做，触发刷新 JWT 基本就要看运气了，如果用户恰巧在最后几分钟访问了服务器，触发了刷新，万事大吉；如果用户连续操作了 27 分钟，只有最后的 3 分钟没有操作，导致未刷新 JWT，无疑会令用户抓狂。 

### 4. 完善Token机制
#### (1) 完善 RefreshToken 
借鉴 OAuth2 的设计，返回给客户端一个 RefreshToken，允许客户端主动刷新 JWT。一般而言，JWT 的过期时间可以设置为数小时，而 RefreshToken 的过期时间设置为数天。

该方案的可行性是存在的，但是为了解决 JWT 的续签把整个流程改变了，为什么不考虑下 OAuth2 的 password 模式和 client 模式呢？ 

#### (2) 使用 Redis 记录独立的过期时间 
为了解决续签问题，我们可以引入Redis，通过维护一张黑名单，用户注销、修改密码时加入黑名单(签名)，过期时间与 JWT 的过期时间保持一致。这样可以有效解决会话管理不易的问题。