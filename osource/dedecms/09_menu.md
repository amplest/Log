# 栏目添加自定义字段教程

## 功能需求

- 后台可针对对应的栏目填写英文名称
- 前台可以在导航栏上调用对应的英文名称

## 涉及到数据库的修改

- 打开数据库，找到名为 `dede_arctype` 表，最后一行添加一个新的字段，字段名称自己喜欢啥就取啥。
- 例如我现在需要添加一个导航英文展示的字段 `englishtype`。

![涉及到数据库的修改][1]

## 涉及到文件的修改

1\. 在改之前首先亲们需要找到如下的文件
- 后台管理模板文件修改：`/dede/templets`
    - catalog_add.htm
    - catalog_edit.htm
- 后台用做数据交互文件：`/dede/`
    - catalog_add.php
    - catalog_edit.php
- channel标签库修改 `/include/taglib/`
    - channel.lib.php
    
`注意，在改之前一定要把之前的备份下，不然一不小心改错了，不好复原哟`

2\. `catalog_edit.htm` 文件的修改
``` html
<!-- 找到如下代码：因为我们需要添加的就是一个简单的字段所以我们复制下栏目名称的HTML排版好了 -->
<tr> 
    <td class='bline' height="26" style="padding-left:10px;"><font color='red'>栏目名称：</font></td>
    <td class='bline'><input name="typename" type="text" id="typename" size="30" value="<?php echo $myrow['typename']?>" class="iptxt" /></td>
</tr>
<!-- 在上述代码下面添加如下代码：需要将name，id，value里面的值换成刚才在数据库里面添加的字段名englishtype -->
<tr> 
    <td class='bline' height="26" style="padding-left:10px;"><font color='red'>栏目英文：</font></td>
    <td class='bline'><input name="englishtype" type="text" id="englishtype" size="30" value="<?php echo $myrow['englishtype']?>" class="iptxt" /></td>
</tr>
```
3\. `catalog_add.htm` 文件的修改
``` html
<!-- 找到如下代码：因为我们需要添加的就是一个简单的字段所以我们复制下栏目名称的HTML排版好了 -->
<tr>
    <td class='bline' height="26" style="padding-left:10px;"><font color='red'>栏目名称：</font></td>
    <td class='bline'><input name="typename" type="text" id="typename" size="30" class="iptxt" /></td>
</tr>
<!-- 在上述代码下面添加如下代码：需要将name，id里面的值换成刚才在数据库里面添加的字段名englishtype -->
<tr>
    <td class='bline' height="26" style="padding-left:10px;"><font color='red'>栏目英文：</font></td>
    <td class='bline'><input name="englishtype" type="text" id="englishtype" size="30" class="iptxt" /></td>
</tr>
```

4\. `catalog_edit.php` 文件的修改

找到如下代码，然后按照注释的要求进行修改
``` php
$upquery = "UPDATE `#@__arctype` SET
    issend='$issend',
    sortrank='$sortrank',
    typename='$typename',
    typedir='$typedir',
    isdefault='$isdefault',
    defaultname='$defaultname',
    issend='$issend',
    ishidden='$ishidden',
    channeltype='$channeltype',
    tempindex='$tempindex',
    templist='$templist',
    temparticle='$temparticle',
    namerule='$namerule',
    namerule2='$namerule2',
    ispart='$ispart',
    corank='$corank',
    description='$description',
    keywords='$keywords',
    seotitle='$seotitle',
    moresite='$moresite',
    `cross`='$cross',
    `content`='$content',
    `crossid`='$crossid',
    `smalltypes`='$smalltypes',
    `englishtype`='$englishtype'
    $uptopsql
