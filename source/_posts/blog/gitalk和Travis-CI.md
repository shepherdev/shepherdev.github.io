---
title: 
date: 2020-4-11 19:37:26
author: shepherd
img: 
top: false
cover: false
coverImg: 
password:
toc: true
mathjax: false
summary: 
categories: 博客
tags:
  - gitalk
  - github
  - Travis CI
---

因为主题自带gitalk插件，虽然版本有点老，所以只需申请Github Application即可

[点击申请](https://github.com/settings/applications/new)

<!--more -->

得到clientID和client Secret填入主题的配置文件即可

然后推送到仓库，使用管理员账户登录这个网页即可创建issue

偶尔会出现Error：network error的问题，原因不明

> 参考[为博客添加 Gitalk 评论插件](https://www.jianshu.com/p/78c64d07124d)

因为文章过多，一个一个取创建太麻烦，于是我找到了自动初始化的方法，先生成站点地图

> 参考[hexo(3)-生成sitemap站点地图](https://www.jianshu.com/p/9c2d6db2f855)

然后我又参考了

> [nodejs版本的Gitalk/Gitment评论自动初始化](https://juejin.im/post/5c0f7951f265da611a47b51a)
>
> [自动初始化 Gitalk 和 Gitment 评论](https://draveness.me/git-comments-initialize/)

一个js脚本，一个ruby脚本

但我用起来都不理想...

------

接着我看到了这个[自动初始化gitalk/gitment评论](https://tenfy.cn/2018/11/15/auto-init-comment/)，然后我看到了[hexo博客push到github的后自动部署到github pages](https://tenfy.cn/2017/09/08/hexo-auto-deploy/)

由于今天Travis在维护，我也不知道我的部署到底有没有成功

为了搞定Travis我参考了很多文章，都列出了吧，我感觉我还用的到

> [Travis CI官网](https://tenfy.cn/2017/09/08/hexo-auto-deploy/)
>
> [阮一峰的持续集成服务 Travis CI 教程](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)
>
> [Github 使用 Travis CI 实现 Hexo 博客自动部署](https://michael728.github.io/2019/06/16/cicd-hexo-blog-travis/)
>
> [使用 Travis CI 实现 Hexo 博客自动部署](https://xirikm.net/2019/826-2)
>
> [使用Travis CI自动部署Hexo博客](https://www.itfanr.cc/2017/08/09/using-travis-ci-automatic-deploy-hexo-blogs/)

因为不知道是维护的缘故还是我操作的缘故导致我一直没弄成功



以上