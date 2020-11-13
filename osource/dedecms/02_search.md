# 搜索页调用自定义字段的方法

1\. 打开 `include/extend.func.php` 文件，在文件末尾添加如下代码：

``` php
function Search_addfields($id,$result){  
    global $dsql;  
    $mnkj = $dsql->GetOne("SELECT * FROM `dede_addonsoft` where aid='$id'");  
    $name=$mnkj[$result];  
    return $name;  
}
```

解析：`dede_addonsoft` 是要调用的软件模型中自定义字段的附加表（查看方法：`后台-->核心-->频道模型-->内容模型管理-->附加表`），这个需要根据具体的情况来更改，其他的不用修改。

2\. 打开 `include/arc.searchview.class.php` 文件，找到`//处理一些特殊字段`在其下面添加如下代码：
``` php
$row["softsize"]=Search_addfields($row["id"],"softsize");
```
解析：`softsize`是软件大小的名称，如果有多个自定义字段则在这添加多行，但是一定要把`softsize`修改下

3\. 打开 `search.htm` 文件，在需要显示软件大小的地方使用`[field:softsize/]`调用，其中`softsize`是软件大小的字段