# typecho模板中常用的调用函数

废话就不多说了，直接上代码吧，在此套模板中运用到的我都给罗列一下。

## 常规调用

站点名称（其实就是调用title标签的内容的）

``` php
<?php $this->options->title() ?>
```

站点网址（其实就是调用首页名称的）

``` php
<?php $this->options ->siteUrl(); ?>
```

站点描述调用

``` php
<?php $this->options->description() ?>
```

完整路径标题（其实就是调用面包屑导航的）

``` php
<?php $this->archiveTitle(' &raquo; ', < span class="string">'', ' | '); ?><?php $this ->options->title(); ?>
```

模板文件夹地址（博主基本不用，因为不知道要这个是干嘛）

``` php
<?php $this->options->themeUrl(); ?>
```

输出指定的php文件，一般起关联作用，起到组合作用

``` php
<?php $this->need('global.php'); ?>
```

输出作者字段

``` php
<?php $this->author(); ?>
```

输出头像（此处40为img元素的宽度和高度）

``` php
<?php $this->author->gravatar('40') ?>
```

该文作者全部文章列表链接

``` php
<?php $this->author->permalink (); ?>
```

该文作者个人主页链接

``` php
<?php $this->author->url(); ?>
```

该文作者的邮箱地址

``` php
<?php $this->author->mail(); ?>
```

typecho 默认的文章上下翻页的调用方法

``` php
<?php $this->thePrev(); ?> 
<?php $this->theNext(); ?>
```

判断是否为首页，输出相关内容（博主不常用）

``` php
<?php if ($this->is('index')): ?>
//首页输出内容
<?php else: ?>
//不是首页输出内容
< span><?php endif; ?>
```

文章列表或页面，评论数目输出方法

``` php
<?php $this->commentsNum('No Comments', '1 Comment' , '%d Comments'); ?>
```

截取部份文章简称“摘要”，200是字数限制

``` php
<?php $this->excerpt(200, '.. .'); ?>
```

调用自定义字段（官方文档坑爹，竟然没有，博主自己摸索出来的）

``` php
<?php $this->fields->fieldName ?>
```

RSS订阅地址输出方法

``` php
<?php $this->options->feedUrl(); ?>
```

获取最新post

``` php
<?php $this->widget('Widget_Contents_Post_Recent', 'pageSize=8&type=category')->parse('<li><a href="{permalink}">{title}</a></li>'); ?>
```

纯文字分类名称，不带链接

``` php
<?php $this->category(',', false); ?>
```

获取文章分类列表

``` php
<ul>
<?php $this->widget('Widget_Metas_Category_List')
                ->parse('<li><a href="{permalink}">{name}</a> ({count})</li>'); ?>
</ul>
// count 输出分类中文章的个数
// name  输出分类名称
```

获取分类列表，带高亮，看好，是带高亮的分类导航

``` php
<?php $this->widget('Widget_Metas_Category_List')->to($category); ?>
<?php while($category->next()): ?>
  <a<?php if($this->is('category', $category->slug)): ?> class="active"<?php endif; ?> href="<?php $category->permalink(); ?>"><?php $category->name(); ?></a>
<?php endwhile; ?>
```

获取某分类post

``` php
<ul>
<?php 
$this->widget('Widget_Archive@indexyc', 'pageSize=8&type=category', 'mid=1')
->parse('<li><a href="{permalink}" title="{title}">{title}</a></li>'); 
?>
</ul>
```

获取最新评论列表

``` php
// 版本A，获取最新评论，但是包含了博主的评论在里面了。
<ul>
    <?php $this->widget('Widget_Comments_Recent')->to($comments); ?>
    <?php while($comments->next()): ?>
        <li><a href="<?php $comments->permalink(); ?>"><?php $comments->author(false); ?></a>: <?php $comments->excerpt(50, '...'); ?></li>
    <?php endwhile; ?>
</ul>
```

``` php
// 版本B，获取最新评论，不包含作者的评论。
<?php $this->widget('Widget_Comments_Recent','ignoreAuthor=true')->to($comments); ?>
<?php while($comments->next()): ?>
    <div class="list-group list-group-alt"> 
      <a href="<?php $comments->permalink(); ?>" class="media list-group-item"> 
      <span class="pull-left thumb-sm"> <?php $comments->gravatar('40', ''); ?> </span> 
      <span class="media-body block m-b-none"><?php $comments->author(false); ?><br /> 
        <small class="text-muted"><?php $comments->excerpt(50, '...'); ?></small> 
      </span> 
      </a> 
     </div> 
<?php endwhile; ?>
```

首页获取最新文章（带条数限制）

``` php
<?php while ($this->next()): ?>
<?php if ($this->sequence <= 3): ?>
	// 中间可使用的输出方法
	<?php $this->permalink() ?> // 输出文章链接
	<?php $this->title() ?> // 输出标题
<?php endif; ?>
<?php endwhile; ?>
```

相关文章输出

``` php
<?php $this->related(5)->to($relatedPosts); ?>
    <?php if ($relatedPosts->have()): ?>    //这句也可以写成 if (count($relatedPosts->stack))
    <?php while ($relatedPosts->next()): ?>
        <li><a href="<?php $relatedPosts->permalink(); ?>" title="<?php $relatedPosts->title(); ?>"><?php $relatedPosts->title(); ?></a></li>
    <?php endwhile; ?>
    <?php else : ?>
        <li>无相关文章</li>
    <?php endif; ?>
```

隐藏head区域的程序版本和模版名称（博主一向尊重版权与知识归属，不会使用这个的）

``` php
<?php $this->header("generator=&template="); ?>
```

