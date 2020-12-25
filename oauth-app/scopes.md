# OAuth 应用的作用域

通过作用域，您可以准确指定所需的访问权限类型。作用域限制 OAuth 令牌的访问权限。它们不会授予超出用户权限范围的任何额外权限。

---

在[授权流程](/zh-cn/oauth-app/authorize?id=authorization-redirect)中，请求的作用域将在页面上显示给用户。

## 可用作用域 :id=scopes

|名称|描述|
|-|-|
|(无作用域)|`user:read`将作为默认作用域。|
|user:read|*默认作用域*。读取用户的常规信息（不包括邮箱地址）。|
|user:email|读取用户的邮箱地址。|