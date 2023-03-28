---
layout: posts
title: OpenSSL2:self-signed certificate as root CA
date: 2023-03-28 22:41:02
updated: {{ date }}
tags: cirrus, openssl
categories: Teckknowledge
password: BAIYUN01++
---

# openssl_2 self-signed certificate as root CA
 
## 1. Generate rootCA

Generate private key as rootCA.key;

Self-signed certificate with the private key as rootCA certificate named rootCA.pem;




### 1.1. Pre-config, create a config file named ca.cnf, context as below:
```bash
[req]
distinguished_name = req_distinguished_name

[req_distinguished_name]
countryName = Country Name (2 letter code)
countryName_default = CN
stateOrProvinceName = State or Province Name (full name)
stateOrProvinceName_default = Shanghai
localityName = Locality Name (eg, city)
localityName_default = Shanghai
organizationName = Organization Name (eg, company)
organizationName_default = Dell
organizationalUnitName  = Organizational Unit Name (eg, section)
organizationalUnitName_default  = Dell
commonName = syslog.west.isilon.com
commonName_max  = 64

[v3_ca]
basicConstraints = critical,CA:TRUE
keyUsage         = critical,keyCertSign
subjectAltName = @alt_names

[alt_names]
DNS.1 = localhost
DNS.2 = west.isilon.com
DNS.3 = keycloak-ali1.west.isilon.com
CIAM-ali1:~/CAcerts #
```





### 1.2. Generate the private key: rootCA.key
### 1.3. Generate the signed crt: rootCA.pem
```bash
CIAM-ali1:~/CAcerts # openssl req -x509 -sha256 -days 1825 -newkey rsa:2048 -keyout rootCA.key -out rootCA.pem -extensions v3_ca -config ca.cnf
Generating a RSA private key
.................+++++
...............................................+++++
writing new private key to 'rootCA.key'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [CN]:CN
State or Province Name (full name) [Shanghai]:Shanghai
Locality Name (eg, city) [Shanghai]:Shanghai
Organization Name (eg, company) [Dell]:Dell
Organizational Unit Name (eg, section) [Dell]:Dell
syslog.west.isilon.com []:west.isilon.com
```

## 2. Generate private key, signed-CA on the server end.
### 2.1. Pre-config, create a file to involve all of the needed parameters.
#### 2.1.1. CA part is for the point 2.3
#### 2.1.2. Also need to create the dir, new_certs_dir, database, serial

```bash
CIAM-ali1:~/CAcerts # cat v3_server.cnf


[ ca ]
default_ca      = CA_default

[ CA_default ]

dir             = /root/CAcerts
new_certs_dir   = $dir/certs/
database        = $dir/index.txt
serial          = $dir/serial

certificate     = rootCA.pem
private_key     = rootCA.key
default_days    = 365
default_md      = sha256
preserve        = no
policy          = policy_match


[ policy_match ]
countryName             = optional
stateOrProvinceName     = optional
organizationName        = optional
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional

[req]
distinguished_name = req_distinguished_name
req_extensions = v3_server

[req_distinguished_name]
countryName = Country Name (2 letter code)
countryName_default = CN
stateOrProvinceName = State or Province Name (full name)
stateOrProvinceName_default = Shanghai
localityName = Locality Name (eg, city)
localityName_default = Shanghai
organizationName = Organization Name (eg, company)
organizationName_default = Dell
organizationalUnitName  = Organizational Unit Name (eg, section)
organizationalUnitName_default  = Dell
commonName = syslog.west.isilon.com
commonName_max  = 64


[v3_server]
basicConstraints = CA:FALSE
subjectKeyIdentifier = hash
extendedKeyUsage = serverAuth
subjectAltName = @alt_names_s

[alt_names_s]
DNS.3 = ciam-ali1.west.isilon.com
```

### 2.2. Generate private key: server.key,  and csr: server.csr
```bash
CIAM-ali1:~/CAcerts # openssl req -new -newkey rsa:2048 -nodes -out server.csr -keyout server.key -extensions v3_server -config v3_server.cnf
Generating a RSA private key
.....................................+++++
................................................................+++++
writing new private key to 'server.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [CN]:CN
State or Province Name (full name) [Shanghai]:Shanghai
Locality Name (eg, city) [Shanghai]:Shanghai
Organization Name (eg, company) [Dell]:Dell
Organizational Unit Name (eg, section) [Dell]:Dell
syslog.west.isilon.com []:ciam-ali1.west.isilon.com
```



