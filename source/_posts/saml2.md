---
layout: posts
title: SAML 2.0
date: 2023-03-15 23:34:02
updated: {{ date }}
tags: cirrus
categories: Teckknowledge
---

# SAML2.0


### Conception
>  **SAML**, Security Assertion Markup Language

>  **IDP identity provider**， for instance, AD,  ADFS, LDAP, 身份认证authentication
    * Performs authentication and passes the user's identity and authorization level to the service provider
    
> **SP service provider**, for instance, cluster, application,  access resources
    * Trusts the identity provider and authorizes the given user to access the requested resource.
`credential 是啥？ idp authorization的是啥？`

* is the link between the authentication of a user’s identity and the authorization to use a service.
* simplifies federated authentication and authorization processes for users, Identity providers, and service providers
* enable sso, users can log in once, and those same credentials can be reused to log into other service providers
* SAML authentication is the process of verifying the user’s identity and credentials (password, two-factor authentication, etc.). SAML authorization tells the service provider what access to grant the authenticated user.
* authentication = verify identity + verify credential  验证身份和凭证， <font bgcolor=##FF7F50>不理解这个credential 是啥</font>
* SAML 做身份和凭证/资格的验证， SAML授权告诉SP 分配什么样的access 给通过验证的user
* SAML 同时做了authentication and authorization
* manage one login per user > manage seperate logins to email, customer relationships management **CRM**, ADs.
* 简化了users, idp，sp之间的认证和授权流程。 authentication and authorization processes for users, Identity providers, and service providers.





###  Workflow
1. IDP 和 SP establish trust
2. user 在IDP 和 SP中都存在
3. user login from IDP, IDP验证通过后，生成SAML assertion，返回给user，
4. user拿着SAML assertion 到SP验证，如果通过，user就可以访问SP；
    1. SP 把SAML assertion中的user信息和 local的 user 对比验证，
5. IDP 拿着SAML assertion 到另一个SP 去验证，也通过的话，user也可以SSO 访问 another application；


### IDP 和 SP 的交流
1. IDP 和 SP 都config xml， contains settings and certificate of the system
2. this metadata exchange is establish the trust between IDP and SP like `Workflow point 1`
3. this XML metadata file contains the form for the user identifier called `the name ID formats`, 

![](/uploads/315430821585906.png)

![](/uploads/149184710911054.png)



### 2 ways of session init

![](/uploads/288243668946728.png)


![](/uploads/409973578816914.png)

 
### A SAML REQ, is generated from SP, request the authentication from the IDP

### A SAML RES, is generated from the IDP, contains the actual assertion of authenticated user, and additional user profile,
such as group, role, depending on what the Service Provider can support.

### note
1. SP 从不直接与IDP交流never directly interacts with IDP，browser 充当acts了执行carry out所有重定向的代理agent，acts as the agent to carry out all the redirections.
2. SP 知道要去找哪个 IDP；
3. SP在拿到 IDP返回的saml assertion之前是不知道 谁在要资源的，也就是说用户输入的用户名和密码，只传递到IDP，until the SAML assertion comes back from the Identity Provider.
4. 可以直接从IDP认证，然后去访问SP，无需必须从SP发起请求；
5. SAML 身份验证流是异步的。SP不会维护生成的任何身份验证请求的任何状态。当 SP 收到来自 IDP 的响应时，响应必须包含所有必要的信息。

### IdP-initiated SSO with SAML Authentication

`IDP 用之前和SP建立互信时产生的私钥来sign assertion， 然后通过user's browser 发给SP， 或者发送对assertion的引用。a reference to the assertion`

`SP receive the assertion, 用pub key来验证这个assertion是否真的来自它信任的IDP，以及验证这个assertion是否被修改过。 it validates the signature using the public key in order to ensure the SAML assertion really came from its trusted IDP`

`然后 SP 从 assertion中提取 extract identity和其他它需要的信息。`

### SP determine which IDP
1. 通过登录用户的域名， The SP may ask the user for their email address and use the domain of the email, such as  prithviraj.gaikwad@dell.com
2. SP display a list of IDP,  user choose one 
3. The resource URL may be specific to one IdP. 
4. The SP may have placed a cookie containing IdP information in the user’s browser the first time the user successfully signed on from the IDP and will use this information on subsequent accesses.

### The saml assertion's context


![](/uploads/589974006532594.png)




### SAML IdP Providers:

1. Active Directory Federation Services (ADFS)
2. Auth0
3. Azure AD (Microsoft Azure Active Directory)
4. Okta
5. OneLogin
6. Ping Identity
7. Salesforce