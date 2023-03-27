###  帐户系统

#### 1. 验证码生成
1.1 接口
`https://api.goowee.cn/v1/users/code`

1.2  请求方式
POST

1.3 输入
```
{"username": "13611778890", "type": 1}
```
字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
username | string | required | 用户名称
type|integer | required | 短信类型 值为：1:登录与注册，2:修改帐号验证码，3，4，5，6
```
type 可能的值说明：
1     :   "身份验证验证码"
2     :   "修改帐号验证码"
4     :   "帐号注销验证码"
```

1.4  输出
```
{
    "code":0,
    'message': "OK"
}
'message': "参数错误"
'message': "验证码发送过于频繁"
'message': "新用户不能找回密码，请选择其他登录方式"
```
1.5. 接口测试
```
# 1. 线上
curl -i -X POST \
-d "username=13611778890&type=1" \
https://api.goowee.cn/v1/users/code

```

#### 1. 验证码验证
1.1 接口
`https://api.goowee.cn/v1/users/check/code`

1.2  请求方式
POST

1.3 输入
```
{"username": "13611778890", "type": 2, "code": 1221}
```
字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
username | string | required | 用户名称
type|integer | required | 短信类型 值为：1:登录与注册，2:修改帐号验证码，3，4，5，6
code | integer | required | 验证码
```
type 可能的值说明：
1     :   "身份验证验证码"
2     :   "修改帐号验证码"
```

1.4  输出
```
{
    "code":0,
    'message': "OK"
	"data": {
		"check": 1 or 0
	}
}
check 1 通过
check 0 不通过
```
1.5. 接口测试
```
# 1. 线上
curl -i -X POST \
-d "username=13611778896&type=2&code=1122" \
https://api.goowee.cn/v1/users/check/code
```

#### 2 用户登录+注册

2.1  接口
`https://api.goowee.cn/v1/users/signIn`
2.2  请求方式
POST

2.3 输入
```
# 登录1——验证码方式
{"username": "13611778890", code: "111111", visitorId: 'abcdefg'}

# 游客绑定手机时用
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
username | string | required | 用户名称
code  | string | required or option | 验证码
visitorId | string | option | 前端生成的随机Id（设备Id），用于确定哪个游客，androidId or idfa，游客注册专用


2.4. 输出
```
{
    "code":0,
    "message":"OK",
    "data":{
		"userInfo":{
			"userId":"740699410",
			"username":"13611778890",
			"nickname":"136******56",
			"visitorId": "abc",
			"gender":"0",
			"birthday":"",
			"keys":"0",
			"icon":"",
			"new":"0"
		},
		"token":"6c6b11b2-ee0b-4825-831b-f3d696677387",
		"expire":0
    }
}
```
- userInfo信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
visitorId  | string | required | 游客Id
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户

- token 为当前session信息
- expire session过期时间

2.5.  接口测试
```
1. 线上测试
curl -i -X POST \
-d "username=13611778890&code=2889" \
https://api.goowee.cn/v1/users/signIn

```

#### 21 华为用户登录+注册

2.1  接口
`https://api.goowee.cn/v1/users/hw/signIn`
2.2  请求方式
POST

2.3 输入
```
# 登录1——验证码方式
{code: "DwEEAPu+7D7Yw4h/g2C0ZzlOCGm4c0UM/ZX7soetFPs25uSRpprL...", visitorId: 'abcdefg'}

# 游客绑定手机时用
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
code  | string | required | 华为 authorization code
visitorId | string | option | 前端生成的随机Id（设备Id），用于确定哪个游客，androidId or idfa，游客注册专用


2.4. 输出
```
{
    "code":0,
    "message":"OK",
    "data":{
		"userInfo":{
			"userId":"740699410",
			"username":"13611778890",
			"nickname":"136******56",
			"visitorId": "abc",
			"gender":"0",
			"birthday":"",
			"keys":"0",
			"icon":"",
			"new":"0",
			"access_token": "",
		},
		"token":"6c6b11b2-ee0b-4825-831b-f3d696677387",
		"expire":0
    }
}
```
- userInfo信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
visitorId  | string | required | 游客Id
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户
access_token | requires | 登录验证码

- token 为当前session信息
- expire session过期时间

2.5.  接口测试
```
1. 线上测试
curl -i -X POST \
-d "code=authorization_code" \
https://api.goowee.cn/v1/users/hw/signIn

