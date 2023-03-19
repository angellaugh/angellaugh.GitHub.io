# keycloak


```
keycloak-ali1:~/keycloak/keycloak-21.0.1/bin # sudo zypper refresh
Repository 'Update repository of openSUSE Backports' is up to date.
Repository 'Non-OSS Repository' is up to date.
Repository 'Main Repository' is up to date.
Repository 'Update repository with updates from SUSE Linux Enterprise 15' is up to date.
Repository 'Main Update Repository' is up to date.
Repository 'Update Repository (Non-Oss)' is up to date.
All repositories have been refreshed.
keycloak-ali1:~/keycloak/keycloak-21.0.1/bin # sudo zypper search openjdk-devel
Loading repository data...
Reading installed packages...

S | Name                     | Summary                            | Type
--+--------------------------+------------------------------------+--------
  | java-1_8_0-openjdk-devel | OpenJDK 8 Development Environment  | package
  | java-9-openjdk-devel     | OpenJDK 9 Development Environment  | package
  | java-10-openjdk-devel    | OpenJDK 10 Development Environment | package
  | java-11-openjdk-devel    | OpenJDK 11 Development Environment | package
  | java-17-openjdk-devel    | OpenJDK 17 Development Environment | package
keycloak-ali1:~/keycloak/keycloak-21.0.1/bin # sudo zypper --non-interactive install java-17-openjdk-devel
Loading repository data...
Reading installed packages...
Resolving package dependencies...

The following 2 recommended packages were automatically selected:
  pcsc-lite timezone-java

The following 11 NEW packages are going to be installed:
  java-17-openjdk java-17-openjdk-devel java-17-openjdk-headless javapackages-filesystem javapackages-tools libXi6 libXtst6 libgif7 libpcsclite1
  pcsc-lite timezone-java

11 new packages to install.
Overall download size: 43.9 MiB. Already cached: 0 B. After the operation, additional 189.6 MiB will be used.
Continue? [y/n/v/...? shows all options] (y): y
Retrieving package libXi6-1.7.9-3.2.1.x86_64                                                                  (1/11),  35.6 KiB ( 66.5 KiB unpacked)
Retrieving: libXi6-1.7.9-3.2.1.x86_64.rpm ....................................................................................................[done]
Retrieving package libXtst6-1.2.3-1.24.x86_64                                                                 (2/11),  17.2 KiB ( 22.4 KiB unpacked)
Retrieving: libXtst6-1.2.3-1.24.x86_64.rpm ...................................................................................................[done]
Retrieving package libgif7-5.2.1-150000.4.8.1.x86_64                                                          (3/11),  28.8 KiB ( 35.5 KiB unpacked)
Retrieving: libgif7-5.2.1-150000.4.8.1.x86_64.rpm ............................................................................................[done]
Retrieving package libpcsclite1-1.9.4-150400.1.9.x86_64                                                       (4/11),  32.8 KiB ( 38.4 KiB unpacked)
Retrieving: libpcsclite1-1.9.4-150400.1.9.x86_64.rpm .........................................................................................[done]
Retrieving package pcsc-lite-1.9.4-150400.1.9.x86_64                                                          (5/11),  82.2 KiB (140.4 KiB unpacked)
Retrieving: pcsc-lite-1.9.4-150400.1.9.x86_64.rpm ............................................................................................[done]
Retrieving package javapackages-filesystem-5.3.1-150200.3.4.4.x86_64                                          (6/11),  17.0 KiB (  1.9 KiB unpacked)
Retrieving: javapackages-filesystem-5.3.1-150200.3.4.4.x86_64.rpm ................................................................[done (2.8 KiB/s)]
Retrieving package timezone-java-2022g-150000.75.18.1.noarch                                                  (7/11), 155.0 KiB (341.2 KiB unpacked)
Retrieving: timezone-java-2022g-150000.75.18.1.noarch.rpm ....................................................................................[done]
Retrieving package javapackages-tools-5.3.1-150200.3.4.4.x86_64                                               (8/11),  37.7 KiB ( 71.6 KiB unpacked)
Retrieving: javapackages-tools-5.3.1-150200.3.4.4.x86_64.rpm ...................................................................[done (129.7 KiB/s)]
Retrieving package java-17-openjdk-headless-17.0.6.0-150400.3.12.1.x86_64                                     (9/11),  38.4 MiB (179.7 MiB unpacked)
Retrieving: java-17-openjdk-headless-17.0.6.0-150400.3.12.1.x86_64.rpm ..........................................................[done (12.9 MiB/s)]
Retrieving package java-17-openjdk-17.0.6.0-150400.3.12.1.x86_64                                             (10/11), 284.4 KiB (447.5 KiB unpacked)
Retrieving: java-17-openjdk-17.0.6.0-150400.3.12.1.x86_64.rpm ..................................................................[done (869.5 KiB/s)]
Retrieving package java-17-openjdk-devel-17.0.6.0-150400.3.12.1.x86_64                                       (11/11),   4.8 MiB (  8.8 MiB unpacked)
Retrieving: java-17-openjdk-devel-17.0.6.0-150400.3.12.1.x86_64.rpm ..............................................................[done (7.8 MiB/s)]

Checking for file conflicts: .................................................................................................................[done]
( 1/11) Installing: libXi6-1.7.9-3.2.1.x86_64 ................................................................................................[done]
( 2/11) Installing: libXtst6-1.2.3-1.24.x86_64 ...............................................................................................[done]
( 3/11) Installing: libgif7-5.2.1-150000.4.8.1.x86_64 ........................................................................................[done]
( 4/11) Installing: libpcsclite1-1.9.4-150400.1.9.x86_64 .....................................................................................[done]
Created symlink /etc/systemd/system/sockets.target.wants/pcscd.socket -> /usr/lib/systemd/system/pcscd.socket.
Updating /etc/sysconfig/pcscd ...
( 5/11) Installing: pcsc-lite-1.9.4-150400.1.9.x86_64 ........................................................................................[done]
( 6/11) Installing: javapackages-filesystem-5.3.1-150200.3.4.4.x86_64 ........................................................................[done]
( 7/11) Installing: timezone-java-2022g-150000.75.18.1.noarch ................................................................................[done]
( 8/11) Installing: javapackages-tools-5.3.1-150200.3.4.4.x86_64 .............................................................................[done]
update-alternatives: using /usr/lib64/jvm/jre-17-openjdk/bin/java to provide /usr/bin/java (java) in auto mode
update-alternatives: using /usr/lib64/jvm/jre-17-openjdk to provide /usr/lib64/jvm/jre-openjdk (jre_openjdk) in auto mode
update-alternatives: using /usr/lib64/jvm/jre-17-openjdk to provide /usr/lib64/jvm/jre-17 (jre_17) in auto mode
( 9/11) Installing: java-17-openjdk-headless-17.0.6.0-150400.3.12.1.x86_64 ...................................................................[done]
(10/11) Installing: java-17-openjdk-17.0.6.0-150400.3.12.1.x86_64 ............................................................................[done]
update-alternatives: using /usr/lib64/jvm/java-17-openjdk/bin/javac to provide /usr/bin/javac (javac) in auto mode
update-alternatives: using /usr/lib64/jvm/java-17-openjdk to provide /usr/lib64/jvm/java-openjdk (java_sdk_openjdk) in auto mode
update-alternatives: using /usr/lib64/jvm/java-17-openjdk to provide /usr/lib64/jvm/java-17 (java_sdk_17) in auto mode
(11/11) Installing: java-17-openjdk-devel-17.0.6.0-150400.3.12.1.x86_64 ......................................................................[done]
Executing %posttrans scripts .................................................................................................................[done]
keycloak-ali1:~/keycloak/keycloak-21.0.1/bin # javac -version
javac 17.0.6
```



