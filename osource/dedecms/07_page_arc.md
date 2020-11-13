# 文章分页自定义样式教程

## 分页样式涉及到的文件修改

分页样式主要集成在了一个文件里面，全部的修改仅针对一个主要文件进行修改即可

文件路径：`/include/arc.archives.class.php`

## 调用代码说明

我们先来看下，织梦内容分页调用是什么样子的
```
// 调用上一篇
{dede:prenext get='pre'/}

// 调用下一篇
{dede:prenext get='next'/}
```
## 案例

下面我们以boostrap来进行讲解如何自定义内容分页样式

- 打开对应的文件后，在文件内部查找`获取上一篇，下一篇链接`，大概在795行。
- 由于我们使用的是前端框架，那么我们先来看下bootstrap前端框架中的内容分页结构展示
``` html
<ul class="pager">
    <li class="previous"><a href="#"><span>←</span> Older</a></li>
    <li class="next"><a href="#">Newer <span>→</span></a></li>
</ul>
```
- 文章分页自定义样式，完整代码结构解析，记住，改的时候要细心得千万注意
``` php
/**
*  获取上一篇，下一篇链接
*
* @access    public
* @param     string  $gtype  获取类型
*                    pre:上一篇  preimg:上一篇图片  next:下一篇  nextimg:下一篇图片
* @return    string
*/
function GetPreNext($gtype='')
{
    $rs = '';
    if(count($this->PreNext)<2)
    {
        $aid = $this->ArcID;
        $preR =  $this->dsql->GetOne("Select id From `#@__arctiny` where id<$aid And arcrank>-1 And typeid='{$this->Fields['typeid']}' order by id desc");
        $nextR = $this->dsql->GetOne("Select id From `#@__arctiny` where id>$aid And arcrank>-1 And typeid='{$this->Fields['typeid']}' order by id asc");
        $next = (is_array($nextR) ? " where arc.id={$nextR['id']} " : ' where 1>2 ');
        $pre = (is_array($preR) ? " where arc.id={$preR['id']} " : ' where 1>2 ');
        $query = "Select arc.id,arc.title,arc.shorttitle,arc.typeid,arc.ismake,arc.senddate,arc.arcrank,arc.money,arc.filename,arc.litpic,
                    t.typedir,t.typename,t.namerule,t.namerule2,t.ispart,t.moresite,t.siteurl,t.sitepath
                    from `#@__archives` arc left join #@__arctype t on arc.typeid=t.id  ";
        $nextRow = $this->dsql->GetOne($query.$next);
        $preRow = $this->dsql->GetOne($query.$pre);
        if(is_array($preRow))
        {
            if ( defined('DEDEMOB') )
            {
                $mlink = 'view.php?aid='.$preRow['id'];
            } else {
                $mlink = GetFileUrl($preRow['id'],$preRow['typeid'],$preRow['senddate'],$preRow['title'],$preRow['ismake'],$preRow['arcrank'],
            $preRow['namerule'],$preRow['typedir'],$preRow['money'],$preRow['filename'],$preRow['moresite'],$preRow['siteurl'],$preRow['sitepath']);
            }
            // 上一篇样式 start
            $this->PreNext['pre'] = "<li class=\"previous\"><a href=\"$mlink\" title=\"{$preRow['title']}\"><span aria-hidden=\"true\"></span>← Previous</a></li>";
            // 上一篇样式 end
            $this->PreNext['preimg'] = "<a href='$mlink'><img src=\"{$preRow['litpic']}\" alt=\"{$preRow['title']}\"/></a> ";
        }
        else
        {
            // 上一篇样式 start：如下样式是当暂无上一篇的时候，默认链接无法点击，同时显示禁用的手型！
            $this->PreNext['pre'] = "<li class=\"previous disabled\"><a href=\"javascript:;\" title=\"No data\"><span aria-hidden=\"true\"></span>← Previous</a></li>";
            // 上一篇样式 end
            $this->PreNext['preimg'] ="<img src=\"/templets/default/images/nophoto.jpg\" alt=\"对不起，没有上一图集了！\"/>";
        }
        if(is_array($nextRow))
        {
            if ( defined('DEDEMOB') )
            {
                $mlink = 'view.php?aid='.$preRow['id'];
            } else {
                $mlink = GetFileUrl($nextRow['id'],$nextRow['typeid'],$nextRow['senddate'],$nextRow['title'],$nextRow['ismake'],$nextRow['arcrank'],
                $nextRow['namerule'],$nextRow['typedir'],$nextRow['money'],$nextRow['filename'],$nextRow['moresite'],$nextRow['siteurl'],$nextRow['sitepath']);
            }
            // 下一篇样式 start
            $this->PreNext['next'] = "<li class=\"next\"><a href=\"$mlink\" title=\"{$nextRow['title']}\">Next → <span aria-hidden=\"true\"></span></a></li>";
            // 下一篇样式 end
            $this->PreNext['nextimg'] = "<a href='$mlink'><img src=\"{$nextRow['litpic']}\" alt=\"{$nextRow['title']}\"/></a> ";
        }
        else
        {
            // 下一篇样式 start：如下样式是当暂无下一篇的时候，默认链接无法点击，同时显示禁用的手型！
            $this->PreNext['next'] = "<li class=\"next disabled\"><a href=\"javascript:;\" title=\"No data\">Next → <span aria-hidden=\"true\"></span></a></li>";
            // 下一篇样式 end
            $this->PreNext['nextimg'] ="<a href='javascript:void(0)' alt=\"\"><img src=\"/templets/default/images/nophoto.jpg\" alt=\"对不起，没有下一图集了！\"/></a>";
        }
    }
    if($gtype=='pre')
    {
        $rs =  $this->PreNext['pre'];
    }
    else if($gtype=='preimg'){

        $rs =  $this->PreNext['preimg'];
    }
    else if($gtype=='next')
    {
        $rs =  $this->PreNext['next'];
    }
    else if($gtype=='nextimg'){

        $rs =  $this->PreNext['nextimg'];
    }
    else
    {
        $rs =  $this->PreNext['pre']." &nbsp; ".$this->PreNext['next'];
    }
    return $rs;
}
```
**备注：** 内容页分页样式的修改，比列表还是要简单些的，但是方法终究只是方法，要创新还得自己要学会举一反三才行！


