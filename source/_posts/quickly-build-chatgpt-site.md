---
title: 10分钟快速搭建可商用的ChatGPT站点-GPTLink
date: 2023-05-31 15:33:05
tags: [ChatGPT,搭建,GPTLink]
categories:
	- 笔记
---

## GPTLink是什么

GPTLink是一个**开源的项目**，项目基于 PHP (Hyperf) + Vue 开发，支持docker部署，开箱即用的控制台，UI完美适配PC和移动端，支持自定义付费套餐，一键导出对话和任务拉客等等功能，只需简单几步，即可快速**搭建可商用的 ChatGPT 站点**，包含用户、订单、任务、付费等功能，有动手能力的同学可以搭建一套系统，赚点饭钱。

![image-20230531152605208](https://raw.githubusercontent.com/wuyueerhao/upimage/master/image-20230531152605208.png)


## 功能概览

- 支持 Docker 部署
- 开箱即用的控制台
- 完美适配移动端
- 自定义付费套餐
- 一键导出对话
- 任务拉新获客
<!-- more -->
## 开始使用

1. 项目基于 PHP (Hyperf) + Vue 开发，推荐使用 Docker 进行部署；

2. 准备好一个 API Key，推荐使用[GPTLINK](https://gpt-link.com/) Key；

   - [GPTLINK](https://gpt-link.com/) Key ，注册完成之后进入个人中心申请开发者后获取 API Key，过程非常简单，无需审核，接口无需代理；
   - OpenAi 官方 Key；

3. 微信相关资源（[网站应用](https://developers.weixin.qq.com/doc/oplatform/Website_App/WeChat_Login/Wechat_Login.html)，[微信公众号](https://mp.weixin.qq.com/)，[微信支付](https://pay.weixin.qq.com/)），网站应用用于 PC 端扫码登录，公众号用于微信内网页登录，缺省情况将无法在对应渠道使用；

## 项目配置

项目提供有限的权限控制功能，项目配置文件位于 `gptserver/.env`，如诺不存在此文件，将 `gptserver/.env.example` 更名为 `.env` 作为配置项进行使用，详细的配置说明 [点此查看](https://github.com/gptlink/gptlink/blob/master/docs/ENV.md)

## 部署

项目支持多种部署方式，部署文档参考：[点此查看](https://github.com/gptlink/gptlink/blob/master/docs/DEPLOY.md)

- PHP 环境部署
- Docker 部署
- Docker Compose 部署
- 云主机镜像部署

### 访问

部署完成后访问 `http://域名或IP` 进入对话页面，`/admin/` 路径访问管理页，管理员账号密码为配置项设置的 `ADMIN_USERNAME` 与 `ADMIN_USERNAME` ，如不传入，默认账号密码为 `admin` `admin888`

GitHub地址：https://github.com/gptlink/gptlink

演示站点：https://gpt-link.com/chat/1685442889946