#### Realms


##### Realms are isolated from one another and can only manage and authenticate the users that they control.

<mark>You create a realm to provide a management space where you can create users and give them permissions to use applications.</mark>


![](/uploads/203023010625203.png)




##### Set Require SSL to one of the following SSL modes:

* External requests Users can interact with Keycloak without SSL so long as they stick to private IP addresses such as localhost, 127.0.0.1, 10.x.x.x, 192.168.x.x, and 172.16.x.x. If you try to access Keycloak without SSL from a non-private IP address, you will get an error.

* None Keycloak does not require SSL. This choice applies only in development when you are experimenting and do not plan to support this deployment.

* All requests Keycloak requires SSL for all IP addresses.

![](/uploads/354284833950954.png)



##### Configuring realm keys

```
Keycloak has a single active keypair at a time, but can have several passive keys as well. 
The active keypair is used to create new signatures, while the passive keypair can be used to verify previous signatures. 
This makes it possible to regularly rotate the keys without any downtime or interruption to users.
Keycloak 一次只有一个主动密钥对，但也可以有多个被动密钥。主动密钥对用于创建新签名，而被动密钥对可用于验证以前的签名。
这样就可以定期轮换密钥，而不会对用户造成任何停机或干扰。

```
虽然reaml setting 的key中，是active，但是到底有没有使用它，取决于第一个选中的 key provider

