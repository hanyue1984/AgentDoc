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

<table><thead><tr><th width="128">Name</th><th width="83">Type</th><th>Description</th><th>示例</th></tr></thead><tbody><tr><td><code>gameId</code></td><td>string</td><td>游戏ID</td><td>49</td></tr><tr><td><code>agentId</code></td><td>string</td><td>代理商账号<a href="../zhun-bei-gong-zuo/quickstart.md">&#x3C;查看位置></a></td><td>173xxxxxxxx@jjserver.com</td></tr></tbody></table>

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
