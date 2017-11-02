# libraries

# 目录

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

## 数据类型检测，特殊情况特殊处理

```js
        function classOf(obj) {
            if (obj === null) return "Null";
            if (obj === undefined) return 'Undefined';
            return Object.prototype.toString.call(obj).slice(8, -1);
        }
```


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
