---
layout: posts
title: RSA
date: 2023-03-13 23:34:02
updated: {{ date }}
tags: cirrus
categories: Teckknowledge
---



# Table of Contents
 <p>


```python
from Crypto import Random
from Crypto.PublicKey import RSA
```


```python
# 伪随机数生成器
random_generator = Random.new().read
# rsa算法生成实例
rsa = RSA.generate(2048, random_generator)
print(rsa)
print(dir(rsa))
```

    Private RSA key at 0x10C4B8EB0
    ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_d', '_decrypt', '_dp', '_dq', '_e', '_encrypt', '_invq', '_n', '_p', '_q', '_u', 'blind', 'can_encrypt', 'can_sign', 'd', 'decrypt', 'dp', 'dq', 'e', 'encrypt', 'exportKey', 'export_key', 'has_private', 'invp', 'invq', 'n', 'p', 'public_key', 'publickey', 'q', 'sign', 'size', 'size_in_bits', 'size_in_bytes', 'u', 'unblind', 'verify']



```python
# 私钥的生成
private_pem = rsa.exportKey()
print(private_pem)
with open("private.pem", "wb") as f:
    f.write(private_pem)
# 公钥的生成
public_pem = rsa.publickey().exportKey()
print(public_pem)
with open("public.pem", "wb") as f:
    f.write(public_pem)
```

    b'-----BEGIN RSA PRIVATE KEY-----\nMIIEowIBAAKCAQEAviJUuLHbi5CByimMRxCqFhQ7B+zga/Tk1k0D06+Lrm6Kb0HO\nvhIcmWIMCW1eeBV0Aa2PniH+qLjRhQMqz7tDhe6O6ZBc046p0CToOlaPyjFFrNvA\nP+jwqeaFFFcDNo0J9lXuv5TH4q4JvQennna/pQUQQg+pTPmC74ampQkq7p7vOb3Z\nvdRmU6IKDUKIIG/3JrWNQJ4lcmNymA9pt/QiduNik33EIRNf+5cHkD3hScrWqKOW\naQYnMdOfCet7/ItfKOIAucuV2GMetuGto+HETm0ZN8Lr05V+otqn0SsU4Wo6xTlL\nxFMIAS698RkZkqbDC0+86dHbT/fiW1MpYGcKVwIDAQABAoIBABEsaambCwECpu57\nLTv4Adznq+NN3oFxx7+hijBzITM6sc9VytY5LZMfG4Y4djlzepxyMFAwst9LfkU8\n4X+M4w70Wr02+GN1ddoik0U9r1QseiYgXS+Im0BFXYzWRSiGubkhzuRIDHvpi2Cj\nEd1KzYmauPq2jmyw9sYqy9+JQfL2iQ9f4mkTu9fN5H+h0Mkx4imFVbBG0cHlxBg+\noI5QAccrLlbVsHoFrykqtilu4kUPQ6u5KnK3xyrIS6QVoA2zsEf01ikua5DDY3UR\naFNNn0IM6U0T6nZiUkvV4ySXSxCsG8Kf0Kr26ly+10HUYniwcGgygtW/cijjjDaZ\nYWAwu0UCgYEAyRvz35KNZrOfQyn2d25QQp8HRKnL9wY8rzL0XojSPzK9T/yvctWa\n4HBxPuzd6oaUOJH9EGmR+/P6S/mfU3Qik7MOQJt7yVD7MWjpBcq0NFtjOD5lwYOV\n82tf7EG45hBIG+IDhFrzxWefQn1axhQeMeGc1U+1V3tq6TIKH8s/umMCgYEA8geE\n69WZWBW9hokSDZThsgETMO2jicAiyO4aKm6+71ibNNyP2wbq21Hk67oOThSaP5g0\nIMMMujEMqW7S3c9H0drCpCiUUB8Nni+uxd/MCbQbZnTCcqFEuyYsOAadmJQlzK/O\n0SxeZxfgOV0N6Lbo36v2SC7INYn2Yhfwcn8dWH0CgYB30i64KzISWbzvIGZXfCNX\nvjZvY5dBo7auT/anCG/z9YAz0wKZscjoJjZi3m/N1sci+WBE0hGHgzLC54RVDaG4\nTHuWZM0ZAiXXp4EG0WISu8xe61ZnOMYz1Oq+8d1/PX4pFr0vs50AJaAO1m8qCzx+\nTcTKlwYdjEwDiqvbi5Z5rwKBgQDWZ9TXuuRaRQAdk4X7pB2APDWNDafnWt810sA5\nQNxCWeM8o/uIU4tweQ7ryGntv5CZr7LWJxQ6SUNnQXbp6js8a6gsFoq0o53DuYgB\nYO121yfCzsKHG9gwVnOruiiYRv1pY4E6iiyi9WK8TnQI6ShJJSRK866Gx04NvhlS\nxMrxXQKBgGzb7nen/DoOhoasJLwTtFqUQI+xs09pkabi4oP0vYYXlYFdbhPoRNdZ\nzaxqSlPN4c/IqEpwv/gCplYrwdkb2+RfPbtIFtsEZIbhjR7qJSME+mqfrd3X4qlK\ngEeJ3howUYPd0y6ppepDQiAxcE4el8FDeV9OHPCfuQGzw0uB+QUe\n-----END RSA PRIVATE KEY-----'
    b'-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAviJUuLHbi5CByimMRxCq\nFhQ7B+zga/Tk1k0D06+Lrm6Kb0HOvhIcmWIMCW1eeBV0Aa2PniH+qLjRhQMqz7tD\nhe6O6ZBc046p0CToOlaPyjFFrNvAP+jwqeaFFFcDNo0J9lXuv5TH4q4JvQennna/\npQUQQg+pTPmC74ampQkq7p7vOb3ZvdRmU6IKDUKIIG/3JrWNQJ4lcmNymA9pt/Qi\nduNik33EIRNf+5cHkD3hScrWqKOWaQYnMdOfCet7/ItfKOIAucuV2GMetuGto+HE\nTm0ZN8Lr05V+otqn0SsU4Wo6xTlLxFMIAS698RkZkqbDC0+86dHbT/fiW1MpYGcK\nVwIDAQAB\n-----END PUBLIC KEY-----'



