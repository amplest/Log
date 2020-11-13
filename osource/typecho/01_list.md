# typecho评论列表自定义样式

因为我们是需要制作模板的，模板又是都集成在 `themes` 中的，其实想改 `typecho` 原始的文件，改起来也简单，但是就脱离了模板的定义了，这样岂不是不好？

所以M君就给就 `molerose` 这套模板来做一个小小的分享，写的不好，请笑纳！

## 关于comments自定义样式分解说明

- `form` 部分我肯定人人都是会的，这里就不做详细讲解了。
- 首先 `comments` 文件肯定是已经被你给独立好了的
- 其次，可去掉 `comments` 文件中的 `if` 的调用，代码如下，删掉即可

``` php
<?php if (!defined('__TYPECHO_ROOT_DIR__')) exit; ?>
```

- 在删掉的部分，重新添加如下代码，此项代码就是自定义 `comments` 留言列表样式的。

``` php
<?php function threadedComments($comments, $options) {
    $commentClass = '';
    if ($comments->authorId) {
        if ($comments->authorId == $comments->ownerId) {
            $commentClass .= ' comment-by-author'; //如果是文章作者的评论添加 .comment-by-author 样式
        } else {
            $commentClass .= ' comment-by-user'; //如果是评论作者的添加 .comment-by-user 样式
        }
    }
 
    $commentLevelClass = $comments->levels > 0 ? ' comment-child' : ' comment-parent'; //评论层数大于0为子级，否则是父级
?>
// 自定义评论代码结构如下
<li id="li-<?php $comments->theId(); ?>" class="comment-body<?php 
if ($comments->levels > 0) {
    echo ' comment-child';
    $comments->levelsAlt(' comment-level-odd', ' comment-level-even');
} else {
    echo ' comment-parent';
}
$comments->alt(' comment-odd', ' comment-even');
echo $commentClass;
?>">
    <div class="comment-txt-box" id="<?php $comments->theId(); ?>">
        <div class="comment-author clearfix">
            <?php $comments->gravatar('40', ''); ?>
            <cite class="fn comment-info-title"><?php $comments->author(); ?></cite>
            <span class="comment-meta" ><?php $comments->date('F jS, Y \a\t h:i a'); ?></span>
        </div>
        <?php $comments->content(); ?>
        <?php if ('waiting' == $comments->status) { ?>  
        <em class="awaiting"><?php $options->commentStatus(); ?></em>  
        <?php } ?>
        // 首次评论审核提示，在自定义评论代码的适当地方添加以下语句，否则将看不到审核提示语句。
        // waiting 后全等的对象 `$comments`，`$options`，对应 threadedComments 的第一，二个对象，因为博主这里是保证实现的样式，所以2者本身就是一样的。
        <div class="comment-meta">
            <span class="comment-reply label bg-info"><?php $comments->reply(); ?></span>
        </div>
    </div>
<?php if ($comments->children) { ?> //是否嵌套评论判断开始
    <div class="comment-children">
        <?php $comments->threadedComments($options); ?> //嵌套评论所有内容
    </div>
<?php } ?>
</li>
<?php } ?>
```

- 变量相关说明

``` php
<?php $comments->gravatar('40', ''); ?> //头像，有两个参数，大小、默认头像？
<?php $comments->author(); ?> //评论作者
<?php $comments->permalink(); ?> //当前评论的连接地址
<?php $comments->date('Y-m-d H:i'); ?>//评论时间，可在括号里设置格式
<?php $comments->reply(); ?> //回复按钮，可在括号里自定义评论按钮的文字
<?php $comments->content(); ?> //评论内容
```

## 写在最后

[官方文档](http://docs.typecho.org/themes/custom-comments)
