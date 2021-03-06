# API说明文档
PS：域名（即API前缀）应以宏定义/常量的方式统一管理，便于修改，如：`#define API_WEBSITE http://example.com`

PS2：后续可能API会删除.php后缀，希望届时注意。

## 登录相关
### 登录

```
POST /login.php
```

输入参数:  

```
username: 用户名
password: 用户名对应密码(明文即可)
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": {
        "token": "xxxxxxxxxxxxxxxxxxxxxxxxxx" //返回Token，仅在status为200时包含此字段
    }
}
```

输出状态码一览：

```
200: 正常，并返回Token
400: 请求方式或传参错误
403: 用户名或密码错误
```

其它说明：

```
返回的Token应当在后续请求中加入到HTTP请求头部的Authorization字段中。
```

### 注册
```
POST /register.php
```

输入参数：

```
username: 用户名(应为字母开头，不含大写字母，4-20字节)
password: 用户名对应密码(应为8-20字节，仅含大小写字母、数字、下划线(_)，明文即可)
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": {
        "token": "xxxxxxxxxxxxxxxxxxxxxxxxxx" //返回Token，仅在status为200时包含此字段
    }
}
```

输出状态码一览：

```
200: 注册成功，并返回Token
400: 请求方式或传参错误
403: 用户名已存在
404: 服务器链接错误，请稍后再试
1001: 用户名长度过短
1002: 密码长度过短
1003: 用户名长度过长
1004: 密码长度过长
1005: 用户名格式非法
1006: 密码格式非法
```

其它说明：

```
返回的Token应当在后续请求中加入到HTTP请求头部的Authorization字段中。
```

## 商家相关
### 设置商家图标
```
POST /store/icon.php
```

输入参数：

```
图片文件，仅支持JPG与PNG格式，设置Content-Type为image/png或image/jpeg。
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": {
        "url": "http://example.com/xxx/xxxx.jpg" //服务器保存的图片路径（服务器保存图片，生成地址，并将其与当前登录用户绑定）
    }
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 获取商家信息(Web前端)
```
GET /store/info.php
```

输入参数：

```
无
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": {
    	"name": "SCBOY", //商家名称
    	"description": "孙一峰永远是我大哥！", //商家描述
    	"icon": "http://example.com/xxx/xxx.jpg" //商家头像
    }
}
```

输出状态码:

```
200: OK
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 更改商家信息

```
POST /store/info.php
```

输入参数：

```
name: 商家昵称
description: 商家描述
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK" //状态码描述
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 生成点餐链接
```
POST /store/link.php
```

输入参数：

```
site: 座位号，可为字母与数字组合的字符串
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": {
    	"link": "https://example.com/xxx/xxx/xxx" //点餐链接，前端根据此链接生成二维码
    }
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```


## 商品相关
### 获取商品列表
```
GET /store/food_list.php
```

输入参数：

```
无
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": [
        {
            "id": 123, //商品编号
            "type_id": "123", //商品分类对应的id
            "name": "小炒肉", //商品名称
            "price": 20, //商品价格
            "description": "美味便宜，店家招牌菜", //商品描述
            "icon": "http://example.com/xxx/xxxx.jpg" //商品图片
        },
        ...
    ]
}
```

输出状态码:

```
200: OK
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 获取商品分类
```
GET /store/type_list.php
```

输入参数：

```
无
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data":[
        {
            "id": 123, //分类编号
            "name": "盖浇饭", //分类名称
        },
        ...
    ]
}
```

输出状态码:

```
200: OK
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```


### 添加商品分类
```
PUT /store/type.php
```

输入参数:

```
typename: 分类名称
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK" //状态码描述
    "data": {
        "id": 123 //添加分类对应的编号
    }
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 删除商品分类
```
DELETE /store/type.php
```

输入参数

```
typeid: 分类编号
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK" //状态码描述
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 上传商品图片

```
POST /store/food_icon.php
```

输入参数：

