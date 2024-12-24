---
icon: sack-dollar
description: 查询地址为主动查询代理服务器的用户钱包方法,可以不设置,但是如果该平台有多个游戏开发商游戏避免回调错误那么最好设置该地址便于主动去同步钱包
---

# 查询地址

## 以下接口只是演示需要在自己平台服务器上

{% hint style="warning" %}
<mark style="color:orange;">该方法可以进行访问调试,但是必须先让用户调用过登录接口,否则无法测试<</mark>[<mark style="color:blue;">设置详情</mark>](../hou-tai-shi-yong-shou-ce/xi-tong-she-zhi.md)<mark style="color:orange;">></mark>
{% endhint %}

<mark style="color:green;">`GET`</mark> `/test/Demo/wallet`

\<Description of the endpoint>

**Headers**

| Name         | Value              |
| ------------ | ------------------ |
| Content-Type | `application/json` |

**Query**

<table><thead><tr><th width="569">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>uid</td><td>string</td><td>用户ID</td></tr></tbody></table>

**Response**

| Name    | Type    | Description |
| ------- | ------- | ----------- |
| wallet  | float64 | 当前用户钱包数量    |
| time    | int64   | 秒时间戳        |
| uid     | string  | 用户ID        |
| success | bool    | 是否相应成功是否有错误 |
| error   | string  | 错误信息        |

{% tabs %}
{% tab title="200" %}
```json
{
  "success":true,
  "uid": "001",//为用户登录得时候令牌中得uid
  "wallet": 4078.90,//
  "time":1609459200,//秒时间戳
  "error":""
}
```
{% endtab %}

{% tab title="400" %}
```json
{
  "success":false
  "error": "Invalid request"//错误信息
}
```
{% endtab %}
{% endtabs %}

## Data说明
