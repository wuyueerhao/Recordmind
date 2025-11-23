---
title: 最新免拔卡解锁Tiktok App实用小技巧
date: 2024-04-25 14:58:45
tags: [Tiktok]
---

TikTok目前应该是放松了检测机制，以前国内用户想要正常的刷TikTok，方法无外乎抓包降级（iOS端）、拔卡（或者国外卡），最近实测仅需更改手机的语言与地区+代理，无需额外使用代理软件（如QuantumultX、Shadowrocket、Surge、Loon）进行MITM解锁，即可正常刷。
<!-- more -->
![IMG_4772](https://raw.githubusercontent.com/wuyueerhao/upimage/master/IMG_4772.png)

### 修改语言与地区

TikTok目前已经开放的国家和地区有：**新加坡、马来西亚、菲律宾、越南、日本、中东、泰国、印尼、俄罗斯、美国、巴西、土耳其、英国、韩国**等。

iOS 用户在【设置- 通用 - 语言与地区 - 地区】这里修改为Tiktok官方提供服务的国家或地区。

![IMG_4771](https://raw.githubusercontent.com/wuyueerhao/upimage/master/IMG_4771.png)

这里如果设置为Tiktok不提供服务的国家或地区，大概率会出现无法登陆、黑屏等奇怪现象。

### 挂代理

**设置完挂上解锁节点**应该就可以刷了，**不需要拔SIM卡**。

Tiktok对节点IP限制不严格，但单个节点高频访问会导致IP被短暂拉黑，出现黑屏、无法加载的情况。

最好是选择和上一步设置的地区相符合的节点，然后设置为全局代理。

当然也可以使用分流规则。

### Tiktok 分流规则

分流规则出处 **毒奶**

https://limbopro.com/

**QuantumultX 适用**

```
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/QuantumultX/TikTok/TikTok.list
```

**Surge 适用**

```
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Surge/TikTok/TikTok.list
```

**小火箭适用**

```
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Shadowrocket/TikTok/TikTok.list
```

### 写在最后

如果不想折腾，可以试一下Tiktok 网页版，挂上相应地区/国家的节点，不要太方便。

https://www.tiktok.com/foryou

![CleanShot 2024-04-24 at 17.24.40](https://raw.githubusercontent.com/wuyueerhao/upimage/master/CleanShot%202024-04-24%20at%2017.24.40.gif)
