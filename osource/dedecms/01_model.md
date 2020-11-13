# 首页/列表/文章调用新建模型中的字段方法

## 功能简介

可新建内容模型，供栏目选择，该新建内容模型，可自定义字段及调用

## 如何添加新的内容模型

1\. 功能位置：`后台-核心-频道模型-内容管理模型-增加新模型` 

![功能位置][1]

2\. 新增模型：填写好模型数据以后，保存即可，只需要填写如下图数据即可

![新增模型][2]

3\. 针对该模型新增字段：字段信息、数据类型填写好以后保存，`字段名称` 即为信息调用的名称

![针对该模型新增字段][4]

![针对该模型新增字段][5]

4\. 字段管理界面`模型字段配置`中出现刚添加的字段信息则表示字段添加成功，同时选择了该模型的栏目也会出现该字段。

![字段管理界面][6]

5\. 字段调用方法
- 在`{dede:list}`列表中调用
``` html
{dede:list pagesize='5' addfields='jiage' channelid='2'}
<p>字段：[field: name /]</p>
{/dede:list}
```
- 在`{dede:list}`列表中如果想正常使用的话，还得在该模型的`基本设置 - 列表附加字段`处填写好需要调用的字段，才能实现在自定义字段在列表方法中的调用。

![自定义字段在列表方法中的调用][3]

- 在`{dede:arclist}`文章列表中调用
``` html
{dede:arclist typeid='3' row='4' addfields='jiage' channelid='2'}
<p>字段：[field: name /]</p>
{/dede:arclist}
```
- 单独调用（首页，文章页）
``` html
{dede:field name='name'/}
```

备注：`addfields='name'` 指定要获得的字段，如 `addfields='字段1,字段2'` 字段中间用逗号隔开，`channelid='2'` 指定内容模型的`id`值

[1]: http://www.molerose.com/usr/uploads/2018/06/133858994.png
[2]: http://www.molerose.com/usr/uploads/2018/06/2928488607.png
[3]: http://www.molerose.com/usr/uploads/2018/06/2060131765.png
[4]: http://www.molerose.com/usr/uploads/2018/06/1753995090.png
[5]: http://www.molerose.com/usr/uploads/2018/06/2074588694.png
[6]: http://www.molerose.com/usr/uploads/2018/06/3410958751.png