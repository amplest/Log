# Vue+ElementUI实战应用

## 课程表项目

## 用户分群项目

## 音频上传

## 三级联动调用

## 表单自定义项目

## 点击对应的表单元素获取相关数据信息

## 数据重组实例

## 表格（带时间）可添加

## 表格可拖拽重新排列元素顺序

## 提交使用for循环遍历对比数据汇总成新数组
``` javascript
onSubmit() {
	let arrFor = [];
    for (let i = 0; i<_this.bigListNew.length; i++) {
        for (let j = 0; j<_this.bigListNew[i].length; j++) {
            for (let k = 0; k<_this.listNew.table.length; k++) {
                if (_this.bigListNew[i][j].id == _this.listNew.table[k].id) {
                    arrFor.push(_this.bigListNew[i][0])
                }
            }
        } 
    }
}
```

## Vue数据提交如何附加新数据给到表单