```

#### 20 用户注销

20.1  接口
`https://api.goowee.cn/v1/platform/logout`
20.2  请求方式
POST

20.3 输入
```
# 用户注销验证码——验证码方式
{"username": "13611778890", code: "111111", token: 'abcdefg'}

# 游客绑定手机时用
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
username | string | required | 用户名称
code  | string | required | 注销验证码
token | string | required | session 字串

20.4. 输出
```
{
    "code":0,
    "message":"OK",
}
# 异常
# "Token 不正确"
# "用户不存在"
# "验证码不正确"
```

20.5.  接口测试
```
1. 线上测试
curl -i -X POST \
-d "username=13611778890&code=2889&token=" \
https://api.goowee.cn/v1/platform/logout

```

#### 3 用户信息更新

3.1  接口
`https://api.goowee.cn/v1/users/update`

3.2  请求方式
POST

3.3 输入
```
{
	userId:740699410,
	token:,
	gender:1,
	birthday:2015-02-20,
	icon:29985757893291,
	nickname:cc,
	email:zh@163.com,
	phoneno:13822773391
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
token  | string | required | token
gender  | integer | option | 性别 1:男  2:女
birthday  | string | required or option | 验证码
icon  | string | option | 头像唯一Id 一般为当前时间戳
nickname  | string | option | 昵称
email  | string |  option | 邮箱
phoneno  | string | option | 绑定手机号

3.4. 输出
```
{
    "code":0,
    "message":"OK",
    "data":{
		"userInfo":{
			"userId":"740699410",
			"username":"13611778890",
			"nickname":"cc",
			"gender":"0",
			"birthday":"2015-02-20",
			"keys":"100",
			"icon":"129985757893291",
			"new":"0"
		}
    }
}
```

- userInfo信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户

3.5.  接口测试

```
1. 线上测试
# 更新用户数据
# nickname,birthday,gender,email,keys,phoneno,icon
curl -i -X POST \
-d "userId=740699410&token=&gender=1&birthday=2015-02-20&icon=129985757893291&nickname=cc&email=zh@163.com&phoneno=13822773391"  \
https://api.goowee.cn/v1/users/update

```


#### 4 主场景信息

4.1  接口
`https://api.goowee.cn/v1/scene/init`
4.2  请求方式
POST

