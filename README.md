# Personal Blog

一个基于 `Hugo` + `PaperMod` 的个人博客模板，适合用 Markdown 写中文文章，并部署到 GitHub Pages。

## 当前已经完成

- 主题已接入：`PaperMod`
- 站点配置已改为中文写作优先
- 已加入 `文章 / 归档 / 搜索 / 关于` 页面
- 已加入 GitHub Pages 自动部署工作流
- 已加入适合中文阅读的排版样式

## 本地预览

```bash
hugo server -D
```

本地地址通常是 `http://localhost:1313/`。

## 新建文章

```bash
hugo new content/posts/my-post.md
```

写完后把文章头部的 `draft = true` 改成 `draft = false`。

## 部署到 GitHub Pages

推荐使用用户站点，这样地址最干净：

1. 在 GitHub 新建仓库，仓库名使用 `<你的用户名>.github.io`
2. 把当前项目推到这个仓库
3. 在仓库 `Settings > Pages` 中把 `Source` 改成 `GitHub Actions`
4. 等待 `.github/workflows/hugo.yaml` 跑完
5. 站点地址会是 `https://<你的用户名>.github.io/`

如果你想用项目站点，仓库名可以任意，但需要把 `hugo.toml` 里的：

- `baseURL = "https://example.github.io/"`

改成：

- `https://<你的用户名>.github.io/<仓库名>/`

## 需要你自己修改的地方

- `hugo.toml` 里的 `baseURL`
- `hugo.toml` 里的 GitHub 社交链接
- 博客标题、副标题、关于页内容
- `content/posts/` 里的实际文章

## 常用命令

```bash
hugo server -D      # 本地预览，包括草稿
hugo                # 生成生产静态文件到 public/
```
# sqs17.github.io