```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_v1_5 as Cipher_pkcs1_v1_5
import base64
```


```python
# 加密  注意每次加密后的字符串都不一样，跟对数据的padding 有关。
# 加密时支持的最大字节数：证书位数/8 -11（比如：2048位的证书，支持的最大加密字节数：2048/8 - 11 = 245）
# 1024位的证书，加密时最大支持117个字节，解密时为128；
# 2048位的证书，加密时最大支持245个字节，解密时为256。
# 如果需要加密的字节数超出证书能加密的最大字节数，此时就需要进行分段加密。
message = "Hello,This is RSA加密"

#拿到公钥
rsakey = RSA.importKey(open("public.pem").read())
print("Here is RSA 读取到的公钥:")
print(rsakey)

#通过公钥，创建用于执行pkcs1_v1_5加密或解密的密码
cipher = Cipher_pkcs1_v1_5.new(rsakey)
print('Here is 用公钥生成的密码:')
print(cipher)
# 用公钥创建的密码来加密message
message_enc = cipher.encrypt(message.encode('utf-8'))
print("Here is 用公钥创建的密码来加密的message:")
print(message_enc)
# base64 将二进制加密过的message转成文本
cipher_text = base64.b64encode(message_enc)
print("base64 将二进制转成文本的message")
print(cipher_text.decode('utf-8'))
```

    Here is RSA 读取到的公钥:
    Public RSA key at 0x10C4BDE50
    Here is 用公钥生成的密码:
    <Crypto.Cipher.PKCS1_v1_5.PKCS115_Cipher object at 0x10c4bd370>
    Here is 用公钥创建的密码来加密的message:
    b'<\xaf\xe3\xc1\x95MmL\x7f\x0b\xe6=J#ba\x98z\xb5\xb2\x9a\xb4\xbc\x9fC\x1c\xc0-\xfd\x80\xfc\xb3\x9c\xe4\x96\xa5\x8b?\xaa)\x84n9\x0c\x8f\x99?uXTp\xf3\xd6\x07lV\xe3\t\xbf\x8b\xea\xa5Dn\x80\xf5\x89\xeag\xa8J\xbc\x08\xabB\x1e\xc9\xbb\xe7m\xf8\t\xc6@\xc7[\x16,2\r\x1d=y?\xa0\x95\xcd\x8bxs\x8d@\x97R-\xea\xa3\xb1\xcc\x0f\x89\xb5\xe0:QPwj\xc0zY\xe9\xeb\x17\xf6\xd0\xef>\xd7*\x7f\x97\x8f\x0f=\x06\xb0<G\xe4\x00 pv\x8f<\xb5\xb4f\x0f\xbd\x06J8\x7f\x84M\xa4js\xc7\x1a\xa6\xaf\xe9:@\x88\x18\xed\x8f\x96fvI\xfd\xe2i\x0e\x87\xa1RR\xaaF\x15\x11\x99~NTLc\x12k\xa4\x86<\x11\xef\x05\xbe64\xf3\x84\xcb\x93\xd9\xf9\xa8R\xb6\xe7%\x95V\xb5\x80:\x81\xb4\xc0\xeaGC\xd1\xba\x90\xf2\x95#\x99>\xe5\xb1\xae\xabo[1\x1f\xed\x10\xd8\xac\x1d\xe6\x1e;\xe8\xd6\x1c\xc4r\x16'
    base64 将二进制转成文本的message
    SVJVsq5On4uFJo8OZdzuVtT/9NQexOoDiKUgeQr8v7j4PktuejMq4Ghoh6AufYwNJXSAyw1Oz6MFHfBxtSQeJU6jHN0ZlIhcv1DteMvv3VAOJRkhf93am7b97bg2KGoP/ux0W5RIKxKGTw0n8dsjhNt1X4lAFJSl3kA6n8RDaBWEzVhIzCq0fKjIZbYam/mB0WNB79/jezBj4Snv4DXX/dNSKmU6IAmV0FdTikX5lLj4xTGnUehSwzOxZzaKwhHEvUNq24W1WiIdvScFU3ycmK8OKvpTf7iEQYpyX9Hwqn5Qk+JHQAxpByOxG+TvI5pd4CPXxyQnkq2eWDOc1hhayw==



