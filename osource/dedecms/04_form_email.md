# 自定义表单信息转发邮箱教程

## 自定义表单能够干啥

- 能够自由组合添加字段
- 能根据不同行业来定制表单，从而收集到企业主想要的讯息
- 能够更好的实现网页的用户体验度与交互性

## 自定义表单的使用

1\. 功能位置：`后台 - 频道模型 - 自定义表单 - 增加新的自定义表单`

![功能位置][1]

2\. 自定义表单信息填写

如果你有强迫症的话，且网站涉及到的表单较多的话，不妨修改下`自定义表单名称`做为区分，填写完成保存即可。

![自定义表单信息填写][2]

保存之后会自动跳转到`自定义表单管理`页面，此刻就代表表单已经创建好了，点击右侧画笔来进行修改自定义表单。

![自定义表单信息填写][3]

3\. 自定义表单字段创建

- 根据自己的需求来进行填写`表单提示文字`，`字段名称`，`数据类型`
- 如果定义数据类型为`select`、`radio`、`checkbox`时，默认值填写被选择的项目(用","分开，如"男,女,人妖")。

![自定义表单字段创建][4]

## 获取自定义表单代码

1\. 当表单字段添加完成保存后，会自动跳转到`自定义表单管理`页面

![获取自定义表单代码，完成样式自定义][5]

2\. 点击`前台预览`进入预览界面，然后再次右上角的`发布信息`，就可以看到刚才添加的字段了。

![获取自定义表单代码，完成样式自定义][6]

3\. 在`发布信息`页面按下组合键`ctrl+u`即可进入源代码界面，如下图即为自定义表单的实现代码，Copy下来放到自己织梦模板中需要调用的位置即可。

![获取自定义表单代码，完成样式自定义][7]

## 如何将自定义表单做到个性化

如果表单的静态是自己写的话，可以按照如下结构来对号入座放入自己的元素即可。

``` html
<!--表单组成结构-->
<form action="/plus/diy.php" enctype="multipart/form-data" method="post">
    <!--公共区域一个不能少-->
    <input type="hidden" name="action" value="post" />
    <input type="hidden" name="diyid" value="1" />
    <input type="hidden" name="do" value="2" />
    <!--表单容器-->
    <div class="form_box"></div>
    <!--数据类型区域：字段添加以后这一块儿是会自动生成的，复制最新的覆盖掉即可-->
    <input type="hidden" name="dede_fields" value="name,text;phonesss,text;sexs,radio" />
    <!--密钥区域：这一块儿是会自动生成的，复制最新的覆盖掉即可-->
    <input type="hidden" name="dede_fieldshash" value="d6170069c46398bbf42ab44e76e3e1db" />
    <!--按钮区域-->
    <input type="submit" name="submit" value="提 交"  />
    <input type="reset" name="reset" value="重 置" />
</form>
```

[1]: http://www.molerose.com/usr/uploads/2018/06/1182743112.png
[2]: http://www.molerose.com/usr/uploads/2018/06/2110170904.png
[3]: http://www.molerose.com/usr/uploads/2018/06/3037391629.png
[4]: http://www.molerose.com/usr/uploads/2018/06/4054543445.png
[5]: http://www.molerose.com/usr/uploads/2018/06/848837184.png
[6]: http://www.molerose.com/usr/uploads/2018/06/3634480855.png
[7]: http://www.molerose.com/usr/uploads/2018/06/27599412.png