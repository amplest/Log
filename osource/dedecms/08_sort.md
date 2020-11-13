# 筛选功能实现教程

## 文件下载

文件包含`gbk`及`utf8`两个版本的内容，分别替换即可。

- incluede `核心文件包`
    - arc.listview.class.php `实现筛选功能`
    - extend.func.php `模型筛选调用的核心函数，24行开始到最后`
- templets `新旧版模板调用方法说明，建议使用新版，因为教程用的就是新版本的第三种方法` 

!!!
<attach>104</attach>
!!!

## 核心函数说明

1\. `extend.func.php`：24行至结束为新增代码，`wwwcms_filter`函数用于过滤字符，防止sql注入；`AddFilter`是用来处理筛选过程的函数。

2\. `arc.listview.class.php`： 实现筛选，主要靠`$filtersql`这个变量增加查询条件，当没有检测到筛选参数时此变量为空值，不会影响原查询。

## 筛选功能实现

1\. 后台功能实现界面

![后台功能实现界面][1]

2\. 在需要调用筛选的地方选择如下方法调用`（3种方法）`

常规调用
``` php
{dede:php} AddFilter(模型ID); {/dede:php} 
```
实例：`{dede:php} AddFilter(1); {/dede:php}`

链接形式筛选
``` php
{dede:php} AddFilter(模型ID,1); {/dede:php}
```
实例：`{dede:php} AddFilter(1,1); {/dede:php}`

下拉列表的形式筛选
``` php
{dede:php} AddFilter(模型ID,2); {/dede:php}
```
实例：`{dede:php} AddFilter(1,2); {/dede:php}`

指定使用自定义参数
``` php
{dede:php} AddFilter(1,2,'字段名1,字段名2,字段名3'); {/dede:php}
```
实例：`{dede:php} AddFilter(1,2,'name1,name2,name3'); {/dede:php}`

备注：`'name1,name2,name3'` 是指定的字段名，多个字段用英文状态下的逗号分隔。

## 实际运用案例
``` html
  <div class="listbox">
   <!-- 筛选调用 -->
   <div>
     {dede:php} AddFilter(1,2,'name1,name2,name3'); {/dede:php}
   </div>
   <!-- 筛选出来数据的列表结构 -->
   <ul>
    {dede:list pagesize='10'}
    <li> [field:array runphp='yes']@me = (empty(@me['litpic']) ? "" : "<a href='{@me['arcurl']}' class='preview'><img src='{@me['litpic']}'/></a>"); [/field:array]
     [<b>[field:typelink/]</b>] <a href="[field:arcurl/]" class="title">[field:title/]</a> <span class="info"> <small>日期：</small>[field:pubdate function="GetDateTimeMK(@me)"/]</span>
     <p class="intro"> [field:description/]... </p>
    </li>
    {/dede:list}
   </ul>
  </div>
```

## 注意事项

- 模型ID可以在核心 - 频道模型 - 内容模型管理 找到，该页面的id号即是`模型ID`。
- 前台调用时，不能嵌套于织梦标签之内，一定是独立调用的。
- 如果前台调不出来，请到后台：`系统 - 系统设置 - 系统基本参数 - 其他选项 - 禁用模板标签` ，把`php`删除后保存。

[1]: http://www.molerose.com/usr/uploads/2018/06/1782277791.png