```
图片文件，仅支持JPG与PNG格式，设置Content-Type为image/png或image/jpeg。
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": {
        "url": "http://example.com/xxx/xxxx.jpg" //服务器保存的图片路径（服务器仅保存图片并生成地址，不会与对应商品进行绑定，需在“添加商品”时将此链接一并上传）
    }
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 添加商品
```
PUT /store/food.php
```

输入参数:

```
typeid: 商品分类id(从已有的分类中选取,传入对应分类编号)
foodname: 商品名称
price: 商品价格
description: 商品描述
imgurl: 商品图标对应的服务器保存的图片路径
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK" //状态码描述
    "data": {
        "id": 123 //添加商品对应的编号
    }
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 修改商品
```
POST /store/food.php
```

输入参数:

```
foodid: 商品id(从数据库中选中需要修改的商品)
typeid: 商品新的分类的id(有修改就是新的，没修改就是原本的，下同，而且只能选取已有分类)
foodname: 商品新的名称
price: 商品新的价格
description: 商品新的描述
imgurl: 商品新的图标对应的服务器保存的图片路径
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK" //状态码描述
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 删除商品
```
DELETE /store/food.php
```

输入参数:

```
foodid: 商品编号
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK" //状态码描述
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

## 订单相关
### 获取订单列表（打开页面时调用）
```
GET /store/order_list.php
```

输入参数:

```
count: 按时间倒序获取的订单数量
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": [
        {
            "id": 123123123, //订单编号
            "time": "2019-01-01 00:00:00", //下单时间
            "status": false //订单状态，false为未完成，true为已完成
        },
        ...
    ]
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 差量获取订单列表
```
GET /store/order_list.php
```

输入参数:

```
offset: 订单编号，只会获取该订单号后的订单
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": [
        {
            "id": 123123123, //订单编号
            "time": "2019-01-01 00:00:00", //下单时间
            "status": false //订单状态，false为未完成，true为已完成
        },
        ...
    ]
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 获取订单详细信息
```
GET /store/order_detail.php
```

输入参数:

```
id: 订单编号
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": {
        "id": 123123123, //订单ID
        "status": false, //订单状态，false为未完成，true为已完成
        "site": 4, //座位号
        "content": [ //订单内容
            {
                "id": 123456, //商品ID
                "name": "香辣小龙虾", //商品名
                "icon": "https://example.com/xxx/xxx.jpg", //商品图标
                "price": 120, //单价
                "number": 1 //数量
            },
            ...
        ],
        "remark": "多放辣"
    }
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```

### 设置订单状态
```
POST /store/order_status.php
```

输入参数:

```
id: 订单编号
status: 要设置的订单状态，0为未完成，1为已完成
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK" //状态码描述
}
```

输出状态码:

```
200: OK
400: 输入参数错误
401: 授权验证失败，可能Token已经过期或无效(应跳转至登录界面)
```


## 点餐相关
点餐客户端仅需关注此部分接口，此部分接口无需鉴权（登录/注册）。

### 获取商家信息
此方法通过扫描二维码形式请求。

```
GET /order.php
```

输入参数:

```
s: 商家ID(二维码对应链接中包含)
n: 座位号(二维码对应链接中包含)
secret: APP标识码
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": {
    	"name": "SCBOY", //商家名称
    	"description": "孙一峰永远是我大哥！", //商家描述
    	"icon": "http://example.com/xxx/xxx.jpg", //商家头像
    	"goods": [ //商品列表
            {
                "id": 123456, //商品ID
                "name": "香辣小龙虾", //商品名
                "icon": "http://example.com/xxx/xxx.jpg", //商品图标
                "tag": { //分类
                    "id": 1, //分类ID
                    "name": "海鲜" //分类名
                },
                "price": 120 //单价
            },
            ...
    	]
    }
    
}
```

输出状态码:

```
200: OK
400: 输入参数错误
```

### 提交订单
```
POST /order.php
```

输入参数:

```
s: 商家ID(二维码对应链接中包含)
n: 座位号(二维码对应链接中包含)
order: 订单内容(JSON格式)
```

订单内容格式: 

```
{
    "content": [ //订单内容
        {
            "id": 123456, //商品ID
            "number": 1 //数量
        },
        ...
    ],
    "remark": "多放辣" //商品备注
}
```

输出(JSON格式):

```
{
    "status": 200, //返回状态码
    "msg": "OK", //状态码描述
    "data": {
    	"order_id": "201901010000231313", //订单ID
    	"time": "2019-01-01 01:00:00" //下单时间
    }
}
```

输出状态码:

```
200: OK
400: 输入参数错误
```


