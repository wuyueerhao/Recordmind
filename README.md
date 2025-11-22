# Recordmind

一个基于 Hexo 的个人博客，使用 NexT 主题。

## 本地开发

### 安装依赖

```bash
npm install
```

### 清理缓存

```bash
npm run clean
```

### 生成静态文件

```bash
npm run build
```

### 启动本地服务器

```bash
npm run server
```

访问 http://localhost:4000 查看博客。

## 部署到 Cloudflare Pages

### 1. 推送代码到 GitHub

如果还没有推送代码到 GitHub，请先创建 GitHub 仓库，然后运行：

```bash
git remote add origin YOUR_GITHUB_REPO_URL
git branch -M main
git push -u origin main
```

### 2. 在 Cloudflare Pages 创建项目

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 进入 **Workers & Pages** > **Create application** > **Pages** > **Connect to Git**
3. 授权并选择你的 GitHub 仓库
4. 配置构建设置：
   - **Framework preset**: 选择 `Hexo` 或 `None`
   - **Build command**: `npm run build`
   - **Build output directory**: `public`
   - **Root directory**: `/` (默认)
5. 点击 **Save and Deploy**

### 3. 环境变量（可选）

如果需要，可以在 Cloudflare Pages 设置中添加环境变量：

- `NODE_VERSION`: `24.10.0` (已在 `.node-version` 文件中指定)

### 4. 自动部署

配置完成后，每次推送到 GitHub 的 `main` 分支都会自动触发 Cloudflare Pages 部署。

## 写作

### 创建新文章

```bash
npx hexo new "文章标题"
```

这将在 `source/_posts/` 目录下创建一个新的 Markdown 文件。

### Front Matter 示例

```markdown
---
title: 文章标题
date: 2025-11-22 22:00:00
tags:
  - 标签1
  - 标签2
categories:
  - 分类
---

文章内容...
```

## 主题配置

本博客使用 NexT 主题的备用配置方式。主题配置文件位于项目根目录的 `_config.next.yml`。

修改主题配置时，只需编辑 `_config.next.yml` 文件，无需修改 `node_modules` 中的主题文件。

### 常用配置

- **Scheme**: 在 `_config.next.yml` 中修改 `scheme` 参数（可选：Muse, Mist, Pisces, Gemini）
- **菜单**: 修改 `menu` 配置添加或删除菜单项
- **侧边栏**: 修改 `sidebar` 配置调整侧边栏行为
- **第三方服务**: 在相应章节配置评论、统计、搜索等功能

详细配置说明请参考：https://theme-next.js.org/docs/

## 目录结构

```
.
├── _config.yml           # Hexo 主配置文件
├── _config.next.yml      # NexT 主题配置文件（备用配置）
├── package.json          # 项目依赖
├── scaffolds/            # 文章模板
├── source/               # 源文件
│   ├── _posts/          # 博客文章
│   └── about/           # 关于页面等
└── themes/              # 主题文件（通过 npm 安装时为空）
```

## 技术栈

- [Hexo](https://hexo.io/) - 静态博客生成器
- [NexT](https://theme-next.js.org/) - Hexo 主题
- [Cloudflare Pages](https://pages.cloudflare.com/) - 静态网站托管

## License

MIT