4.3 输入
```
{
	userId:740699410,
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
token  | string | required | token

4.4. 输出
```
{
    "code":0,
    "message":"OK",
    "data":{
		"userInfo":{
			"userId":"740699410",
			"username":"13611778890",
			"nickname":"cc",
			"gender":"0",
			"birthday":"2015-02-20",
			"keys":"100",
			"icon":"129985757893291",
			"new":"0"
		},
		"readInfo":{
			"bookId":"880001",
			"readTime":"2021-09-18"
		},
		"listenInfo":{
			"audioId":"610001"
		},
		"unlockInfo":[
			"880001","880002","880003",...
		],
		"complete":[
			"880001","880002","880003",...
		],
		"settingInfo":{
			"readingBooks"	:	3 ,
			"updateTime"	:	1632732973
		},
		"configInfo":{
			"books":[
				{
				"bookId":"880001","name":"糊涂的小海獭",
				"labelId":["某某科",""]
				"desc":"","icon":"Icon_880001",
				"price":"0","unlock":"0","discount":"",
				"order":"1","show":"1"
				},
				...
			],
			"orders":[
				{
					"orderId":"18001","name":"1把钥匙",
					"icon":"Icon_18001","prime":"5",
					"present":"3","discount":"7","keys":"1"
				},
			],
			"show":[
				"880001", "880002", "880003", ...
			]
		} ,
    }
}
```

- userInfo信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户

- readInfo 最新阅读信息说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
bookId | string |require | 最后阅读绘本Id
readTime | string | required | 最后阅读时间

- unlockInfo 解锁绘本信息为绘本Id列表

- complete 读完的绘本Id列表

- settingInfo 设置项信息说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
readingBooks | integer | required | 每天可阅读的书数量
updateTime | integer | required | 配置更新时间

- orders 商品配置信息说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
orderId | string |require | 商品Id
name | string | required | 商品名称
icon  | string | required | 商品图标
prime  | integer | required | 原价
present  | integer | required | 现价
discount  | integer | required | 折扣
keys  | integer | required | 获取钥匙数

- books 书籍配置信息说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
bookId | string |require | 绘本Id
name | string | required | 绘本名称
desc  | string | required | 绘本描述
icon  | string | required | 绘本图标
price  | integer | required | 钥匙价格
unlock  | integer | required | 解锁类型 0:免费解锁 1:钥匙解锁
discount  | integer | required | 折扣
order | integer | required | 货架排序
show | integer | required | 是否显示在大厅

4.5.  接口测试

```
1. 线上测试
# 场景初始化接口
curl -i -X POST \
-d "userId=532188470&token=" \
https://api.goowee.cn/v1/scene/init

```

#### 5 点击阅读绘本

5.1  接口
`https://api.goowee.cn/v1/books/click`
5.2  请求方式
POST

5.3 输入
```
{
	userId:740699410 ,
	bookId:880001 ,
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
bookId | string | required | 绘本Id
token  | string | required | token

5.4. 输出
```
{
    "code":0,
    "message":"OK"
}

# 异常
10000：参数不正常
10001：Token不正确
30001：绘本不存在

```

- userInfo信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户

5.5.  接口测试

```
1. 线上测试
# 场景初始化接口
curl -i -X POST \
-d "userId=532188470&bookId=880001&token=" \
https://api.goowee.cn/v1/books/click

```

#### 6 使用钥匙解锁

6.1  接口
`https://api.goowee.cn/v1/books/unlock`
6.2  请求方式
POST

6.3 输入
```
{
	userId:740699410 ,
	bookId:880001 ,
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
bookId | string | required | 绘本Id
token  | string | required | token

6.4. 输出
```
{
    "code":0,
    "message":"OK",
	"data":{
		"userInfo":{
			"userId":"740699410",
			"username":"13611778890",
			"nickname":"136******56",
			"gender":"0",
			"birthday":"",
			"keys":"0",
			"icon":"",
			"new":"0"
		},
		"unlockInfo" : [
			"880003",
			"880004",
			"880005"
		]
	}
}

# 异常
10000：参数不正常
10001：Token不正确
30002：绘本已经购买
30003：绘本是免费的
40011：钥匙不足

```

- userInfo信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户

- unlockInfo 解锁信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户

6.5.  接口测试

```
1. 线上测试
# 场景初始化接口
curl -i -X POST \
-d "userId=532188470&bookId=880001&token=" \
https://api.goowee.cn/v1/books/unlock

```



#### 7 支付发货接口

7.1  接口
`https://api.goowee.cn/v1/orders/deliver`
7.2  请求方式
POST

