- 想在安卓手机上部署一个http server，通过浏览器进行图片列项查看。找到这个免费开源的项目。记录一下使用经验。

# 基础配置
- 更新apt安装源为国内镜像源`sed -i 's@termux.org/packages/@mirrors.ustc.edu.cn/termux/apt/termux-main@'   $PREFIX/etc/apt/sources.list`
- 更新apt `apt update && apt upgrade`
- 安装python
- 安装git`apt install git openssh`
- 生产ssh密钥对`ssh-keygen -t rsa`