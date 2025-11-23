---
title: 如何屏蔽 YouTube Ad blockers violate YouTube's Terms of Service
date: 2023-10-27 18:10:11
tags: [YouTube]
categories:
	- 笔记
---

![image-20231027175702241](https://raw.githubusercontent.com/wuyueerhao/upimage/master/image-20231027175702241.png)

操作步骤： （已 chrome 为例）

1. 下载并安装uBlock Origin。

   https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm

2.  添加“自订静态过滤规则”。 

```javascript
   youtube.com##+js(set, yt.config_.openPopupConfig.supportedPopups.adBlockMessageViewModel, false)
   youtube.com##+js(set, Object.prototype.adBlocksFound, 0)
   youtube.com##+js(set, ytplayer.config.args.raw_player_response.adPlacements, [])
   youtube.com##+js(set, Object.prototype.hasAllowedInstreamAd, true)
```

   正常情况下，步骤 2 之后刷新当前视频页面即可，如果不生效，可以按步骤 3 的操作

3. 在uBlock Origin设置--规则列表 选项中，点击“清除所有缓存”，然后点击“立即更新”。 

   ![image-20231027180205452](https://raw.githubusercontent.com/wuyueerhao/upimage/master/image-20231027180205452.png)

4. 在Youtube页面，关闭除了uBlock Origin以外的广告拦截插件，如Adblock。 

5. 刷新页面即可。

6. 若突然无法观看，请重复步骤3。