登陆与未登录用户展示不同内容（考虑到注入的风险，作者暂时没想过开放注册）

``` php
<?php if($this->user->hasLogin()): ?>
	// 登陆可见
<?php else: ?>
	// 未登录和登陆均可见
<?php endif; ?>
```

导航页面列表调用隐藏特定的页面 这个演示隐藏了album和search两个页面（1.0及1.1版本后台自己就支持隐藏，所以，可以忽略）

``` php
<ul>
<li<?php if($this->is('index')): ?> class="current"<?php endif; ?>><a href="<?php $this->options->siteUrl(); ?>">主页</a></li>
<?php $this->widget('Widget_Contents_Page_List')->to($pages); ?>
    <?php while($pages->next()): ?>
    <?php if (($pages->slug != 'album') && ($pages->slug != 'search')): ?>
    <li<?php if($this->is('page', $pages->slug)): ?> class="current"<?php endif; ?>><a href="<?php $pages->permalink(); ?>" title="<?php $pages->title(); ?>"><?php $pages->title(); ?></a></li>
    <?php endif; ?>
    <?php endwhile; ?>
</ul>
```

后台地址输出

``` php 
<?php $this->options->adminUrl(); ?>
```

当前登录用户名称

``` php
<?php $this->user->screenName(); ?>
```

退出登录地址

``` php
<?php $this->options->logoutUrl(); ?>
```

遍历输出文章

``` php
<?php while($this->next()): ?>
	// 文章标题内容等
<?php endwhile; ?>
```

RSS评论地址

``` php
<?php $this->options->commentsFeedUrl(); ?>
```

文章列表及页面“阅读更多分界线”

``` php
<?php $this->content('阅读剩余部分'); ?>
```

文章所在分类输出

``` php
<?php $this->category(','); ?>
```

文章标签调用

``` php
<?php $this->tags(', ', true, 'none'); ?>
````

全部文章列表输出，可应用于归档或网站地图，蜘蛛指引

``` php 
<?php $this->widget('Widget_Contents_Post_Recent', 'pageSize=10000')->parse('<li>{year}-{month}-{day} : <a href="{permalink}">{title}</a></li>'); ?>
```

全部标签列表，按照MID排序

``` php
<?php $this->widget('Widget_Metas_Tag_Cloud')
->to($taglist); ?><?php while($taglist->next()): ?>
<li><a href="<?php $taglist->permalink(); ?>" title="<?php $taglist->name(); ?>"><?php $taglist->name(); ?></a></li>
<?php endwhile; ?>
```

自定义标签数量（就这里面的20），按照文章数量排序

``` php
<?php $this->widget('Widget_Metas_Tag_Cloud', array('sort' => 'count', 'ignoreZeroCount' => true, 'desc' => true, 'limit' => 20))->to($tags); ?>
<?php while($tags->next()): ?>
<li><a rel="tag" href="<?php $tags->permalink(); ?>"><?php $tags->name(); ?></a></li>
<?php endwhile; ?>
```

自定义分类、标签、搜索、首页等文章分页数量，修改 functions.php 文件

``` php
function themeInit($archive) {
if ($archive->is('index')) {
$archive->parameter->pageSize = 10; // 自定义条数
}
}
或者：
function themeInit($archive) {
if ($archive->is('category', 'default')) {
$archive->parameter->pageSize = 10; // 自定义条数
}
}
```

调用某分类文章，pageSize是数量，mid是分类号

``` php
<?php $this->widget('Widget_Archive@index', 'pageSize=6&type=category', 'mid=47')
->parse('<li><a href="{permalink}">{title}</a></li>'); ?>
```

判断为当前页的第几篇文章，并单独输出代码，可应用于第一篇文章底部广告

``` php
<?php if ($this->sequence == 0): ?>
	//需要的插入
<?php endif; ?>
```

判断当前分类，输出内容

``` php
<?php if($this->category == "help"): ?>
	//当前分类为help缩略图，则输出内容。
<?php endif; ?>
```

首页不显示某分类内容（博主不用）

``` php
<?php while($this->next()): ?>
<?php if($this->category != "cateslug"): ?>
	//正常输出循环
<?php endif; ?>
<?php endwhile; ?>
```

可自定文章列表首篇显示样式

``` php
<?php if (($this->_currentPage == 1) && ($this->sequence == 1)): ?>
	 //首页第一篇文章
<?php else: ?>
	 //其它文章
<?php endif; ?>
```

单独输出文字内容

``` php
<?php _e('MoleRose'); ?>
```

文章输出最后修改时间

``` php
<?php echo date('Y年m月d日 H:i A', $this->modified);?>
```

## 页面制作

归档页面制作（推荐）

``` php
<?php $this->widget('Widget_Contents_Post_Recent', 'pageSize=10000')->to($archives);
    $year=0; $mon=0; $i=0; $j=0;
    $output = '<div id="archives" class="caption wrapper-lg blog-file-box"><dl class="clearfix">';
    while($archives->next()):
        $year_tmp = date('Y',$archives->created);
        $mon_tmp = date('m',$archives->created);
        $y=$year; $m=$mon;
        // if ($mon != $mon_tmp && $mon > 0) $output .= '';
        // if ($year != $year_tmp && $year > 0) $output .= '';
        if ($year != $year_tmp) {
            $year = $year_tmp;
            $output .= '<h3>'. $year .' 年</h3>'; //输出年份
        }
        if ($mon != $mon_tmp) {
            $mon = $mon_tmp;
            $output .= '<dt><small class="label bg-light">'. $mon .' 月</small></dt>'; //输出月份
        }
        $output .= '<dd class="col-md-6 col-sm-12 col-xs-12"><a href="'. $archives-