```python
# 解密
cipher_text = "SVJVsq5On4uFJo8OZdzuVtT/9NQexOoDiKUgeQr8v7j4PktuejMq4Ghoh6AufYwNJXSAyw1Oz6MFHfBxtSQeJU6jHN0ZlIhcv1DteMvv3VAOJRkhf93am7b97bg2KGoP/ux0W5RIKxKGTw0n8dsjhNt1X4lAFJSl3kA6n8RDaBWEzVhIzCq0fKjIZbYam/mB0WNB79/jezBj4Snv4DXX/dNSKmU6IAmV0FdTikX5lLj4xTGnUehSwzOxZzaKwhHEvUNq24W1WiIdvScFU3ycmK8OKvpTf7iEQYpyX9Hwqn5Qk+JHQAxpByOxG+TvI5pd4CPXxyQnkq2eWDOc1hhayw=="
encrypt_text = cipher_text.encode('utf-8')
# 拿到私钥
rsakey = RSA.importKey(open("private.pem").read())
print("Here is RSA 读取到的私钥:")
print(rsakey)
#创建用于执行pkcs1_v1_5加密或解密的密码
cipher = Cipher_pkcs1_v1_5.new(rsakey)      
print('Here is 用私钥生成的密码:')
print(cipher)
# base64 将文本string 转回二进制
message_dec = base64.b64decode(encrypt_text)
print("Here is 用base64将string转回二进制:")
print(message_enc)
# 用私钥生成的密码来解密 二进制message
text = cipher.decrypt(message_dec, "解密失败")
print("Here is 用私钥生成的密码来接二进制的message")
print(text.decode('utf-8'))
```

    Here is RSA 读取到的私钥:
    Private RSA key at 0x10C0D16D0
    Here is 用私钥生成的密码:
    <Crypto.Cipher.PKCS1_v1_5.PKCS115_Cipher object at 0x10c19f340>
    Here is 用base64将string转回二进制:
    b'<\xaf\xe3\xc1\x95MmL\x7f\x0b\xe6=J#ba\x98z\xb5\xb2\x9a\xb4\xbc\x9fC\x1c\xc0-\xfd\x80\xfc\xb3\x9c\xe4\x96\xa5\x8b?\xaa)\x84n9\x0c\x8f\x99?uXTp\xf3\xd6\x07lV\xe3\t\xbf\x8b\xea\xa5Dn\x80\xf5\x89\xeag\xa8J\xbc\x08\xabB\x1e\xc9\xbb\xe7m\xf8\t\xc6@\xc7[\x16,2\r\x1d=y?\xa0\x95\xcd\x8bxs\x8d@\x97R-\xea\xa3\xb1\xcc\x0f\x89\xb5\xe0:QPwj\xc0zY\xe9\xeb\x17\xf6\xd0\xef>\xd7*\x7f\x97\x8f\x0f=\x06\xb0<G\xe4\x00 pv\x8f<\xb5\xb4f\x0f\xbd\x06J8\x7f\x84M\xa4js\xc7\x1a\xa6\xaf\xe9:@\x88\x18\xed\x8f\x96fvI\xfd\xe2i\x0e\x87\xa1RR\xaaF\x15\x11\x99~NTLc\x12k\xa4\x86<\x11\xef\x05\xbe64\xf3\x84\xcb\x93\xd9\xf9\xa8R\xb6\xe7%\x95V\xb5\x80:\x81\xb4\xc0\xeaGC\xd1\xba\x90\xf2\x95#\x99>\xe5\xb1\xae\xabo[1\x1f\xed\x10\xd8\xac\x1d\xe6\x1e;\xe8\xd6\x1c\xc4r\x16'
    Here is 用私钥生成的密码来接二进制的message
    Hello,This is RSA加密