7.3 输入
```
{
	userId:740699410 ,
	purchaseInfo:{\"autoRenewing\":false,\"orderId\":\"202109241542584543abc77331.104738673\",\"packageName\":\"org.goowee.animalcastle\",\"applicationId\":104738673,\"kind\":0,\"productId\":\"18000\",\"productName\":\"1把钥匙测试\",\"purchaseTime\":1632469387000,\"purchaseTimeMillis\":1632469387000,\"purchaseState\":0,\"developerPayload\":\"FeeHuawei\",\"purchaseToken\":\"0000017c16c20cf10c9ce3c4d8a147de9e4862b9b97bb3496b85b53cf60ff92259758f664659ad5dx434e.1.104738673\",\"consumptionState\":0,\"confirmed\":0,\"currency\":\"CNY\",\"price\":1,\"country\":\"CN\",\"payOrderId\":\"A2d04c9ab8253b6ffaeb1dc8e653f473\",\"payType\":\"4\",\"sdkChannel\":\"1\"}"   // json字符串
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
purchaseInfo | json string | required | 购买信息 inAppPurchaseData->purchaseTokenData 部分
token  | string | required | token

7.4. 输出
```
{
    "code":0,
    "message":"OK",
	"data":{
		"userInfo":{
			"userId":"740699410",
			"username":"13611778890",
			"nickname":"136******56",
			"gender":"0",
			"birthday":"",
			"keys":"0",
			"icon":"",
			"new":"0"
		}
	}
}

# 异常
10000：参数不正常
10001：Token不正确

```

- userInfo信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户

7.5.  接口测试

```
1. 线上测试

# 验证并发货接口
curl -i -X POST \
-d "userId=740699410&token=&purchaseInfo={\"autoRenewing\":false,\"orderId\":\"202109241542584543abc77331.104738673\",\"packageName\":\"org.goowee.animalcastle\",\"applicationId\":104738673,\"kind\":0,\"productId\":\"18000\",\"productName\":\"1把钥匙测试\",\"purchaseTime\":1632469387000,\"purchaseTimeMillis\":1632469387000,\"purchaseState\":0,\"developerPayload\":\"FeeHuawei\",\"purchaseToken\":\"0000017c16c20cf10c9ce3c4d8a147de9e4862b9b97bb3496b85b53cf60ff92259758f664659ad5dx434e.1.104738673\",\"consumptionState\":0,\"confirmed\":0,\"currency\":\"CNY\",\"price\":1,\"country\":\"CN\",\"payOrderId\":\"A2d04c9ab8253b6ffaeb1dc8e653f473\",\"payType\":\"4\",\"sdkChannel\":\"1\"}" \
https://api.goowee.cn/v1/orders/deliver

```


#### 8 支付订单列表

8.1  接口
`https://api.goowee.cn/v1/orders/list`

8.2  请求方式
POST

8.3 输入
```
{
	userId:740699410 ,
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
token  | string | required | token

8.4. 输出
```
{
    "code":0,
    "message":"OK",
	"data":{
		"orderList":[
			{
				"orderId":"GY202109261625011695shpaul4jax",
				"createTime":1632644701,
				"name":"钥匙X1测试",
				"keys":1,
				"price":0.01,
				"purchaseState":0
			}
		]
	}
}

# 异常
10000：参数不正常
10001：Token不正确

```

- orderInfo 信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
orderId | string |require | 订单Id
createTime | string | required | 订单创建时间 例如：2021-01-01 12:00:00
name  | string | required | 商品名称
keys  | integer | required | 钥匙购买量
price  | float | required | 支付金额
purchaseState  | integer | required | -1：初始化 0：已购买 1：已取消 2：已退款

8.5.  接口测试

```
1. 线上测试

# 获取订单列表信息
curl -i -X POST \
-d "userId=740699410&token=" \
https://api.goowee.cn/v1/orders/list

```

#### 9 设置更新

9.1  接口
`https://api.goowee.cn/v1/setting/update`

9.2  请求方式
POST

9.3 输入
```
{
	userId:740699410 ,
	token:,
	readingBooks: 3
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
token  | string | required | token
readingBooks | integer | option | 每次阅读数量 1-6 本

9.4. 输出
```
{
    "code":0,
    "message":"OK"
}

# 异常
10000：参数不正常
10001：Token不正确

```

9.5.  接口测试

```
1. 线上测试

# 设置参数接口
curl -i -X POST \
-d "userId=740699410&token=&readingBooks=3" \
https://api.goowee.cn/v1/setting/update

