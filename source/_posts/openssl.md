---
layout: posts
title: OpenSSL
date: {{ date }}
updated: {{ date }}
tags: cirrus
categories: Teckknowledge
---

# openssl

在SSL/TLS/HTTPS通信中，证书虽然不是TLS/SSL协议的一部分，却是HTTPS非常关键的一环，网站引入证书才能避免中间人攻击。证书涉及了很多密码学知识，理解证书后，再深入理解TLS/SSL协议，效果会更好。

啥是TLS？？？Mutual TLS (mTLS) certificate authentication 双向证书认证


OpenSSL 是一个安全套接字层密码库，囊括主要的密码算法、常用密钥、证书封装管理功能及实现ssl协议。
OpenSSL整个软件包大概可以分成三个主要的功能部分：
1. SSL协议库libssl
2. 应用程序命令工具
3. 密码算法库libcrypto

.KEY 私钥。

**.CSR** 是 Certificate Signing Request 的缩写，即证书签名请求，这不是证书，只是包含申请证书的基本信息。生成证书时要把这个提交给权威的证书颁发机构，颁发机构审核通过之后，再根据这些申请信息生成相应的证书。

.CRT 即 certificate的缩写，即证书。

**X.509** 定义公钥证书格式， structure， 不同的密钥有不同的结构体。
> X.509的证书文件，一般以.crt结尾，根据该文件的内容编码格式，可以分为以下二种格式：

> .PEM - Privacy Enhanced Mail，以base64编码，将二进制文件转为文本，打开看文本格式，以"-----BEGIN…“开头，”-----END…"结尾，Apache 和 *NIX 服务器偏向于使用这种编码格式。

> .DER - Distinguished Encoding Rules，是二进制格式，不可读。Java 和 Windows 服务器偏向于使用这种编码格式。


## 1. 自签证书， CA、证书链、证书的撤销

### 1.1. For instances:

```bash
openssl genrsa -out server.key 2048  # 先生成一个私钥                 
openssl req -new -key server.key -out server.csr # 用这个私钥加密证书请求信息，生成一个证书请求
openssl x509 -req -in server.csr -out server.crt -signkey server.key -days 3650 # 用证书请求 和 私钥自签名来申请一个x509公钥
# 用x509公钥 和 私钥自签名来发请求
python3 manage.py runserver_plus --cert-file ./server.crt --key-file ./server.key 0.0.0.0:8000
```

```bash Generate Root CA
openssl genrsa -out private/ca.key 2048 #先生成一个私钥

CIAM-ali1:~/certification # openssl genrsa -out private/ca.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
.....................+++++
.............+++++
e is 65537 (0x010001)
CIAM-ali1:~/certification # cat private/ca.key
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEArPbCOb9to
……
XfJxLjmrJIxzlAn5/3OI9sAuZW0ZQu3VhpMXow/d/IN4T3i5UNOjdA==
-----END RSA PRIVATE KEY-----



# -x509生成一个CA
openssl req -new -x509 -days 365 -key private/ca.key -out certs/ca.pem -subj "/C=CN/ST=Shanghai/L=Shanghai/O=Dell/OU=Dell/CN=west.isilon.com" -extensions v3_ca -config openssl.cnf
```

#### 1.1.1. 字段解释

<mark>openssl genrsa -out ca.key 2048</mark>
```bash
genrsa:  生成RSA加密的私钥。 generates an RSA private key, which essentially involves the generation of two prime numbers.
  -out: The output file to write to
  numbits: the size of the private key to generate in bits, default:2048
  

不加密的RSA私钥。
openssl genrsa -out ca.key 2048

加密的RSA私钥。
openssl genrsa -des3 -out ca.key 2048

指定随机数创建RSA私钥。
openssl genrsa -des3 -out ca.key 2048 -seed abc
openssl genrsa -des3 -out ca.key 2048 -rand file:hello.txt


```

<mark>openssl req -new -key server.key -out server.csr</mark>
```bash
req: 创建和处理PKCS\#10格式的证书请求。 creates and processes certificate requests in PKCS#10 format
     还可以添加额外信息来创建自签名CA证书。  也就是拿私钥签名之后当做 CA。上一步已经创建了私钥。可以拿私钥来创CA。
-new -key： 生成一个新的 certificate req， 如果不指定-key, 那么就读取configfile中的配置项。


PKCS\#10: 证书请求文件，类似于CSR文件，一般是一个base64文件。
          是一段可以向CA申请证书的P10请求。
          该请求一般是通过硬件生成密钥对后，将私钥单独存放，但是将公钥放入p10中，
          CA受到该p10请求后，可以校验，并根据p10中的信息制作一张没有私钥的公钥证书。
意思是PKCS10 --> 本地会有一对公私钥  --> CA 拿到P10带来的公钥 --> CA generate --> 一张没有私钥的公钥。
            
```

<mark>openssl req -new -x509 -days 365 -key private/ca.key -out certs/ca.pem -subj "/C=CN/ST=Shanghai/L=Shanghai/O=Dell/OU=Dell/CN=west.isilon.com" -extensions v3_ca -config openssl.cnf</mark>

