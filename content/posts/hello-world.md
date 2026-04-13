+++
title = "第一篇文章：博客已经搭起来了"
date = 2026-04-13T14:35:00+08:00
draft = false
slug = "hello-world"
summary = "这是一篇示例文章，用来确认博客结构、中文排版和部署流程都已经可用。"
tags = ["Hugo", "GitHub Pages", "建站"]
categories = ["博客"]
+++

这是博客的第一篇文章。

站点当前已经具备这些基础能力：

- 使用 `Hugo` 生成静态页面
- 使用 `PaperMod` 作为简洁主题
- 支持中文写作与站内搜索
- 可以通过 GitHub Actions 自动部署到 GitHub Pages

之后你写文章时，常用流程会很简单：

```bash
hugo new content/posts/my-note.md
```

把 `draft = true` 改成 `draft = false`，写完后本地预览：

```bash
hugo server -D
```

如果内容确认没问题，提交并推送到 GitHub，站点就会自动更新。

## 之后建议先做的三件事

1. 把 `hugo.toml` 里的 `baseURL` 改成你自己的 GitHub Pages 地址。
2. 把社交链接中的 GitHub 用户名改成你自己的。
3. 再写 2 到 3 篇真正的文章，把博客内容撑起来。