```

#### 10 头像上传

10.1  接口
`https://api.goowee.cn/v1/images/upload`

10.2  请求方式
POST

10.3 输入
```
{
	userId:740699410 ,
	token:,
	type: "icon",
	image: "base64字符串"
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户名称
token  | string | required | 验证码
type | string | required | 类型 icon:头像 ....
image | string required | 图片base64字符串

10.4. 输出
```
{
    "code":0,
    "message":"OK",
    "data":{
		"imageUrl":"https://oss.goowee.cn/x.png"
    }
}
```

10.5.  接口测试
```
1. 线上测试
curl -i -X POST \
-d "userId=740699410&token=&type=icon&image=...." \
https://api.goowee.cn/v1/images/upload

```


#### 10 修改绑定手机号

10.1  接口
`https://api.goowee.cn/v1/users/bind/phone`

10.2  请求方式
POST

10.3 输入
```
{"userId": "123", code: "111111", "phoneno":13611778890}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户帐号Id
code  | string | required or option | 验证码
phoneno | string | required | 绑定手机号

10.4. 输出
```
{
	"code":0,
	"message":"OK",
	"data":{
		"userInfo":{
			"userId":"1073741825",
			"username":"13611778187",
			"nickname":"136******56",
			"gender":"1","birthday":"2015-02-20",
			"age":6,"keys":"0",
			"icon":"https://oss.goowee.cn/Icon_46dc30c9eb1aa22b8f4634006743a91d.jpg?v=51948725.73980427",
			"new":"0"
		}
	}
}
```
- userInfo信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户

- token 为当前session信息
- expire session过期时间

10.5.  接口测试
```
# 绑定手机号
curl -i -X POST \
-d "userId=1073741825&token=&code=2021&phoneno=13611778890" \
http://localhost:60006/v1/users/bind/phone

```


#### 11 反馈信息接收

11.1  接口
`https://api.goowee.cn/v1/users/questions/receiver`

11.2  请求方式
POST

11.3 输入
```
{"userId": "123", visitorId:'abcd',types: "1,2,3", "content":"", "contact":"","token":"abc"}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户帐号Id
visitorId  | string | option | 游客Id
types  | string | required | 问题快捷选择 [0-8]中一项或几项
content | string | required | 内容描述 500字符以内
contact  | string | required or option | 联系方式
token | string | required | 当前登录凭证

11.4. 输出
```
{
	"code":0,
	"message":"OK"
}
```

11.5.  接口测试
```
# 绑定手机号
curl -i -X POST \
-d "userId=1073741825&token=&types=1,2,3&content=发生了一个错误，hello world&contact=zhaofei@goowee.cn" \
http://localhost:60006/v1/users/questions/receiver

```

#### 12 活动奖励领取

12.1  接口
`https://api.goowee.cn/v1/reward/claim`

12.2  请求方式
POST

12.3 输入
```
{"userId": "123", activityId: "60001",token":"abc"}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户帐号Id
activityId  | integer | required | 活动Id 60001:880001, // 绑定手机号奖励.  60002:880003  // 问卷调查奖励
token | string | required | 当前登录凭证

12.4. 输出
```
{
	"code":0,
	"message":"OK",
	"data": {
	 "rewardInfo": [{"activityId": 60001, "reward": 880001}, ...]
	}
}
```

12.5.  接口测试
```
# 活动奖励领取
curl -i -X POST \
-d "activityId=60001&userId=1532188470&token=" \
https://api.goowee.cn/v1/reward/claim

```

#### 13 完成阅读绘本

13.1  接口
`https://api.goowee.cn/v1/books/complete`
13.2  请求方式
POST

13.3 输入
```
{
	userId:740699410 ,
	bookId:880001 ,
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
bookId | string | required | 绘本Id
token  | string | required | token

13.4. 输出
```
{
    "code":0,
    "message":"OK"
}

# 异常
10000：参数不正常
10001：Token不正确
30001：绘本不存在

