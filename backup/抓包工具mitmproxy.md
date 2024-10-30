# 前言
之前受一个博主推荐，就关注这个python的抓包工具。最近有个需求，才想起拿来用一下。

# 安装步骤
- 使用AI推荐的pip直接安装，报错了。。。
- 改查看官方文档，使用本地源安装的方式[CONTRIBUTING.md](https://github.com/mitmproxy/mitmproxy/blob/main/CONTRIBUTING.md) 。
```
venv/bin/pip install -e ".[dev]"
```

## 支持https
- mitmproxy CA 证书在首次启动 mitmproxy 时生成后位于~/.mitmproxy中。`cat ~/.mitmproxy/mitmproxy-ca-cert.pem`
- 只需要 mitmproxy 配置正确的证书就行。客户端（如curl、requests）不需指定证书。
- 如果mitmproxy未自动生成证书。可以将 mitmproxy 证书安装到系统信任存储中，你主要需要使用 .crt 或 .pem 格式的证书文件。具体来说：
```
# 1. 复制 mitmproxy-ca-cert.pem 到证书目录
sudo cp ~/.mitmproxy/mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy-ca-cert.crt

# 2. 更新系统证书存储
sudo update-ca-certificates
```
- 其中mitmproxy-ca-cert.pem是主要需要的文件，包含 PEM 格式的 CA 证书。

# 客户端使用示例
## requests
- 必须使用verify去信任mitmproxy使用的证书
```
import os
import requests
# 设置代理配置
proxies = {
    'http': 'http://127.0.0.1:8080',
    'https': 'http://127.0.0.1:8080'
}
# 方式 1: 在 requests 中直接使用代理
response = requests.get('https://baidu.com', proxies=proxies,verify='/home/news_yu/.mitmproxy/mitmproxy-ca-cert.pem')
```

## django的runserver该如何


# mitmproxy的使用
![image](https://github.com/user-attachments/assets/b1440069-0200-4459-a43b-6be5b2620481)