```python
# 加签名，用私钥，私钥加签名后，可以用公钥验证签名有无被篡改
from Crypto.Signature import PKCS1_v1_5 as Signature_pkcs1_v1_5
from Crypto.Hash import SHA
```


```python
# 加签 使用私钥加签名，每次签名是一致的
message = "This is a request message..."
# 拿到私钥
rsakey = RSA.importKey(open("private.pem").read())
print("私钥："+f"{rsakey}")

# 使用私钥拿到一个新的签名密码
signer = Signature_pkcs1_v1_5.new(rsakey)
print(f"使用私钥生成新的签名密码：{signer}")

# 摘要认证 digest authentication
"""
摘要访问认证是一种协议规定的Web服务器用来同网页浏览器进行认证信息协商的方法。它在密码发出前，先对其应用哈希函数，这相对于HTTP基本认证发送明文而言，更安全。

从技术上讲，摘要认证是使用随机数来阻止进行密码分析的MD5加密哈希函数应用。它使用HTTP协议。
"""
digest = SHA.new()
print("digest:{0}".format(digest))
# 用digest方式加密message
digest.update(message.encode("utf-8"))

# 加签，使用私钥生成的signature密码来加密 digest哈希过的message
sign = signer.sign(digest)
print(f"使用私钥生成的signature密码来加密 digest哈希过的message： {sign}")

# 用base64 将二进制转化为string存储
signature = base64.b64encode(sign)
print(signature.decode('utf-8'))
```

    私钥：Private RSA key at 0x10C4BD670
    使用私钥生成新的签名密码：<Crypto.Signature.pkcs1_15.PKCS115_SigScheme object at 0x10c198d90>
    digest:<Crypto.Hash.SHA1.SHA1Hash object at 0x10c4bd700>
    使用私钥生成的signature密码来加密 digest哈希过的message： b'&\xe7\x8b\xd1\xb5\xd2\xb3\x12\xccq\xf7\xf9\xb3\x94\\\xaaw\x8dJ\xc8\x9c\x89\xe5\xd3]\x16F\x91\xa1\x11\xff}\xe9=C+\x1c\xd0\x83\xdb\x13\x12<:\xd0\xcd>\x99uM\xf9\xf3\xbf\x8ax\xab\xac\x87\xca%\xc6\t\xef\xcb\xfb\xbc\xba\x1f\'\x19\xf0\xf9\xef\xdd\xb5N\xf4H\xca#\xe4\x9c\xa3\xee\x95\xa52Cq\x9bj\x91\xa6\xcf\x15\xcdt\xd2\xed\x10\xfe\xf7\xce\xde\xcc Y\xd3f\xe3.R\xf9\xe1@\xfc\x80\xd4\x9f\x9a\xa1\xf8\x8f\xc8\x8e\xbf\xd3w\xad\x1c\xcb\xb39\x83\x92UW\x17\x92,\xd3{$\xb2~\t\xb0\x1c\xa2\xaf\xe2\x91\xc1\x98\xd9;\x18\xeb\x1aW\\\x9e\n\xe5\xa3\xdd\x07\xacr\x1c\x9e\x18^\xe0xR\x8c\xafp\x938\xcc3<\xed-\xdb\x04\x9c\xa2\xdb\xb6\xecRn\xf7\xb5=\xcb\xdf\x8aP\x06\x82\xe65\xfa\xd2q:\x04\x8c\xea\x05\x1a +\x9d,&\x9e\x86\xc2\xa7\xf3\x8b\t\x9d\xa7\xd2\x93LO\n\xd3\xd9V\xa97R\t%\x8e\xde\xb7ZD\xdc\xb8\xc5\x9c"5\xe8\x14\xc5'
    JueL0bXSsxLMcff5s5RcqneNSsicieXTXRZGkaER/33pPUMrHNCD2xMSPDrQzT6ZdU3587+KeKush8olxgnvy/u8uh8nGfD57921TvRIyiPknKPulaUyQ3GbapGmzxXNdNLtEP73zt7MIFnTZuMuUvnhQPyA1J+aofiPyI6/03etHMuzOYOSVVcXkizTeySyfgmwHKKv4pHBmNk7GOsaV1yeCuWj3QeschyeGF7geFKMr3CTOMwzPO0t2wScotu27FJu97U9y9+KUAaC5jX60nE6BIzqBRogK50sJp6Gwqfziwmdp9KTTE8K09lWqTdSCSWO3rdaRNy4xZwiNegUxQ==



