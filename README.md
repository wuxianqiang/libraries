# libraries

# 目录
- [仿ECMAScript5中Object.create()函数](#仿ecmascript5中objectcreate函数)
- [仿ECMAScript5中Array.reduce()函数](#仿ecmascript5中arrayreduce函数)

## 仿ECMAScript5中Object.create()函数
```js
        function inherit(obj) {
            if (obj === null) throw TypeError();
            if (Object.create) return Object.create(obj);
            var t = typeof obj;
            if (t !== "object" && t !== "function") throw TypeError();
            function Fn() {};
            Fn.prototype = p;
            return new Fn();
        }
```
## 仿ECMAScript5中Array.reduce()函数

```js
        var reduce = Array.prototype.reduce ? function (ary, fn, initial) {
            if (arguments.length > 2) { //如果reduce()方法存在的话
                return ary.reduce(fn, initial); //如果传入了一个初始值
            } else {
                return ary.reduce(fn); //否则初始值
            }
        } : function (ary, fn, initial) { //以特定的初始值开始，否则第一个值取自ary
            var i = 0,
                len = ary.length,
                accumulator;
            if (arguments.length > 2) {
                accumulator = initial;
            } else { //找到数组中第一个已经定义的索引
                if (len == 0) throw TypeError();
                while (i < len) {
                    if (i in ary) {
                        accumulator = ary[i++];
                        break;
                    } else {
                        i++;
                    }
                }
                if (i == len) throw TypeError();
            }
            while (i < len) { //对于数组中剩下的元素依次调用fn
                if (i in ary) {
                    accumulator = fn.call(undefined, accumulator, ary[i], i, ary)
                }
                i++;
            }
            return accumulator;
        }
```
**[⬆ back to top](#readme)**
## 在数组中查找所有出现的元素方法

```js
        function findAll(ary, ele) {
            var results = [],
                len = a.length,
                pos = 0;
            while (pos < len) {
                pos = ary.indexOf(ele, pos);
                if (pos === -1) break;
                results.push(pos);
                pos++;
            }
            return results;
        }
```
**[⬆ back to top](#readme)**
## 数据类型检测，特殊情况特殊处理

```js
        function classOf(obj) {
            if (obj === null) return "Null";
            if (obj === undefined) return 'Undefined';
            return Object.prototype.toString.call(obj).slice(8, -1);
        }
```
**[⬆ back to top](#readme)**
## 仿ECMAScript5中Object.keys()函数

```js
        function keys(obj) {
            if (typeof obj !== "object") {
                throw TypeError();
            }
            var result = [];
            for (var prop in obj) {
                if (obj.hasOwnProperty(prop)) {
                    result.push(prop);
                }
            }
            return result;
        }
```
**[⬆ back to top](#readme)**
## 仿Math.max方法（不定实参函数）

```js
        function max() {
            var max = Number.NEGATIVE_INFINITY;
            for (var i = 0; i < arguments.length; i++) {
                if (arguments[i] > max) max = arguments[i];
            }
            return max;
        }
```
**[⬆ back to top](#readme)**
## 仿ECMAScript5中Function.bind()函数

```js
        if (!Function.prototype.bind) {
            Function.prototype.bind = function (obj) {
                var self = this,
                    boundArgs = arguments;
                return function () {
                    var args = [],
                        i;
                    for (i = 1; i < boundArgs.length; i++) args.push(boundArgs[i]);
                    for (i = 1; i < arguments.length; i++) args.push(arguments[i]);
                    return self.apply(obj, args);
                }
            }
        }
```
**[⬆ back to top](#readme)**
## 仿ECMAScript5中Array.map()函数

```js
        var map = Array.prototype.map ? function (ary, fn) {
            return ary.map(fn);
        } : function (ary, fn) {
            var results = [];
            for (var i = 0, len = ary.length; i < len; i++) {
                if (i in ary) {
                    results[i] = fn.call(null, ary[i], i, ary);
                }
            }
            return results;
        }
```
**[⬆ back to top](#readme)**
## 数组去重

```js
        Array.prototype.unique = function unique() {
            var obj = {};
            for (var i = 0; i < this.length; i++) {
                var current = this[i];
                if (obj[current] === current) {
                    current = this[this.length - 1];
                    this.length--;
                    i--;
                    continue;
                }
                obj[current] = current
            }
            obj = null;
            return this;
        }
```
**[⬆ back to top](#readme)**
## 冒泡排序

```js
        Array.prototype.bubbleSort = function bubbleSort() {
            var temp = null;
            for (var i = 0; i < this.length - 1; i++) {
                for (var k = 0; k < this.length - 1 - i; k++) {
                    if (this[k] > this[k + 1]) {
                        temp = this[k];
                        this[k] = this[k + 1];
                        this[k + 1] = temp;
                    }
                }
            }
            return this;
        }

```
**[⬆ back to top](#readme)**
## 仿String.match()方法实现

```js
        String.prototype.mymatch = function (reg) {
         var ary = [];
         var res = reg.exec(this);
         while (res) {
            ary.push(res[0]);
            res = reg.exec(this);
         }
         return ary;
        }
```
**[⬆ back to top](#readme)**
## 返回元素的第n层祖先元素

```js
        /**
        *返回元素ele的第n层祖先元素，如果不存在此类祖先或祖先不是Element，
        *（例如Document或者DocumentFragment）则返回null
        *如果n为0，则返回e本身。如果n为1（或省略），则返回其父元素
        *如果n为2，则返回其祖父元素，依次类推
        */
        function parent(ele, n) {
            if (n === nudefined) n = 1;
            while (n-- && ele) {
                ele = ele.parentNode;
            }
            if (!ele || ele.nodeTope !== 1) return null;
            return ele;
        }
```
**[⬆ back to top](#readme)**
## 返回元素的第n个兄弟元素

```js
/**
 *返回元素ele的第n个兄弟元素
 *如果n为正，返回后续的第n个兄弟元素
 *如果n为负，返回前面的第n个兄弟元素
 *如果n为零，返回ele本身
 */
function sibling(ele, n) {
    while (ele && n !== 0) { //如果ele未定义，即刻返回它
        if (n > 0) { //查找后续的兄弟元素
            if (ele.nextElementSibling) {
                ele = ele.nextElementSibling;
            } else {
                for (ele = ele.nextSibling; ele && ele.nodeType !== 1; ele = ele.nextSibling) /*空循环*/;
            }
            n--;
        } else { //查找前面的兄弟元素
            if (ele.previousElementSibing) {
                ele = ele.previousElementSibling;
            } else {
                for (ele = ele.previousSibling; ele && ele.nodeType !== 1; ele = ele.previousSibling) /*空循环*/;
            }
            n++;
        }
    }
    return ele;
}
```
**[⬆ back to top](#readme)**
## 返回元素的第n个子代元素

```js
/**
 *返回元素ele的第n代子元素，如果不存在则为null
 *负值n代表从后往前计数。0表示第一个子元素，而-1代表最后一个，-2代表倒数第二个，依次类推
 */
function child(ele, n) {
    if (ele.children) { //如果children数组存在
        if (n < 0) n += ele.children.length; //转换负的n为数组索引
        if (n < 0) return null; //如果它仍然为负，说明没有子元素
        return ele.children[n]; //返回指定的子元素
    }
    //如果e没有children数组，找到第一个子元素并向前数，或找到最后一个子元素并往回数
    if (n >= 0) { //n非负：从第一个子元素向前数
        //找到元素e的第一个子元素
        if (ele.firstElementChild) {
            ele = ele.firstElementChild;
        } else {
            for (ele = ele.firstChild; ele && ele.nodeType !== 1; ele = ele.nextSibling) /*空循环*/;
        }
        return sibling(ele, n); //返回第一个子元素的第n个兄弟元素
    } else { //n为负：从最后一个子元素往回数
        if (ele.lastElementChild) {
            ele = ele.lastElementChild;
        } else {
            for (ele = ele.lastChild; ele && ele.nodeType !== 1; ele = ele.previousSibling) /*空循环*/;
        }
        return sibling(ele, n + 1); //+1来转化最后1个子元素为最后1个兄弟元素
    }
}
```
**[⬆ back to top](#readme)**
## 表格的行排序

```js
//根据指定表格每行第n个单元格的值，对第一个＜tbody＞中的行进行排序
//如果存在comparator函数则使用它，否则按字母表顺序比较
function sortrows(table, n, comparator) {
    var tbody = table.tBodies[0]; //第一个＜tbody＞，可能是隐式创建的
    var rows = tbody.getElementsByTagName("tr"); //tbody中的所有行
    rows = Array.prototype.slice.call(rows, 0); //真实数组中的快照
    //基于第n个＜td＞元素的值对行排序
    rows.sort(function (row1, row2) {
        var cell1 = row1.getElementsByTagName("td")[n]; //获得第n个单元格
        var cell2 = row2.getElementsByTagName("td")[n]; //两行都是
        var val1 = cell1.textContent || cell1.innerText; //获得文本内容
        var val2 = cell2.textContent || cell2.innerText; //两单元格都是
        if (comparator) return comparator(val1, val2); //进行比较
        if (val1 < val2) {
            return -1;
        } else if (val1 > val2) {
            return 1;
        } else {
            return 0;
        }
    }); //在tbody中按它们的顺序把行添加到最后
    //这将自动把它们从当前位置移走，故没必要预先删除它们
    //如果＜tbody＞还包含了除了＜tr＞的任何其他元素，这些节点将会悬浮到顶部位置
    for (var i = 0; i < rows.length; i++) tbody.appendChild(rows[i]);
}
//查找表格的＜th＞元素（假设只有一行），让它们可单击，
//以便单击列标题，按该列对行排序
function makeSortable(table) {
    var headers = table.getElementsByTagName("th");
    for (var i = 0; i < headers.length; i++) {
        (function (n) { //嵌套函数来创建本地作用域
            headers[i].onclick = function () {
                sortrows(table, n);
            };
        }(i)); //将i的值赋给局部变量n
    }
}
```
**[⬆ back to top](#readme)**

