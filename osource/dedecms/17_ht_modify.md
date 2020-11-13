# 后台修改小技巧

## 去掉织梦版权（Power by DedeCms）方法

删除，或者注释 `include/dedesql.class.php` 文件中如下代码

``` php
/*
$arrs1=array(0x63,0x66,0x67,0x5f,0x70,0x6f,0x77,0x65,0x72,0x62,0x79);
$arrs2=array(0x20,0x3c,0x61,0x20,0x68,0x72,0x65,0x66,0x3d,0x68,0x74,0x74,0x70,0x3a,0x2f,0x2f,0x77,0x77,0x77,0x2e,0x64,0x65,0x64,0x65,0x63,0x6d,0x73,0x2e,0x63,0x6f,0x6d,0x20,0x74,0x61,0x72,0x67,0x65,0x74,0x3
d,0x27,0x5f,0x62,0x6c,0x61,0x6e,0x6b,0x27,0x3e,0x50,0x6f,0x77,0x65,0x72,0x20,0x62,0x79,0x20,0x44,0x65,0x64,0x65,0x43,0x6d,0x73,0x3c,0x2f,0x61,0x3e);
*/
```

## 缩略图defaultpic.gif的修改方法

**方式一（换图）：** 可自行选择其它的图片将其也命名成 `defaultpic.gif` 然后替换掉。

**方式二（换格式）：** 找到 `include` 文件夹，然后就将里面所有的文件进行查找 `defaultpic.gif` 替换成 `defaultpic.png`，就完成了暂无缩略图的替换了，前提是 `images` 里面要有这个新的 `.png` 格式的文件，不过换图肯定是来的最快的一种方式了。

## 去掉域名后面的index.html

**第一步：** 建立一个 `.htaccess` 格式的文件，可先建立一个 `.txt` 格式的文档，然后另存为 `.htaccess` 格式文件即可。

**第二步：** 在 `.htaccess` 文件中写入 `DirectoryIndex index.html index.php index.htm`

## 删除“系统基本参数”中的新增变量方法

**方法一，执行sql语句**

**格式：** `Delete FROM dede_sysconfig where varname="变量的名称"`

在后台执行-系统-SQL 命令行工具，输入下面的命令：

``` sql
Delete FROM dede_sysconfig where varname="cfg_dianhua"
```

**方法二，利用工具手动删除**

进入 `phpmyadmin` 后，进入对应项目的数据库找到表：`dede_sysconfig`或者利用 `Navicat for MySQL` 工具远程打开数据库找到 `dede_sysconfig` 手动删除新增加的变量。

## 文章标题被限制更改方法

**第一步：** 进入后台，系统—系统基本参数—其他选顷—文档标题最大长度修改为 200 或更大(255 为最大)。

**第二步：** 系统 `SQL` 命令行工具中运行如下代码：

``` sql
alter table dede_archives change title title varchar(90)
```

或者直接使用数据库管理工具`（Navicat for MySQL）` 直接对表 `dede_archives` 进行编辑，修改表中 `title` 长度即可。

## 后台原始样式修改

后台样式的修改，所需要的文件基本都在 `dede` 文件夹

### 后台五大区域及LeftMenu修改

后台 `Body` 区修改文件：`\dede\templets\index_body.htm` ，对应的删除掉即可

后台 `Body` 安全提示区修改文件：`\dede\index_testenv.php` ，对应修改即可

后台 `Top` 区修改文件：`\dede\templets\index2.htm` ，对应修改即可

后台 `LeftMenu` 区修改文件：`\dede\templets\index_top2.htm` ，对应修改即可

后台 `LeftMenuHelp` 去掉修改文件：`\dede\inc\ inc_menu.php` ，对应修改即可

后台 `TopLogo` 区修改文件：`\dede\images\admin_top_logo.gif` ，对应换图即可

### 后台版权与提示相关修改

#### 织梦后台“标题织梦内容管理系统”修改方法

文件：`include\common.inc.php`

``` php
$cfg_version = 'V57_GBK_SP1';
$cfg_soft_lang = 'gb2312';
$cfg_soft_public = 'base';
$cfg_softname = '织梦内容管理系统';
$cfg_soft_enname = 'DedeCMS';
$cfg_soft_devteam = 'DedeCMS 官方团队';
```

#### 织梦信息提示修改方法

打开文件 `include/common.func.php` 找到 `DedeCMS 提示信息！` ，修改成自己想要的即可

#### 登录界面样式修改

修改代码文件：`Default\dede\login.php`

修改样式文件：`Default\dede\css\login.css`

修改图片文件：`dede\images`

*仅供学习*