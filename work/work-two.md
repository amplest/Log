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
      for (let i=0; i< list.length; i++) {
        const item = list[i];
        let isEqual = true;
        //console.log('=================item==================');
        //console.log(item);
        for (let j=0; j< keys.length; j++) {
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
3. **Constructor** - JS中控制器中读取参数方式
	``` js
 	constructor(p1?, p2?, p3?) {
        super()
        let arr = Array.from(arguments);
        if (arr.length == 1) {
            arr[1] = 0;
        }
    }
    // 常规函数中直接可以用arguments.length直接读取
	```
4. **ArrayList** - arraycopy方法JS重构
	``` js
	// arraycopy 重构 TODO: length 传入长度-1
    static _arraycopy(
        sourceData,
        sourceStartIndex,
        targetData,
        targetStartIndex,
        length
    ) {
        if (targetStartIndex <= sourceStartIndex) {
            // 往前移
            for (
                let i = targetStartIndex;
                i <= targetStartIndex + length;
                i++
            ) {
                targetData[i] = sourceData[sourceStartIndex++]
            }
        } else {
            // 往后移
            for (
                let i = targetStartIndex + length;
                i >= sourceStartIndex;
                i--
            ) {
                targetData[i] = sourceData[i - 1]
            }
        }
        console.log('arraycopy后最终目标数组: ', targetData)
        return targetData
    }
	```
5. **ArrayList** - 简单重写
	``` js
	class ArrayList {
		constructor() {
			// 默认的list容量大小
			this.defaultCapacity = 10
			// list中元素的个数
			this.size = 0
			// 数据集，存储的元素支持泛型
			this.data = new Array(this.defaultCapacity)
		}

		/**
		 * 获取list element个数
		 * @returns {number}
		 */
		length() {
			return this.size
		}

		/**
		 * 检查list是否为空
		 * @returns {boolean}
		 */
		isEmpty() {
			return this.size === 0
		}

		/**
		 * 检查一个元素是否存在list中
		 * @param {any} element
		 * @returns {boolean}
		 */
		contains(element) {
			return this.indexOf(element) !== -1
		}

		/**
		 * 返回element在list中第一次出现的索引位置，从前往后扫描，不存在返回-1
		 * @param {any} element
		 * @returns {number}
		 */
		indexOf(element) {
			for (let i = 0; i < this.size; i++) {
				if (Object.is(element, this.data[i])) {
					return i
				}
			}
			return -1
		}

		/**
		 * 返回element在list中第一次出现的索引位置，从后往前扫描，不存在返回-1
		 * @param {any} element
		 * @returns {number}
		 */
		lastIndexOf(element) {
			for (let i = this.size; i >= 0; i--) {
				if (Object.is(element, this.data[i])) {
					return i
				}
			}
			return -1
		}

		/**
		 * 根据索引查找对应元素
		 * @param {number} index
		 * @returns {any}
		 */
		get(index) {
			this.checkBoundExclusive(index)
			return this.data[index]
		}

		/**
		 * 修改索引位置中的值
		 * @param {number} index
		 * @param {any} element
		 * @returns {any}
		 */
		set(index, element) {
			this.checkBoundExclusive(index)
			const oldValue = this.data[index]
			this.data[index] = element
			return oldValue
		}

		/**
		 * 添加element到list
		 * @param {any} element
		 * @returns {boolean}
		 */
		add(element) {
			if (this.size === this.data.length) {
				this.ensureCapacity(this.size + 1)
			}
			this.data[this.size++] = element
			return true
		}

		/**
		 * 在指定索引位置插入element
		 * @param {number} index
		 * @param {any} element
		 */
		insert(index, element) {
			this.checkBoundExclusive(index)
			if (this.size === this.data.length) {
				this.ensureCapacity(this.size + 1)
			}
			if (index !== this.size) {
				this.arrayCopy(
					this.data,
					index,
					this.data,
					index + 1,
					this.size - index
				)
			}
			this.data[index] = element
			this.size++
		}

		/**
		 * 数组拷贝
		 * @param {array} sourceData 源list
		 * @param {number} sourceStartIndex 源起始位置
		 * @param {array} targetData 目标list
		 * @param {number} targetStartIndex 目标起始位置
		 * @param {number} length 操作的长度
		 */
		arrayCopy(
			sourceData,
			sourceStartIndex,
			targetData,
			targetStartIndex,
			length
		) {
	        if (targetStartIndex <= sourceStartIndex) {
	            // 往前移
	            for (
					let i = targetStartIndex;
					i <= targetStartIndex + length;
					i++
				) {
					targetData[i] = sourceData[sourceStartIndex++]
				}
	        
	        } else {
	            // 往后移
	            for (
					let i = targetStartIndex + length;
					i >= sourceStartIndex;
					i--
				) {
					targetData[i] = sourceData[i - 1]
				}
	        }
		}

		/**
		 * 移除指定索引位置元素
		 * @param {number} index
		 */
		remove(index) {
			this.checkBoundExclusive(index)
			const r = this.data[index]
			if (index !== --this.size) {
				this.arrayCopy(this.data, index + 1, this.data, index, this.size - index)
			}
			this.data[this.size] = null
			return r
		}

		/**
		 * 清空list
		 */
		clear() {
			this.data = new Array[this.defaultCapacity]()
			this.size = 0
		}

		/**
		 * 检查数组是否越界
		 * @param {number} index
		 */
		checkBoundExclusive(index) {
			if (index >= this.size) {
				throw new Error(`Index: ${index} , Size: ${(this, this.size)}`)
			}
		}
		/**
		 * 动态扩容
		 * @param {number} minCapacity
		 */
		ensureCapacity(minCapacity) {
			if (minCapacity > this.data.length) {
				const newData = new Array(
					Math.max(2 * this.defaultCapacity, minCapacity)
	            )
				this.arrayCopy(this.data, 0, newData, 0, this.size)
				this.data = newData
			}
		}
	}

	// test case

	// const list = new ArrayList()
	// console.log('是否为空', list.isEmpty())
	// console.log('当前长度', list.length())
	// list.add(1)
	// list.add(2)
	// list.add(3)
	// list.add(4)
	// list.add(5)
	// list.add(6)
	// list.add(7)
	// list.add(8)
	// list.add(9)
	// list.add(10)
	// console.log("当前长度", list.length())
	// list.add(11)
	// console.log(list)
	// console.log(list.contains(12))
	// console.log(list.indexOf(2), list)
	// console.log(list.lastIndexOf(10))
	// console.log(list)
	// list.add(12)
	// list.insert(11, '替换之前的11')
	// console.log(list)
	// list.remove(10)
	// console.log(list)
	// list.insert(0, '替换之前的0')
	// console.log(list)
	```