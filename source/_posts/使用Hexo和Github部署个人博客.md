---
title: 使用Hexo和Github部署个人博客
date: 2023-10-28 15:24:33
tags:
---

# Motivation

研二的时候曾经使用Hexo部署过个人博客，当时参与前端开发，所以主要记录的内容都是前端技术相关的内容。现在回头看当时记录博客的动机，主要是觉得新颖，想要尝试一下使用前端技术记录博客。博客记录了一段时间就停更了，这与我博文内容有很大关系，因为博文大部分都是复制粘贴其他博客中的内容，用于存档，并没有融入自己的想法，导致在重复的低级cv中丧失了耐心，于是没有坚持记录下去。

为什么现在又重新开始记录博客呢？主要是为了强迫自己思考，当然记录也是原因之一。书写文字的过程，同样也是思考的过程，平时一些遇见的问题，如果不深入思考，很容易误以为自己理解了。因此，落实到具体的文字中，可以强迫自己理解问题。

在春招求职的过程中，阅读了不少“图解”系列，图形能够很直观的表达出思维过程，相比抽象的文字，具象的图形能够更快的表达思路和含义，给自己订下个目标，要在年末前掌握制作”图解“思路的能力。

# 使用工具

- Hexo：将Markdown文件生成html等文件的博客框架，基于nodejs
- Fluid：Hexo主题
- PicGo：图床，用于将图片上传到CDN
- Typora：Markdown文件编辑器
- Github：代码托管平台
- Git：代码版本管理软件

# 环境配置

## Hexo安装

提前安装：nodejs、git

安装步骤

```bash
npm install -g hexo-cli # 全局安装hexo
hexo init <floder> # 初始化文件夹
cd <floder>
npm install # 安装第三方库
```

文件目录

```
.
├── _config.yml  # 配置文件
├── package.json  # nodejs应用程序
├── scaffolds  # 模版文件
├── source  # Markdown源文件，除开_posts外，其他隐藏
|   ├── _drafts
|   └── _posts
└── themes  # 主题
```

## Fluid安装

安装步骤

```bash
npm install --save hexo-theme-fluid  # 本地安装fluid
npm update --save hexo-theme-fluid  # 更新fluid
```

配置文件

```yaml
# _config.yml

theme: fluid
language: zh-CN
```

生成about文件

```bash
hexo new page about
```

## Typora + Picgo + Github + jsDelivr CDN制作Markdown文档图床 

Github新建公共图床仓库，例如 picbed

Pico进入Github设置页面填写字段

```
仓库名：<github_id>/<repo_name>
Token：github setting中的access tokens
自定义域名：https://cdn.jsdelivr.net/hg/<github_id>/<repo_name>
```

Typora在`图像`设置插入图片上传

# 日常使用

配置完上面的步骤后，就可以日常使用了

Hexo常用命令

```bash
hexo init <floder>  # 初始化文件夹
hexo new [layout] <title>  # 添加新日志文件
hexo g  # 生成静态文件
hexo s  # 启动服务器
hexo clean  # 清除缓存db.json和静态文件public/
hexo list  # 列出网站数据
```

# 参考

[hexo](https://hexo.io/zh-cn/docs/)

[fluid](https://fluid-dev.github.io/hexo-fluid-docs/start/)