### 2.3. Issue CA with the server.csr, signed by the rootCA.pem and rootCA.key
```bash
CIAM-ali1:~/CAcerts #  openssl ca -batch -notext -in server.csr -out server.pem   -extensions v3_server -config v3_server.cnf
Using configuration from v3_server.cnf
Enter pass phrase for rootCA.key:
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
countryName           :PRINTABLE:'CN'
stateOrProvinceName   :ASN.1 12:'Shanghai'
localityName          :ASN.1 12:'Shanghai'
organizationName      :ASN.1 12:'Dell'
organizationalUnitName:ASN.1 12:'Dell'
commonName            :ASN.1 12:'ciam-ali1.west.isilon.com'
Certificate is to be certified until Mar 26 08:22:59 2024 GMT (365 days)

Write out database with 1 new entries
Data Base Updated
```
### 2.4. View the issued CA
```bash
CIAM-ali1:~/CAcerts # openssl x509 -text -noout -in server.pem
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 4096 (0x1000)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = CN, ST = Shanghai, L = Shanghai, O = Dell, OU = Dell
        Validity
            Not Before: Mar 27 08:22:59 2023 GMT
            Not After : Mar 26 08:22:59 2024 GMT
        Subject: C = CN, ST = Shanghai, O = Dell, OU = Dell, CN = ciam-ali1.west.isilon.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
                    00:e8:53:ef:bd:b0:e9:99:2b:af:35:da:2e:b5:81:
                    61:18:17:7a:7b:69:a0:46:c1:c2:ee:24:d7:36:52:
                    e9:5c:3d:49:ea:be:31:54:e0:a5:ff:a9:e1:19:b3:
                    81:8a:38:9e:a3:fa:62:23:b0:85:07:d1:35:3e:40:
                    7c:1a:68:1d:a8:6b:08:01:c8:95:d3:85:09:b0:7c:
                    60:85:dc:13:dc:91:a2:73:ee:f3:7e:42:80:99:24:
                    4f:09:13:91:9b:da:87:ee:ae:f2:ec:d9:9e:a6:75:
                    1d:e3:c6:cf:78:58:7d:2a:7e:8e:cf:65:30:60:ab:
                    42:6b:6d:76:f8:f2:46:de:c3:b5:4b:c9:60:39:24:
                    a0:5e:59:0c:b3:30:9e:3b:67:ae:d4:5f:53:6e:63:
                    47:92:dc:0c:39:9f:a6:39:b0:45:82:5b:e1:4d:a8:
                    ed:75:96:8a:98:32:49:f6:9b:a3:6e:69:91:f0:18:
                    2e:66:9e:ca:77:7e:b3:a6:96:d6:4a:e1:b1:51:a3:
                    1f:7d:d2:5f:41:ce:1c:02:c2:42:aa:b5:3b:61:1c:
                    9f:b9:45:70:fe:85:d0:d8:a3:6f:e5:e8:3d:9f:9e:
                    6d:53:57:f9:0f:0a:56:a4:f5:77:bb:4a:ff:fc:8a:
                    4a:67:e1:82:43:f1:7b:16:5a:80:1b:8f:da:f4:c4:
                    25:0d
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            X509v3 Subject Key Identifier:
                D1:4F:3F:CD:DC:17:7E:50:38:A8:50:D6:EA:2C:BA:3D:6C:62:35:A1
            X509v3 Extended Key Usage:
                TLS Web Server Authentication
            X509v3 Subject Alternative Name:
                DNS:ciam-ali1.west.isilon.com
    Signature Algorithm: sha256WithRSAEncryption
         09:6b:25:4e:ca:a1:fc:2c:bb:35:5f:65:c8:47:92:7a:a1:53:
         b7:fd:6e:41:5a:39:32:35:87:b1:24:de:83:6e:23:6c:67:49:
         47:ba:91:de:7a:b1:81:f3:0a:3a:d3:25:35:13:6d:0f:d9:77:
         a4:6a:7e:a6:72:06:d9:b1:95:15:36:61:46:a4:b7:0f:16:c3:
         41:55:aa:e1:5c:93:28:4b:f0:9b:74:04:a1:08:59:97:23:c9:
         1b:17:d0:38:7d:74:e4:9a:42:e2:f7:0a:2d:01:8d:d3:7f:57:
         eb:b8:08:2e:fe:5f:b1:86:90:d2:97:64:9e:9a:61:51:06:9e:
         7c:25:a9:87:5c:63:e6:d5:18:9b:cb:0e:20:71:77:06:fd:2e:
         0f:c1:b9:21:75:34:ec:5a:11:4d:48:f8:4d:de:67:7a:f4:e6:
         3c:a1:4a:01:40:8f:ce:2c:c1:ae:5e:52:66:0d:6c:29:7b:f0:
         b3:ce:ae:b2:cf:13:d1:0e:ed:83:e3:15:16:5a:8c:ba:15:45:
         08:46:b6:63:92:66:ab:2c:2c:1c:f6:2c:34:d9:7d:9f:b6:d3:
         07:34:72:bb:89:81:27:d0:88:0f:b9:3e:e8:f2:3a:3c:7f:5c:
         88:88:1b:77:7a:41:a9:c9:99:2f:e5:76:87:8e:26:ec:85:c0:
         12:ac:02:25
CIAM-ali1:~/CAcerts #
```

