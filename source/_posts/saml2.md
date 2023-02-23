---
layout: posts
title: SAML 2.0
date: 2023-02-23 14:50:46
tags: cirrus
categories: Teckknowledge
---

# SAML2.0


### Conception
> IDP identity provider， for instance, AD,  ADFS, LDAP, 身份认证authentication
> SP service provider, for instance, cluster, application,  access resources

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

![](/uploads/149184710911054.png =800x)



### 2 ways of session init

![](/uploads/288243668946728.png =800x)


![](/uploads/409973578816914.png =800x)



### The saml assertion's context


![](/uploads/589974006532594.png =800x)
