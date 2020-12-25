# 授权 OAuth 应用

您可以让其他用户授权您的 OAuth 应用。

Guizhan 账号的 OAuth 实现支持标准[授权代码授予类型](https://tools.ietf.org/html/rfc6749#section-4.1)。

## 授权流程 :id=authorization

授权应用程序用户的流程为：

1. 用户由应用站点重定向至 Guizhan 账号，以请求用户进行授权

2. 用户被 Guizhan 账号重定向回应用站点

3. 应用站点使用用户的访问令牌访问 API

### 1. 重定向至 Guizhan 账号 :id=authorization-redirect

```
GET https://account.guizhanss.com/oauth/authorize
```

当用户由应用站点重定向至此页面时，页面将提示用户登录以及授权应用。

*参数*

|名称|类型|描述|
|-|-|-|
|client_id|`字符串`|**必填**。您[创建应用](/zh-cn/oauth-app/create)后获得的**应用ID**。|
|response_type|`字符串`|**必填**。这里填写`code`。|
|redirect_uri|`字符串`|**必填**。需要与应用设置中填写的回调地址完全一致。|
|scope|`字符串`|使用逗号`,`分割的[作用域](/zh-cn/oauth-app/scopes?id=scopes)列表。如果未提供，则使用默认的作用域。|
|state|`字符串`|一串随机字符，用于防止跨站请求伪造攻击。|

### 2. 用户重定向回应用站点 :id=authorization-callback

当用户同意授权应用后，Guizhan 账号会将用户重定向回应用站点，并将`code`与`state`（如果有）作为参数传递。

检查`state`的有效性后，应使用`code`发送POST请求以换取访问令牌。

```
POST https://account.guizhanss.com/oauth/token
```

*参数*

|名称|类型|描述|
|-|-|-|
|client_id|`字符串`|**必填**。您[创建应用](/zh-cn/oauth-app/create)后获得的**应用ID**。|
|client_secret|`字符串`|**必填**。您[创建应用](/zh-cn/oauth-app/create)后获得的**应用密钥**。|
|grant_type|`字符串`|**必填**。这里填写`authorization_code`。|
|redirect_uri|`字符串`|**必填**。需要与应用设置中填写的回调地址完全一致。|
|code|`字符串`|**必填**。上一步请求收到的`code`参数。|

*响应*

Guizhan 账号返回一个包含`access_token`、`refresh_token`和`expires_in`属性的JSON响应。

```json
{
    "token_type": "Bearer",
    "expires_in": 31536000,
    "access_token": "返回的access_token",
    "refresh_token": "返回的refresh_token"
}
```

### 3. 使用访问令牌访问 API :id=authorization-api

访问令牌可用于以用户的身份访问API。

```
GET https://api.account.guizhanss.com/user
Authorization: Bearer access_token
```

## 常见问题 :id=faq

### 重定向至 Guizhan 账号后显示错误页面

导致显示错误页面的原因有很多。请确保：

- 您的账号处于正常状态。
- 应用处于正常状态。
- 每个参数都按照文档中的填写。
    - `redirect_uri`需要与应用设置中填写的回调地址**完全一致**。
    - `scope`作用域列表以逗号`,`分割，仅列出文档[可用作用域](/zh-cn/oauth-app/scopes?id=scopes)中展示的作用域。