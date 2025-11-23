---
title: WordPress获取文章的上一篇和下一篇
date: 2023-02-24 21:45:05
tags: [Wordpress,小技巧]
categories:
	- 记录
---
WordPress 提供了两个快捷函数来获取上一篇/下一篇文章，自动生成完整的 a 标签，格式为：`<a href="文章链接">文章标题</a>`。

{% codeblock %}
<?php previous_post_link('上一篇：%link'); ?>
完整语法：previous_post_link( $format, $name, $in_same_cat, $excluded_categories = "");
<?php next_post_link('下一篇：%link'); ?>
完整语法：next_post_link($format, $name, $in_same_cat, $excluded_categories = "");
{% endcodeblock %}


参数介绍：
$format：格式化被显示的字符串，缺省值是”‘? %link”，第二个函数缺省值是”%link ?”。
$name：被显示的字符串，缺省值是上一篇或下一篇的”$title”，也可以设置为其它你想显示的字符串。
$in_same_cat ：是否显式同一类别下的文章，缺省值false表示不区分类别。
$excluded_categories：是否排除掉某分类，缺省值不排除 ，多个以英文逗号分隔。
例：

> previous_post_link("%link","< 上一篇",true) // 

显示： < 上一篇
使用默认的方式会遇到一个小问题，如果当前文章是最新或者最后一篇，在输出结果时会在上一篇/下一篇 显示当前文章，只需要在输出时加一个判断即可。
Wordpress判断文章是否有上一篇和下一篇
<!-- more -->

{% codeblock %}
<?php
  $prev_post = get_previous_post();
  if ( ! empty( $prev_post ) ): ?>
    <a href="<?php echo get_permalink( $prev_post->ID ); ?>">
      <?php echo apply_filters( 'the_title', $prev_post->post_title ); ?>
    </a>
  <?php else: ?>
    <span>没有上一页了</span>
  <?php endif;
?>
{% endcodeblock %}

如果有上一篇：显示上一篇的标题及链接，没有提示：没有上一篇了

{% codeblock %}
<?php 
  $next_post = get_next_post();
  if(!empty($next_post)):?>
    <a href="<?php echo get_permalink( $next_post->ID ); ?>">
      <?php echo apply_filters( 'the_title', $next_post->post_title ); ?>
    </a>
  <?php else: ?>
    <span>没有下一页了</span>
  <?php endif;
?> 
{% endcodeblock %}

如果有下一篇：显示下一篇的标题及链接，没有提示：没有下一篇了