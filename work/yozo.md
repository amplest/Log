# 工作积累

## 记录

- clone操作有两种方式: `System.clone(argument)`,`CloneUtil.clone(argument)`
- `_$SSView`, `_$Workbook` 合并为 `workBookView`
- 单元格数据 `sheet.getCell()`
- sheetId: `workbookview.getWorksheet().sheet.id`;
- sheet: `workbookview.getWorksheet().sheet`;
- 调用单元格默认属性, 现在取属性同意使用`CellAttrOp.getEntireAttrs`
``` javascript
// 单个单元格, 设置超链接的单元格字符属性
dca = System.clone(CellAttrOp.getEntireAttrs(sheet.getBook(), sheet, sr, sc))
if (dca == null) {
    dca = new CellAttribute()
}
if (dca.getFont() == undefined) {
    let font: SSFont = new SSFont();
    font.clear()
    dca.setFont(font)
}
let targetFont = dca.getFont();
targetFont.setColor(SSWrench.getTargetSsColor("(0, 0, 255)"));
targetFont.setUnderline(0)
```
- 前端拿到区域及活动单元格: `this.workBookView.getWorksheet().selectionRange`
- 取默认样式: `System.clone(this.book.defAttr)`
- 取区域一个区域的默认属性
``` javascript
for (let i = sr; i <= er; i++) {
    for (let j = sc; j <= ec; j++) {
        dca = System.clone(CellAttrOp.getEntireAttrs(sheet.getBook(), sheet, i, j))
        if (dca == null) {
            dca = new CellAttribute();
        }
        if (dca.getFont() == null) {
            let font = new SSFont();
            font.clear()
            dca.setFont(font)
        }
        let targetFont = dca.getFont();
        targetFont.setColor(SSWrench.getTargetSsColor("rgb(0, 0, 255)"));
        targetFont.setUnderline(0);
    }
}
```
- 排序: 0:升序, 1:降序
- 筛选: 1:数字, 2:日期, 0:文本
- `getCellXByCol` 根据列数得X,x为单元格左边距离画布x轴原点距离, 如果需要得到右边距离画布原点距离,需要加上列宽 - WorkSheetView
- `getColWidth` 获取常规列宽 - WorkSheetView
- `getRowHeight` 获取常规行高 - WorkSheetView
- `_getRowHeight` 获取带有缩放值的行高
- `_getColumnWidth` 获取带有缩放值的列宽
- `getCursorTypeFromXY` 通过XY得到对应的单元格信息 - WorkSheetView
- `groupRowClick` 实现鼠标点击之类的方法 - WorkSheetView
- `worksheetview` 相当于单张工作表
- `worksheet` 相当于EIO里的操作
- 绘制暂时解决方案
``` javascript
this.workBookView.getWorksheet().resetPaintData();
this.workBookView.getWorksheet().repaint()
```
- ar/ac指的设置区域或选择单元格的时候点击的区域即为活动单元格
- `fireFunChanged`该方法是内容设置完成以后,更新其他方法
- `BasicRange`数据
``` json
endColumn: 0
endRow: 0
startColumn: 0
startRow: 0
```
- 如何注册对象
``` javascript
// ModelCons中定义常量座位独一无二的ID
// 当前对象增加getFunID方法
public getFunID(): any {
    return ModelCons.FUN_HYPERLINK;
}
// 构造器中注册该类
constructor(sheet: WorkSheet) {
    this.sheet = sheet;
    sheet.registerFunListener(this);
}
// 需要的地方调用
let hyperLinkManager:HyperLinkManager = this.sheet.getFunction(ModelCons.FUN_HYPERLINK);
if (!hyperLinkManager) {
	hyperLinkManager = new HyperLinkManager(this.sheet);
}
// 这种方式其实会弃用,以后直接使用类调用
```
- `_getCoordinates` 获取坐标`EventsController`
- `mouseMove`中只需要控制元素隐藏与展示即可
- `Dialog`调用方式`Dialog.show()`,`SweetDialog.showDialog(string)`
- 考虑到电脑高分屏可以使用`ratioToEvent(转化为event原始x，y)`/`eventToRatio(转化为高分屏下x，y)`方法
- 菜单栏的状态涉及文件`workbookview`/`MenuSystem`
- 取用户信息`UserData`
- 取文档信息`AppData`
- 3.2中所有的`DocumentData`全部替换成`AppData`
- 取前端区域`ViewBasicRange`:`let range = this.workBookView.getWorksheet(null, null).selectionRange.ranges;`
- 转成后端`BasicRange`:`let ranges: BasicRange[] = SSWrench.multipleViewRangeToModelRange(range)`
``` javascript
let clearSelectRange = this.workBookView.getWorksheet(null, null).selectionRange.ranges; // 前端区域
let clearFeranges: BasicRange[] = Array<BasicRange>(SSWrench.viewRangeToModelRange(clearSelectRange[0])) // 后端区域
```
- 协作中需要将传给协作者的对象转成对应类型对象的方法`System.objectToClass(HyperLink, temporaryHl)`
- 存档&协作(最终协作需要在`SsChange`再操作一次)通用
``` javascript
// 方法入口第一行
 this.workBookView.getWorkBook().methodDataList = [];
// 方法绘制完成后紧接
if (Team.isCollaborative()) {
    // 协作
    this.workBookView.optCollaboration(this.workBookView.getWorkBook().methodDataList)
} else {
    // 存档
    ModifyManager.sendChanges(this.workBookView.getWorkBook().methodDataList);
}
// 进入对应功能中
// 对应功能位置存档数据重组
let data = {};
// SsMethod 中存放对应功能的数字编号
let change = new SsChange(SsMethod.setHyperLink, -1);
change.data = data;
this.getWorkBook().methodDataList.push(change);
```
- 判断是否为中文
``` javascript
function isChinese(s){
    return /[\u4e00-\u9fa5]/.test(s);
    // 0x4E06 丆 0x4E64 乤 ,都不是汉字，但是在汉字区间内
    // return c <= 0x9FA5 && c >= 0x4E00 && (c != 0x4E06 && c != 0x4E64);
}    
```
- 中文字符串转Unicode
``` javascript
toUnicode (s) {
    let str = "";
    for (let i = 0; i < s.length; i++) {
        str +="\\u"+s.charCodeAt(i).toString(16)+"\t";
    }
    return str;
}
```
- 十六进制Unicode编码 转换为 中文字符串
``` javascript
toStr(n){
    var str = "";
    var s = n.split('\\u');
    for(var i = 0;i < s.length;i++){
        str += String.fromCharCode(parseInt(s[i],16))+"\t";
    }
    return str;
}
var b = "\\u80dc    \\u591a    \\u8d1f    \\u5c11";
document.write(toStr(b));    // 胜    多    负    少
```
- 取单元格属性
``` javascript
// worksheet
getCellAttr(row, column): any {
    let cell = this.getCell(row, column);
    if (cell && cell.attr) {
        return cell.attr;
    }
    return null;
}
```
- 取单元格属性之后的比较方法
``` javascript
// CellAttribute
getHash(){
    let align = this.align == undefined ? "" : this.align.getHash();
    let border = this.border == undefined ? "" : this.border.getHash();
    let font = this.font == undefined ? "" : this.font.getHash();
    let fill = this.fill == undefined ? "" : this.fill.getHash();
    let numFmt = this.numFmt == undefined ? "" : this.numFmt.getHash();
    let hashStr = align + "|" + border + "|" + font + "|" + fill + "|" + numFmt +"|" + this.pivotButton + "|" + this.quotePrefix;
    return System.strToHash(hashStr);
}
```
- 所有键盘按键入口`EventsController文件下_onWindowKeyDown方法`
- JS styleSheets对象：读取页面的所有CSS样式

