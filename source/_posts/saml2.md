---
layout: posts
title: SAML 2.0
date: {{ date }}
updated: {{ date }}
tags: cirrus
categories: Teckknowledge
---

# SAML2.0


### Conception
> SAML, Security Assertion Markup Language
> IDP identity provider， for instance, AD,  ADFS, LDAP, 身份认证authentication
> SP service provider, for instance, cluster, application,  access resources



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



### The saml assertion's context


![](/uploads/589974006532594.png)