```python
# 验签，使用公钥验签
"""
用公钥生成的新签名密码来验证：
哈希过的message ，  和  私钥生成的新签名密码加密了的（哈希过的message）

"""
message = "This is a request message..."
# step1， 拿到公钥
rsakey = RSA.importKey(open("public.pem").read())
print(f"Get public key: {rsakey}")
# step2, 用公钥生成一个新签名密码
verifier = Signature_pkcs1_v1_5.new(rsakey)
print(f"generate a signature key via public key: {verifier}")

# step3, 生成个哈希值
hsmsg = SHA.new()
print(f"Hashi: {hsmsg}")

# step4, 用哈希加密message
hsmsg.update(message.encode("utf-8"))
print("哈希加密message: " + f"{hsmsg}")

# step5, 用base64 将string 的signature解密为二进制
# 就变成了加密阶段的那个 用私钥生成的signature密码来加密 digest哈希过的message
sign = base64.b64decode(signature)
print(f"base64 decode sig: {sign}")
# 用 公钥生成的新签名密码来验证，哈希过的message， 和 签过名的sign二进制
is_verify = verifier.verify(hsmsg, sign)
print(is_verify)
```

    Get public key: Public RSA key at 0x10C3B8880
    generate a signature key via public key: <Crypto.Signature.pkcs1_15.PKCS115_SigScheme object at 0x10c3b8a30>
    Hashi: <Crypto.Hash.SHA1.SHA1Hash object at 0x10c663160>
    哈希加密message: <Crypto.Hash.SHA1.SHA1Hash object at 0x10c663160>
    base64 decode sig: b'&\xe7\x8b\xd1\xb5\xd2\xb3\x12\xccq\xf7\xf9\xb3\x94\\\xaaw\x8dJ\xc8\x9c\x89\xe5\xd3]\x16F\x91\xa1\x11\xff}\xe9=C+\x1c\xd0\x83\xdb\x13\x12<:\xd0\xcd>\x99uM\xf9\xf3\xbf\x8ax\xab\xac\x87\xca%\xc6\t\xef\xcb\xfb\xbc\xba\x1f\'\x19\xf0\xf9\xef\xdd\xb5N\xf4H\xca#\xe4\x9c\xa3\xee\x95\xa52Cq\x9bj\x91\xa6\xcf\x15\xcdt\xd2\xed\x10\xfe\xf7\xce\xde\xcc Y\xd3f\xe3.R\xf9\xe1@\xfc\x80\xd4\x9f\x9a\xa1\xf8\x8f\xc8\x8e\xbf\xd3w\xad\x1c\xcb\xb39\x83\x92UW\x17\x92,\xd3{$\xb2~\t\xb0\x1c\xa2\xaf\xe2\x91\xc1\x98\xd9;\x18\xeb\x1aW\\\x9e\n\xe5\xa3\xdd\x07\xacr\x1c\x9e\x18^\xe0xR\x8c\xafp\x938\xcc3<\xed-\xdb\x04\x9c\xa2\xdb\xb6\xecRn\xf7\xb5=\xcb\xdf\x8aP\x06\x82\xe65\xfa\xd2q:\x04\x8c\xea\x05\x1a +\x9d,&\x9e\x86\xc2\xa7\xf3\x8b\t\x9d\xa7\xd2\x93LO\n\xd3\xd9V\xa97R\t%\x8e\xde\xb7ZD\xdc\xb8\xc5\x9c"5\xe8\x14\xc5'
    True



