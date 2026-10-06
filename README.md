# 猫窝

静态博客 + 猫娘展示站。托管在 GitHub Pages（<https://remtg.github.io>）。

无数据库、无服务器、无续费 —— 改一行字 `git push` 就上线。

## 站点结构

```text
index.html              文章列表（首页）
archive.html            按年月归档
tags.html               标签索引
tags/<标签>.html        每个标签独立页
posts/<slug>.html       单篇文章
about.html              关于
rss.xml                 订阅源
posts.json              文章元数据
css/style.css           站点样式
legacy/                 旧站存档（《躲在超市后门抽烟的两人》落地页）
```

## 源码在哪

这个仓库是**构建产物**，不是源码。真正的源码在本地：

```text
posts/*.md文章源文件（Markdown）
src/build.py            构建器（只依赖 markdown + pygments）
static/css/style.css样式源文件
site/                   构建产物 = 本仓库内容
```

改文章 → 本地改 `.md` → 跑 `python src/build.py` → 把 `site/` 的产物同步回本仓库 → push。

## 本地构建

```bash
pip install markdown pygments
python src/build.py        # 产物输出到 site/
python src/serve.py        # 本地预览 http://127.0.0.1:8000/
```

文章头部用 front matter：

```markdown
---
title: 文章标题
date: 2026-10-06
tags: 部署, 静态站点
summary: 可选，不写会自动截取正文首段
draft: true          # 写了这行就不发布
---
```

## 猫娘展示页（NekoWebShow）

站内 `/neko/` 计划嵌入 [NekoWebShow](https://github.com/Chocola-X/NekoWebShow)——
在浏览器里展示《猫娘乐园》E-mote 动态立绘，可触摸互动、播放语音、口型同步，背景随本地时间变化。

采用 `html_version_v2` 分支（单页面静态，`#模型名` 切换角色，不需要 URL 重写）。
模型与背景素材约 500 MB，托管方式待定。

**素材版权**：该项目代码为 AGPL v3，但**模型与背景图是从《猫娘乐园》解包的素材，版权归原权利人所有**。
作者 Chocola-X 在其 README 中亦如此声明。本仓库仅作个人学习与展示用途。
详见 `posts/neko-web-show.html` 与 `about.html`。

## 旧站存档

`legacy/` 是本仓库原先的内容 —— 一个《躲在超市后门抽烟的两人》主题的静态落地页，
由 commit `91d07cd` 保留。`backup-before-blog` 分支保存了完整的部署前状态。
