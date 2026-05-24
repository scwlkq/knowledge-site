# 我的知识站

基于 Hugo + GitHub Pages 搭建的个人知识库。

**在线访问：** https://scwlkq.github.io/knowledge-site/

## 项目简介

这是一个使用 Hugo 静态站点生成器和 PaperMod 主题搭建的个人知识站，通过 GitHub Actions 实现自动部署。

**特点：**
- 📝 Markdown 写作，简单高效
- 🚀 Git 提交自动发布到线上
- 🎨 简洁现代的 PaperMod 主题
- 🏷️ 支持标签和分类管理
- 📦 完全免费，托管在 GitHub Pages

## 快速使用

### 方式一：使用快捷命令（推荐）

已在 `~/.zshrc` 中配置了快捷命令：

```bash
# 1. 创建新文章
blog-new 文章标题.md

# 2. 编辑文章（使用你喜欢的编辑器）
# 文章位置：content/posts/文章标题.md

# 3. 本地预览
blog-serve
# 访问 http://localhost:1313 查看效果

# 4. 发布到线上
blog-publish
```

**其他快捷命令：**
- `blog` - 快速进入项目目录

**注意：** 新终端需要执行 `source ~/.zshrc` 或重启终端才能使用快捷命令。

### 方式二：使用完整命令

```bash
# 进入项目目录
cd /Users/suncong32/Desktop/knowledge-site

# 创建新文章
hugo new content/posts/文章标题.md

# 本地预览
hugo server -D

# 发布到线上
git add .
git commit -m "更新文章"
git push
```

## 文章格式

每篇文章的开头需要包含 Front Matter（元数据）：

```markdown
---
title: "文章标题"
date: 2026-05-24T16:50:00+08:00
draft: false
tags: ["标签1", "标签2"]
categories: ["分类"]
---

文章内容使用 Markdown 格式...
```

**重要：** `draft: false` 文章才会发布到线上。

## 目录结构

```
knowledge-site/
├── content/
│   ├── posts/          # 博客文章目录
│   └── archives.md     # 归档页面
├── themes/
│   └── PaperMod/       # PaperMod 主题
├── .github/
│   └── workflows/
│       └── deploy.yml  # 自动部署配置
├── archetypes/
│   └── default.md      # 文章模板
├── hugo.toml           # 站点配置
├── 写作指南.md         # 详细写作说明
└── README.md           # 本文件
```

## 技术栈

- **Hugo**: 静态站点生成器
- **PaperMod**: 简洁现代的 Hugo 主题
- **GitHub Pages**: 免费静态网站托管
- **GitHub Actions**: 自动化部署

## 常见问题

### 如何修改站点信息？

编辑 `hugo.toml` 文件：
- `title`: 站点标题
- `params.author`: 作者名称
- `params.homeInfoParams`: 首页欢迎信息

### 标签和分类如何使用？

在文章的 Front Matter 中添加：
```yaml
tags: ["Hugo", "博客"]
categories: ["技术"]
```

标签和分类会自动出现在对应的页面中。

### 如何查看部署状态？

访问 GitHub Actions 页面：
https://github.com/scwlkq/knowledge-site/actions

部署通常需要 1-2 分钟完成。

### 快捷命令不生效？

执行以下命令重新加载配置：
```bash
source ~/.zshrc
```

或者打开新的终端窗口。

## 参考资料

- [Hugo 官方文档](https://gohugo.io/documentation/)
- [PaperMod 主题文档](https://github.com/adityatelange/hugo-PaperMod/wiki)
- [Markdown 语法指南](https://www.markdownguide.org/)
- [详细写作指南](./写作指南.md)