```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP
import binascii
```


```python
# 生成了个 private RSA KEY?????
keyPair = RSA.generate(3072)
print(f"keyPair: {keyPair}")
```

    keyPair: Private RSA key at 0x10C663940



```python
pubKey = keyPair.publickey()
print(f"Public key:  (n={hex(pubKey.n)}, e={hex(pubKey.e)})")
pubKeyPEM = pubKey.exportKey()
print(pubKeyPEM.decode('ascii'))
```

    Public key:  (n=0xa1ed1f3d0be71914eee28da05492820df0f21666e64a68ba698f93933d2b2ffe71fa3a8221da24354dbb5b756dd64d70696fe650dd607d2a255fbbae4f668a2115ca22b172a0d9d694adc160713b8508f0ffe403c89b7dcf6a388e2bfb58c57c920841945e13edbd0c8baf6c921b7af4647b545ed5e404776fb0e2374ef4e343f32b1cfbba3b76ff57957fc6f67924472485c3bd96ef67ce79e05f0229621257fd803e3fdcfa026b481bd61c6e202821d4b829e0393bc7137c444180b81fc6bccc34f327da2dbd5daa750d60cdca3a5a026e11c25e2d74c5ea43b54640a93c0daf94f5e5eece42fbc9ae0efb65af7503ab3ab5415dd87d4a49c6de79ffb4a1c3682108456c514822cc11d6f849eb88f083292b5128995bf77594448946af5fba1be4d7612fc79cf8f37e8b692c151500441d5d47c989cbadd7f3dee4ad188faf1e4b52fcab8ab483c6324356994215ed080f23451c700e3c89d12ea608a7b96728a025b4a657f0f25f82dc0a27ea852c3294cee81cff2a692df08c7833cd1fd5, e=0x10001)
    -----BEGIN PUBLIC KEY-----
    MIIBojANBgkqhkiG9w0BAQEFAAOCAY8AMIIBigKCAYEAoe0fPQvnGRTu4o2gVJKC
    DfDyFmbmSmi6aY+Tkz0rL/5x+jqCIdokNU27W3Vt1k1waW/mUN1gfSolX7uuT2aK
    IRXKIrFyoNnWlK3BYHE7hQjw/+QDyJt9z2o4jiv7WMV8kghBlF4T7b0Mi69skht6
    9GR7VF7V5AR3b7DiN07040PzKxz7ujt2/1eVf8b2eSRHJIXDvZbvZ8554F8CKWIS
    V/2APj/c+gJrSBvWHG4gKCHUuCngOTvHE3xEQYC4H8a8zDTzJ9otvV2qdQ1gzco6
    WgJuEcJeLXTF6kO1RkCpPA2vlPXl7s5C+8muDvtlr3UDqzq1QV3YfUpJxt55/7Sh
    w2ghCEVsUUgizBHW+EnriPCDKStRKJlb93WURIlGr1+6G+TXYS/HnPjzfotpLBUV
    AEQdXUfJicut1/Pe5K0Yj68eS1L8q4q0g8YyQ1aZQhXtCA8jRRxwDjyJ0S6mCKe5
    ZyigJbSmV/DyX4LcCifqhSwylM7oHP8qaS3wjHgzzR/VAgMBAAE=
    -----END PUBLIC KEY-----