```

13.5.  接口测试

```
1. 线上测试
# 场景初始化接口
curl -i -X POST \
-d "userId=532188470&bookId=880001&token=" \
https://api.goowee.cn/v1/books/complete

```

#### 14 苹果支付发货接口

14.1  接口
`https://api.goowee.cn/v1/orders/receiver`
14.2  请求方式
POST

14.3 输入
```
{
	userId:740699410 ,
	receipt:abc ,
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
receipt | string | required | Base64 App前端订单数据
env | integer | option | 0:正式 1:测试 环境
token  | string | required | token

14.4. 输出
```
{
    "code":0,
    "message":"OK",
	"data": {
		"userInfo":{
			"userId":"740699410",
			"username":"13611778890",
			"nickname":"136******56",
			"gender":"0",
			"birthday":"",
			"keys":"0",
			"icon":"",
			"new":"0"
		}
	}
}

# 其他
```

14.5.  接口测试

```
1. 线上测试
# 场景初始化接口
curl -i -X POST \
-d "userId=532188470&receipt=abc&token=" \
https://api.goowee.cn/v1/orders/receiver

```

#### 15 点击听音频

15.1  接口
`https://api.goowee.cn/v1/audio/listen`
15.2  请求方式
POST

15.3 输入
```
{
	userId:740699410 ,
	visitorId:"",
	audioId:620001 ,
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
visitorId | string | option | 游客Id
audioId | string | required | 音频Id
token  | string | required | token

15.4. 输出
```
{
    "code":0,
    "message":"OK"
}

# 异常
10000：参数不正常
10001：Token不正确
30001：音频不存在

```

15.5.  接口测试

```
1. 线上测试
# 场景初始化接口
curl -i -X POST \
-d "userId=532188470&audioId=620001&visitorId=&token=" \
https://api.goowee.cn/v1/audio/listen

```

#### 16 绘本分享

16.1  接口
`https://api.goowee.cn/v1/books/share`
16.2  请求方式
POST

16.3 输入
```
{
	userId:740699410 ,
	bookId: ,
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
bookId | string | required | 绘本Id
token  | string | required | token

16.4. 输出
```
{
    "code":0,
    "message":"OK",
	"data":{
		"userInfo":{
			"userId":"740699410",
			"username":"13611778890",
			"nickname":"136******56",
			"gender":"0",
			"birthday":"",
			"keys":"0",
			"icon":"",
			"new":"0"
		}
	}
}

# 异常
10000：参数不正常
10001：Token不正确

```

- userInfo信息结构说明

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string |require | 用户Id
username | string | required | 用户名称
nickname  | string | required or option | 用户昵称
gender  | integer | required | 性别 1:男 2:女
birthday  | string | required | 生日
keys  | integer | required | 钥匙
icon  | string | required | 用户头像
new | integer | required | 是否新用户

16.5.  接口测试

```
1. 线上测试
# 场景初始化接口
curl -i -X POST \
-d "userId=&bookId=&token=" \
https://api.goowee.cn/v1/books/share

```

#### 17 绘本事件

17.1  接口
`https://api.goowee.cn/v1/books/trigger`
17.2  请求方式
POST

17.3 输入
```
{
	userId:740699410 ,
	bookId:880001 ,
	eventId: 'scene0', # 事件key
	token:
}
```

字段名称 | 类型 | 字段必须 | 备注
--- | --- | --- | ---
userId | string | required | 用户Id
bookId | string | required | 绘本Id
eventId | string | required | 事件key
token  | string | required | token

17.4. 输出
```
{
    "code":0,
    "message":"OK",
	"data":{
		"userEventInfo":{
			"880001": ['eventId1', 'eventId2', ...],
			"880002": ...
		}
	}
}

# 异常
10000：参数不正常
10001：Token不正确
```

17.5.  接口测试

```
1. 线上测试
# 场景初始化接口
curl -i -X POST \
-d "userId=&bookId=&token=" \
https://api.goowee.cn/v1/books/trigger

```
