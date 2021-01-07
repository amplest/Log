# Vue

## UI基于ElementUI

### 懒加载应用(el-tree)

- HTML 结构
  - default-expanded-keys 默认展开第一个节点，这个很重要，因为使用懒加载的时候，只会展示节点一，有了他会默认展开第一级下的children
  - props设置数据接收格式可自定义字段

``` html
<el-tree :props="props" node-key="id" :default-expanded-keys="[1]" :load="loadNode" lazy></el-tree>
```
``` javascript
data() {
    return {
        props: {
          label: 'label',
          children: 'children',
          isLeaf: 'leaf' // 用于判断左侧三角箭头是否展示，大多数应用于判断是否存在下级传值为true/false
        },
        node: null,
        resolve: null
    }
},
methods: {
    loadNode(node, resolve) {
        if (node.level === 0) {
         // 当节点级别是0的时候默认展示第一级
         // 树形结构初始化之后，接口只需要返回一个一级对象即可，前端拿到对象后将其resolve即可展示到树形结构中
         this.node = node
         this.resolve = resolve  // 如果树形结构上存在增删改查操作的时候，改了之后需要进行刷新操作的，就需要用到这两个操作
          return resolve([{ name: 'region' }]);
        } else {
            // 当节点为1或N级的时候走这边
            // 接口只需要返回下级数组即可数据格式如下data，最终resolve
            const data = [{
                label: '这是名称1',
                isLeaf: true
            }, {
                label: '这是名称2'
            }];
            return resolve(data)
        }
    },
    handleAdd() {
        // 如果存在添加操作，添加动作完成刷新接口后，这个时候需要刷新树结构则需要进行如下更新操作
        this.loadNode(this.node, this.resolve)
    }
}
```

### 表单验证

#### 手机号

``` javascript
// 手机号11位自定义验证
const validatePhone = (rule, value, callback) => {//定义规则
    let reg = /^1[345789]\d{9}$/;
    if (value != '' && reg.test(value)) {
        callback()
    } else {
        callback(new Error('请输入正确的手机号'))
    }
}
```

#### 表单自定义

- 使用嵌套循环进行渲染结构
- 涉及字段读取可以使用`ruleForm[secondIndex.first_field]`的方式来进行读取对应字段中的值

#### 链接验证
``` javascript
var urlValid = (rule, value, callback) => {
    if (value == '') {
        callback(new Error('请输入链接'));
    } else {
        if (value.indexOf("http://") !== 0 && value.indexOf("https://") !== 0) {
                callback(new Error('链接格式请以http:// 或 https:// 开头'));
            } else {
                callback();
        }
    }
}
```

### Cascader组件

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
``` javascript
// 后端数据结构
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
``` html
<!-- selectedOptions 应该是以数组的方式传给后端 -->
<el-cascader 
    :options="cityList" 
    :props="optionProps" 
    v-model="selectedOptions" 
    @change="handleChange">
</el-cascader>
```

### 表格(e-table)

#### 表格添加自定义实例

![效果图01](/assets/table-01.gif)

``` html
<el-row>
  <el-col class="header">
    <div>天</div>
    <div>上课时间段</div>
    <div>操作</div>
  </el-col>
  <el-table :data="ele_form.weeks">
    <el-table-column>
      <template slot-scope="scope">
        <div class="item">
          <el-select class="form-cw" v-model="scope.row.w" placeholder="请选择">
            <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
          </el-select>
          <el-time-select class="itemw" placeholder="起始时间" v-model="scope.row.start_time" :picker-options="{ start: '00:00', step: '00:30', end: '23:30'}">
          </el-time-select>
          至
          <el-time-select class="itemw" placeholder="结束时间" v-model="scope.row.end_time" :picker-options="{ start: '00:00', step: '00:30', end: '23:30', minTime: scope.row.start_time}"></el-time-select>
          <el-button type="text" @click="handleDelete(scope.$index, ele_form.weeks)">删除</el-button>
        </div>
      </template>
    </el-table-column>
  </el-table>
  <el-col class="item-add">
    <el-button type="text" @click="handleAdd(ele_form.weeks)"><i class="el-icon-plus"></i> 添加课时</el-button>
  </el-col>