## 3. Generate private key, signed-CA on the Client end
### 3.1. Pre-config, create a file to involve all of the needed parameters.
```bash
[ ca ]
default_ca      = CA_default

[ CA_default ]

dir             = /root/CAcerts
new_certs_dir   = $dir/certs/
database        = $dir/index.txt
serial          = $dir/serial

certificate     = rootCA.pem
private_key     = rootCA.key
default_days    = 365
default_md      = sha256
preserve        = no
policy          = policy_match


[ policy_match ]
countryName             = optional
stateOrProvinceName     = optional
organizationName        = optional
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional

[req]
distinguished_name = req_distinguished_name

[req_distinguished_name]
countryName = Country Name (2 letter code)
countryName_default = CN
stateOrProvinceName = State or Province Name (full name)
stateOrProvinceName_default = Shanghai
localityName = Locality Name (eg, city)
localityName_default = Shanghai
organizationName = Organization Name (eg, company)
organizationName_default = Dell
organizationalUnitName  = Organizational Unit Name (eg, section)
organizationalUnitName_default  = Dell
commonName = syslog.west.isilon.com
commonName_max  = 64

[v3_client]
basicConstraints = CA:FALSE
subjectKeyIdentifier = hash
extendedKeyUsage = clientAuth
subjectAltName = @alt_names_c

[alt_names_c]
DNS.1 = keycloak-ali1.west.isilon.com
```



### 3.2. Generate private key: keycloak.key,  and csr: keycloak.csr
```bash
CIAM-ali1:~/CAcerts # openssl req -new -newkey rsa:2048 -nodes -out keycloak.csr -keyout keycloak.key -extensions v3_client -config v3_client.cnf
Generating a RSA private key
...+++++
....+++++
writing new private key to 'keycloak.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [CN]:CN
State or Province Name (full name) [Shanghai]:Shanghai
Locality Name (eg, city) [Shanghai]:Shanghai
Organization Name (eg, company) [Dell]:Dell
Organizational Unit Name (eg, section) [Dell]:Dell
syslog.west.isilon.com []:keycloak-ali1.west.isilon.com
```





### 3.3. Issue CA with the keycloak.csr, signed by the rootCA.pem and rootCA.key
```bash
CIAM-ali1:~/CAcerts # openssl ca -batch -notext -in keycloak.csr -out keycloak.crt -extensions v3_client -config v3_client.cnf
Using configuration from v3_client.cnf
Enter pass phrase for rootCA.key:
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
countryName           :PRINTABLE:'CN'
stateOrProvinceName   :ASN.1 12:'Shanghai'
localityName          :ASN.1 12:'Shanghai'
organizationName      :ASN.1 12:'Dell'
organizationalUnitName:ASN.1 12:'Dell'
commonName            :ASN.1 12:'keycloak-ali1.west.isilon.com'
Certificate is to be certified until Mar 26 08:38:25 2024 GMT (365 days)

Write out database with 1 new entries
Data Base Updated
```