```python
print(f"Private key: (n={hex(pubKey.n)}, d={hex(keyPair.d)})")
privKeyPEM = keyPair.exportKey()
print(privKeyPEM.decode('ascii'))
```

    Private key: (n=0xa1ed1f3d0be71914eee28da05492820df0f21666e64a68ba698f93933d2b2ffe71fa3a8221da24354dbb5b756dd64d70696fe650dd607d2a255fbbae4f668a2115ca22b172a0d9d694adc160713b8508f0ffe403c89b7dcf6a388e2bfb58c57c920841945e13edbd0c8baf6c921b7af4647b545ed5e404776fb0e2374ef4e343f32b1cfbba3b76ff57957fc6f67924472485c3bd96ef67ce79e05f0229621257fd803e3fdcfa026b481bd61c6e202821d4b829e0393bc7137c444180b81fc6bccc34f327da2dbd5daa750d60cdca3a5a026e11c25e2d74c5ea43b54640a93c0daf94f5e5eece42fbc9ae0efb65af7503ab3ab5415dd87d4a49c6de79ffb4a1c3682108456c514822cc11d6f849eb88f083292b5128995bf77594448946af5fba1be4d7612fc79cf8f37e8b692c151500441d5d47c989cbadd7f3dee4ad188faf1e4b52fcab8ab483c6324356994215ed080f23451c700e3c89d12ea608a7b96728a025b4a657f0f25f82dc0a27ea852c3294cee81cff2a692df08c7833cd1fd5, d=0x1b6c1223947b1e9447a061110b41847a37ee723cc58df463c80763469c254ee0c30ca2bbf1b504f76ca2936a8a4a05f348e7da694cbefa736e163857dcba7b74a996ac7ec09adcf3ff59d855d781eceb06baa48814d66b07fa9a794e997cff1f89863c6a3e99c163dd56b543fdba5efd8f1c13bdbbc5fc6652fe9c50cc33a209453517ee1ed67de08aef41246b63e3463e63d387ad4d5341c9acbb14f2a283d8c2e58fa5366c60872a9461938efd8323b7928e04f05709efbd8111ac158276cab2457512d111abba989e2ef5c0cc8309a32173c206eee8a10102bb20f19c76c1182759052e34a120bb87c220df920f11019b3ebda111c634ccc94937acb0ef23d7114cd13a8a154c909efe9700fae3fabae91529d4bf6dc70aea942ba5128c4d61eef66a857fbfd47cd30b20595c3a609a0348a3cfeca5c5bd3c5e6174e042c16a6262fcac07604536d1a1037ee4c88d6df0713347957e9a1185b30c25c4842a9b184eff70533e799cfb8519c414ea84a5936f4e16d5841175f3bd0d0436705)
    -----BEGIN RSA PRIVATE KEY-----
    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
    -----END RSA PRIVATE KEY-----



