# 前台标签调用汇总

## 公共区域调用（头部/底部）

1\. 头部调用： `{dede:include filename="head.htm"/}`

1-1\. 栏目调用

- 顶级栏目调用
``` html
{dede:channel row="5" type="top"}
<a href='[field:typeurl/]'>[field:typename/]</a>
{/dede:channel}
```
- 顶级栏目高亮
``` html
<li {dede:field name=typeid runphp="yes"}(@me=="")? @me=" class='menu_on'":@me="";{/dede:field}>
    <a href="/">网站首页</a>
</li>
{dede:channel type='top' row='8' currentstyle="<li class='menu_on'><a href='~typelink~' >~typename~</a> </li>" }
    <li><a href="[field:typelink/]">[field:typename/]</a></li>
{/dede:channel}
```
- 二级栏目调用
``` html
// 调用固定id的下的二级栏目
{dede:channel type='son' typeid='1'}
    <a href='[field:typeurl/]'>[field:typename/]</a>
{/dede:channel} 

// 调用当前顶级栏目下的所有二级栏目
{dede:channel type='son'}
    <a href='[field:typeurl/]' title="[field:typename/]">[field:typename/]</a>
{/dede:channel}
```
- 二级栏目高亮
``` html
{dede:channel type='son' row='100' currentstyle="<a href='~typelink~' class='left-nav-on'>~typename~</a>"}
    <a href='[field:typelink/]'>[field:typename/]</a>
{/dede:channel}
```
- 当前页栏目名称调用：`{dede:field name='typename'/}` 或 `{dede:type }[field:typename/]{/dede:type} `
- 单独调用栏目：`{dede:type typeid='17'}[field:typename/]{/dede:type}`
- 单独调用栏目（带连接）：`{dede:type typeid='17'}<a href="[field:typelink/]">[field:typename/]</a>{/dede:type}`

1-2\. 栏目调用案例汇总

1-2-1\. 案例一（顶级栏目与二级栏目分开调用）
``` html
<li><a href="">Home</a></li>
<li>{dede:type typeid='1'}<a href='[field:typelink/]'>[field:typename/]</a>{/dede:type}
<div class="navdow">
    {dede:channel type="son" typeid="1"}
    <a href="[field:typeurl/]" title="[field:typename/]">[field:typename/]</a>
    {/dede:channel}
</div>
```
1-2-2\. 案例二（调用id为3，5的顶级栏目及相对应的二级栏目）
``` html
{dede:channelartlist typeid='3,5'}
    <a href="{dede:field name='typeurl'/}">
        <b>{dede:field name='typename'/}</b>
    </a>
    {dede:channel type='son' noself='yes'}
        <a href="[field:typelink/]">[field:typename/]</a>
    {/dede:channel}
{/dede:channelartlist} 
```
1-2-3\. 案例三（调用id为3的顶级栏目及相对应的二级栏目）
``` html
{dede:channelartlist typeid='3,3'}
    <a href="{dede:field name='typeurl'/}">
        {dede:field name='typename'/}
    </a>
    {dede:channel type='son' noself='yes'}
        <a href="[field:typelink/]">[field:typename/]</a>
    {/dede:channel}
{/dede:channelartlist} 
```
1-2-4\. 案例四（调用id为4的顶级栏目及相对应的产品，带高亮）
``` html
{dede:channelartlist typeid='2,53' currentstyle='active'}
<li class="{dede:field.currentstyle/}">
	<a href="{dede:field name='typeurl'/}">
		{dede:field name='typename'/}
	</a>
	<div class="pro-subnav">
		{dede:arclist isweight='Y' orderby='weight' orderway='asc' row='100' titlelen='500'}
		<a href="[field:arcurl/]">- [field:title/]</a>
		{/dede:arclist}
	</div>
</li>
{/dede:channelartlist}
```
1-2-5\. channelartlist标签支持currentstyle属性的方法
1-2-5-1\. 打开`include\taglib\channelartlist.lib.php`
``` php
// 找到如下代码
$pv->Fields['typeurl'] = GetOneTypeUrlA($typeids[$i]);
// 再此代码下方添加如下代码
if($typeids[$i]['id'] == $refObj->TypeLink->TypeInfos['id'] || $typeids[$i]['id'] == $refObj->TypeLink->TypeInfos['topid'] ){
  $pv->Fields['currentstyle'] = $currentstyle ? $currentstyle : 'current';
}
else{
 $pv->Fields['currentstyle'] = '';
}
```
1-2-5-2\. 调用方法
``` php
// currentstyle='current' 中的current也可以修改为自己想要的类名即可
{dede:channelartlist typeid='2' currentstyle='current'}
 <li class='{dede:field.currentstyle/}'><a href='{dede:field name='typeurl'/}'>{dede:field name='typename'/}</a></li>
{/dede:channelartlist}
```
1-2-6\. 案例五（调用id为53的顶级栏目及相对应的产品，带高亮，同时调用id为57栏目的产品）
``` html
{dede:channelartlist typeid='53,53' currentstyle='active'}
<li class="{dede:field.currentstyle/}">
	<a href="{dede:field name='typeurl'/}">
		{dede:field name='typename'/}
	</a>
	<div class="pro-subnav">
		{dede:channel type='son' noself='yes' row='3'}
			<a href="[field:typelink/]">- [field:typename/]</a>
		{/dede:channel}
		{dede:arclist isweight='Y' typeid='57' orderby='weight' orderway='asc' row='100' titlelen='500'}
		<a href="[field:arcurl/]">- [field:title/]</a>
		{/dede:arclist}
	</div>
</li>
{/dede:channelartlist}
```

