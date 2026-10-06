# 猫窝 · 博客 + 猫娘展示站

静态博客，附带一个 [NekoWebShow](https://github.com/Chocola-X/NekoWebShow) 猫娘展示页。
无数据库、无服务器、无续费 —— `git push` 即上线。

## 目录结构

```text
posts/                文章源文件（Markdown + front matter）
  single-serve-static-site.md
  neko-web-show.md
  about.md            渲染成 about.html，不进文章列表
static/
  css/style.css       站点样式
src/
  build.py            构建器（只依赖 markdown + pygments）
  serve.py            本地预览服务
site/                 构建产物 = 网站本体
  index.html          文章列表
  archive.html        按年月归档
  tags.html           标签索引
  tags/<标签>.html    每个标签独立页
  posts/<slug>.html   单篇文章
  rss.xml             订阅源
  posts.json          文章元数据（供前端取用）
  neko/               猫娘展示页（素材到位后放入）
```

## 写文章

在 `posts/` 新建 `.md`，头部写元信息：

```markdown
---
title: 文章标题
date: 2026-10-06
tags: 部署, 静态站点
summary: 可选。不写就自动截取正文首段。
draft: true          # 写了这行就不发布
---

正文用Markdown。
```

`tags` 用逗号分隔。日期用 `YYYY-MM-DD`。
草稿（`draft: true`）构建时会被跳过。

## 本地预览

```bash
# 构建
python src/build.py

# 起服务（Agent 会话内会自动被回收，人工终端里常驻）
python src/serve.py
```

然后访问 <http://127.0.0.1:8000/>。

> 本机有全局代理环境变量，测试回环请求时要清掉：
> `env -u http_proxy -u https_proxy curl http://127.0.0.1:8000/`

## 部署（Cloudflare Pages）

1. 把仓库 push 到 GitHub。
2. Cloudflare Dashboard → Workers & Pages → Create → Pages，连 GitHub 选仓库。
3. 构建设置：

   | 项 | 值 |
   | --- | --- |
   | Framework preset | None |
   | Build command | `python src/build.py` |
   | Build output directory | `site` |

4. 环境变量加 `PYTHON_VERSION = 3.12`（Pages 默认 Python 版本较老）。
5. 部署完成会给一个 `xxx.pages.dev` 地址。
6. 要自定义域名：Pages 项目 → Custom domains → 接自己的域名（DNS 会自动配）。

## 猫娘页说明

`site/neko/` 放 NekoWebShow 的 `html_version_v2` 分支内容（单页面静态版）。
模型与背景素材约 500 MB，放在哪里需权衡：

- **直接提交仓库** —— 最简单，但仓库体积大、clone 慢。
- **Git LFS** —— 存大文件的正道，Pages 构建时注意 LFS 拉取配置。
- **Cloudflare R2** —— 对象存储，Cloudflare 内部访问免费，仓库保持轻量。

## 素材版权

NekoWebShow 代码为 AGPL v3。**但模型与背景图是从《猫娘乐园》解包的素材，版权归原权利人所有**，
作者 Chocola-X 在其 README 中亦如此声明。本仓库仅作个人学习与展示用途。
