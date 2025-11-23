---
title: 如何防止Wordpress的xmlrpc.php被恶意利用
date: 2023-07-31 16:13:54
tags: [wordpress，xmlrpc]
categories:
  - 笔记	
---

最近搭建了一个个人网站，安装了一些防护插件后，邮件总是提示有用户试图登录被组织，即使设置了屏蔽也没办法拦截，检查了登录路径，发现均是通过xmlrpc.php的方式，网上搜集整了了一些关于xmlrpc.php的资料以及如何禁用xmlrpc.php的方法。

![1674059990250](https://raw.githubusercontent.com/wuyueerhao/upimage/master/1674059990250.png)

### 什么是 XML-RPC

要理解为什么 `xmlrpc.php` 文件会被扫描，首先要明白什么是 XML-RPC，它的全称是 XML Remote Procedure Call，即 XML 远程过程调用，它是一套允许运行在不同操作系统、不同环境的程序实现基于网络过程调用的规范和一系列的实现。

简单说就是一个程序可以要求另外一个程序做事情。随着 REST API的兴起，xmlrpc.php已经被代替了，但WordPress仍然保留着这个文件。很可能给网站带来一些安全风险，被利用来攻击 WordPress 网站。

XML-RPC 使用 http 作为传输协议，XML 作为传送信息的编码格式，一个 XML-RPC 消息就是一个请求体为 XML 的 http-post 请求，被调用的方法在服务器端执行并将执行结果以 XML 格式编码后返回。

一个 XML-RPC 协议包括两部分：

- RPC client，用来向 RPC 服务端调用方法，并接收方法的返回数据。
- RPC server，用于响应 RPC 客户端的请求，执行方法，并回送方法执行结果。

WordPress 源代码（ `xmlrpc.php` 文件）中已经包含了完整的 RPC 服务端代码，它支持对文章，媒体，评论，分类，选项等等各方面数据的管理。

换句话说，只要懂 XML-RPC 协议，就可以使用 XML-RPC 对 WordPress 博客的各个方面进行操作，也就是说可以使用 XML-RPC 做 WordPress 的客户端。
<!-- more -->
### XML-RPC 安全隐患

XML-RPC 那么好用，也造成了一定的安全隐患，主要是给攻击者提供了便利，所以攻击者的一项工作就是扫描 xmlrpc.php 文件，以便可以实现：

1. **XML-RPC pingbacks 攻击**，攻击者可以利用 **XML-RPC 的 pingbacks** 方法对 WordPress 实行 **DDoS** （分布式拒绝服务）攻击，如果你使用了 CDN 服务商的 DNS 保护服务，攻击者还可以使用 **pingbacks** 方法获取站点的真实 IP，剩下不用我说了吧。
2. 即使 WordPress 设置了登录次数限制，但是**使用 XML-RPC 暴力破解 WordPress 的账号密码**却逃过了限制，并且 XML-RPC 一次请求就可以执行上百次密码的暴力破解。

![image-20230731155550076](https://raw.githubusercontent.com/wuyueerhao/upimage/master/image-20230731155550076.png)

### 禁用 XMLRPC

防止 WordPress 的 `xmlrpc.php` 被扫描可以采取以下措施：

1. 如果你手动更新WordPress版本，那么就直接删除 xmlrpc.php，以后更新版本时也手动删除掉安装包里面的 xmlrpc.php。这是最直接有效的方法，带来的弊端是你也无法使用远程发布文章功能了，当然绝大多数人根本用不到。

2. **禁用 XML-RPC 功能：** 如果你不使用 XML-RPC 或不打算与其他应用程序（如移动应用或远程发布工具）进行集成，可以通过禁用 XML-RPC 功能来防止扫描。

   在当前主题的 functions.php 文件添加下面这行代码就能关闭它：

    ```php 
    add_filter('xmlrpc_enabled', '__return_false');
    ```

3. Apache环境可以**修改 .htaccess 文件：** 可以通过修改 WordPress 网站的根目录中的 `.htaccess` 文件来限制对 `xmlrpc.php` 的访问。在 `.htaccess` 文件中添加以下代码：

   ```php
   # Block access to xmlrpc.php
   <Files xmlrpc.php>
   Order Deny,Allow
   Deny from all
   </Files>
   ```

   这将阻止对 `xmlrpc.php` 的任何访问。

4. Nginx环境可以添加下面的规则：

   ```php 
   location ~* ^/xmlrpc.php$ {
   return 403;
   }
   ```

5. 如果以上两个方式都不好用，还可以在 WordPress 的wp-config.php里添加：

   ```php
   if(strpos($_SERVER['REQUEST_URI'], 'xmlrpc.php') !== false){
       $protocol   = $_SERVER['SERVER_PROTOCOL'] ?? '';
       if(!in_array($protocol, ['HTTP/1.1', 'HTTP/2', 'HTTP/2.0', 'HTTP/3'], true)){
           $protocol   = 'HTTP/1.0';
       }
       header("$protocol 403 Forbidden", true, 403);
       die;
   }
   ```

6. **使用 Web 应用程序防火墙（WAF）：** 如果你的主机支持 Web 应用程序防火墙，可以配置 WAF 来拦截对 `xmlrpc.php` 的请求。WAF 可以帮助你阻止恶意请求和扫描。

7. **更新 WordPress 版本：** 确保你的 WordPress 版本是最新的，因为更新通常会包含对安全漏洞的修复。

8. **使用安全插件：** 可以使用安全插件来增强 WordPress 的安全性，例如 "Wordfence" 或 "iThemes Security" 等插件。这些插件通常有阻止恶意请求的功能。

9. 彻底屏蔽 XML-RPC

总的来说，采取综合性的安全措施可以帮助保护你的 WordPress 网站免受恶意扫描和攻击。