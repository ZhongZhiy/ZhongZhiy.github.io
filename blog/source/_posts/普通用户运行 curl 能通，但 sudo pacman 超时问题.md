---
title: 解决普通用户运行 curl 能通，但 sudo pacman 超时问题
date: 2026-01-10
categories: linux
tags: linux

---

# 解决普通用户运行 curl 能通，但 sudo pacman 超时问题

当我使用`sudo pacman`时超时, 于是我用`curl`测试连接
```shell
 laugh@fate ~ [1]> curl -I https://www.baidu.com

HTTP/1.1 200 Connection established
HTTP/1.1 200 OK

Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
Content-Length: 0
Content-Type: text/html
Pragma: no-cache
Server: bfe
Date: Fri, 09 Jan 2026 15:54:00 GMT 
```
**原因**是:`sudo`会默认重置(清空)所有的环境变量, 因此我配置的代理就失效了

**临时解决方案**是使用`-E`选项(--preserve-environment), 保留环境变量:
```shell
sudo -E pacman -Syu
```
