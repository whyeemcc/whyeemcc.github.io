---
layout: post
title:  "Jekyll中关于MathJax的坑"
date:   2017-07-30 21:50:01
categories: blog
headerImage: false
tag:
- jekyll
- blog
star: true
category: blog
author: whyeemcc
description: none
---

## $LaTeX$公式无法渲染的问题

基于 Jekyll 的博客架好后（模板fork自[indigo](https://github.com/sergiokopplin/indigo)），准备加载 MathJax 已便显示矢量的公式，但是按照教程，却无论如何也无法渲染 Latex 公式，博文上该显示公式的地方什么都没有，查看页面源码，其实公式的 html 的代码是存在的，就是渲染不了。

* 原因在于：

目前大部分网上教程中，给出添加到 _layouts\default.html 里 <head>…</head> 间的代码是这样的：

```html
  <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
  </script>
```

但是，MathJax 的CDN服务地址还有个基于 SSL 的更安全的 'https'，我将上面的代码改成 https 后，竟然就可以显示了：

```html
  <script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
  </script>
```

## MathJax 的其他配置

#### 支持行内公式

默认状态下是不支持用两个 $ 符号框起来的行内公式的，所以需要在 _layouts\default.html 里同样位置添加如下配置代码，去自定义：

```html
  <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        inlineMath: [ ['$','$'], ["\\(","\\)"] ],
        processEscapes: true
      }
    });
  </script>
```

这样就可以在行内插入公式了，如 $y=x^2$ 这样子。

#### 支持公式序号

添加配置代码：

```html
  <script type="text/x-mathjax-config"> 
  MathJax.Hub.Config({ TeX: { equationNumbers: { autoNumber: "all" } } }); 
  </script>
```
这样行间公式显示时，会在末尾自动添加公式的序号：

$$\sum_{n=1}^\infty 1/n^2 = \frac{\pi^2}{6}$$