```python
#encryption
msg = 'A message for encryption'
encryptor = PKCS1_OAEP.new(pubKey)
encrypted = encryptor.encrypt(msg.encode("utf-8"))
print("Encrypted:", binascii.hexlify(encrypted))
```

    Encrypted: b'7afbfdb44b4b507b4e000a0b16ff3b64e9b570f932b3467b06b69d2071f92886f62f612f57caa9d1a46189759f51f46784388f7e52ee08f2434a8f7918e5e92a639f32bcf8c5ec36ce5cb5328279b793e33c1a9dc4af373875b9b230772d51f9818292b178153f172ca8ce5aea84396032b4ab26f29ba7808ef335d072cfaca61cc311e1542cc404c11fed5db0349ee4f56568e702bfc3a2203f521a282b1cd5b22a8f6d0c731589da8169b078c9be5d00b8df31b40a26f4e92d8d049dd3b9f5bd43d9c1680688cb81af3bca4bb44c8a8227941f5a874c7fb043e7dd9351117e1d286b6ac26486514681805c66b8bab5ebad7d7f9556a14678342c6278268bab1e98867ece9c253b48e4ca71411bc9728b7844b1ec1c007c6006701b3951f4e7b5b4558b140121422960a7fb1e1f8a9be1cb499be33517602d7a231d69741d0a50318e0bd3ce771f62b6161c0f945da9bfe28a05ccebfabedc38821f7048a5c6c6288435757d632192386ba0f607a6f96fe41fafbbf26dd6aafe46ddeb313de9'



```python
import rsa
```


```python
publicKey, privateKey = rsa.newkeys(2024)
message = "hello geeks"

encMessage = rsa.encrypt(message.encode("utf-8"), publicKey)
print("original string: ", message)
print("encrypted string: ", encMessage)

# the encrypted message can be decrypted
# with ras.decrypt method and private key
# decrypt method returns encoded byte string,
# use decode method to convert it to string
# public key cannot be used for decryption
decMessage = rsa.decrypt(encMessage, privateKey).decode()

print("decrypted string: ", decMessage)
```

    original string:  hello geeks
    encrypted string:  b"\x1f~m\x87\xe0_\x8f\x89w1B$|l\x04t$\xa8j\xd4\xc7\xbdr>y\xfa\xd1c\xc2\nm\x88\xd02\x05\x999\xae\x96O\xf8s\x83\xe3\xccw\xcc\xc5c\x97\xec\xdc\xefpe\xe9xkg\xdan\x83\xafzYJv\xdb\xf8\x8a\xa3\x1c\t\xb0\x96\xa6\xbci\xeb`'\xe9\x15\x15\x97\x10\xca\x867\xa1\xb2d\xfc\xc4~:\xc5Q\xa1\xbe\x1aq-6\xc8\xd3>'!H\x15\x03F\x94V\xb5e\xaaBp\x13Nx\\\xac\xe1\x96\xf2\xde2\x98\xe1\xe0;_|\xefa\x17b\x9a\x1b@\x0f\xe9%\xd9\xfe@g\xecDxA\x8cS\x12\x8fI#\xf6\xa04qN\xe9g\x1f4Y\x19\x06\xff\xa1\xa0\xad\xec\x983Y\xd8%\xb1L\xa2r\xd8wr3\xbb\xa5\x06\xd7o\xed\xc4\xb5:JGr\xc2\xdb\x8bz\xb3$-\x89Xj\\2\xc5\x1f\xac\xa1\xfdm\xf5\x02m=\x9a\x0b\xd7\xb3\xc1\x80\xffY\\\x06V\x868\x81\xe54\t\x81\xaf\xae\xf7\xdd\x18\xb2{\xec\xb7\x9e`"
    decrypted string:  hello geeks



```python

```
