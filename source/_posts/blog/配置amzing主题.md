---
title: amazing主题
date: 2020-4-13 21:37:26
author: shepherd
categories: [博客,主题]
tags: [icraus,amazing]
---

发现了一个很美的主题，icraus

<!-- more -->

我使用的是[remove](https://github.com/removeif/hexo-theme-amazing)修改的版本

照着readme文件进行简单配置可以了

## 使用LaTeX语法

主题的_config.yml配置

```yaml
plugins:
	mathjax: true
```

哪篇文章需要使用，就在m头部加上

```yaml
mathJax: true
```

## 添加encrypt插件

在博客下打开终端，输入

```bash
npm install --save hexo-blog-encrypt
```

博客`_config.yml`加上全局配置

```yaml
encrypt: 
  abstract: 有东西被加密了, 请输入密码查看.
  message: 您好, 这里需要密码.
  wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试.
  wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容.
```

也可以在文章开头单独设置