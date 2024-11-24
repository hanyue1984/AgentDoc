---
icon: volleyball
---

# 开始游戏

游戏<mark style="color:green;">`GET`</mark> /games/v1/StartGame

\<Description of the endpoint>

**Headers**

| Name                   | Value                                        |
| ---------------------- | -------------------------------------------- |
| Content-Type           | `application/json`                           |
| Authorization          | `Bearer <`[`令牌`](ling-pai-chuang-jian.md)`>` |
| gateway-identification | 49ba59abbe56e057                             |

**Query**

| Name     | Type   | Description | 示例    |
| -------- | ------ | ----------- | ----- |
| `gameId` | string | 游戏ID        | 49    |
| `lang`   | string | 语言          | en-US |

**Response**

{% tabs %}
{% tab title="成功示例" %}
```json
{
    "cache": false,
    "data": "https://dev.angellparty.com/fullhouse/index.html?apiId=1410&be=moc.ytrapllegna.ipa&domain_gs=hr-fgp&domain_platform=moc.ytrapllegna.ipa&gameId=49&gs=moc.ytrapllegna.ipa&lang=en-US&legalLang=true&ssoKey=494bb7d0ae4fb88315dd80b120480223",
    "duration": 1074,
    "error": null,
    "hash": "HBRSoxpritUHeEiADwcY10QG0uRgW80C",
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
        "code": 4006,
        "info": "is not games",
        "message": "没有找到用户信息"
    },
    "hash": "Rq8TLr5ljTrtHEuFgSmC1FWZboKpZw64",
    "success": false
}
```
{% endtab %}
{% endtabs %}
