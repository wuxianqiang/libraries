# libraries

# 目录
- [仿ECMAScript5中Object.create()函数](#仿ecmascript5中objectcreate函数)
- [仿ECMAScript5中String.trim()函数](#仿ecmascript5中stringtrim函数)
- [仿ECMAScript5中Array.reduce()函数](#仿ecmascript5中arrayreduce函数)
- [在数组中查找所有出现的元素方法](#在数组中查找所有出现的元素方法)
- [数据类型检测，特殊情况特殊处理](#数据类型检测，特殊情况特殊处理)
- [仿ECMAScript5中Object.keys()函数](#仿ecmascript5中objectkeys函数)
- [仿Math.max方法（不定实参函数）](#仿mathmax方法不定实参函数)
- [仿ECMAScript5中Function.bind()函数](#仿ecmascript5中functionbind函数)
- [仿ECMAScript5中Array.map()函数](#仿ecmascript5中arraymap函数)
- [数组去重](#数组去重)
- [冒泡排序](#冒泡排序)
- [仿String.match()方法实现](#仿stringmatch方法实现)
- [返回元素的第n层祖先元素](#返回元素的第n层祖先元素)
- [返回元素的第n个兄弟元素](#返回元素的第n个兄弟元素)
- [返回元素的第n个子代元素](#返回元素的第n个子代元素)
- [表格的行排序](#表格的行排序)
- [生成目录表](#生成目录表)
- [从URL解析参数](#从url解析参数)
- [获取纯文本的元素内容](#获取纯文本的元素内容)
- [手写一个JSONP实现](#手写一个jsonp实现)
- [插入节点](#插入节点)
- [使用innerHTML实现outerHTML属性](#使用innerhtml实现outerhtml属性)
- [倒序排列子节点](#倒序排列子节点)
- [查询窗口滚动条的位置](#查询窗口滚动条的位置)
- [查询窗口的视口尺寸](#查询窗口的视口尺寸)

## 仿ECMAScript5中Object.create()函数
```js
        function inherit(obj) {
            if (obj === null) throw TypeError();
            if (Object.create) return Object.create(obj);
            var t = typeof obj;
            if (t !== "object" && t !== "function") throw TypeError();
            function Fn() {};
            Fn.prototype = obj;
            return new Fn();
        }
```

## 仿ECMAScript5中String.trim()函数
```js
        String.prototype.mytrim = function () {
            String.prototype.trim || function () {
                if (!this) return this; //空字符串不做处理
                return this.replace(/^\s+|\s+$/g, "") //使用正则表达式经行空格替换
            }
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
## 生成目录表
```js
/**
 *
 *这个模块注册一个可在页面加载完成后自动运行的匿名函数。当执行这个函数时会去文档中查找
 *id为"TOC"的元素。如果这个元素不存在，就创建一个元素
 *
 *生成的TOC目录应当具有自己的CSS样式。整个目录区域的样式className设置为"TOCEntry"
 *同样我们为不同层级的目录标题定义不同的样式。＜h1＞标签生成的标题
 *className为"TOCLevel1"，＜h2＞标签生成的标题className为"TOCLevel2"，以此类推
 *段编号的样式为"TOCSectNum"
 *
 *完整的CSS样式代码如下:
 *
 *#TOC{border:solid black 1px;margin:10px;padding:10px;}
 *.TOCEntry{font-family:sans-serif;}
 *.TOCEntry a{text-decoration:none;}
 *.TOCLevel1{font-size:16pt;font-weight:bold;}
 *.TOCLevel2{font-size:12pt;margin-left:.5in;}
 *.TOCSectNum:after{content:":";}
 *
 *这段代码的最后一行表示每个段编号之后都有一个冒号和空格符。要想隐藏段编号，
 *请使用这行代码：
 *.TOCSectNum{display:none}
 *
 **/
(function () { //匿名函数定义了一个局部作用域
    //查找TOC容器元素
    //如果不存在，则在文档开头处创建一个
    var toc = document.getElementById("TOC");
    if (!toc) {
        toc = document.createElement("div");
        toc.id = "TOC";
        document.body.insertBefore(toc, document.body.firstChild);
    }
    //查找所有的标题元素
    var headings;
    if (document.querySelectorAll) //我们是否能用这个简单的方法？
        headings = document.querySelectorAll("h1,h2,h3,h4,h5,h6");
    else //否则，查找方法稍微麻烦一些
        headings = findHeadings(document.body, []); //递归遍历document的body，查找标题元素
    function findHeadings(root, sects) {
        for (var c = root.firstChild; c != null; c = c.nextSibling) {
            if (c.nodeType !== 1) continue;
            if (c.tagName.length == 2 && c.tagName.charAt(0) == "H")
                sects.push(c);
            else
                findHeadings(c, sects);
        }
        return sects;
    }
    //初始化一个数组来保持跟踪章节号
    var sectionNumbers = [0, 0, 0, 0, 0, 0]; //现在，循环已找到的标题元素
    for (var h = 0; h < headings.length; h++) {
        var heading = headings[h]; //跳过在TOC容器中的标题元素
        if (heading.parentNode == toc) continue; //判定标题的级别
        var level = parseInt(heading.tagName.charAt(1));
        if (isNaN(level) || level < 1 || level > 6) continue; //对于该标题级别增加sectionNumbers对应的数字
        //重置所有标题比它级别低的数字为零
        sectionNumbers[level - 1]++;
        for (var i = level; i < 6; i++) sectionNumbers[i] = 0; //现在，将所有标题级别的章节号组合产生一个章节号， 如2 .3 .1
        var sectionNumber = sectionNumbers.slice(0, level).join(".") //为标题级别增加章节号
        //把数字放在＜span＞中，使得其可以用样式修饰
        var span = document.createElement("span");
        span.className = "TOCSectNum";
        span.innerHTML = sectionNumber;
        heading.insertBefore(span, heading.firstChild); //用命名的锚点将标题包起来，以便为它增加链接
        var anchor = document.createElement("a");
        anchor.name = "TOC" + sectionNumber;
        heading.parentNode.insertBefore(anchor, heading);
        anchor.appendChild(heading); //现在为该节创建一个链接
        var link = document.createElement("a");
        link.href = "#TOC" + sectionNumber; //链接的目标地址
        link.innerHTML = heading.innerHTML; //链接文本与实际标题一致
        //将链接放在一个div中，div用基于级别名字的样式修饰
        var entry = document.createElement("div");
        entry.className = "TOCEntry TOCLevel" + level;
        entry.appendChild(link); //该div添加到TOC容器中
        toc.appendChild(entry);
    }
}());
```
**[⬆ back to top](#readme)**
## 从URL解析参数

```js
/*
 *这个函数用来解析来自URL的查询串中的name=value参数对
 *它将name=value对存储在一个对象的属性中，并返回该对象
 *这样来使用它
 *
 *var args=urlArgs();//从URL中解析参数
 *var q=args.q||"";//如果参数定义了的话就使用参数；否则使用一个默认值
 *var n=args.n?parseInt(args.n):10;
 */
function urlArgs() {
    var args = {}; //定义一个空对象
    var query = location.search.substring(1); //查找到查询串，并去掉'?'
    var pairs = query.split("&"); //根据"&"符号将查询字符串分隔开
    for (var i = 0; i < pairs.length; i++) { //对于每个片段
        var pos = pairs[i].indexOf('='); //查找"name=value"
        if (pos == -1) continue; //如果没有找到的话，就跳过
        var name = pairs[i].substring(0, pos); //提取name
        var value = pairs[i].substring(pos + 1); //提取value
        value = decodeURIComponent(value); //对value进行解码
        args[name] = value; //存储为属性
    }
    return args; //返回解析后的参数
}
```
**[⬆ back to top](#readme)**

## 获取纯文本的元素内容

```js
/**
 *一个参数，返回元素的textContent或innerText
 *两个参数，用value参数的值设置元素的textContent或innerText
 */
function textContent(element, value) {
    var content = element.textContent; //检测textContent是否有定义
    if (value === undefined) { //没传递value，因此返回当前文本
        if (content !== undefined) {
            return content;
        } else {
            return element.innerText;
        }
    } else { //传递了value，因此设置文本
        if (content !== undefined) {
            element.textContent = value;
        } else {
            element.innerText = value;
        }
    }
}
```
**[⬆ back to top](#readme)**
## 手写一个JSONP实现

```js
//从指定的URL，异步加载和执行脚本
function loadasync(url) {
    var head = document.getElementsByTagName("head")[0]; //查找文档的＜head＞标签
    var s = document.createElement("script"); //创建一个＜script＞元素
    s.src = url; //设置它的src属性值
    head.appendChild(s); //将该＜script＞插入到head中
}
```
**[⬆ back to top](#readme)**
## 插入节点

```js
//将child节点插入到parent中，使其成为第n个子节点
function insertAt(parent, child, n) {
    if (n < 0 || n > parent.childNodes.length) {
        throw new Error("invalid index");
    } else if (n == parent.childNodes.length) {
        parent.appendChild(child);
    } else {
        parent.insertBefore(child, parent.childNodes[n]);
    }
}
```
**[⬆ back to top](#readme)**
## 使用innerHTML实现outerHTML属性
```js
//为那些不支持它的浏览器实现outerHTML属性
//假设浏览器确实支持innerHTML，并有个可扩展的Element.prototype，
//并且可以定义getter和setter
(function () { //如果outerHTML存在，则直接返回
    if (document.createElement("div").outerHTML) return; //返回this所引用元素的外部HTML
    function outerHTMLGetter() {
        var container = document.createElement("div"); //虚拟元素
        container.appendChild(this.cloneNode(true)); //复制到该虚拟节点
        return container.innerHTML; //返回虚拟节点的innerHTML
    }
    //用指定的值设置元素的外部HTML
    function outerHTMLSetter(value) { //创建一个虚拟元素，设置其内容为指定的值
        var container = document.createElement("div");
        container.innerHTML = value; //将虚拟元素中的节点全部移动到文档中
        while (container.firstChild) //循环，直到container没有子节点为止
            this.parentNode.insertBefore(container.firstChild, this); //删除所被取代的节点
        this.parentNode.removeChild(this);
    }
    //现在使用这两个函数作为所有Element对象的outerHTML属性的getter和setter
    //如果它存在则使用ES5的Object.defineProperty()方法，
    //否则，退而求其次，使用__defineGetter__()和__defineSetter__()
    if (Object.defineProperty) {
        Object.defineProperty(Element.prototype, "outerHTML", {
            get: outerHTMLGetter,
            set: outerHTMLSetter,
            enumerable: false,
            configurable: true
        });
    } else {
        Element.prototype.__defineGetter__("outerHTML", outerHTMLGetter);
        Element.prototype.__defineSetter__("outerHTML", outerHTMLSetter);
    }
}());
```
**[⬆ back to top](#readme)**
## 倒序排列子节点
```js
//倒序排列节点n的子节点
function reverse(n) { //创建一个DocumentFragment作为临时容器
    var f = document.createDocumentFragment(); //从后至前循环子节点，将每一个子节点移动到文档片段中
    //n的最后一个节点变成f的第一个节点，反之亦然
    //注意，给f添加一个节点，该节点自动地会从n中删除
    while (n.lastChild) f.appendChild(n.lastChild); //最后，把f的所有子节点一次性全部移回n中
    n.appendChild(f);
}
```
**[⬆ back to top](#readme)**
## 查询窗口滚动条的位置
```js
//以一个对象的x和y属性的方式返回滚动条的偏移量
function getScrollOffsets(w) { //使用指定的窗口，如果不带参数则使用当前窗口
    w = w || window; //除了IE 8及更早的版本以外，其他浏览器都能用
    if (w.pageXOffset != null) return {
        x: w.pageXOffset,
        y: w.pageYOffset
    }; //对标准模式下的IE（或任何浏览器）
    var d = w.document;
    if (document.compatMode == "CSS1Compat")
        return {
            x: d.documentElement.scrollLeft,
            y: d.documentElement.scrollTop
        }; //对怪异模式下的浏览器
    return {
        x: d.body.scrollLeft,
        y: d.body.scrollTop
    };
}
```
**[⬆ back to top](#readme)**
## 查询窗口的视口尺寸
```js
//作为一个对象的w和h属性返回视口的尺寸
function getViewportSize(w) { //使用指定的窗口，如果不带参数则使用当前窗口
    w = w || window; //除了IE 8及更早的版本以外，其他浏览器都能用
    if (w.innerWidth != null) return {
        w: w.innerWidth,
        h: w.innerHeight
    }; //对标准模式下的IE（或任何浏览器）
    var d = w.document;
    if (document.compatMode == "CSS1Compat")
        return {
            w: d.documentElement.clientWidth,
            h: d.documentElement.clientHeight
        }; //对怪异模式下的浏览器
    return {
        w: d.body.clientWidth,
        h: d.body.clientWidth
    };
}
```