```
A keypair can have the status Active, but still not be selected as the currently active keypair for the realm. 
The selected active pair which is used for signatures is selected based on the first key provider sorted by priority that is able to provide an active keypair.


```

<mark>Once new keys are available all new tokens and cookies will be signed with the new keys.</mark>


```bash
1. Once new keys are available all new tokens and cookies will be signed with the new keys.
# 一旦新的keys 生效，所有的token cookie都被signed with the new key.
2. When a user authenticates to an application the SSO cookie is updated with the new signature.
# 当user authenticates login via sso，  那么 cookie 就随着 new signature update. 
3. When OpenID Connect tokens are refreshed new tokens are signed with the new keys. 
#  当OpenID token 被refresh， 新的token 就被signed with new key.
4. This means that over time all cookies and tokens will use the new keys and after a while the old keys can be removed.
# 意味着 所有的cookie 和 token 都在使用 new keys， 并且，after a while old key 就被remove 了。

这里的 after a while：删除旧密钥的频率是安全性与确保所有 cookie 和令牌更新之间的权衡。
The frequency of deleting old keys is a tradeoff between security and making sure all cookies and tokens are updated.

for instance:  create new keys 3~6 months,  delete old keys 1~2 months. 如果user 在这期间old key 被删了，新key没有，那么重新登录认证即可。

```


###### Adding a generated keypair

更改优先级不会生成新key，但是edit keysize 会触发生成新key
![](/uploads/595696697899358.png)


####### Compromised keys
keycloak has the signing keys stored just locally 将签名密钥存储在本地， 不会分享给其他user或者application
如果认为签名密钥已经泄露，可以立即生成新密钥对，并且删除旧密钥，甚至考虑删除provider。

##### Using external storage 正是需要的

一般不会把包含有信息，密码，其他凭证等的数据库迁移到keycloak去。
keycloak支持已经存在的外部用户数据库，比如LDAP, AD。
用户也可以通过keycloak user storage API 来为任何 custom user database 编写扩展代码。

当用户尝试登陆时，keycloak 会检查该用户的存储以查找该用户，如果找不到的话，就会遍历当前realm的所有 user storage provider，直到直到匹配项。

来自外部data storage 的user 会 映射map到keycloak运行时使用的标准用户模型。然后，这个标准用户模型映射到 OIDC token claims 令牌声明和 SAML assertion attributes。

external user database 几乎不会完整支持keycloak的所有功能。
因此，User storage provider 可以选择性的存储items 到keycloak user data storage中。 
User storage provider 可以在本地导入用户，并定期与外部数据存储同步。
但是这取决于external user database是否支持OTP， OTP可以通过Keycloak处理和存储。
> 一次性密码（One Time Password，简称OTP），又称“一次性口令”，是指只能使用一次的密码。 一次性密码是根据专门算法、每隔60秒生成一个不可预测的随机数字组合  


