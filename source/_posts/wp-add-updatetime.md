---
title: WordPress显示文章最后更新时间的方法
date: 2023-03-07 17:53:20
tags: [Wordpress,小技巧]
categories:
	- 记录
---
一些技术性文章，添加更新时间可以提示阅读者注意当前代码是否适用

#### 方法1--自定义代码
内容之前显示，插入的带吗会在文章内容前显示

```php
function wpb_last_updated_date( $content ) {
    $u_time = get_the_time('U'); 
    $u_modified_time = get_the_modified_time('U'); 
    $custom_content = ''; 
    if ($u_modified_time >= $u_time + 8640000) { //判断文章更新的时间是否大于100天
        $updated_date = get_the_modified_time('Y-m-j');
        $updated_time = get_the_modified_time('h:i'); 
        $custom_content .= '<div class="c-alert c-alert-warning"><i class="far fa-exclamation-triangle"></i>提醒：本文最后更新于 '. $updated_date . ' at '. $updated_time .' 文中所描述的信息可能已发生改变，请仔细核实。</div>';  
    } 
    $custom_content .= $content;
    return $custom_content;
}
add_filter( 'the_content', 'wpb_last_updated_date' );
```
css样式根据自己的需求调整
<!-- more -->

#### 方法2--调用主题代码

```php
<?php
$u_time = get_the_time( 'U' );
$u_modified_time = get_the_modified_time( 'U' );
if ( $u_modified_time >= $u_time + 86400 ) {
	echo "<p>最后更新时间 ";
	the_modified_time( 'Y-m-j h:s a' );
	echo " at ";
	the_modified_time();
	echo "</p> ";
} ?>
```

在主题中需要的页面标签位置添加，同样css样式自行调整

#### 方法3--修改发布时间
把文章的发布时间修改为当前修改时间

```php
<?php $u_time = get_the_time( 'U' );
$u_modified_time = get_the_modified_time( 'U' );
if ( $u_modified_time >= $u_time + 86400 ) {
	echo "最后更新时间 ";
	the_modified_time( 'F jS, Y' );
	echo ", ";
} else {
	echo "发布时间 ";
	the_time( 'F jS, Y' );
} ?>
```

#### 方法4--使用插件
推荐 **Last Modified Timestamp** 或 **Latest Post Shortcode**， 原理一样