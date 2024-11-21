---
icon: arrow-right-to-bracket
---

# 登录获取

<mark style="color:green;">`POST`</mark> `/games/v1/singleWallet/Login`

<通过令牌登录并且获取游戏列表>

**Headers**

| Name          | Value                                            |
| ------------- | ------------------------------------------------ |
| Content-Type  | `application/json`                               |
| Authorization | `Bearer <`[`获取令牌方法`](ling-pai-chuang-jian.md)`>` |

**Query**

| 定义        | 类型     | 描述                 | 示例                         |
| --------- | ------ | ------------------ | -------------------------- |
| `agentId` | string | 账号ID也就是登录后左上角显示的账号 | 1723205701015@jjserver.com |

**Response**

{% tabs %}
{% tab title="成功示例" %}
```json
{
    "cache": false,
    "data": {
        "games": [{
            "id": 2,
            "title": "疯狂老虎机"
        }, {
            "id": 49,
            "title": "FullHouse"
        }, {
            "id": 223,
            "title": "伽罗宝石2 FortuneGems2"
        }, {
            "id": 403,
            "title": "Fullhouse3"
        }],
        "token": "LHgsTGbF+GdhOS4lQSn3p72mumuptCzGzOTN0sqTD9R7efiVwvgIP8jzN4TZICkgDzkEjFZ9OJCHpzzBoUvZ7JFeJef/ef8KfBGy9Savp+NuKsn5VLL4eUeIqz9Vh9bvRUkjwFRhtk62VXtuNvUJVgRlZgQ2Uvzq1sScywwUazY=",
        "userInfo": {
            "aid": 3,
            "hash": "d12e4331e0690341e2e9c1724e8338cd",
            "nickname": "Aerith",
            "uid": "110"
        },
        "wallet": 4000
    },
    "duration": 1175,
    "error": null,
    "hash": "c00pgGm5d6ORGVaKNhD3AUeUxDNTCVh0",
    "success": true
}
```
{% endtab %}

{% tab title="失败示例" %}
```json
{
    "cache": false,
    "data": null,
    "duration": -1,
    "error": {
        "code": 4003,
        "info": "非法签名",
        "message": "缺少必要参数token"
    },
    "hash": "BH2DRadu4QGoRh9Of2evqUXPn12DrvUn",
    "success": false
}
```
{% endtab %}
{% endtabs %}
