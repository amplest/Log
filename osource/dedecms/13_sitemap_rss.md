# 站点地图及RSS生成与生成模板路径修改

## dede文件夹中的文件修改
- 文件目录：`\dede\`
    - makehtml_map.php
- 普通网站地图
``` php
// 将17行的
$cfg_cmspath."/data/sitemap.html";
// 改为
$cfg_cmspath."/sitemap.html";
```
- RSS的网站地图
``` php
// 将22行的
$cfg_cmspath."/data/rssmap.html";
// 改为
$cfg_cmspath."/rssmap.html";
```
## include文件夹中的文件修改
- 文件目录：`\include\`
    - arc.rssview.class.php
``` php
$murl = $GLOBALS['cfg_cmspath']."/data/rss/".$this->TypeID.".xml";
// 改为
$murl = $GLOBALS['cfg_cmspath']."/rss/".$this->TypeID.".xml";
```
- 文件目录：`\include\`
    - sitemap.class.php （2处修改）
``` php
// 将
$typelink = $GLOBALS['cfg_cmsurl']."/data/rss/".$row->id.".xml";
// 改为
$typelink = $GLOBALS['cfg_cmsurl']."/rss/".$row->id.".xml";
```
``` php
// 将
$typelink = $GLOBALS['cfg_cmsurl']."/data/rss/".$row->id.".xml";
// 改为
$typelink = $GLOBALS['cfg_cmsurl']."/rss/".$row->id.".xml";
```
## 站点地图接RSS链接方式
``` html
<a href="/sitemap.html">网站地图</a>
<a href="/rssmap.html">RSS订阅</a>
<!-- 生成RSS之前，需要手动在根目录下建立一个 RSS 的文件夹 -->
```
## 如何实现站点地图及RSS样式自定义
### 方法一
- 文件目录：`\templets\plus\`
    - sitemap.htm
- 找到文件后手动修改对应的内容即可，例如样式调用自己网站中的一些公共样式（此处自由发挥）重点我们是需要讲方法二的。
### 方法二
将站点地图模板融入到自己做的模板文件夹中，样式完全跟着自己的模板走，且可以在站点地图页面调用文章
- 文件目录：`\dede\`
    - makehtml_map.php
``` php
// 修改(一)
require_once(DEDEINC."/dedetag.class.php");
// 改成
require_once(DEDEINC."/arc.partview.class.php");

// 修改(二)
$dtp = new DedeTagParse();
$dtp->LoadTemplet($tmpfile);
$dtp->SaveTo($cfg_basedir.$murl);
// 改成
$dtp = new PartView();
$GLOBALS['_arclistEnv'] = 'index';
$dtp->SetTemplet($tmpfile);
$dtp->SaveToHtml($cfg_basedir.$murl);

// 修改(三)
$dtp->Clear();
// 改成：其实就是注释掉
//$dtp->Clear();

// 修改(四)
$murl = $cfg_cmspath."/data/sitemap.html";
$tmpfile = $cfg_basedir.$cfg_templets_dir."/plus/sitemap.htm";
// 改成
$murl = $cfg_cmspath."/sitemap.html";
$tmpfile = $cfg_basedir.$cfg_templets_dir."/".$cfg_df_style."/sitemap.htm";

// 修改(五)
$murl = $cfg_cmspath."/data/rssmap.html";
$tmpfile = $cfg_basedir.$cfg_templets_dir."/plus/sitemap.htm";
// 改成
$murl = $cfg_cmspath."/rssmap.html";
$tmpfile = $cfg_basedir.$cfg_templets_dir."/".$cfg_df_style."/rssmap.htm";
```
**修改四，五重点解读：**
原来网站地图模板的路径是固定在`/plus/`目录中的，即sitemap.htm的位置为：`/templets/plus/sitemap.htm`，更改之后sitemap.htm的位置改成网站模版所在目录，这样我们在自己的模版文件夹中新建个sitemap.htm文 件任意编辑成自己喜欢的网站地图模板就可以了。
- 调用方式：`{dede:global name='maplist'/}`
- 如果觉得调用出来的站点地图列表不好看那可以修改：`\include\sitemap.class.php`文件样式可以自己写好后再往如下代码里面套用即可。
``` php
$mapString .= "<div class=\"linkbox\">\r\n<h3><a href='$typelink'>".$row->typename."</a></h3>";
$mapString .= "\t<ul class=\"f6\">\t\t\r".$this->LogicListAllSunType($row->id,$maptype)."\t\n</ul></div>\r\n";
```
## 小技巧分享
- `DedeTag Engine Create File False` 问题处理方法
    - 找到文件：`\include\dedetag.class.php`
    - 在该文件夹内搜索：`DedeTag Engine Create File False`
``` php
$fp = @fopen($filename,"w") or die("DedeTag Engine Create File False");
// 修改如下形式
$fp = @fopen($filename,"w") or die("DedeTag Engine Create File False:$filename");
```
这样，重新生成的时候，就会显示哪里出错，根据提示去解决具体的错位原因

一般出现这个问题的都是没有权限写入，在服务器修改下权限即可解决。