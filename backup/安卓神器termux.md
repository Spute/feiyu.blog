- 想在安卓手机上部署一个http server，通过浏览器进行图片列项查看。找到这个免费开源的项目。记录一下使用经验。

# 基础配置
- 允许termux访问手机的文件，这个命令会新增一些共享目录。`termux-setup-storage`
- 更新apt安装源为国内镜像源`sed -i 's@termux.org/packages/@mirrors.ustc.edu.cn/termux/apt/termux-main@'   $PREFIX/etc/apt/sources.list`
- 更新apt `apt update && apt upgrade`
- 安装python
- 安装git`apt install git openssh`
- 生产ssh密钥对`ssh-keygen -t rsa`
- 开机自启动sshd，在用户根目录下创建` echo "sshd" >> .bashrc`

# 内网穿透
部署在内网的NAS设备，没有公网IP，如何将这个内网IP对外暴露，让在外的设备能够访问到。有两种方式：
- 一种是端口转发，
- 一种是VPN实现点对点链接，创建加密隧道连接内外网，类似建立了一个虚拟专用网络。
- Cloudflare Zero Trust开通tunnel：https://mymuwu.net/?p=1369
## WireGuard协议
WireGuard协议必须有个服务端，然后多个客户端，服务端必须在公网IP
Tailscale 是一种基于 WireGuard 的虚拟组网工具，它在用户态实现了 WireGuard 协议。
还有一个完全开源的工具headscale。好像要自己部署服务端？
- tailscale实现流程
  - 注册tailscale：https://tailscale.com/
  - 分别下载windows、安卓客户端登陆账号
  - 客户端就可以互相访问

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

# 常用软件
vim编辑器，设置粘贴模式。在 Vim 中，粘贴模式（Paste Mode）用于防止在粘贴文本时出现不必要的自动缩进、格式化等操作，从而避免格式错乱。
`:set paste`



# 运行docker
好像root才能运行docker
https://www.reddit.com/r/docker/comments/unghjr/how_do_i_install_docker_on_termux/
```
pkg install root-repo
pkg install docker
```
- 换一种方式可行：proot-distro
- proot-distro 工具安裝Linux發行版
```
# 安装 proot-distro
pkg install proot-distro

# 安装 Ubuntu
proot-distro install ubuntu

# 登录 Ubuntu
proot-distro login ubuntu

# 在 Ubuntu 中安装 Docker
apt update
apt install docker.io
```