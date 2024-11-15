# 前言
# webdav协议
上传下载，类似anyshare软件的功能。
是什么？基于http协议的拓展更多的请求方法。

WebDAV （Web-based Distributed Authoring and Versioning） 一种基于 HTTP 1.1协议的通信协议。它扩展了HTTP 1.1，在GET、POST、HEAD等几个HTTP标准方法以外添加了一些新的方法，使应用程序可对Web Server直接读写，并支持写文件锁定(Locking)及解锁(Unlock)，还可以支持文件的版本控制。

CMS系统的实现

部分网盘软件支持WebDAV协议，如坚果。。。等

# webdav服务

选择python开发的wsgidav项目。

# webdav客户端

浏览器直接作为webdav的客户端只能支持一部分功能。比如只能下载文件，并不能上传文件。

支持webdav协议的客户端。
- 安卓端
mixplorer：安卓端。支持webdav协议。允许作为客户端，也可以作为一个webdav服务器。开启webdav server后可以正常使用，可是过段时间会自动终止掉。

- 桌面应用：
railDrive：可以安装成功，但是免费版有广告，感觉有点重，可以连接其他云盘。又卸载了。
filecxx：github一个开源项目，安装失败提示：code:-1