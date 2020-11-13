# typecho自定义上下翻页样式

因为我们是需要制作模板的，模板又是都集成在 `themes` 中的，其实想改 `typecho` 原始的文件，改起来也简单，但是就脱离了模板的定义了

## 关于文章上下翻页自定义样式分解说明

- 首先，我们针对翻页的功能需要重新定义，此处就得用到 `function.php` 文件
- 翻页功能定义代码如下

``` php
/**
* 上一篇
* @access public
* @param string $default 如果没有上一篇,显示的默认文字
* @return void
*/
function theNext($widget, $default = NULL)
{
  $db = Typecho_Db::get();
  $sql = $db->select()->from('table.contents')
  ->where('table.contents.created > ?', $widget->created)
  ->where('table.contents.status = ?', 'publish')
  ->where('table.contents.type = ?', $widget->type)
  ->where('table.contents.password IS NULL')
  ->order('table.contents.created', Typecho_Db::SORT_ASC)
  ->limit(1);
  $content = $db->fetchRow($sql);

  if ($content) {
  $content = $widget->filter($content);
  $link = '<li class="previous"> <a href="' . $content['permalink'] . '" title="' . $content['title'] . '" data-toggle="article-tooltip" data-placement="right"> 上一篇 </a></li>';
  // $link 输出的为翻页的样式
  echo $link;
  } else {
  $link = '<li class="previous disabled"><a href="javascript:;" data-toggle="article-tooltip" data-placement="right" title="没有了，亲！">上一篇</a></li>';
  echo $link;
  }
}
 
/**
* 下一篇
* @access public
* @param string $default 如果没有下一篇,显示的默认文字
* @return void
*/
function thePrev($widget, $default = NULL)
{
  $db = Typecho_Db::get();
  $sql = $db->select()->from('table.contents')
  ->where('table.contents.created < ?', $widget->created)
  ->where('table.contents.status = ?', 'publish')
  ->where('table.contents.type = ?', $widget->type)
  ->where('table.contents.password IS NULL')
  ->order('table.contents.created', Typecho_Db::SORT_DESC)
  ->limit(1);
  $content = $db->fetchRow($sql);
   
  if ($content) {
  $content = $widget->filter($content);
  $link = '<li class="next"> <a href="' . $content['permalink'] . '" title="' . $content['title'] . '" data-toggle="article-tooltip" data-placement="left"> 下一篇 </a></li>';
  // $link 输出的为翻页的样式
  echo $link;
  } else {
  $link = '<li class="next disabled"><a href="javascript:;" data-toggle="article-tooltip" data-placement="left" title="没有了，亲！">下一篇</a></li>';
  echo $link;
  }
}
```

- 翻页功能重定义以后的调用代码如下

``` php
<?php thePrev($this); ?>
<?php theNext($this); ?>
```
