# 基本

#### 简介
##### 介绍
> 123
##### API设置
> 唯一需要的设置是转到“API接入”页面并生成一个API密钥。您将获得一个用于验证API调用的私钥和公钥。确保您不与任何第三方共享您的私钥！
##### 认证方式
> 每个API调用都有一个使用您的私钥生成的SHA-512 HMAC签名。我们的服务器会生成自己的HMAC签名，并将其与API调用者的签名进行比较。如果它们不匹配，则将放弃API调用。HMAC签名作为称为“sign”的字段发送。

1. 对所有字段除sign外，进行字典排序
2. 所有请求参数编码成json string，用您的私钥照上述方式签名
3. 注意每个字段都应该是string型，如果字段里出现integer情况，也需要转成string型
4. 如果请求字段里有对象，则也需要转成json string，并且转成json string 时输入 JSON_UNESCAPED_UNICODE，JSON_UNESCAPED_SLASHES 两个参数

##### API响应
> 123
#### 接受付款
> 123
#### 使用API接受付款
> 123
#### 示例代码
> 123

# 付款

#### 创建交易

> 系统把创建交易的能力以API的形式开放给商家，使您能用更灵活的方式集成开发支付功能。此API用于创建商家自己的自定义结帐页面，因此买家不必离开商家的网站即可完成付款。

| 接口说明 | |
| ------------ | ------------ |
| 请求路径 | http://{host}/api/v1/submit_trans |
| 请求方式 | POST |
| Content-Type | application/json |
| 字符编码 | UTF-8 |

请求参数说明

| 参数 | 类型 | 备注 |
| ------------ | ------------ | ------------ |
| sign | string | 签名 |
| pubkey | string | 系统分配的公钥 |
| item_name | string | (可选)此交易的商品名 |
| state | string | (可选)此交易的code码，该code码将在回调时通知给商家，用于商家识别订单 |

响应参数说明

| 参数 | 类型 | 备注 |
| ------------ | ------------ | ------------ |
| status | integer | 系统状态码，=0为操作成功，>0为操作失败，msg有相关错误说明 |
| msg | string | 当status>0时，错误说明信息（该字段在status=0时返回交易TXID）|
| data | object | 交易实体对象 |
| expired_at | datetime | 交易过期时间，过期后订单结束，不再回调通知 |

请求示例

```
{
	"sign": "42306036e6345609e5c4edd6bc830c4bfc9575d306a4d2950d3611e29ee798267953f010cba0f7471842c0f27e83e59018ea01c345f82be07c3e5f148c822dbc",
	"pubkey": "0b6dbe660366b5a8b3ccbfeec373eeec1567b89a96fe0c5ef502091cc8249c42",
	"item_name": "手机",
	"state": "1001"
}
```

#### 查询交易

> 查询交易能力开放是为了商家更好的控制自己的自定义结帐页面，前端可以采取轮询的方式请求此接口，以获得订单实时状态。也可以通过此接口获取到的过期时间设置定时器，定时关闭订单以达到控制交易的效果。

| 接口说明 | |
| ------------ | ------------ |
| 请求路径 | http://{host}/api/v1/query_trans |
| 请求方式 | GET |
| Content-Type | N/A |
| 字符编码 | UTF-8 |


请求参数说明

| 参数 | 类型 | 备注 |
| ------------ | ------------ | ------------ |
| sign | string | 签名 |
| pubkey | string | 系统分配的公钥 |
| uniqid | string(uuid) | 交易的TXID |

响应参数说明

| 参数 | 类型 | 备注 |
| ------------ | ------------ | ------------ |
| status | integer | 系统状态码，=0为操作成功，>0为操作失败，msg有相关错误说明 |
| msg | string | 当status>0时，错误说明信息（该字段在status=0时返回交易TXID）|
| data | object | 交易实体对象 |
| expired_at | datetime | 交易过期时间，过期后订单结束，不再回调通知 |

请求示例

```
http://{host}/api/v1/query_trans?sign=42306036e6345609e5c4edd6bc830c4bfc9575d306a4d2950d3611e29ee798267953f010cba0f7471842c0f27e83e59018ea01c345f82be07c3e5f148c822dbc&pubkey=0b6dbe660366b5a8b3ccbfeec373eeec1567b89a96fe0c5ef502091cc8249c42&uniqid=3a6765db-5d2c-4907-8e32-36dbb5e31170
```



#### 回调地址


1. {host}地址
	- 测试环境：
	- 正式环境：