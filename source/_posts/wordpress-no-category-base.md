---
title: WordPress去掉分类url中category
date: 2023-03-03 11:02:49
tags: [Wordpress,小技巧]
categories:
	- 记录
---
正常情况下Wordpress的分类url中会包含category，去除与否实际上没有多大的关系。有些用户博客的分类就是二级目录，希望去掉分类目录 URL 中的 category，如何操作呢？

> 目前没有数据表明去除该字段对SEO是否有好处，大家根据自己的需求，酌情处理。

#### 方法1：使用插件

网上这类的插件很多，WP No Category Base、No category parents、WP Remove Category Base，自行搜索选择即可。这些插件的实现方式是重写Wordpress的rewrite规则，核心代码基本上就是一段php，不想安装插件的朋友可以直接把代码放到主题的function里，效果一样。

#### 方法2: 修改分类目录前缀

WordPress后台—设置—固定链接—分类目录前缀里输入半角字符： “.”。保存即可去掉分类前缀category。
<!-- more -->
![image-20230303105323713](https://raw.githubusercontent.com/wuyueerhao/upimage/master/image-20230303105323713.png)
这个方法只是让人看起来讲分类页面变成了二级目录，因为实际的地址变成了：“yousite.com/./分类/” ，只是浏览器过滤了“`/./`”而已。
适用于新站，没有分类栏目和文章，这样才不会出错，如果已有文章，这样的方法会使你的文章和分类栏目不存在，容易出现错误，**老站点不推荐此方法。**

#### 方法3: 纯代码 --- 推荐方法

如果仔细观察一下去掉 category 的分类目录 WordPress 的页面的 URL：
去掉 category 的分类目录页面：`https://vnvnv.com/easysays/`
比如关于页面：`https://vnvnv.com/message/`
是不是这两种页面的页面rewrite 规则是不是一样的，那么我们可以直接使用页面的 rewrite 规则来处理。
当 WordPress 进入页面 rewrite 规则的时候，我们首先判断一下，当前的 `pagename` 是不是某个分类的 slug，如果是，就把当前的 `query_var` 中的 `pagename` 换成 `category_name`，这样就可以实现了分类目录页面的正确跳转。

``` php
add_filter('request', function($query_vars) {
	if(!isset($_GET['page_id']) && !isset($_GET['pagename']) && !empty($query_vars['pagename'])){
		$pagename	= $query_vars['pagename'];
		$categories	= get_categories(['hide_empty'=>false]);
		$categories	= wp_list_pluck($categories, 'slug');
		if(in_array($pagename, $categories)){
			$query_vars['category_name']	= $query_vars['pagename'];
			unset($query_vars['pagename']);
		}
	}
	return $query_vars;
});
add_filter('pre_term_link', function($term_link, $term){
	if($term->taxonomy == 'category'){
		return '%category%';
	}
	return $term_link;
}, 10, 2);
```

>  此方法来源：我爱水煮鱼：[去掉 WordPress 分类目录 URL 中的 category 最佳方法](https://m.wpjam.com/m/wordpress-no-category-base/)

此方式可以正常访问去除前的分类链接，前台显示为去除后的分类链接。