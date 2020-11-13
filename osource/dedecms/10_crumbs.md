# 面包屑导航最后一级去掉链接

## 涉及到的文件修改

文件路径：`\include\`
- typelink.class.php

## 代码实现

1\. 针对`GetPositionLink`方法的修改
``` php
$this->valuePosition = $this->GetOneTypeLink($this->TypeInfos);
// 更改为
$this->valuePosition = $this->TypeInfos['typename'];
```
2\. 针对`GetPositionLink`方法的修改，去除面包屑导航最后的 `>` 号
``` php
return $this->valuePosition.$this->SplitSymbol;
// 更改为
return $this->valuePosition;
```

更改完了之后二级栏目和三级栏目直接的连接也会相应的更改，那么如何防止其更改呢？ 接着往下看 ↓↓↓

3\. 针对`LogicGetPosition`方法的修改
``` php
$this->valuePositionName = $tinfos['typename'].$this->SplitSymbol.$this->valuePositionName;
// 更改为
$this->valuePositionName = $tinfos['typename'].$GLOBALS['cfg_list_symbol'].$this->valuePositionName;
``` 

4\. 此刻会发现文章详细页中面包屑导航最后一级不显示文章标题，解决办法就是在面包屑后面接着调用文章标题即可 `仅限文章页哦，这是我觉得最简单的办法了`
``` html
{dede:field name='position'/} > {dede:field.title/}
```

备注：可能有的人会问，能不能不用 `>` 号，想用 `/` 斜杠，可以的，只需要修改 `后台-系统-系统基本参数-核心设置-栏目位置的间隔符号`，保存之后再生成下界面即可。