# 文章按权重进行排序

## 涉及的文件修改

1\. 文件路径：`\include\taglib\`
- arclist.lib.php

2\. 文件路径：`\include\`
- arc.listview.class.php

## 功能实现

1\. 原始功能说明：`arclist.lib.php文件74-75行`
``` php
// arclist是否需要weight排序,默认为"N",如果需要排序则设置为"Y"
$isweight = $ctag->GetAtt('isweight');
```
所以我们在使用`orderby="weight"`需要在前面加上`isweight="Y"`来开启权重的使用。也可以加上`orderway="asc"`或`orderway="desc"`来限定排序的方式是升序还是降序。

这样一来`{dede:arclist isweight="Y" orderby="weight" orderway="asc"}{/dede:arclist}`上的调用就实现了。

2\. 针对`{dede:list}{/dede:list}`中的调用实现方法

2-1\. 打开文件`arc.listview.class.php`并按照下述注释中的方法进行修改
``` php
//排序方式
$ordersql = '';
if($orderby=="senddate" || $orderby=="id") {
    $ordersql=" ORDER BY arc.id $orderWay";
}
else if($orderby=="hot" || $orderby=="click") {
    $ordersql = " ORDER BY arc.click $orderWay";
}
else if($orderby=="lastpost") {
    $ordersql = " ORDER BY arc.lastpost $orderWay";
}
// 增加如下代码：start
else if($orderby=="weight") {
    $ordersql = " ORDER BY arc.weight $orderWay";
}
// 增加如下代码：end
else {
    $ordersql=" ORDER BY arc.sortrank $orderWay";
}
```
2-2\. 接着我们往下走：在`lastpost`后加上`weight`
``` php
//如果不用默认的sortrank或id排序，使用联合查询（数据量大时非常缓慢）
if(preg_match('/hot|click|lastpost|weight/', $orderby))
{
    $query = "SELECT arc.*,tp.typedir,tp.typename,tp.isdefault,tp.defaultname,
    tp.namerule,tp.namerule2,tp.ispart,tp.moresite,tp.siteurl,tp.sitepath
    $addField
    FROM `#@__archives` arc
    LEFT JOIN `#@__arctype` tp ON arc.typeid=tp.id
    $addJoin
    WHERE {$this->addSql} $ordersql LIMIT $limitstart,$row";
}
```
2-3\. 都改完以后就可以在`{dede:list}{/dede:list}`中实现了权重排序功能了，代码如下
``` html
{dede:list pagesize="20" isweight="Y" orderby="weight" orderway="asc"}{/dede:list}
```