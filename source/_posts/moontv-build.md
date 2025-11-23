---
title: 免费影视媒体库 MoonTV 部署全教程：跨设备同步 · 多用户管理
date: 2025-08-05 15:40:12
tags:
  - MoonTV
categories:
  - 笔记
---

MoonTV 是一个开源、完全免费的影视聚合平台，支持 Cloudflare Pages、Vercel、Docker 等多种部署方式。一键部署即可拥有网页端、手机端、电视端都能访问的私有影视库，多源搜索、无广告、跨设备同步——真正的看片神器。

![CleanShot 2025-07-26 at 11.01.42](https://raw.githubusercontent.com/wuyueerhao/pigoimg/master/20250726110153683.png)

## 🌟 MoonTV 能做什么？为什么选择它？

在如今视频平台会员费高涨、资源分散、广告泛滥的大环境下，MoonTV 提供了一个极具吸引力的解决方案：

- **聚合搜索全网资源**：支持多个免费资源站，一次搜索即可获取全源结果，覆盖电影、电视剧、动漫、综艺等。
- **高清流畅在线播放**：集成 ArtPlayer 和 HLS.js，播放体验媲美专业平台。
- **跨设备同步记录**：支持本地存储和云端（如 Redis、Cloudflare D1），在多个设备之间同步播放进度和收藏。
- **PWA 支持**：可添加到桌面、主屏幕使用，离线缓存提升使用便捷度。
- **响应式布局**：电脑、手机、电视屏幕完美适配，操作流畅无压力。
- **完全开源 + 私有部署**：可运行在你自己的服务器或免费平台上，数据完全掌控。
- **管理员后台**：支持多账户、用户管理、自定义配置（需使用云存储）。
- **无广告 / 实验性跳广告功能**：告别片头片尾烦人广告。

MoonTV 不仅适合个人用户建立私有观影平台，也逐渐成为自建 NAS 用户与影视爱好者的首选播放器。
<!-- more -->
## 🔧 技术栈一览

| 分类     | 技术                                           |
| -------- | ---------------------------------------------- |
| 前端框架 | Next.js 14 (App Router)                        |
| UI/样式  | Tailwind CSS 3                                 |
| 编程语言 | TypeScript 4                                   |
| 播放器   | ArtPlayer + HLS.js                             |
| 代码质量 | ESLint + Prettier + Jest                       |
| 部署方式 | 支持 Vercel / Cloudflare / Docker / 飞牛 OS 等 |

## 最新上线特性：Docker + Redis 搭建更强大

据项目官方更新（2025 年 7 月初），最新版本已支持：

- **后台管理面板**：docker + redis 存储方式下，通过访问 `/admin` 页面，可管理站点配置、视频源、权限和用户状态。
- **定时剧集刷新功能**：每小时自动更新影片 / 剧集总数，使播放记录和收藏显示更准确。
- **音量调节快捷键**：桌面端支持键盘上下键调节音量，移动端直接使用设备音量键控制音量。

目前即正在开发支持 D1 和 PG 存储的版本，未来会有更多部署选择。

## 🚀 快速部署 MoonTV（以 Vercel 为例）

MoonTV 支持多种部署方式，其中 Vercel 是最便捷的方式之一，**无需服务器、永久免费、自动构建上线**，适合没有服务器的新手用户。

### 📦 Vercel 一键部署步骤：

1. 前往 [MoonTV GitHub 项目](https://github.com/senshinya/MoonTV) 并点击右上角 `Fork` 本仓库；

2. 登陆 [Vercel](https://vercel.com/)，点击 **Add New → Project**，选择 Fork 后的仓库。

   ![CleanShot 2025-07-26 at 10.56.16](https://raw.githubusercontent.com/wuyueerhao/pigoimg/master/20250726105629589.png)

3. （强烈建议）设置 PASSWORD 环境变量。

4. 保持默认设置完成首次部署。

   ![CleanShot 2025-07-26 at 10.57.46](https://raw.githubusercontent.com/wuyueerhao/pigoimg/master/20250726105805446.png)

   ![CleanShot 2025-07-26 at 11.00.53](https://raw.githubusercontent.com/wuyueerhao/pigoimg/master/20250726110109228.png)

5. 如需自定义 `config.json`，请直接修改 Fork 后仓库中该文件。

   ![CleanShot 2025-07-26 at 11.13.18](https://raw.githubusercontent.com/wuyueerhao/pigoimg/master/20250726111330349.png)

📌 每次修改代码并提交到 `main` 分支，Vercel 都会自动重新构建并部署最新版本。

部署完成后即可通过分配的域名访问，也可以绑定自定义域名。

## 🌐 其他部署方式

MoonTV 还支持通过 Cloudflare Pages、Docker、群晖 NAS、飞牛 OS 等方式部署，适合进阶用户或有本地服务器的场景。

👉 **请访问官网或 GitHub 获取完整部署文档**：https://github.com/senshinya/MoonTV

## 🔑 管理后台与高级配置

如使用 Redis、D1、Upstash 等云存储方式，MoonTV 可开启多用户与后台管理功能：

1. 完成普通部署并成功访问。

2. 在 [upstash](https://upstash.com/) 注册账号并新建一个 Redis 实例，名称任意。

3. 复制新数据库的 **HTTPS ENDPOINT 和 TOKEN**

   ![CleanShot 2025-07-26 at 14.05.08](https://raw.githubusercontent.com/wuyueerhao/pigoimg/master/20250726140517706.png)

4. 返回你的 Vercel 项目，新增环境变量 **UPSTASH_URL 和 UPSTASH_TOKEN**，值为第二步复制的 endpoint 和 token

5. 设置环境变量 NEXT_PUBLIC_STORAGE_TYPE，值为 **upstash**；设置 USERNAME 和 PASSWORD 作为站长账号

6. 重试部署

- 设置环境变量：
  - `USERNAME`：管理员用户名
  - `PASSWORD`：管理员密码
  - `NEXT_PUBLIC_STORAGE_TYPE`：存储类型，如 redis/d1/upstash
- 登录后访问 `/admin` 即可进行：
  - 用户权限管理
  - 修改 config.json
  - 查看访问记录

![CleanShot 2025-07-26 at 14.03.39@2x](https://raw.githubusercontent.com/wuyueerhao/pigoimg/master/20250726140354153.png)
