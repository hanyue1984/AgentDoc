---
icon: rotate-left
---

# 回调说明

投注金额订单号回调地址设置请参考 [<系统设置>](../hou-tai-shi-yong-shou-ce/xi-tong-she-zhi.md)

回调会在用户Spin点击后发送当前游戏数据给代理商进行同步内容见Data说明.调用失败会最多3次尝试

如果4次都失败则可以在[<钱包流水>](../hou-tai-shi-yong-shou-ce/ri-zhi-qian-bao-liu-shui.md) 中看到.可以手动自己在服务器上补全日志信息

第一次: 直接调用 如果失败

第二次:一分钟后尝试,如果失败

第三次:30分钟后尝试,如果失败

第四次:50分钟后尝试

## 以下接口只是演示需要在自己平台服务器上

<mark style="color:green;">`POST`</mark> `/Demo/notice`

\<Description of the endpoint>

**Headers**

| Name          | Value                                            |
| ------------- | ------------------------------------------------ |
| Content-Type  | `application/json`                               |
| Authorization | `Bearer <`[`获取令牌方法`](ling-pai-chuang-jian.md)`>` |

**Body**

| Name      | Type   | Description                                             |
| --------- | ------ | ------------------------------------------------------- |
| order\_no | string | 订单号                                                     |
| data      | string | 订单详细数据(<mark style="color:red;">**AES-128-CBC**</mark>) |
| time      | int64  | 时间戳                                                     |
| uid       | string | 用户ID                                                    |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "id": 1,//这里内容随意,只要返回状态是200就认为是成功
  "name": "John",
  "age": 30
}
```
{% endtab %}

{% tab title="400" %}
```json
{
  "error": "Invalid request"//内容随机只要返回得状态不是200 那么就认定为失败
}
```
{% endtab %}
{% endtabs %}

## Data说明

<table><thead><tr><th width="242">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>order_no</td><td>string</td><td>订单号</td></tr><tr><td>total_bet</td><td>float</td><td>投注金额</td></tr><tr><td>total_win</td><td>float</td><td>输赢金额</td></tr><tr><td>net_win</td><td>float</td><td>收益</td></tr><tr><td>business</td><td>string</td><td>业务</td></tr><tr><td>game_id</td><td>int</td><td>游戏ID</td></tr><tr><td>time</td><td>int</td><td>时间戳</td></tr></tbody></table>

该Data需要接收后用<mark style="color:red;">**AES-128-CBC**</mark> 进行解密成json字符串后才可以读取
