---
title: 解决gemini-cli不能登录
date: 2025-12-24
categories: [小问题]
tags: [gemini-cli]

---

# 解决gemini-cli不能登录的问题
## 问题描述
在pwsh中使用gemini-cli时, 打开浏览器登录不能登录
{% asset_img "gemini-login.png"%}
{% asset_img "login.png" %}

## 解决
根据[github讨论区](https://github.com/google-gemini/gemini-cli/issues/2081)的建议, 对终端设置代理
```powershell
netsh winhttp set proxy 127.0.0.1:7897
```
检查:
```powershell
PS C:\Users\zzy20> netsh winhttp show proxy

当前的 WinHTTP 代理服务器设置:

    代理服务器:  127.0.0.1:7897
    绕过列表     :  (无)

```

之后再使用`gemini`登录就可以成功了.