###### Add a provider: kerberos provider or LDAP provicer

```
user federation > can choose kerberos provider or LDAP provicer
```

一个 keycloak realm 可以 federate multiple different LDAP servers. 并且把ldap user 映射成keycloak的通用user model。

keycloak 默认映射username,  email， first/last name, 也可以configure additional mappings.
usermap: email

fullname: firstname and lastname

Hardcoded Attribute Mapper

Role Mapper: 通常是group， 从LDAP map 到 keycloak对应的role group， 比如：
 1. from groups under <mark>ou=main</mark>,dc=example,dc=org map to <mark>realm role mappings</mark>, 
 2. from groups under <mark>ou=finance</mark>,dc=example,dc=org map to <mark>client role mappings of client finance</mark>.
 
Group Mapper


<mark>Certificate Mapper</mark>

此映射器映射 X.509 证书。Keycloak 将其与 X.509 身份验证和 PEM 格式的完整证书结合使用，作为身份源。此映射器的行为类似于用户属性映射器，但 Keycloak 可以过滤存储 PEM 或 DER 格式证书的 LDAP 属性。使用此映射器启用始终从 LDAP 读取值。
```
This mapper maps X.509 certificates. Keycloak uses it in conjunction with X.509 authentication and Full certificate in PEM format as an identity source. This mapper behaves similarly to the User Attribute Mapper, but Keycloak can filter for an LDAP attribute storing a PEM or DER format certificate. Enable Always Read Value From LDAP with this mapper.
```



###### Dealing with provider failures


* Keycloak 先查询本地数据库来解析user，然后才考虑LDAP 或者其他custom user storage provider.
最好在keycloak 本地建立admin account，以防连接到 LDAP 和后端时出现问题。

* 任何external user storage provider 在admin 控制台都有`一个enable 开关`
external user storage provider(ldap or custom user storage provicer) `has an enable toggle` on 

* 如果您的提供程序使用导入策略并被禁用，则导入的用户仍可在只读模式下进行查找。
If your provider uses an import strategy and is disabled, imported users are still available for lookup in read-only mode.

 * Duplicate usernames and emails can cause problems
 
###### Storage mode

* keycloak 将LDAP user 同步到本地（定期或后台），但是不会同步密码， `password validation 只会在LDAP server上执行。`
Password validation always occurs on the LDAP server.

*  用户第一次登录keycloak时候，keycloak都会将它写入本地database。 Keycloak performs a corresponding database insert.
*  可以但非必要，同步LDAP。 can synchronize， but unnecessary.
* 您可以将 LDAP 与 Keycloak 一起使用，而无需将用户导入 Keycloak 用户数据库。LDAP 服务器备份 Keycloak 运行时使用的通用用户模型



<mark>如果选择 disable import users, 就无法将 user profile attributes 用户配置文件属性保存到keycloak中。</mark>
<mark>user profile attributes metadata mapped to the ldap 可以保存到keycloak中， 但是 user metadata不能被保存，user metadata=[role mappings, group mappings, other metadata based on the ldap mappers.]</mark>

<mark>如果禁用导入用户，则无法将用户配置文件属性保存到 Keycloak 数据库中。 此外，除了映射到 LDAP 的用户配置文件元数据之外，您无法保存元数据。 此元数据可以包括角色映射、组映射和其他基于 LDAP 映射器配置的元数据。</mark>
```
If you disable Import Users, you cannot save user profile attributes into the Keycloak database. 
Also, you cannot save metadata except for user profile metadata mapped to the LDAP. 
This metadata can include role mappings, group mappings, and other metadata based on the LDAP mappers' configuration.
```

**UNSYNCED**
keycloak 会把 更改 username, email, password 的动作保存在local，这些信息需要同步回ldap， must synchronize this data back to LDAP