</el-row>
```
``` javascript
data: {
    return: {
        // 周一~周日下拉选择数据
        options: [
            {
                value: '1',
                label: '周一',
            },
            {
                value: '2',
                label: '周二',
            },
            {
                value: '3',
                label: '周三',
            },
            {
                value: '4',
                label: '周四',
            },
            {
                value: '5',
                label: '周五',
            },
            {
                value: '6',
                label: '周六',
            },
            {
                value: '0',
                label: '周日',
            }
        ],
        // 返给后端的数据结构
        ele_form: {
            weeks: [
                {
                    w: '',
                    start_time: '',
                    end_time: ''
                }
            ]
        }
    }
},
methods: {
    // 添加新的行，使用数组的push方法
    handleAdd:function(rows) {
        rows.push({w:'',start_time: '',end_time: ''})
    },
    // 删除行，使用splice方法，设置删除一个
    handleDelete:function(index, rows) {
        rows.splice(index, 1)
    },
}
```

#### el-table 单选

``` html
<el-table ref="singleTable" @row-click="chooseone" @current-change="handleCurrentChange" highlight-current-row v-loading="tableLoading" tooltip-effect="dark" size="small" style="width: 100%" show-header="false">
    <el-table-column label="选择" width="65" class="btn-center">
        <template scope="scope">
            <el-radio :label="scope.row.id" v-model="templateRadio">&nbsp;</el-radio>
        </template>
    </el-table-column>
    <el-table-column  prop="content" label="文本素材" show-overflow-tooltip></el-table-column>
</el-table>
```

``` javascript
// data 中定义 templateRadio: null
handleCurrentChange(val) {
    this.newSendData.content = val.content;
},
chooseone(row) {
    this.templateRadio = row.id
},
```


### el-table 全选数据只获取id

做一个列表页，勾选列表前面的checkbox只拿当前数据的id拼一个数组，重点代码如下

``` html
<el-table :data="studentList" ref="multipleTable" @selection-change="handleSelectionChange">
    <el-table-column
      type="selection"
      width="55">
    </el-table-column>
</el-table>
```

``` javascript
data() {
    return {
        multipleSelection: []
    }
}
methods: {
    handleSelectionChange(val) {
      if (val) {
        let list = [];
        let listId = [];
        list = val;
        for (let i = 0; i<list.length; i++) {
            listId.push(list[i].id);
        }
        this.multipleSelection = listId;
        console.log(this.multipleSelection)
      }
    }
}
```

### el-input监听enter键

`@keyup.enter.native`

## el-date-picker 用法 

`daterange` 与 `datetimerange` 实际中的使用

``` html
<el-date-picker v-model="date_range" type="daterange" unlink-panels range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期" value-format="yyyy-MM-dd"></el-date-picker>
```

``` javascript
// data
date_range: '',
formInline: {
    begin_at: '',
    end_at: '',
}
// methods 往接口里面推送数据的时候需要做拆分
let date_range = this.date_range
if (date_range) {
    this.formInline.begin_at = date_range[0]
    this.formInline.end_at = date_range[1]
}
```

### v-if里面的三元表达

`v-if="categoryNum != -1 && categoryNum != 0 ? true:false"`

### 动态类:class

``` html
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">
<!-- 对象 -->
<a :class="{ 'active': hash == 'finish','nav-link':true}">已完成</a>
<!-- 数组加对象 -->
<div :class="['tab-btn', {'option-act': categoryNum == -1}]"  @click="navbarCategoryEvent(-1)">全部</div>
<!-- 字符串拼接 -->
<div :class="'classify'+(current=='0'?' active':'')">全部</div>
<!-- 数组形式 -->
<div :class='["classify",current=="0" ? "active" : ""]'>全部</div>
```

### el-checkbox 实现单选

``` html
<el-checkbox-group class="checked-hide" max=2 v-model="checkedLinks" @change="handleCheckedLinkChange(item)">
    <el-checkbox :label="item.id" class="button">&nbsp;</el-checkbox>
</el-checkbox-group>
```

``` javascript
handleCheckedLinkChange: function(val) {
    this.checkedLinks = new Array;
    this.checkedLinks.push(val.id); // data 中预设一个数组
}
```

### el-checkbox 单个选择成数组的操作

``` html
<el-form-item label="题型选择">
    <el-checkbox v-model="autoRadio" label="1" @change="autoCheckBox(1)">单选</el-checkbox>
    <el-checkbox v-model="autoMany"  label="2" @change="autoCheckBox(2)">多选</el-checkbox>
    <el-checkbox v-model="autoJudge" label="3" @change="autoCheckBox(3)">判断</el-checkbox>
    <el-checkbox v-model="autoBlanks" label="4" @change="autoCheckBox(4)">填空</el-checkbox>
</el-form-item>  
```

``` javascript
var checkTypeList = []; // 全局设置一个数组用来接收

autoCheckBox(type) {
    if (checkTypeList.indexOf(type) == -1) {
        checkTypeList.push(type);
    } else {
        checkTypeList.splice(checkTypeList.indexOf(type), 1);
    }
    this.typeList = checkTypeList;
},
```

## JSON字符串中带html的进行转义

`<pre>{{xxx}}</pre>`

## ElementUI中涉及Dilalog弹窗关闭,但是验证依然存在问题

``` javascript
this.$refs[formName].clearValidate();  // dialog 的时候
this.$refs[formName].resetFields(); // 单独的重置的时候
```

## 复杂项目实例

- [课程表项目](frontend/vue/project/Class_Schedule_Card_01.md)