```bash
用原有的RSA密钥生成证书请求文件，指定-batch选项，不询问申请者的信息。主体信息由命令行subj指定，且输出公钥
-config openssl.cnf
-days  默认30天，与-x509一起使用
-extensions 指定可选sections 从configfile中， 与-x509一起使用
-subj Replaces the subject field of an input request, /type0=value0/type1=value1/type2=
-x509  输出自签名证书，而不是证书请求， 在这里就是CA。
#This is typically used to generate a test certificate or a self-signed root CA.
```




<mark>openssl x509 -req -in server.csr -out server.crt -signkey server.key -days 3650</mark>
```bash
x509: 是个公钥证书，主要有个sign certificate requests like a mini CA
-req: 期望有个证书请求csr（而不是证书本身）在input中，
-signkey: 使用提供的私钥对文件进行自签名
```




1. Create a Certificate Authority private key (this is your most important key):

`openssl req -new -newkey rsa:1024 -nodes -out ca.csr -keyout ca.key`

2. Create your CA self-signed certificate:

`openssl x509 -trustout -signkey ca.key -days 365 -req -in ca.csr -out ca.pem`

3. Issue a client certificate by first generating the key, then request (or use one provided by external system) then sign the certificate using private key of your CA:
```
openssl genrsa -out client.key 1024
openssl req -new -key client.key -out client.csr
openssl ca -in client.csr -out client.cer
```




#### 1.1.2. Manually stopping Pacific keycloak
`ps -ef | grep "/opt/keycloak" | awk '{print $2}' | xargs kill`



### 1.2. 了解CA， root CA， 以及自签名，CA签名

只有root 是自签名，其他都是CA签名
ROOT证书是用来签发和验证CA证书的，防止在不知道签名者私钥的情况下恶意篡改证书的内容
Root CA -> Server CA -> Server(最终用户)

<mark>其实就是一个使用私钥签发（issue）的关系。</mark>
1.      ROOT 证书
2.                    的私钥
3.                            签发   `用私钥加过密的sign签名`, `签名算法，通常是Sha256WithHash`
4.                                 CA 证书
5.                                           签发
6.                                                 x.509证书（公钥证书), 以.cert, .crt后缀，是ASN.1编码 Abstract syntax notation dot one 抽象语法标记
> 秘钥和证书都是 SEQUENCE 类型，
> 而 SEQUENCE 的 type 是 0x30，且长度是大于 127 的，因此第2 个字节是 0x82.   
> ASN.1 编码表示的数据是二进制数据，通常通过 BASE64 转化成字符串保存在 pem 文件中，  
> 而 0x3082 经过 BASE64 编码后，就是字符串 MI，<mark>因此所有 PEM 文件存储的秘钥开始的前两个字符是 MI</mark>。

```bash
pkcs#1 用于定义 RSA 公钥、私钥结构
pkcs#7 用于定义证书链
pkcs#8 用于定义任何算法公私钥
pkcs#12 用于定义私钥证书
X.509 定义公钥证书
```


![](/uploads/69094374817016.png)
![](vx_images/69094374817016.png)


#### 1.2.1. 看个keytool生成证书的范例


`keytool -genkeypair -storepass password -storetype PKCS12 -keyalg RSA -keysize 2048 -dname "CN=server" -alias server -ext "SAN:c=DNS:localhost,IP:127.0.0.1" -keystore conf/server.keystore`

-genkeypair  生成密钥对
```
Generates a key pair (a public key and associated private key).
              Wraps the public key into an X.509 v3 self-signed certificate,
              which is stored as a single-element certificate chain. This
              certificate chain and the private key are stored in a new
              keystore entry identified by alias.
```
-storepass 更改密钥库的存储口令,
-storetype   PKCS12 私钥证书
-keyalg      RSA, DSA, EC, DES, DESede
-keysize 
-dname
```
The dname value specifies the X.500 Distinguished Name to be
    associated with the value of alias, and is used as the issuer
    and subject fields in the self-signed certificate. If no
    distinguished name is provided at the command line, then the
    user is prompted for one.
```
-ext
-keystore
-validity
-keypass

```
SAN or SubjectAlternativeName
            Values: type:value(,type:value)*, where type can be EMAIL, URI,
            DNS, IP, or OID. The value argument is the string format value
            for the type.
```


## 2. genkeypair keyalg keysize
OpenSSL一共实现了4种非对称加密算法， 
DH算法一般用户密钥交换。RSA 算法既可以用于密钥交换，也可以用于数字签名，DSA算法则一般只用于数字签名
DH、
RSA、
DSA
椭圆曲线算法（EC）。


```bash
-genkeypair 生成密钥对      -keyalg算法   RSA             -keysize   2048  
                                         DSA                        1024 
                                         EC                         256
-genseckey 生成密钥          -keyalg      DES                        56   
                                         DESede                     168
 ```

##### 2.1.1.1. ROOT证书

##### 2.1.1.2. CA证书和

##### 2.1.1.3. 使用CA证签发的X.509证书

