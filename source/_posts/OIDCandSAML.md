---
layout: posts
title: OIDC and SAML
date: 2023-02-19 17:59:46
updated: 2023-04-20 23:11
tags: cirrus
categories: Teckknowledge
---


## 1. OIDC 实现的业务场景

举例：常用办公场景，上班第一件事，登录github，登录confluence，登录office365，登录论坛。
### 1.1. 需要SSO 来免登陆：去IDP 认证，拿到identity token, 
1. 带identity token 访问github,                 github        拿identity token去IDP 确认，         然后 允许/拒绝；
2. 带identity token 访问confluence,           confluence 拿identity token去IDP 确认，          然后 允许/拒绝；
3. 带identity token 访问office365,             office365   拿identity token去IDP 确认，          然后 允许/拒绝；
4. 带identity token 访问论坛,                    论坛           拿identity token去IDP 确认，          然后 允许/拒绝；


### 1.2. IDP access token issuer:  IDP 带 authorization 功能后，变成 user 去IDP认证，同时返回access token:
    1. 带access token 访问github,                 github         在 local 用签名验证access token 真实性，有效性，然后 允许/拒绝；
    2. 带access token 访问confluence,           confluence       在 local 用签名验证access token 真实性，有效性，然后 允许/拒绝；
    3. 带access token 访问office365,             office365       在 local 用签名验证access token 真实性，有效性，然后 允许/拒绝；
    4. 带access token 访问论坛,                    论坛           在 local 用签名验证access token 真实性，有效性，然后 允许/拒绝；
在这里，user 申请的时候，提供2元组 scope（也是OIDC login 需要的一个参数）： 1. resource 类型， 2. 执行的操作


### 1.3. Resource access token issuer: 由 resource 来 authorization 颁发 access token:
    1. 带identity token 访问github,                 github        在 local 用签名验证token 真实性，有效性，           然后 颁发 access token；
    2. 带identity token 访问confluence,           confluence 在 local 用签名验证token 真实性，有效性 ，          然后 颁发 access token；
    3. 带identity token 访问office365,             office365   在 local 用签名验证token 真实性，有效性 ，          然后 颁发 access token；
    4. 带identity token 访问论坛,                    论坛           在 local 用签名验证token 真实性，有效性 ，          然后 颁发 access token；
token存储在 用户的cookie中（身份 & signed）,   或者作为一个index key 指向 应用后端的来自IDP authentication的信息。

access token: 短期有效
refresh token：

![](/uploads/136865505836214.png)
![](vx_images/136865505836214.png =1000x)

## 2. OIDC and SAML

### 2.1. OIDC  OpenID Connect

#### 2.1.1. an open authentication protocol that works on top of the OAuth 2.0 framework

```bash
OIDC 允许个人使用单点登录(SSO)的方式，访问那些用OPs验证身份的网站。
OIDC 为应用程序或服务提供有关用户、其身份验证上下文以及访问其配置文件信息的信息。

OIDC 是基于 OAuth 2.0 的身份认证协议，增加了 Id Token
```

#### 2.1.2. OIDC 的目的

> 为用户提供一组认证信息来访问多个sites. for users to provide one set of credentials and access multiple sites.

> 当用户登录网站或者应用程序时，先redirect to OPs, 在OPs 先进行身份认证，然后再 redirect back to 网站或应用程序 site or application

> 旨在保护基于浏览器的应用程序、API 和 移动原生应用程序 mobile native applications

> 它将用户身份验证委托给 "托管用户帐户" 的服务提供商，并授权第三方应用程序访问用户帐户。It delegates user authentication to the service provider that hosts the user account and authorizes third-party applications to access the user’s account.

```bash
For example, there are currently two ways of creating a Spotify account. You can register with Spotify or you can sign on through Facebook. Facebook sends your name and email address to Spotify, which uses that information to authenticate you.
```


#### 2.1.3. Identity tokens: 证明用户已经过身份验证,并且是 JSON Web 令牌 （JWT）
> be read by client
> prove that users were authenticated and are JSON Web Tokens (JWTs),
标识令牌（旨在由客户端读取）证明用户已经过身份验证，并且是 JSON Web 令牌 （JWT），发音为“jots”。这些文件包含有关用户的信息，例如其用户名、尝试登录到应用程序或服务时以及允许他们访问联机资源的时间长度。

> 是基于 OAuth 2.0 的身份认证协议，增加了 Id Token

#### 2.1.4. Access tokens: 访问被保护的资源
> be read and validated by the API.
> are used to access protected resources
> 目的是通知API，此令牌的持有者已被授权访问API并执行特定操作(由已授予的范围指定)

```bash
Their purpose is to inform the API that the bearer of this token has been authorized to access the API and perform specific actions (as specified by the scope that has been granted).
```

#### 2.1.5. ID tokens
> Id Token 由 OpenID Provider 颁发，包含关于终端用户的信息字段

#### 2.1.6. OpenID Providers (OPs)
> OpenID Provider，指授权服务器，负责签发 Id Token。Authing 是 OpenID Provider。
> Id Token 由 OpenID Provider 颁发，包含关于终端用户的信息字段

#### 2.1.7. JSON Web Tokens (JWTs)  pronounced “jots.”


```zsh
OpenID Provider，指授权服务器，负责签发 Id Token。Authing 是 OpenID Provider。
终端用户，Id Token 的信息中会包含终端用户的信息。
调用方，请求 Id Token 的应用。
Id Token 由 OpenID Provider 颁发，包含关于终端用户的信息字段。
Claim 指终端用户信息字段。
```

![](/uploads/528803599836013.png)







### 2.2. Differences between SAML, OAuth, OpenID Connect



#### 2.2.1. 侧重点
* OAuth 是framework, 用于保护特定资源（如应用程序或文件集）的授权框架，
* Oauth2.0, 主要用于资源授权。
OAuth is an authorization framework used to protect specific resources

* SAML 和 OIDC 是 身份验证标准，authentication standards, 用于创建安全登录体验的身份验证标准。
SAML and OIDC are authentication standards used to create secure sign-on experiences. 

* OIDC 能够认证用户并完成资源授权。 侧重于身份认定，他/她是谁， 而OAuth侧重于允许/限制他们能做什么。
who someone is，  what they are allowed to do
如果你希望实现单点登录或先鉴权用户再返回资源，建议使用 OIDC 协议。

#### 2.2.2. 广泛性
SAML 不支持移动设备的单点登录或API访问。does not support SSO for mobile devices or provide API access
OIDC 都支持。

#### 2.2.3. 格式
 1. SAML： tokens written in XML
 2. OIDC： JWTs， 它们是可移植的，并支持一系列签名和加密算法。

#### 2.2.4. 易用性
OIDC比SAML 更简单， is less complex than SAML




### 2.3. 常见的 OAuth 2.0 授权流程如下：

在你的应用中，让用户访问登录链接，浏览器跳转到 Authing，用户在 Authing 完成认证。
浏览器接收到一个从 Authing 服务器发来的授权码。
浏览器通过重定向将授权码发送到你的应用后端。
你的应用服务将授权码发送到 Authing 获取 AccessToken，如果需要，还会返回 refresh token。
你的应用后端现在知道了用户的身份，后续可以保存用户信息，重定向到前端其他页面，使用 AccessToken 调用资源方的其他 API 等等。



> https://docs.authing.cn/v2/concepts/user-pool.html



### 2.4. saml

> 允许 IdP 和 SP 彼此独立


> SAML 使用 XML 元数据文档， 也就是 SAML 令牌进行断言，验证用户身份和访问权限。

![](vx_images/24513571558970.png =800x)