### 3.4. View the issued CA
```bash
CIAM-ali1:~/CAcerts # openssl x509 -text -noout -in keycloak.crt
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 4097 (0x1001)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = CN, ST = Shanghai, L = Shanghai, O = Dell, OU = Dell
        Validity
            Not Before: Mar 27 08:38:25 2023 GMT
            Not After : Mar 26 08:38:25 2024 GMT
        Subject: C = CN, ST = Shanghai, O = Dell, OU = Dell, CN = keycloak-ali1.west.isilon.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
                    00:cb:87:42:78:91:f8:a7:2a:21:4c:7d:89:58:0f:
                    3d:a0:a7:93:6a:45:36:1c:29:d5:f9:a7:52:10:9d:
                    90:5b:39:d5:2b:6a:24:e6:ac:97:90:bf:e1:24:92:
                    10:bd:c0:21:14:35:db:5d:0d:8f:af:cf:62:2a:17:
                    cc:87:de:b8:8e:a3:41:6c:99:f0:37:f1:58:d4:61:
                    5e:9e:d0:4a:3e:b1:28:0a:26:64:f8:a8:76:ae:28:
                    ef:2b:bd:23:dd:7c:bd:cb:0c:7b:e7:94:1c:08:15:
                    2b:55:6b:d4:7f:27:58:83:8a:f4:d9:1a:fc:0d:76:
                    5d:de:87:56:74:53:00:49:6d:80:c2:2c:71:4a:db:
                    46:f4:bd:fb:ad:90:2b:32:6e:90:02:04:ae:44:bb:
                    b3:4a:0b:88:6f:0a:5b:fb:eb:08:18:7d:d3:2c:72:
                    27:73:8a:e0:8b:78:18:86:85:52:d2:53:1f:bb:17:
                    2b:62:3c:74:ec:51:f5:67:d2:0a:7c:94:7e:b5:56:
                    00:4a:11:c4:44:17:7e:de:ce:26:1f:89:c2:9d:80:
                    88:c3:a3:f8:c9:4d:68:dc:40:3f:60:2f:9c:ab:ba:
                    7a:da:d1:7a:41:5f:e9:b0:b3:5b:4f:9d:ca:4f:70:
                    9d:42:4b:78:77:ed:57:73:9f:28:05:37:66:8f:33:
                    c9:df
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            X509v3 Subject Key Identifier:
                04:E5:D2:A9:A1:75:A9:84:1C:00:82:0D:D0:7E:2C:53:97:B7:94:47
            X509v3 Extended Key Usage:
                TLS Web Client Authentication
            X509v3 Subject Alternative Name:
                DNS:keycloak-ali1.west.isilon.com
    Signature Algorithm: sha256WithRSAEncryption
         99:e4:9f:6c:58:54:d4:ec:6c:dc:09:de:1e:64:cc:fd:d8:0d:
         90:89:ab:11:8b:c9:79:b4:5a:f6:84:c4:10:f6:81:59:6a:6d:
         35:3e:58:61:d1:cd:cf:d9:ad:a0:2a:cd:0f:5f:e0:d6:57:10:
         d7:e4:69:71:ca:46:fd:23:8d:8a:eb:6a:14:95:a0:d0:4b:02:
         20:75:75:73:83:22:8f:cb:91:1e:f1:06:69:c6:4d:f8:b7:45:
         c1:e5:e2:cf:72:0f:81:ac:bd:ee:57:f2:9c:79:ff:04:68:e1:
         0f:e2:b3:9a:bd:52:18:6a:1f:6f:b2:62:65:eb:e6:ee:9f:6f:
         a5:89:96:6e:a5:5c:27:ad:35:b5:7c:45:5a:5c:7e:9a:ed:a7:
         65:46:44:a6:af:ed:e2:41:92:b9:e0:dc:ad:64:9b:6a:16:59:
         6b:d2:59:fa:d0:ec:98:2e:32:34:6a:48:b1:cd:2a:fc:4f:62:
         38:ca:d0:a8:f1:41:e2:0a:13:29:50:8b:8e:d8:73:5e:93:f9:
         0d:6b:58:f5:2d:84:bb:72:32:ef:e7:f6:73:f6:ab:63:49:7a:
         15:be:d7:2a:4b:46:19:0c:05:e2:5d:5b:4e:46:bd:41:7a:cb:
         68:3e:e2:72:4b:92:20:1a:34:cb:70:1d:2d:26:de:ef:77:f3:
         ec:9a:10:ee
CIAM-ali1:~/CAcerts #
```