WHERE id='$id' ";
// 在字段最后添加刚才我们在数据库里面添加的字段即可 `englishtype`='$englishtype'
```
5\. `catalog_add.php` 文件的修改

当前文件第一处修改，找到如下代码，并按照注释的要求进行修改
``` php
$queryTemplate = "INSERT INTO `#@__arctype`(reid,topid,sortrank,typename,typedir,isdefault,defaultname,issend,channeltype,
tempindex,templist,temparticle,modname,namerule,namerule2,ispart,corank,description,keywords,seotitle,moresite,siteurl,sitepath,ishidden,`cross`,`crossid`,`content`,`smalltypes`,`englishtype`)
VALUES('~reid~','~topid~','~rank~','~typename~','~typedir~','$isdefault','$defaultname','$issend','$channeltype',
'$tempindex','$templist','$temparticle','default','$namerule','$namerule2','0','0','','','~typename~','0','','','0','0','0','','','$englishtype')";
// $queryTemplate 的最后一个添加 englishtype 字段， VALUES 的最后一个也加上 englishtype 字段
```
当前文件第二处修改
``` php
$in_query = "INSERT INTO `#@__arctype`(reid,topid,sortrank,typename,typedir,isdefault,defaultname,issend,channeltype,
tempindex,templist,temparticle,modname,namerule,namerule2,
ispart,corank,description,keywords,seotitle,moresite,siteurl,sitepath,ishidden,`cross`,`crossid`,`content`,`smalltypes`,`englishtype`)
VALUES('$reid','$topid','$sortrank','$typename','$typedir','$isdefault','$defaultname','$issend','$channeltype',
'$tempindex','$templist','$temparticle','default','$namerule','$namerule2',
'$ispart','$corank','$description','$keywords','$seotitle','$moresite','$siteurl','$sitepath','$ishidden','$cross','$crossid','$content','$smalltypes','$englishtype')";
// $in_query 的最后一个添加 englishtype 字段，VALUES 的最后一个也加上 englishtype 字段
```
完成以后则可以通过`{dede:field name='englishtype'/} `进行调用了，但是你会发现在`channel`中无法调用，我们接着往下看 ↓↓↓

6\. 如何实现在导航中进行调用，专业点儿就说在`channel`中的使用

6-1\. 上述的方式完成以后，你会发现在`channel`中调用不出来，例如如下的调用，是不作用的。
``` html
{dede:channel type='son' row='2' typeid='1'}
<a href="[field:typeurl/]">[field:typename/][field:englishtype/]</a>
{/dede:channel} 
```
6-2\. `channel.lib.php`文件修改

第一处修改，找到如下代码，按照注释中的内容进行修改即可
``` php
if($type=='top')
{
    $sql = "SELECT id,typename,englishtype,typedir,isdefault,ispart,defaultname,namerule2,moresite,siteurl,sitepath
        From `#@__arctype` WHERE reid=0 And ishidden<>1 order by sortrank asc limit 0, $line ";
}
// 在 typename 后面增加一个字段 englishtype
else if($type=='son')
{
    if($typeid==0) return '';
    $sql = "SELECT id,typename,englishtype,typedir,isdefault,ispart,defaultname,namerule2,moresite,siteurl,sitepath
        From `#@__arctype` WHERE reid='$typeid' And ishidden<>1 order by sortrank asc limit 0, $line ";
}
// 在 typename 后面增加一个字段 englishtype
else if($type=='self')
{
    if($reid==0) return '';
    $sql = "SELECT id,typename,englishtype,typedir,isdefault,ispart,defaultname,namerule2,moresite,siteurl,sitepath
        FROM `#@__arctype` WHERE reid='$reid' And ishidden<>1 order by sortrank asc limit 0, $line ";
}
// 在 typename 后面增加一个字段 englishtype
```
第二处修改
``` php
//如果用子栏目模式，当没有子栏目时显示同级栏目
if($type=='son' && $reid!=0 && $totalRow==0)
{
    $sql = "SELECT id,typename,englishtype,typedir,isdefault,ispart,defaultname,namerule2,moresite,siteurl,sitepath
        FROM `#@__arctype` WHERE reid='$reid' And ishidden<>1 order by sortrank asc limit 0, $line ";
    $dsql->SetQuery($sql);
    $dsql->Execute();
}
// 在 typename 后面增加一个字段 englishtype
```
增加查询字段就可以在channel中使用了，还有type，channelartlist等标签页是在相应的lib类中添加查询的字段，就不具体细说了。 

6-3\. 附加：处理同级栏目中，当前栏目的样式，`currentstyle`中的调用（其实就是高亮）
``` php
$linkOkstr = str_replace("~typename~",$row['typename'],$linkOkstr);
// 找到上述代码，然后另起一行添加如下代码即可
$linkOkstr = str_replace("~englishtype~",$row['englishtype'],$linkOkstr);
```

[1]: http://www.molerose.com/usr/uploads/2018/06/3564078963.png