``` javascript
var cssRules = document.styleSheets[0].cssRules || document.styleSheets[0].rules;
```
``` javascript
var mycssstyles;
//判断是否支持DOM2级样式表。
if(document.implementation.hasFeature("StyleSheets", "2.0")) {
    for(var i = 0; i < document.styleSheets.length; i++) {
        if(document.styleSheets[i].type == "text/css") {
            mycssstyles = document.styleSheets[i];
        }
    }
    //ie不存在cssrules。 firefox不存在rules
    if(mycssstyles.cssRules) {
        mycssstyles = mycssstyles.cssRules;
    } else {
        //火狐浏览器没有rules
        if(mycssstyles.rules) {
            mycssstyles = mycssstyles.rules;
        }
    }
    console.log(mycssstyles);
}
```
在上面代码中，先判断浏览器是否支持 cssRules 对象，如果支持则使用 cssRules（非 IE 浏览器），否则使用 rules（IE 浏览器）
- 取节点中的样式
``` javascript
function getStyle(node, property) {
    if (node.style[property]) {
        return node.style[property]
    } else if (node.currentStyle) {
        return node.currentStyle[property]
    } else if (document.defaultView && document.defaultView.getComputedStyle) {
        console.log(style.getPropertyValue(property)) return style.getPropertyValue(property)
    }
    return null
}
console.log(getStyle(document.getElementById('xl65'), 'mso-number-format'))
```
- 查看SSUndoManager
``` javascript
YozoOffice.Application.ActiveSheet.__sheet.sheet.parent.ssUndoManager
```
- 只读
``` javascript
if (UserInfo.isReadonly()) {
    return;
}
```
- 快捷键入口从这边儿找: `EventsController`, 如:`esc`

## 技巧

- 新功能断点从3.1中`SsRequestItem`进断else
- 一个功能的所有管理可以考虑放在一起,哪怕是从控制层过来的,也可以这么进行处理,可以参考AutoFilterManager中的init方法(此方法需要在workbookview中进行重置)
- 初始化表之后单元格属性中是不存在行头相关信息的,做操作的时候如果涉及到(如筛选),需要手动进行添加操作`HeadItem`
- 前端读属性相关信息`_addCellTextToCache`(worksheetview中)断点
