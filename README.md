# libraries

## jQuery源码解读

1. selector: [string]选择器的类型,[object]JS原生对象（把原生的对象转换为jQuery对象），[function]回掉函数（等级于$(document).ready...）
2. context: 默认document(如果传递的是一个JS原生对象，更够修改上下文，如果是一个jQuery对象，默认会重构为选择器)
3. jQuery选择器是创建一个jQuery的一个实例，所以$(#box)===$(#box)返回false，所以每次获取选择器最好用变量存储起来。
4. $.ajax(),把jQuery作为普通函数执行，使用它自己属性上提供的一写方法

```js
var jQuery = function (selector, context) {
    return new jQuery.fn.init(selector, context);
    // $()都是创建这个类的实例
}
```
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
## 在数组ary中查找所有出现的元素ele，并返回一个包含匹配索引的数组

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

## 获取对象obj的类，并且这个函数包含了对null和undefined的特殊处理，（在ECMAScript5中不要对这些情况做处理）

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

## 代码

```js
        //数组去重
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
        //冒泡排序
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
        //match方法
        String.prototype.mymatch = function (reg) {
         var ary = [];
         var res = reg.exec(this);
         while (res) {
            ary.push(res[0]);
            res = reg.exec(this);
         }
         return ary;
        }
        //原型练习题
        function Fn(num) {
            this.x = this.y = num;
        }
        Fn.prototype = {
            x: 20,
            sum: function () {
                console.log(this.x + this.y);
            }
        }

        var f = new Fn(10);
        console.log(f.sum === Fn.prototype.sum); // ture
        f.sum(); //10+10=20
        Fn.prototype.sum(); //20+undefined=NaN
        console.log(f.constructor); //Object
        // 初始化数组
        var ary = [];
        for(var i = 0; i < 6; ary[i++] = i); /* empty */
        console.log(ary); // => [ 1, 2, 3, 4, 5, 6 ]
        
        //仿ECMAScript5定义的Object.keys()方法
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
