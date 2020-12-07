# 工作积累

## 记录

- clone操作有两种方式: `System.clone(argument)`,`CloneUtil.clone(argument)`
- `_$SSView`, `_$Workbook` 合并为 `workBookView`
- 单元格数据 `sheet.getCell()`
- sheetId: `workbookview.getWorksheet().sheet.id`;
- sheet: `workbookview.getWorksheet().sheet`;
- 调用单元格默认属性, 现在取属性同意使用`CellAttrOp.getEntireAttrs`
``` javascript
// 取某个单元格的属性
dca = System.clone(CellAttrOp.getEntireAttrs(sheet.getBook(), sheet, sr, sc))
if (dca === null) {
    dca = new CellAttribute()
    dca.setFont(new SSFont())
}
dca.getFont();
```
- 前端拿到区域及活动单元格: `this.workBookView.getWorksheet().selectionRange`
- 取默认样式: `System.clone(this.book.defAttr)`
- 取区域一个区域的默认属性
``` javascript
for (let i = sr; i <= er; i++) {
    for (let j = sc; j <= ec; j++) {
        dca = System.clone(CellAttrOp.getEntireAttrs(sheet.getBook(), sheet, i, j))
        if (dca === null) {
            dca = new CellAttribute()
            dca.setFont(new SSFont())
        }
        dca.getFont();
        let targetFont = dca.getFont(); // 获取Font对象
        targetFont.setColor(SSWrench.getTargetSsColor("(0, 0, 255)")); // 设置颜色
        targetFont.setUnderline(0); // 设置下划线
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

## 技巧

- 新功能断点从3.1中`SsRequestItem`进断else
- 一个功能的所有管理可以考虑放在一起,哪怕是从控制层过来的,也可以这么进行处理,可以参考AutoFilterManager中的init方法(此方法需要在workbookview中进行重置)
- 初始化表之后单元格属性中是不存在行头相关信息的,做操作的时候如果涉及到(如筛选),需要手动进行添加操作`HeadItem`
- 前端读属性相关信息`_addCellTextToCache`(worksheetview中)断点

## 问题

下一个工作日的任务衔接, 用于快速进入工作状态!





## 复制粘贴功能重做

### 文件释义

- `clipboard.d.ts` : 剪切板常量管理
- `ClipboardTrigger` : 剪切板触发器
- `Clipboard` : 剪切板