## 4. Generate private key, signed-CA on the RSA end
### 4.1. Pre-config, add parameters into the v3_server_rsa.cnf
```conf
[req]
distinguished_name = req_distinguished_name
req_extensions = v3_req

[req_distinguished_name]
countryName = Country Name (2 letter code)
countryName_default = CN
stateOrProvinceName = State or Province Name (full name)
stateOrProvinceName_default = Shanghai
localityName = Locality Name (eg, city)
localityName_default = Shanghai
organizationName = Organization Name (eg, company)
organizationName_default = Dell
organizationalUnitName  = Organizational Unit Name (eg, section)
organizationalUnitName_default  = Dell
commonName = Common Name (e.g. server FQDN or YOUR name)
commonName_default = localhost
commonName_max  = 64

[v3_req]
basicConstraints = CA:FALSE
extendedKeyUsage=serverAuth
subjectAltName = @alt_names

[alt_names]
DNS.1 = west.isilon.com
DNS.2 = localhost
DNS.3 = remotedev-ali1-rdu.west.isilon.com
IP.1 = 127.0.0.1
IP.2 = 10.224.66.91
```

### 4.2. Generate private key: rsa.key,  and csr: rsa.csr
```bash
ali1@remotedev-ali1-rdu:~/git/mock_rsa> openssl req -new -newkey rsa:4096 -sha256 -nodes -out rsa.csr -keyout rsa.key -subj "/C=CN/ST=Shanghai/L=Shanghai/O=Dell/OU=Dell/CN=CIAM" -config v3_server.cnf -extensions v3_req
Generating a RSA private key
............................++++
..........................................................................................................................................................++++
writing new private key to 'rsa.key'
-----
```

### 4.3. Issue CA with the rsa.csr, signed by the rootCA.pem and rootCA.key
```bash
ali1@remotedev-ali1-rdu:~/git/mock_rsa/certsrsa> openssl x509 -days 365 -req -in rsa.csr -CA rootCA.pem -CAkey rootCA.key -set_serial 01 -out rsa.crt  -CAcreateserial -extfile v3_server.cnf -extensions v3_req  -passin pass:ciam-ali1.west.isilon.com
Signature ok
subject=C = CN, ST = Shanghai, L = Shanghai, O = Dell, OU = Dell, CN = CIAM
Getting CA Private Key
```