当 Keycloak 创建 LDAP 提供程序时，Keycloak 还会创建一组初始 LDAP 映射器。Keycloak 根据供应商、编辑模式和导入用户开关的组合来配置这些映射器。
based on a combination of the Vendor, Edit Mode, and Import Users switches.


**Sync Registrations**
Toggle this switch to ON if you want new users created by Keycloak added to LDAP

* 当选择 import user， 用户第一次登录后，从keycloak admin那里查看all user，会看到验证过的user展示在这里，意思是已经导入了，keycloak只在user 第一次login时候导入，而不是一次性导入整个LDAP的所有user。

* 如果想一次性同步所有user，有2种方式

```txt
If you want to sync all LDAP users into the Keycloak database, configure and enable the Sync Settings on the LDAP provider configuration page.

1. Periodic Full sync
此类型将所有 LDAP 用户同步到 Keycloak 数据库中。已经在Keycloak中但与LDAP不同的LDAP用户直接在Keycloak数据库中更新。
2. Periodic Changed users sync
同步时，Keycloak 仅创建或更新在上次同步后创建或更新的用户。
```

* 最佳同步策略： 在创建LDAP provider时，选择 `Synchronize all users`, 然后 `Synchronize all users`
The best way to synchronize is to click Synchronize all users when you first create the LDAP provider, then set up Synchronize all usersSynchronize all users



![](/uploads/86293028586002.png)

###### Password hashing
当 Keycloak 更新密码时，Keycloak 以纯文本格式发送密码。
此操作不同于在内置 Keycloak 数据库中更新密码，Keycloak 在将密码发送到数据库之前对密码进行哈希处理和加盐处理。
对于 LDAP，Keycloak 依赖于 LDAP 服务器对密码进行散列和加盐。

1. By default, LDAP servers such as MSAD, RHDS, or FreeIPA hash and salt passwords. 
2. Other LDAP servers such as OpenLDAP or ApacheDS store the passwords in plain-text unless you use the LDAPv3 Password Modify Extended Operation as described in RFC3062. 
3. Always verify that user passwords are properly hashed and not stored as plaintext by inspecting a changed directory entry using ldapsearch and base64 decode the userPassword attribute value.


#### Authentication flow

flow是一个在登陆，注册或者其他workflow时候要进行的 身份验证，屏幕 和 操作。
An authentication flow is a container of 
authentications, screens, and actions, during log in, registration, and other Keycloak workflows. 
To view all the flows, actions, and checks, each flow requires:

##### **browser 里面的 Forms：**
先说cookie，user 首次登陆成功后，keycloak会 sets a session cookie.
如果这个cookie已经存在了，authentication type=success, cookie provider return success,
而同级其他验证方式都是alternative可选的， 那么keycloak不会再执行任何操作，这次请求会返回一个successful login.

而这里的Forms， 是个可选的子flow alternative sub-flow，它能不能被执行，取决于它的parent status. 
它包含了一个需要被执行的额外验证项目，因此如果想这个form被执行，就需要删掉cookie 验证。
```
Since this sub-flow is marked as alternative, it will not be executed if the Cookie authentication type passed. 
This sub-flow contains an additional authentication type that needs to be executed. Keycloak loads the executions for this sub-flow and processes them.
```

##### 验证步骤
1. Username Password Form, 在browser flow中，这个选项是required，并且不可更改， 需要用户输入uname+pwd
2.  Browser - Conditional OTP sub-flow， 它是based on the Condition-user configured
3. Condition - User Configured. This authentication checks if Keycloak has configured other executions in the flow for the user
4. OTP Form Keycloak 将此执行标记为必需，但仅当用户由于条件子流中的设置而设置了 OTP 凭据时，它才会运行。否则，用户不会看到 OTP 表单。

<mark>可以添加新的空白flow，也可以copy已经存在的flow， 里面的action有很多种， from sending a reset email to validating an OTP.</mark>
<mark>可以建立sub-flow, 可以拖动顺序</mark>



![](/uploads/151286775946824.png)

> https://www.keycloak.org/docs/latest/server_admin/#configuring-authentication_server_administration_guide
