# YOZO

1. **Array** - JS中无数组的`contains`方法时候处理方案
	``` js
	// 实现方法
	function contains(a, obj) {
	    var i = a.length;
	    while (i--) {
	       if (a[i] === obj) {
	           return true;
	       }
	    }
	    return false;
	}
	// 扩展Array类
	Array.prototype.contains = function(obj) {
	    var i = this.length;
	    while (i--) {
	        if (this[i] === obj) {
	            return true;
	        }
	    }
	    return false;
	}
	```
2. **Array** - JS中对象与数组中的对象比较处理方案
	``` js
	let vec = [{endColumn: 4 ,endRow: 3 ,startColumn: 4 ,startRow: 3}]
	let vecobj = {endColumn: 4 ,endRow: 3 ,startColumn: 4 ,startRow: 3}

	// 没匹配到的话返回-1
	// 可以用>-1的判断是否匹配到对象
	function compareObjEqual(list, obj) {
      const keys = Object.keys(obj);
      let index = 0;
      for (let i=0; i<list.length; i++) {
        const item = list[i];
        let isEqual = true;
        console.log('=================item==================');
        console.log(item);
        for (let j=0; j<keys.length; j++) {
          const key = keys[j];
          if (obj[key] !== item[key]) {
            isEqual = false;
            break;
          }
        }
        if(isEqual) {
          return index = i;
        }
      }
      return -1;
    }
	```