# 按权重排序不准或BUG的处理方法

## dede:list 的方法

1、找到"根目录`\include\arc.listview.class.php`"文件。

2、修改代码：在文件第727行处添加按weight排序判断代码。

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
            $ordersql = "  ORDER BY arc.lastpost $orderWay";
        }
       // 新增 weight
       else if($orderby=="weight") {
            $ordersql = "  ORDER BY arc.weight $orderWay";
        }
        else {
            $ordersql=" ORDER BY arc.sortrank $orderWay";
        }
```

3、再在第778行处找到此段代码

``` php
//如果不用默认的sortrank或id排序，使用联合查询（数据量大时非常缓慢）
if(preg_match('/hot|click|lastpost|weight/', $orderby))
```

4、标签调用

``` html
{dede:list orderby='weight' orderway='asc'}
```

## dede:arclist 的方法

在织梦系统中找到以下目录`\include\taglib`中的`arclist.lib.php`文件并打开

大约在74 、75行找到：

``` PHP
// arclist是否需要weight排序,默认为"N",如果需要排序则设置为"Y"
$isweight = $ctag->GetAtt('isweight');
// 修改为如下代码：
$weight = $ctag->GetAtt('weight');
// 大约在327行找到，并修改
```

``` php
    //文档排序的方式
    $ordersql = '';
    if($orderby=='hot' || $orderby=='click') $ordersql = " ORDER BY arc.click $orderWay";
    else if($orderby == 'sortrank' || $orderby=='pubdate') $ordersql = " ORDER BY arc.sortrank $orderWay";
    else if($orderby == 'id') $ordersql = "  ORDER BY arc.id $orderWay";
    else if($orderby == 'near') $ordersql = " ORDER BY ABS(arc.id - ".$arcid.")";
    else if($orderby == 'lastpost') $ordersql = "  ORDER BY arc.lastpost $orderWay";
    else if($orderby == 'scores') $ordersql = "  ORDER BY arc.scores $orderWay";
    else if($orderby == 'rand') $ordersql = "  ORDER BY rand()";
    else if($orderby == 'weight') $ordersql = "  order by arc.weight asc";//插入这句 从小到大
    else $ordersql = " ORDER BY arc.sortrank $orderWay";
```

然后在`arclist`方法中用`orderby='weight'`即可