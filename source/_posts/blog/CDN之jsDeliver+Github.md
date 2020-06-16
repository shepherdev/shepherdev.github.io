---
title: 免费的CDN+Picgo
date: 2020-4-13 21:37:26
author: shepherd
categories: [博客,工具]
tags: [CDN,jsDeliver,Github]
---

> CDN的全称是Content Delivery Network，即内容分发网络。CDN是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。——百度百科

通过CDN加快网站访问速度，提升用户体验

<!-- more -->

1.在github创建仓库，将访问的资源放上去并发布

2.通过jsDeliver引用资源

`https://cdn.jsdelivr.net/gh/你的用户名/你的仓库名@发布的版本号/文件路径`

如下：

```html
<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/20203f67c6b831ace4d1fe4d068600c2cfd6.jpg" style="zoom:20%;" />
<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/943a485d76fbbcc50c15a49f5644cbb1.png" style="zoom:33%;" />
```

<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/20203f67c6b831ace4d1fe4d068600c2cfd6.jpg" style="zoom:20%;" />

<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/943a485d76fbbcc50c15a49f5644cbb1.png" style="zoom:33%;" />

再配合PicGo，typora，简直不要太爽

参考文章

> [免费CDN：jsDeliver+Github 使用方法](https://zhuanlan.zhihu.com/p/76951130)
>
> [github page网站cdn优化加速](https://removeif.github.io/theme/github-page%E7%BD%91%E7%AB%99cdn%E4%BC%98%E5%8C%96%E5%8A%A0%E9%80%9F.html#lg=1&slide=0)