2\. 底部调用：`{dede:include filename="footer.htm"/}`

- 版权调用：`{dede:global.cfg_powerby/}`
- 备案编号调用：`{dede:global.cfg_beian/}`

3\. 手机端导航调用（响应式）

同头部底部的调用方式一样 `{dede:include filename="name.htm"/}` 调用对应的模板到合适的位置即可，模板的名称自己取，其它的公共模块调用的方式也是一样的，为的就是更方便管理网站。

4\. 栏目id调用方法：`{dede:field.typeid/}`

## TKD调用

1\. 首页

``` html
<title>{dede:global.cfg_webname/}</title>
<meta name="description" content="{dede:global.cfg_description/}" />
<meta name="keywords" content="{dede:global.cfg_keywords/}" />
```

2\. 列表/单页

``` html
<title>{dede:field.seotitle /}_{dede:global.cfg_webname/}</title>
<meta name="description" content="{dede:field.description function='html2text(@me)'/}" />
<meta name="keywords" content="{dede:field.keywords /}" />
```

3\. 文章页

``` html
<title>{dede:field.title /}_{dede:global.cfg_webname/}</title>
<meta name="description" content="{dede:field.description function='html2text(@me)'/}" />
<meta name="keywords" content="{dede:field.keywords /}" />
```

## 单页内容调用

1\. 常规单页调用语句：`{dede:field.content/}`
2\. sql语句调用
``` sql
// 方式一，限制字数，一般适用于首页简介
{dede:sql sql="SELECT content FROM dede_arctype where id=6"}
    [field:content function=cn_substr(Html2Text(@me),500)/]
{/dede:sql}

// 方式二，正常展示文字
{dede:sql sql="SELECT content FROM dev_arctype where id=30"}
    [field:content/]
{/dede:sql}
```

## 常规信息调用相关

### 时间调用

1\. 文章页：`{dede:field.pubdate function="MyDate('Y-m-d',@me)"/}`

2\. 列表页：
``` 
[field:pubdate function="MyDate('Y-m-d',@me)"/]
```
3\. 自定义样式时间调用
```
[field:pubdate function="MyDate('<b>Y</b><p>d-m</p>',@me)"/]
```

### 文章页信息调用

1\. 文章标题：`{dede:field.title/}`

2\. 文章内容：`{dede:field.body/}`

3\. 文章作者：`{dede:field.writer/}`

4\. 文章摘要：`{dede:field.description/}`

5\. 文章分页
```
// 上一篇
{dede:prenext get='pre'/} 

// 下一篇
{dede:prenext get='next'/}
```
6\. 文章点击量/浏览量
``` html
<script src="{dede:field name='phpurl'/}/count.php?view=yes&aid={dede:field name='id'/}&mid={dede:field name='mid'/}" type='text/javascript' language="javascript"></script>

<script src="/plus/count.php?view=yes&aid=[field:id/]&mid=[field:mid/]" type='text/javascript' language="javascript"></script>
```

### 列表页信息调用

1\. 链接：`[field:arcurl /]`

2\. 缩略图：`[field:litpic/]`

3\. 标题：`[field:title /]`

4\. title属性全标题：`[field:fulltitle/]`

5\. 简介
```
// 常规调用
[field:infos /]

// 限制字数调用
[field:infos function="cn_substr(@me,字符数)"/]
```
6\. 摘要
```
// 常规调用
[field.description]

// 限制字数调用
[field:description function="cn_substr(@me,字符数)"/]
```

7\. 分页：`{dede:pagelist istitem="index,pre,next,end,option,info," listsize="5"/}`