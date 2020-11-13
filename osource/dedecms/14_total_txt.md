# 实现文章字数统计

## 涉及到修改的文件

- 文件目录：`\include\helpers\`
  - extent.helper.php
- 在`extent.helper.php`文件最下方添加如下代码，保存即可
``` php
// 统计文章字数
function strlen_utf8($str) {
    $i = 0;
    $count = 0;
    $str = Html2text($str);
    $len = strlen($str);
    while ($i < $len) {
        $chr = ord($str[$i]);
        $count++;
        $i++;
        if ($i >= $len) {
            break;
        }
        if ($chr & 0x80) {
            $chr <<= 1;
            while ($chr & 0x80) {
                $i++;
                $chr <<= 1;
            }
        }
    }
    return $count;
}
```
## 代码实现
``` html
<!-- 输出代码 -->
{dede:field.body function='strlen_utf8(@me)'/}
<!-- 实例应用 -->
<span> 字数：{dede:field.body function='strlen_gbk(@me)'/} 字 </span>
```
## 鸣谢

感谢我[狗哥](https://www.seogo.me/tec/213.html)百忙之中抽出时间去找的教程