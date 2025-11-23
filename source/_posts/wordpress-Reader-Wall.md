---
title: Wordpress添加读者墙
date: 2023-03-06 15:01:07
tags: [Wordpress,小技巧,读者墙]
categories:
	- 记录
---
>  实现原理：获取站点读者的评论信息（头像、评论数、链接）

代码如下：

```php+HTML
if(!function_exists("deep_in_array")) {
    function deep_in_array($value, $array) { // 返还数组序号
        $i = -1;
        foreach($array as $item => $v) {
            $i++;
            if($v["email"] == $value){
                return $i;
            }
        }
        return -1;
    }
}

//获取评论信息
function get_active_friends($num = null,$size = null,$days = null) {
    $num = $num ? $num : 15;
    $size = $size ? $size : 34;
    $days = $days ? $days : 30;
    $array = array();
    $comments = get_comments( array('status' => 'approve','author__not_in'=>1,'date_query'=>array('after' => $days . ' days ago')) );
    if(!empty($comments))    {
        foreach($comments as $comment){
            $email = $comment->comment_author_email;
            $author = $comment->comment_author;
            $url = $comment->comment_author_url;
            $data = human_time_diff(strtotime($comment->comment_date));
            if($email!=""){
                $index = deep_in_array($email, $array);
                if( $index > -1){
                    $array[$index]["number"] +=1;
                }else{
                    array_push($array, array(
                        "email" => $email,
                        "author" => $author,
                        "url" => $url,
                        "date" => $data,
                        "number" => 1
                    ));
                }
            }
        }
        foreach ($array as $k => $v) {
            $edition[] = $v['number'];
        }
        array_multisort($edition, SORT_DESC, $array); // 数组倒序排列
    }
    $output = '<ul class="avabook">';
    if(empty($array)) {
        $output = '<li>none data.</li>';
    } else {
        $max = ( count($array) > $num ) ? $num : count($array);
        
        for($o=0;$o < $max;$o++) {
            $v = $array[$o];
            $active_avatar = get_avatar($v["email"],$size);
            $active_url = $v["url"] ? $v["url"] : "javascript:;";
            $active_alt = $v["author"] . ' - 共'. $v["number"]. ' 条评论，最后评论于'. $v["date"].'前。';
            $output .= '<li class="active-item" data-info="'.$active_alt.'"><a target="_blank" rel="external nofollow" href="'.$active_url.'" title="'.$active_alt.'">'.$active_avatar.'</a></li>';
        }
        
    }
    $output .= '</ul>';
    return  $output;
}
//生成短代码函数 --- 
function active_shortcode( $atts, $content = null ) {
    extract( shortcode_atts( array(
            'num' => '',
            'size' => '',
            'days' => '',
        ),
        $atts ) );
    return get_active_friends($num,$size,$days);
}
add_shortcode('active', 'active_shortcode');
```

根据主题情况调用
```php
<?php echo get_active_friends();?>
或
<?php echo do_shortcode('[active num=15 size=45 days=60]');?>
```

页面中直接使用该方法即可，短代码那一部份是为了方便配合其它函数的调用