### 4.4. View the issued CA
```
CIAM-ali1:~/CAcerts # openssl x509 -text -noout -in rsa.crt
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 4098 (0x1002)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = CN, ST = Shanghai, L = Shanghai, O = Dell, OU = Dell
        Validity
            Not Before: Mar 28 06:43:54 2023 GMT
            Not After : Mar 27 06:43:54 2024 GMT
        Subject: C = CN, ST = Shanghai, O = Dell, OU = Dell, CN = remotedev-ali1-rdu.west.isilon.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
                    00:cb:07:4b:74:27:d3:21:60:01:9b:be:10:e3:8f:
                    f4:11:4a:92:13:0b:81:6f:8f:89:91:a9:7e:37:92:
                    9a:52:31:a8:0a:56:4f:85:54:a4:f2:7e:e0:62:fa:
                    2f:f3:fb:fd:92:88:21:5a:a7:bc:b5:85:eb:7c:af:
                    03:a0:76:2b:41:8d:2b:bf:ff:a0:d7:84:47:82:65:
                    de:27:cb:87:8b:45:8c:1b:75:ec:84:85:8f:7d:7b:
                    d4:f0:36:8b:47:70:7c:db:71:7c:3c:83:38:2f:89:
                    fb:21:d4:46:b8:0b:b9:2e:fa:0b:6e:b9:09:f4:da:
                    c4:46:a0:a8:77:ef:a0:5e:c5:08:6f:f3:c9:a0:88:
                    47:49:09:f8:be:46:06:2f:74:c7:f4:56:48:06:b7:
                    5a:cd:27:c2:20:3f:ca:39:88:43:62:2c:88:6e:2f:
                    f0:9e:c7:cf:27:60:0b:8e:39:2f:06:ed:1b:bd:b9:
                    41:09:8e:f1:9a:dd:b7:da:02:76:95:9d:f2:32:2b:
                    af:73:93:09:3d:03:e1:a6:db:43:d5:e3:70:5a:e4:
                    2b:e0:f1:6a:37:34:64:2a:a1:30:eb:3e:7c:cf:c7:
                    1c:34:f1:8d:d6:37:55:d4:9b:ce:14:8a:3f:8a:22:
                    d2:42:f6:97:34:fd:51:58:16:3e:fa:43:1c:ba:dd:
                    8c:a3
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            X509v3 Subject Key Identifier:
                87:64:BE:F0:E4:AA:FA:5E:DA:90:D1:05:81:78:C6:B0:39:A1:5B:58
            X509v3 Extended Key Usage:
                TLS Web Client Authentication
            X509v3 Subject Alternative Name:
                DNS:rsa-ali1.west.isilon.com, DNS:remotedev-ali1-rdu.west.isilon.com
    Signature Algorithm: sha256WithRSAEncryption
         44:4c:4e:2d:79:3d:04:64:8c:cd:33:9f:e8:6b:71:98:e8:13:
         c4:3c:af:cd:bf:d2:83:80:71:4c:78:1f:ac:36:2a:9c:78:34:
         1c:b7:62:fa:58:09:d5:3b:04:96:fd:a2:c3:41:f8:b0:24:ba:
         90:fd:54:e5:75:8c:4a:1d:41:57:d9:14:1e:ee:93:8d:a0:cf:
         9e:ab:58:27:17:40:5d:19:d2:a2:78:5c:6b:4d:4f:5e:64:33:
         09:34:eb:53:8d:65:06:ea:b2:e4:31:e0:19:3d:44:e6:50:65:
         af:8c:2b:33:a9:02:cc:79:19:fd:4e:c0:ff:29:f8:22:48:0a:
         a7:f3:47:ed:cc:0c:2c:d3:88:5c:54:98:1e:28:84:0a:98:08:
         05:16:21:cc:11:cf:71:0c:8b:19:d7:47:ce:e0:49:56:d8:05:
         d0:3e:93:de:ad:7a:ec:9a:17:17:6b:b1:8f:d3:1c:f1:88:4d:
         df:db:54:31:71:0e:fd:9f:fc:a3:91:cb:f4:4a:de:46:18:9a:
         81:8e:8c:f4:30:92:7b:5a:0a:3a:50:21:1c:25:08:f2:db:f9:
         fd:d2:26:ea:f9:a2:49:83:f7:63:7d:62:04:7e:4b:a1:5c:da:
         88:b6:02:34:e4:84:d6:42:22:b3:a7:1c:ea:7c:d4:c0:82:a9:
         2a:3d:0b:e5
CIAM-ali1:~/CAcerts #
```

## 5. Generated certs:

```tree
├── 1
├── a.pem
├── ca.cnf
├── certs
│   ├── 1000.pem
│   ├── 1001.pem
│   └── 1002.pem
├── index.txt
├── index.txt.attr
├── index.txt.attr.old
├── index.txt.old
├── keycloak.crt
├── keycloak.csr
├── keycloak.key
├── rootCA.key
├── rootCA.pem
├── rsa.crt
├── rsa.csr
├── rsa.key
├── serial
├── serial.old
├── server.csr
├── server.key
├── server.pem
├── server.private
├── v3_client.cnf
└── v3_server.cnf

1 directory, 26 files
```




