---
title: 让 Google/Bing 收录我的博客
date: 2026/7/4
categories: 博客
tags: 博客

---


我的blog是使用[hexo](https://github.com/hexojs/hexo)生成的, 同时使用 [cactus](https://github.com/probberechts/hexo-theme-cactus) 主题, 托管在 GitHub 上的静态网页, 可以通过以下步骤让 Google 抓取我的 blog

1. 在 [Google Search Console](https://search.google.com/search-console/about) 中选择右边的填入网址([ZhongZhiy's blog!](https://zhongzhiy.github.io/)), 
2. 下载 Google 的验证 html文件, 
3. 放入 `/blog/source/`文件夹下, 
4. 在`_config.yml` 添加 `skip_render = '*.html`禁止渲染这个文件, 
5. 过一会, 验证即可

Bing 可以在 [Bing Webmaster](https://www.bing.com/webmasters/about)中直接导入 [Google Search Console](https://search.google.com/search-console/about) 的结果, 这样随便就可以实现被 Bing 收录
