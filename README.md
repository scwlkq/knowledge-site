# 我的知识站

基于 Hugo + GitHub Pages 搭建的个人知识库。

## 快速开始

### 本地预览

```bash
hugo server -D
```

访问 http://localhost:1313 查看效果。

### 写新文章

在 `content/posts/` 目录下创建新的 Markdown 文件：

```bash
hugo new content/posts/my-new-post.md
```

或者直接创建 `.md` 文件，添加 Front Matter：

```markdown
---
title: "文章标题"
date: 2026-05-24T16:50:00+08:00
draft: false
tags: ["标签1", "标签2"]
categories: ["分类"]
---

文章内容...
```

### 发布到线上

```bash
git add .
git commit -m "添加新文章"
git push
```

GitHub Actions 会自动构建并部署到 GitHub Pages。

## 目录结构

```
knowledge-site/
├── content/          # 文章内容
│   └── posts/       # 博客文章
├── themes/          # 主题目录
│   └── PaperMod/   # PaperMod 主题
├── .github/
│   └── workflows/
│       └── deploy.yml  # 自动部署配置
├── hugo.toml        # 站点配置
└── README.md        # 本文件
```

## 配置说明

### 安装主题

由于网络原因，主题需要手动安装：

```bash
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

### 修改站点信息

编辑 `hugo.toml`：

- `baseURL`: 改为你的 GitHub Pages 地址
- `title`: 站点标题
- `params.author`: 你的名字

### GitHub Pages 设置

1. 在 GitHub 创建仓库（建议命名为 `knowledge-site`）
2. 推送代码到 GitHub
3. 进入仓库 Settings → Pages
4. Source 选择 "GitHub Actions"
5. 等待部署完成

## 主题文档

PaperMod 主题文档：https://github.com/adityatelange/hugo-PaperMod/wiki

## 常用命令

### 基础命令

```bash
# 创建新文章
hugo new content/posts/文章名.md

# 本地预览（包含草稿）
hugo server -D

# 构建静态文件
hugo

# 查看 Hugo 版本
hugo version
```

### 快捷命令（已配置）

为了提高效率，已在 `~/.zshrc` 中配置了以下快捷命令：

```bash
# 进入项目目录
blog

# 创建新文章
blog-new 文章名.md

# 启动本地预览
blog-serve

# 一键发布（add + commit + push）
blog-publish
```

**使用示例：**

```bash
# 1. 创建新文章
blog-new my-article.md

# 2. 编辑文章
# 使用你喜欢的编辑器编辑 content/posts/my-article.md

# 3. 本地预览
blog-serve

# 4. 发布到线上
blog-publish
```

**注意：** 新终端窗口需要先执行 `source ~/.zshrc` 或重启终端才能使用这些命令。

## 注意事项

- 文章的 `draft: false` 才会发布
- 提交前先本地预览确认效果
- GitHub Actions 构建需要几分钟
