- 想在安卓手机上部署一个http server，通过浏览器进行图片列项查看。找到这个免费开源的项目。记录一下使用经验。

# 基础配置
- 允许termux访问手机的文件，这个命令会新增一些共享目录。`termux-setup-storage`
- 更新apt安装源为国内镜像源`sed -i 's@termux.org/packages/@mirrors.ustc.edu.cn/termux/apt/termux-main@'   $PREFIX/etc/apt/sources.list`
- 更新apt `apt update && apt upgrade`
- 安装python
- 安装git`apt install git openssh`
- 生产ssh密钥对`ssh-keygen -t rsa`


# termux局域网公开sshd连接

## 密码连接
- 开启sshd服务：`pkg install openssh 、pkg install openssl`
- 开机启动：`echo "sshd" >> ~/.bashrc`
- 查询用户名：`whoami`
- 修改用户密码：`passwd`
- 查询局域网ip: `ifconfig`
- 用密码连接：`ssh -p 8022 u0_a633@10.112.0.115`

## 私钥连接
1. **`-i`后面接的是私钥**：在SSH命令中，`-i`选项后面需要指定你的私钥文件的路径。这是你在本地机器上生成的密钥对中的私钥部分。示例命令为：
   ```bash
   ssh -i ~/.ssh/id_rsa -p 8022 用户名@IP地址
   ```

2. **公钥要配置到需要连接的`sshd`服务**：公钥需要添加到你要连接的SSH服务器上。具体步骤如下：
   - 将生成的公钥（通常位于`~/.ssh/id_rsa.pub`）的内容复制。
   - 在目标服务器（例如Termux中的SSH服务器）上，打开或创建`~/.ssh/authorized_keys`文件，并将公钥粘贴到文件中。

这样设置后，你就可以使用私钥通过SSH连接到已配置公钥的服务器，而无需输入密码。