# 生成GoogleSitemap谷歌地图教程

## GoogleSitemap生成方法一

第一步：复制下面的这些代码保存为`googlesitemap.htm`模板文件然后上传到模板所在目录
``` html
<!-- “http://填写自己的域名”需要换成自己的实际域名 -->
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.google.com/schemas/sitemap/0.9">
  <url>
    <loc>http://填写自己的域名</loc>
    <lastmod>{dede:arclist row=1 titlelen=24 orderby=pubdate}
    [field:pubdate function=strftime('%Y-%m-%d',@me)/]
    {/dede:arclist}</lastmod>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
  {dede:channel row='100' type='top'}
  <url>
    <loc>http://填写自己的域名[field:typelink /]</loc>
    <changefreq>daily</changefreq>
    <priority>0.8</priority>
  </url>
  {/dede:channel}
  {dede:arclist row=10000 orderby=pubdate}
  <url>
    <loc>http://填写自己的域名[field:arcurl/]</loc>
    <lastmod>[field:pubdate function=strftime('%Y-%m-%d',@me)/]</lastmod>
    <changefreq>monthly</changefreq>
  </url>
  {/dede:arclist}
</urlset>
```

第二步：生成HTML就可以了，如图所示:选择主页模板选择刚才制作的`googlesitemap.htm`主页位置改为`googlesitemap.xml`然后生成。

![GoogleSitemap生成方法一][1]

## GoogleSitemap生成方法二

当然你也可以制作一个单页，就不用每次生成的时候选择`googlesitemap.htm`模板了

位置：`后台-核心-频道模型-单页文档管理-增加一个单页`

按照如下方式进行填写，[`关联标识`](http://help.dedecms.com/help/install-use/2011/0614/61.html)其实就是单页多的时候用做区分的一个标识

填写完成以后即可保存，在网站根目录就可以看到xml文件了。

![GoogleSitemap生成方法二][2]

[1]:http://www.molerose.com/usr/uploads/2018/06/3279202627.png
[2]:http://www.molerose.com/usr/uploads/2018/06/986894057.png