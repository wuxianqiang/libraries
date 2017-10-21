# libraries
## 方法库
1. 原生JS实现简易动画库 [仿jQuery实现动画的方法](https://github.com/wuxianqiang/libraries/blob/animate/animate/animate.js)
2. 原生JS实现DOM操作方法库  [仿jQuery操作DOM的方法](https://github.com/wuxianqiang/libraries/blob/dom/dom/dom.js)
3. 原生JS实现处理DOM2事件库兼容库 [仿jQuery绑定事件的方法](https://github.com/wuxianqiang/libraries/blob/event/event/event.js)

## jQuery源码解读
1. selector: [string]选择器的类型,[object]JS原生对象（把原生的对象转换为jQuery对象），[function]回掉函数（等级于$(document).ready...）
2. context: 默认document(如果传递的是一个JS原生对象，更够修改上下文，如果是一个jQuery对象，默认会重构为选择器)
3. jQuery选择器是创建一个jQuery的一个实例，所以$(#box)===$(#box)返回false，所以每次获取选择器最好用变量存储起来。
4. $.ajax(),把jQuery作为普通函数执行，使用它在原型上提供的一写方法

```js
var jQuery = function (selector, context) {
    return new jQuery.fn.init(selector, context);
    // $()都是创建这个类的实例
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
```
