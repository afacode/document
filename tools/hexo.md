[hexo.io官网](https://hexo.io/zh-cn/docs/)

## 安装
`git && node` <br>
`npm install -g hexo-cli` <br>
`hexo init blog && cd blog && npm install`
## 新建文章
` hexo new "new article" `
> `hexo new [layout] <title>`

layout默认为 post
| 布局 | 路径 | 
| ------ | ------ | 
| post | source/_posts | 
| page | source | 
| draft | source/_drafts |

#### 草稿
`hexo publish [layout] <title>`

## Front-matter
Front-matter 是文件最上方以 --- 分隔的区域，用于指定个别文件的变量
```
---
title: Hello World
date: 2013/7/13 20:46:25
updated: 2019/5/7 20:12:22
categories: 
  - demo
tags:
  - demo
  - hexo
---


```



## 本地查看
`hexo g || hexo generate` <br>
`hexo s || hexo server` <br>
`localhost:4000` <br>

`hexo clean && hexo g && hexo s`
## 发布
`hexo clean && hexo g && hexo d`




