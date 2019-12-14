# Vue+ElementUI实战应用

## 课程表项目

## 音频上传

## Cascader级联组件使用

级联最核心的理解是:props，如果不是自带的字段，就可以用props来指定

``` json
{
    "cityList": [{
        "id": "1",
        "name": "北京",
        "child": {
            "id": "11",
            "name": "大兴区",
            "child": [{
                "id": "111",
                "name": "亦庄"
            }]
        }
    }]
}
```

data中的定义

``` js
data() {
    return {
        optionProps: {
            value: 'id',
            label: 'name',
            children: 'child'
        }
    }
}
```
代码结构
``` html
<!-- selectedOptions 应该是以数组的方式传给后端 -->
<el-cascader 
    :options="cityList" 
    :props="optionProps" 
    v-model="selectedOptions" 
    @change="handleChange">
</el-cascader>
```


## 表单自定义项目

## 点击对应的表单元素获取相关数据信息

## 数据重组实例

## 表格（带时间）可添加

## 表格可拖拽重新排列元素顺序

## 提交使用for循环遍历拿到新数组

主要思路就是for循环嵌套，将bigListNew的数据与listNew中的数据对比然后匹配的就拿出来形成新的数组（listNew数据是固定的）

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
    console.log(arrFor)
}
```

## Vue数据提交附加新数据

比方说项目完成了，但是需要在增加一个字段，你也不知道后端需要你提交的字段是啥，所以，你可以先预设一个新的字段，最终知道后端字段以后就可以将这个新字段赋给后端接受的字段就可以了

``` javascript
let formData = this.productList;
formData.info = this.info;
// 最终给到data的应该是formData
```

## el-table 内部嵌套数组

使用el-table组件中的（展开行），需要实现一个点击下拉，下拉中展示的内容是一个数组

数据结构如下

``` json
{
    "tableData": [{
        "id": "1",
        "date": "2016-05-02",
        "name": "王小虎",
        "address": "上海市普陀区金沙江路 1518 弄",
        "detail": {
            "one": [{
                    "id": "1",
                    "content": "我是一个好人",
                    "name": "阿星"
                },
                {
                    "id": "3",
                    "content": "我是一个美人",
                    "name": "阿呆"
                }
            ],
            "two": {
                "type": "1",
                "list": ["1", "2", "3"]
            },
            "time": "2019-12-14 08:55:10"
        }
    }]
}
```

HTML结构如下

``` html
<el-table :data="tableData" style="width: 100%">
    <el-table-column label="ID" prop="id"></el-table-column>
    <el-table-column label="姓名" prop="name"></el-table-column>
    <el-table-column label="时间" prop="date"></el-table-column>
    <el-table-column label="地址" prop="address"></el-table-column>
    <el-table-column type="expand" label="操作">
      <template slot-scope="scope">
        <!-- 这里布局展开的内容：start -->
        <div v-for="item in tableData[scope.$index].detail.one" :key="item">
            <div v-if="item.id == 1">
                <!-- 数据模板1 -->
                姓名：{{item.name}}
                座右铭：{{item.content}}
            </div>
            <div v-if="item.id == 2">
                <!-- 数据模板2 -->
                姓名：{{item.name}}
                座右铭：{{item.content}}
            </div>
        </div>
        <div>
            <div v-if="tableData[scope.$index].detail.two.type == 1">
                <!-- 当type=1的时候展示的内容 -->
            </div>
            <div v-else>
                <!-- 当type!=1的时候展示的内容 -->
            </div>
            <!-- two中list数组 -->
            <el-table style="width:100%" :data="tableData[scope.$index].detail.two.list">
                <el-table-column
                    type="index"
                    width="120"
                    label="优先级">
                </el-table-column>
                <el-table-column label="通知项目">
                    <template slot-scope="scope">    
                        <span v-if="scope.row == '1'">校长</span>
                        <span v-if="scope.row == '2'">学生</span>
                        <span v-if="scope.row == '3'">老师</span>
                    </template>
                </el-table-column>
            </el-table>
        </div>
        <div>{{tableData[scope.$index].detail.time}}</div>
         <!-- 这里布局展开的内容：end -->
      <template>
    </el-table-column>
</el-table>
```