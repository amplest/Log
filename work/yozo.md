# 工作积累


## 记录

- `_$SSView`, `_$Workbook` 合并为 `workBookView`
- 单元格数据 `sheet.getCell()`
- sheetId: `workbookview.getWorksheet().sheet.id`;
- sheet: `workbookview.getWorksheet().sheet`;
- 调用单元格默认属性: `CellAttrOp.getViewAttribute(book.styleSet, sheet, row, col)`;
- 前端拿到区域及活动单元格: `this.workBookView.getWorksheet().selectionRange`
- 取默认样式: `System.clone(this.book.defAttr)`
- 取区域一个区域的默认属性
``` javascript
for (let i = sr; i <= er; i++) {
    for (let j = sc; j <= ec; j++) {
        let dca: CellAttribute = System.clone(CellAttrOp.getViewAttribute(this.sheet.getBook().getStyleSet(), this.sheet, i, j))
        console.log(dca)
    }
}
```
- 筛选: 1:数字, 2:日期, 0:文本
- `getCellXByCol` 根据列数得X,x为单元格左边距离画布x轴原点距离, 如果需要得到右边距离画布原点距离,需要加上列宽 - WorkSheetView
- `getColWidth` 获取列宽 - WorkSheetView
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
```
- `_getCoordinates` 获取坐标`EventsController`
- `mouseMove`中只需要控制元素隐藏与展示即可
- `Dialog`调用方式`Dialog.show()`,`SweetDialog.showDialog(string)`

## 技巧

- 新功能断点从3.1中`SsRequestItem`进断else
- 一个功能的所有管理可以考虑放在一起,哪怕是从控制层过来的,也可以这么进行处理,可以参考AutoFilterManager中的init方法(此方法需要在workbookview中进行重置)



## 遇事不要慌,不要忙,一点点来解决.


## 问题

- SheetData 中`getRowHeader`方法中cell为null, 行头信息获取失败
- 