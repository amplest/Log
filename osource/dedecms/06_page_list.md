# 列表分页自定义样式教程

## 分页样式涉及到的文件修改

分页样式主要集成在了一个文件里面，全部的修改仅针对一个主要文件进行修改即可

文件路径：`/include/arc.listview.class.php`

## 调用代码说明

我们先来看下，原始的织梦分页调用是什么样子的
```
{dede:pagelist istitem="index,pre,next,end,option,info," listsize="5"/}
```
调用的内容：首页，上一页，下一页，末页，下拉跳转框，条数信息，页码个数，当然页码个数是默认携带的，所以上述调用并未表明页码个数的字段

如果有的字段是不需要的可以自行去除即可。

## 案例

下面我们以boostrap来进行讲解如何自定义分页样式

- 打开主要文件以后查找 `获取静态的分页列表` 大概在文件的977行，因为我们使用的是生成静态的页面，所以我们调用的也是静态的分页列表。
- 由于我们使用的是[bootstrap前端框架](http://v3.bootcss.com/components/#禁用和激活状态)，列表分页