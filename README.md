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

## 数据类型检测，特殊情况处理特殊处理

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

## ECMAScript3版本的Function.bind()方法

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

## ECMAScript3版本中实现Array.map()方法

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

## 仿match方法

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

