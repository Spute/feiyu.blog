# 前言
回顾一下之前碰到的问题

# 原理
非对称加密，在 SSH 密钥认证过程中，实际上是相反的 - 它使用公钥加密，私钥解密。

1. 基本概念：
- 私钥：存放在客户端(你的电脑)，必须保密
- 公钥：存放在服务器端(~/.ssh/authorized_keys)，可以公开

2. SSH 认证流程：
```
客户端                                        服务器
(持有私钥)                                   (持有公钥)
   |                                           |
   |        1. 发起连接请求                    |
   |----------------------------------------->|
   |                                          |
   |        2. 生成随机字符串并用公钥加密       |
   |<-----------------------------------------|
   |                                          |
   |        3. 用私钥解密并返回结果            |
   |----------------------------------------->|
   |                                          |
   |        4. 验证成功，建立连接              |
   |<---------------------------------------->|
```

3. 详细步骤：
   - 客户端发起连接请求
   - 服务器生成一个随机字符串，用存储的公钥加密后发送给客户端
   - 客户端用私钥解密这个字符串，并发回服务器
   - 服务器验证解密结果是否正确
   - 如果正确，则证明客户端确实持有配对的私钥，允许登录

这种方式的安全性在于：
- 即使有人截获了公钥，也无法用来解密信息
- 只有持有私钥的人才能正确解密服务器发送的加密信息
- 私钥始终不会在网络上传输

为什么不用私钥加密？
- 用私钥加密：任何人都可以用公钥解密，不安全
- 用公钥加密：只有私钥持有者能解密，更安全


# 步骤

1. 在本地机器生成SSH密钥对:
```bash
ssh-keygen -t rsa -b 4096
```
按回车后会在 ~/.ssh/ 目录下生成两个文件:
- id_rsa (私钥)
- id_rsa.pub (公钥)

2. 将公钥复制到远程服务器:
```bash
ssh-copy-id username@remote_host
```
或者手动复制（**手动复制时，谨慎把换行符增加了，否则不生效**）:
```bash
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

3. 设置远程服务器SSH配置的权限:
```bash
ssh username@remote_host "chmod 700 ~/.ssh; chmod 600 ~/.ssh/authorized_keys"
```

4. 测试SSH密钥登录:
```bash
ssh username@remote_host
```
指定密钥登录：
**是-i后面接的是私钥文件，而不是公钥。因为对于本地机器是用私钥解密的。**
```
 ssh -i ~/.ssh/id_rsa flexivsu@10.24.13.30
```


5. (可选)如果要提高安全性,可以编辑远程服务器的SSH配置文件禁用密码登录:
```bash
sudo nano /etc/ssh/sshd_config
```
修改或添加以下行:
```
PasswordAuthentication no
```
然后重启SSH服务:
```bash
sudo systemctl restart sshd
```

6. 检查Termux的sshd配置，确保开启公钥验证:
```
# 在Termux中
cat $PREFIX/etc/ssh/sshd_config

# 确保包含以下设置:
PubkeyAuthentication yes
```


# 技巧
在~/.ssh/config中添加配置使连接更方便:
```
Host git.flexiv.cloud
  Hostname git.flexiv.cloud
  Port 522
  IdentityFile ~/.ssh/gitlab/id_rsa

Host termux
  HostName 100.102.133.20
  Port 8022
  User u0_a633

```
添加配置后，使用`ssh termux`就能直接连接远程机器。