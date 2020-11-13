# 标签TAG调用及标签云实现方法

## tag标签的语法解释
``` html
{dede:tag row='30' sort='new' getall='0'}
　　<a href='[field:link/]'>[field:tag /]</a>
{/dede:tag}
```
参数说明：
- `row='30'` 调用条数为30条
- `sort='new'` 排序方式`month`，`rand`，`week`，`new`
- `getall='0'` 获取类型`0`为当前内容页TAG标记，`1`为获取全部TAG标记。
- 底层模板字段：`link`，`tag`
### 最新tag标签调用
``` html
{dede:tag row='30' sort='new'}
　　<a href='[field:link/]'>[field:tag/]([field:total/])</a>
{/dede:tag}
```
### 当月热门tag标签调用
``` html
{dede:tag row='30' sort='month'}
　　<a href='[field:link/]'>[field:tag/]([field:total/])</a>
{/dede:tag}
```
### 随机tag标签调用
``` html
{dede:tag row='60' sort='rand'}
　　<a href='[field:link/]'>[field:tag/]([field:total/])</a>
{/dede:tag}
```
### 内容页tag添加
``` html
<!-- 我个人觉得应该是在单页使用这个可能会多一点：例如你新建一个单页模板然后调用 -->
{dede:tag type='current'/}
```
### 采集热门Tags，刷新一次变换一次
``` html
<!-- 需要搭配上颜色：命名规则为tag后面加数字即可 -->
<style>
.tag1 { color:#339900;font-weight:bold;} 
.tag2 { color:#e65730;} 
.tag3 { color:#00b9da;} 
.tag4 { color:#FE3981;font-weight:bold;font-size:14px;}
</style>
<!-- 
    * 不仅仅是tag也可以使用loop标签来实现调用
    * 注意：class需要为tag如果变掉了，记得也跟着修改过来，元素中的和脚本中的
-->
{dede:tag row='10' sort='month' }
　　<a class="tag" href='[field:link/]'>[field:tag/] </a>
{/dede:tag}
<script> 
  var tag_a=document.getElementsByTagName("a"); 
  for( i in tag_a){ 
    var offset=6; 
    var num=4; 
    if(tag_a[i].className=="tag"){ 
    var rnd=Math.ceil((num+offset)*Math.random()); 
    if(rnd>offset){ 
      tag_a[i].className="tag"+(rnd-offset); 
    } 
    } 
  } 
</script> 
```
### 文章页tag标签调用
``` html
{dede:tag table='dede_search_keywords' sort='keyword' row='2' if=''}
　　<a class='blue' href='[field:link/]' target='_blank'>[field:tag /]</a>
{/dede:tag}
```
### 全站调用最新关键词标签
``` html
<!-- 调用最新tag：点击触发关键词搜索 -->
{dede:tag table='#@__search_keywords' row='3' sort='new' if=''}
	<a title="[field:tag /]" href="/plus/search.php?keyword=[field:keyword/]">[field:keyword/]</a>
{/dede:tag}
<!-- loop循环标签：可以使用它来调用任意表中的数据，调用关键词标签，大多使用在search搜索框边儿 -->
{dede:loop table='#@__search_keywords' sort='keyword' row='40' if=''} 
    <a title="[field:tag /]" href="/plus/search.php?keyword=[field:keyword/]"> [field:keyword/]</a>
{/dede:loop}
```
## 列表页调用TAG标签的方法
织梦CMS默认在列表是无法调用tag标签的，现给大家再提供`dedecms5.7版本`版本的tag标签调用方法。
- 涉及文件修改：`\include\helpers\archive.helper.php`
``` php
function GetTags_list($aid) {
    global $dsql;
    $tags = '';
    $query = "SELECT tag FROM `#@__taglist` WHERE aid='$aid' ";
    $dsql->Execute('tag',$query);
    while($row = $dsql->GetArray('tag'))
    {
    $tags .= ($tags=='' ? "<a href='/tags.php?/".urlencode($row['tag'])."'>".$row['tag']."</a>" : ','."<a href='/tags.php?/".urlencode($row['tag'])."'>".$row['tag']."</a>");
    }
    return $tags;
}
```
- 列表中的调用方式：`[field:id function=GetTags_list(@me)/]`
## 首页调用文章TAG标签的方法
- dedecms5.7版本调用方式：`[field:id function=GetTags(@me)/]`
## 如何给调用出来的tag标签增加链接
- 涉及文件修改：`\include\helpers\archive.helper.php`
``` php 
// 找到并注释掉如下代码
$tags .= ($tags=='' ? $row['tag'] : ','.$row['tag']);
// 替换成如下代码
$tags .= "<a href='/tags.php?/".urlencode($row['tag'])."/'>".$row['tag']."</a> ";
// 在该文件最下方位置再加上如下代码
if ( ! function_exists('GetTagk')){
function GetTagk($aid)  
{
    global $dsql;  
    $tagk = '';  
    $query = "SELECT tag,aid FROM `#@__taglist` WHERE aid='$aid' ";
    $dsql->Execute('tag',$query);
    while($row = $dsql->GetArray('tag'))
    {  
    $tagk .= ($tagk=='' ? $row['tag'] : ','.$row['tag']);
    }
    return $tagk;
    }
}
```
- `\dede\article_edit.php`文件涉及修改
``` php
$tags = GetTags($aid);
// 找到如上代码的下方加上如下代码
$tagk = GetTagk($aid);
```
- `\dede\templets\article_edit.htm`文件涉及修改
``` php
<?php echo $tags; ?>
// 改为
<?php echo $tagk; ?>
```
## 织梦DedeCMS实现多彩标签云
- 涉及修改的文件：`\include\`
    - common.func.php
``` php
// 在该文件最下方加入如下函数：此函数的作用是输出随机的样式，包括font-size和color
function getTagStyle(){
$minFontSize=14; //最小字体大小,可根据需要自行更改
$maxFontSize=14; //最大字体大小,可根据需要自行更改
return 'font-size:'.($minFontSize+lcg_value()*(abs($maxFontSize-$minFontSize))).'px;color:#'.dechex(rand(0,255)).dechex(rand(0,196)).dechex(rand(0,255));
}
// 如果你想指定只显示几个字体大小，而不是完全随机，请将步骤一中的函数代码替换为
// $sizearray = array('8','9','10','11','12','20'); 
// return 'font-size:'.$sizearray[rand(0,count($sizearray))].'pt;color:#'.dechex(rand(0,255)).dechex(rand(0,196)).dechex(rand(0,255)); 
}
```
- 调用方式
``` html
{dede:tag row='1000' getall='1' sort='hot'}
    <a href='[field:link/]' title="[field:tag /]" style="[field:total runphp=yes]@me=getTagStyle();[/field:total]" >
        [field:tag /]
    </a>
{/dede:tag}
```
## 织梦DedeCMS统计Tag标签个数
``` html
<!-- 重点是：[field:total/] -->
{dede:tag row='30' sort='month'}
    <a href='[field:link/]'>[field:tag /]([field:total/])</a>
{/dede:tag}
```