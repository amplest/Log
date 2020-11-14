# 工作积累

## 取值

- 4.0 _$SSView, _$Workbook 合并为 workBookView
- 单元格数据 sheet.getCell()
- sheetId: workbookview.getWorksheet().sheet.id;
- sheet: workbookview.getWorksheet().sheet;
- 调用单元格默认属性: CellAttrOp.getViewAttribute(book.styleSet, sheet, row, col);
- 前端拿到区域及活动单元格: this.workBookView.getWorksheet().selectionRange
- 取默认样式: System.clone(this.book.defAttr)

## 记录

- 筛选: 1:数字, 2:日期, 0:文本
- getCellXByCol 根据列数得X,x为单元格左边距离画布x轴原点距离, 如果需要得到右边距离画布原点距离,需要加上列宽 - WorkSheetView
- getColWidth 获取列宽 - WorkSheetView
- getCursorTypeFromXY 通过XY得到对应的单元格信息 - WorkSheetView
- groupRowClick 实现鼠标点击之类的方法 - WorkSheetView
- worksheetview 相当于单张工作表
- worksheet 相当于EIO里的操作

## 技巧

- 断点关闭前 Ctrl+F8
- IDEA 断开断点后执行完断点不卡
- 新功能断点从3.1中SsRequestItem进断else
- 一个功能的所有管理可以考虑放在一起,哪怕是从控制层过来的,也可以这么进行处理,可以参考AutoFilterManager中的init方法(此方法需要在workbookview中进行重置)

## 问题
