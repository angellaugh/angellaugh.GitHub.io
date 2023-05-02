---
layout: posts
title: Oauth2.0 client authentication
date: 2023-05-02 19:30
updated: 2023-05-02 19:30
tags: cirrus
categories: Teckknowledge
---

# Client Authentication Methods


## 1. Token Endpoint
![](/uploads/182031511625388.png)
![](vx_images/182031511625388.png)


## 2. client_type
![](/uploads/316472234951139.png)
![](vx_images/316472234951139.png)


## 3. client_secret_post [(OIDC Core, 9. Client Authentication).](https://openid.net/specs/openid-connect-core-1_0.html)

在传统的方式中，首先授权服务器会生成一对clientID 和 client secret，并提前将这对信息提供给客户端应用程序。
当client 发送 token request时， 会带上 clientID 和 client secret， authorization server 会校验这一对密码是否正确。

![](/uploads/477822074947007.png)
![](vx_images/477822074947007.png)



## 4. client_secret_basic[rfc7617](https://tools.ietf.org/html/rfc7617)

### 4.1. 将 clientID 和 client secret 写成string格式：
```
"{Client ID}: {Client Secret}"
```
### 4.2. 用base64 加密

### 4.3. 在令牌请求的授权头中嵌入BASE64字符串。

![](vx_images/581553216911333.png)




client_secret_post and client_secret_basic are client authentication methods described in RFC 6749, 2.3.1. Client Password.

## 5. client_secret_jwt  无需把clientID 和 clientSecret 加入token request 请求参数中

### 5.1. client端准备一个data
### 5.2. 用client secret 为 data生成 签名 signature
### 5.3. 在token request 请求中包括data 和signature。

![](/uploads/345122512532873.png)
![](vx_images/345122512532873.png)


### 5.4. data 是有格式要求的：
```
POST /token.oauth2 HTTP/1.1
     Host: as.example.com
     Content-Type: application/x-www-form-urlencoded

     grant_type=authorization_code&
     code=n0esc3NRze7LTCu7iYzS6a5acc3f0ogp4&
     client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3A
     client-assertion-type%3Ajwt-bearer&
     client_assertion=eyJhbGciOiJSUzI1NiIsImtpZCI6IjIyIn0.
     eyJpc3Mi[...omitted for brevity...].
     cC4hiUPo[...omitted for brevity...]
```
### 5.5. data 是有格式要求的：
```
POST /token.oauth2 HTTP/1.1
     Host: as.example.com
     Content-Type: application/x-www-form-urlencoded

     grant_type=authorization_code&
     code=n0esc3NRze7LTCu7iYzS6a5acc3f0ogp4&
     client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3A
     client-assertion-type%3Ajwt-bearer&
     client_assertion=eyJhbGciOiJSUzI1NiIsImtpZCI6IjIyIn0.
     eyJpc3Mi[...omitted for brevity...].
     cC4hiUPo[...omitted for brevity...]
```

### 5.6. 签名后生成 JWT， JWT 被称为 client assertion
#### 5.6.1. JWT 被作为 client_assertion 请求参数的值包含在token request 请求中；
#### 5.6.2. 同时需要包括 client_assertion_type 请求参数，是一个固定字符串：urn:ietf:params:oauth:client-assertion-type:jwt-bearer；

![](/uploads/393604423658709.png)
![](vx_images/393604423658709.png)
## 6. private_key_jwt

这个是在client端，生成一对非对称密钥： public key & private key；
public key 发送到 authorization server中；
private key 来加密data， 签名；
然后将这种JWT 加入token request，发给authorization sever， 进行验证；

![](/uplo/25595270559249.png)
![](vx_images/25595270559249.png)
## 7. tls_client_auth
## 8. self_signed_tls_client_auth



![](/uploads/169442698899543.png)
![](vx_images/169442698899543.png)

