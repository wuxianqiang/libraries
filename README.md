# libraries

# 目录
- [仿ECMAScript5中Object.create()函数](#仿ecmascript5中objectcreate函数)
- [仿ECMAScript5中String.trim()函数](#仿ecmascript5中stringtrim函数)
- [仿ECMAScript5中Array.reduce()函数](#仿ecmascript5中arrayreduce函数)
- [仿ECMAScript5中Object.keys()函数](#仿ecmascript5中objectkeys函数)
- [仿ECMAScript5中Function.bind()函数](#仿ecmascript5中functionbind函数)
- [仿ECMAScript5中Array.map()函数](#仿ecmascript5中arraymap函数)
- [仿Math.max()方法实现](#仿mathmax方法不定实参函数)
- [仿String.match()方法实现](#仿stringmatch方法实现)
- [仿HTML5的classList属性实现](#仿html5的classlist属性)
- [仿Function.name属性实现](#返回函数的名字)
- [返回元素的第n层祖先元素](#返回元素的第n层祖先元素)
- [返回元素的第n个兄弟元素](#返回元素的第n个兄弟元素)
- [返回元素的第n个子代元素](#返回元素的第n个子代元素)
- [原生JS实现CSS动画之震动](#原生js实现css动画1)
- [原生JS实现CSS动画之隐藏](#原生js实现css动画2)
- [在数组中查找所有出现的元素方法](#在数组中查找所有出现的元素方法)
- [数据类型检测之特殊情况特殊处理](#数据类型检测特殊情况特殊处理)
- [使用innerHTML实现outerHTML属性](#使用innerhtml实现outerhtml属性)
- [插入节点](#插入节点)
- [倒序排列子节点](#倒序排列子节点)
- [查询窗口滚动条的位置](#查询窗口滚动条的位置)
- [查询窗口的视口尺寸](#查询窗口的视口尺寸)
- [表格的行排序](#表格的行排序)
- [生成目录表](#生成目录表)
- [数组去重](#数组去重)
- [冒泡排序](#冒泡排序)
- [从URL解析参数](#从url解析参数)
- [获取纯文本的元素内容](#获取纯文本的元素内容)
- [手写一个JSONP实现](#手写一个jsonp实现)
- [查询纯文本形式的内容](#查询纯文本形式的内容)
- [查找元素的后代中节点中的所有Text节点](#查找元素的后代中节点中的所有text节点)
- [使用innerHTML实现insertAdjacentHTML](#使用innerhtml实现insertadjacenthtml)
- [拖拽](#拖拽)
- [在谷歌地图上显示地理位置信息](#在谷歌地图上显示地理位置信息)
- [使用所有地理位置特性](#使用所有地理位置特性)
- [优雅的图片翻转实现](#优雅的图片翻转实现)
- [使用canvas绘制多边形](#使用canvas绘制多边形)
- [使用canvas绘制雪花](#使用canvas绘制雪花)
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
//根据指定的URL发送一个JSONP请求
//然后把解析得到的响应数据传递给回调函数
//在URL中添加一个名为jsonp的查询参数，用于指定该请求的回调函数的名称
function getJSONP(url, callback) { //为本次请求创建一个唯一的回调函数名称
    var cbnum = "cb" + getJSONP.counter++; //每次自增计数器
    var cbname = "getJSONP." + cbnum; //作为JSONP函数的属性
    //将回调函数名称以表单编码的形式添加到URL的查询部分中
    //使用jsonp作为参数名，一些支持JSONP的服务
    //可能使用其他的参数名，比如callback
    if (url.indexOf("?") === -1) //URL没有查询部分
        url += "?jsonp=" + cbname; //作为查询部分添加参数
    else //否则
        url += "＆jsonp=" + cbname; //作为新的参数添加它
    //创建script元素用于发送请求
    var script = document.createElement("script"); //定义将被脚本执行的回调函数
    getJSONP[cbnum] = function (response) {
        try {
            callback(response); //处理响应数据
        } finally { //即使回调函数或响应抛出错误
            delete getJSONP[cbnum]; //删除该函数
            script.parentNode.removeChild(script); //移除script元素
        }
    }; //立即触发HTTP请求
    script.src = url; //设置脚本的URL
    document.body.appendChild(script); //把它添加到文档中
}
getJSONP.counter = 0; //用于创建唯一回调函数名称的计数器
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
**[⬆ back to top](#readme)**
## 返回函数的名字
```js
Function.prototype.getName = function () {
    return this.name || this.toString().match(/function\s*(\w*)\s*\(/)[1];
}
```
**[⬆ back to top](#readme)**
## 原生JS实现CSS动画1
```js
//将e转化为相对定位的元素，使之左右"震动"
//第一个参数可以是元素对象或者元素的id
//如果第二个参数是函数，以e为参数，它将在动画结束时调用
//第三个参数指定e震动的距离，默认是5像素
//第四个参数指定震动多久，默认是500毫秒
function shake(e, oncomplete, distance, time) { //句柄参数
    if (typeof e === "string") e = document.getElementById(e);
    if (!time) time = 500;
    if (!distance) distance = 5;
    var originalStyle = e.style.cssText; //保存e的原始style
    e.style.position = "relative"; //使e相对定位
    var start = (new Date()).getTime(); //注意，动画的开始时间
    animate(); //动画开始
    //函数检查消耗的时间，并更新e的位置
    //如果动画完成，它将e还原为原始状态
    //否则，它更新e的位置，安排它自身重新运行
    function animate() {
        var now = (new Date()).getTime(); //得到当前时间
        var elapsed = now - start; //从开始以来消耗了多长时间？
        var fraction = elapsed / time; //是总时间的几分之几？
        if (fraction < 1) { //如果动画未完成
            //作为动画完成比例的函数，计算e的x位置
            //使用正弦函数将完成比例乘以4pi
            //所以，它来回往复两次
            var x = distance * Math.sin(fraction * 4 * Math.PI);
            e.style.left = x + "px"; //在25毫秒后或在总时间的最后尝试再次运行函数
            //目的是为了产生每秒40帧的动画
            setTimeout(animate, Math.min(25, time - elapsed));
        } else { //否则，动画完成
            e.style.cssText = originalStyle //恢复原始样式
            if (oncomplete) oncomplete(e); //调用完成后的回调函数
        }
    }
}
```
**[⬆ back to top](#readme)**
## 原生JS实现CSS动画2
```js
function fadeOut(e, oncomplete, time) {
    if (typeof e === "string") e = document.getElementById(e);
    if (!time) time = 500; //使用Math.sqrt作为一个简单的“缓动函数”来创建动画
    //精巧的非线性：一开始淡出得比较快，然后缓慢了一些
    var ease = Math.sqrt;
    var start = (new Date()).getTime(); //注意：动画开始的时间
    animate(); //动画开始
    function animate() {
        var elapsed = (new Date()).getTime() - start; //消耗的时间
        var fraction = elapsed / time; //总时间的几分之几？
        if (fraction < 1) { //如果动画未完成
            var opacity = 1 - ease(fraction); //计算元素的不透明度
            e.style.opacity = String(opacity); //设置在e上
            setTimeout(animate, //调度下一帧
                Math.min(25, time - elapsed));
        } else { //否则，动画完成
            e.style.opacity = "0"; //使e完全透明
            if (oncomplete) oncomplete(e); //调用完成后的回调函数
        }
    }
}
```
**[⬆ back to top](#readme)**
## 仿HTML5的classList属性
```js
/*
 *如果e有classList属性则返回它。否则，返回一个为e模拟DOMTokenList API的对象
 *返回的对象有contains()、add()、remove()、toggle()和toString()等方法
 *来检测和修改元素e的类集合。如果classList属性是原生支持的，
 *返回的类数组对象有length和数组索引属性。模拟DOMTokenList不是类数组对象，
 *但是它有一个toArray()方法来返回一个含元素类名的纯数组快照
 */
function classList(e) {
    if (e.classList) return e.classList; //如果e.classList存在，则返回它
    else return new CSSClassList(e); //否则，就伪造一个
}
//CSSClassList是一个模拟DOMTokenList的JavaScript类
function CSSClassList(e) {
    this.e = e;
} //如果e.className包含类名c则返回true否则返回false
CSSClassList.prototype.contains = function (c) { //检查c是否是合法的类名
    if (c.length === 0 || c.indexOf(" ") != -1)
        throw new Error("Invalid class name:'" + c + "'"); //首先是常规检查
    var classes = this.e.className;
    if (!classes) return false; //e不含类名
    if (classes === c) return true; //e有一个完全匹配的类名
    //否则，把c自身看做一个单词，利用正则表达式搜索c
    //\b在正则表达式里代表单词的边界
    return classes.search("\\b" + c + "\\b") != -1;
}; //如果c不存在，将c添加到e.className中
CSSClassList.prototype.add = function (c) {
    if (this.contains(c)) return; //如果存在，什么都不做
    var classes = this.e.className;
    if (classes && classes[classes.length - 1] != "")
        c = "" + c; //如果需要加一个空格
    this.e.className += c; //将c添加到className中
}; //将在e.className中出现的所有c都删除
CSSClassList.prototype.remove = function (c) { //检查c是否是合法的类名
    if (c.length === 0 || c.indexOf(" ") != -1)
        throw new Error("Invalid class name:'" + c + "'"); //将所有作为单词的c和多余的尾随空格全部删除
    var pattern = new RegExp("\\b" + c + "\\b\\s*", "g");
    this.e.className = this.e.className.replace(pattern, "");
}; //如果c不存在，将c添加到e.className中，并返回true
//否则，将在e.className中出现的所有c都删除，并返回false
CSSClassList.prototype.toggle = function (c) {
    if (this.contains(c)) { //如果e.className包含c
        this.remove(c); //删除它
        return false;
    } else { //否则
        this.add(c); //添加它
        return true;
    }
}; //返回e.className本身
CSSClassList.prototype.toString = function () {
    return this.e.className;
}; //返回在e.className中的类名
CSSClassList.prototype.toArray = function () {
    return this.e.className.match(/\b\w+\b/g) || [];
};
```
**[⬆ back to top](#readme)**
## 查询纯文本形式的内容
```js
/**
 *一个参数，返回元素的textContent或innerText
 *两个参数，用value参数的值设置元素的textContent或innerText
 */
function textContent(element, value) {
    var content = element.textContent; //检测textContent是否有定义
    if (value === undefined) { //没传递value，因此返回当前文本
        if (content !== undefined) return content;
        else return element.innerText;
    } else { //传递了value，因此设置文本
        if (content !== undefined) element.textContent = value;
        else element.innerText = value;
    }
}
```
textContent属性在除了IE的所有当前的浏览器中都支持。在IE中，可以用Element的innerText属性来代替。
**[⬆ back to top](#readme)**
## 查找元素的后代中节点中的所有Text节点
```js
//返回元素e的纯文本内容，递归进入其子元素
//该方法的效果类似于textContent属性
function textContent(e) {
    var child, type, s = ""; //s保存所有子节点的文本
    for (child = e.firstChild; child != null; child = child.nextSibling) {
        type = child.nodeType;
        if (type === 3 || type === 4) //Text和CDATASection节点
            s += child.nodeValue;
        else if (type === 1) //递归Element节点
            s += textContent(child);
    }
    return s;
}
```
**[⬆ back to top](#readme)**
## 使用innerHTML实现insertAdjacentHTML()
```js
//本模块为不支持它的浏览器定义了Element.insertAdjacentHTML
//还定义了一些可移植的HTML插入函数，它们的名字比insertAdjacentHTML更符合逻辑：
//Insert.before()、Insert.after()、Insert.atStart()和Insert.atEnd()
var Insert = (function () { //如果元素有原生的insertAdjacentHTML，
    //在4个函数名更明了的HTML插入函数中使用它
    if (document.createElement("div").insertAdjacentHTML) {
        return {
            before: function (e, h) {
                e.insertAdjacentHTML("beforebegin", h);
            },
            after: function (e, h) {
                e.insertAdjacentHTML("afterend", h);
            },
            atStart: function (e, h) {
                e.insertAdjacentHTML("afterbegin", h);
            },
            atEnd: function (e, h) {
                e.insertAdjacentHTML("beforeend", h);
            }
        };
    }
    //否则，无原生的insertAdjacentHTML
    //实现同样的4个插入函数，并使用它们来定义insertAdjacentHTML
    //首先，定义一个工具函数，传入HTML字符串，返回一个DocumentFragment，
    //它包含了解析后的HTML的表示
    function fragment(html) {
        var elt = document.createElement("div"); //创建空元素
        var frag = document.createDocumentFragment(); //创建空文档片段
        elt.innerHTML = html; //设置元素内容
        while (elt.firstChild) //移动所有的节点
            frag.appendChild(elt.firstChild); //从elt到frag
        return frag; //然后返回frag
    }
    var Insert = {
        before: function (elt, html) {
            elt.parentNode.insertBefore(fragment(html), elt);
        },
        after: function (elt, html) {
            elt.parentNode.insertBefore(fragment(html), elt.nextSibling);
        },
        atStart: function (elt, html) {
            elt.insertBefore(fragment(html), elt.firstChild);
        },
        atEnd: function (elt, html) {
            elt.appendChild(fragment(html));
        }
    }; //基于以上函数实现insertAdjacentHTML
    Element.prototype.insertAdjacentHTML = function (pos, html) {
        switch (pos.toLowerCase()) {
            case "beforebegin":
                return Insert.before(this, html);
            case "afterend":
                return Insert.after(this, html);
            case "afterbegin":
                return Insert.atStart(this, html);
            case "beforeend":
                return Insert.atEnd(this, html);
        }
    };
    return Insert; //最后返回4个插入函数
}());
```

**[⬆ back to top](#readme)**
## 拖拽
```js
/**
 *Drag.js：拖动绝对定位的HTML元素
 *
 *这个模块定义了一个drag()函数，它用于mousedown事件处理程序的调用
 *随后的mousemove事件将移动指定元素，mouseup事件将终止拖动
 *这些实现能同标准和IE两种事件模型一起工作
 *
 *参数：
 *
 *elementToDrag：接收mousedown事件的元素或某些包含元素
 *它必须是定位的元素,元素的样式必须是行内样式
 *它的style.left和style.top值将随着用户的拖动而改变
 *
 *event：mousedown事件对象
 **/
function drag(elementToDrag, event) { //初始鼠标位置，转换为文档坐标
    var startX = event.clientX;
    var startY = event.clientY; //在文档坐标下，待拖动元素的初始位置
    //因为elementToDrag是绝对定位的，
    //所以我们可以假设它的offsetParent就是文档的body元素
    var origX = parseFloat(elementToDrag.style.left);
    var origY = parseFloat(elementToDrag.style.top); //计算mousedown事件和元素左上角之间的距离
    //我们将它另存为鼠标移动的距离
    if (document.addEventListener) { //标准事件模型
        //在document对象上注册捕获事件处理程序
        document.addEventListener("mousemove", moveHandler, true);
        document.addEventListener("mouseup", upHandler, true);
    } else if (document.attachEvent) { //用于IE5～8的IE事件模型
        //在IE事件模型中，
        //捕获事件是通过调用元素上的setCapture()捕获它们
        elementToDrag.setCapture();
        elementToDrag.attachEvent("onmousemove", moveHandler);
        elementToDrag.attachEvent("onmouseup", upHandler); //作为mouseup事件看待鼠标捕获的丢失
        elementToDrag.attachEvent("onlosecapture", upHandler);
    }
    //我们处理了这个事件，不让任何其他元素看到它
    if (event.stopPropagation) event.stopPropagation(); //标准模型
    else event.cancelBubble = true; //IE
    //现在阻止任何默认操作
    if (event.preventDefault) event.preventDefault(); //标准模型
    else event.returnValue = false; //IE
    /**
     * 当元素正在被拖动时， 这就是捕获mousemove事件的处理程序
     *它用于移动这个元素 
     **/
    function moveHandler(e) {
        if (!e) e = window.event; //IE事件模型
        //移动这个元素到当前鼠标位置，
        //通过滚动条的位置和初始单击的偏移量来调整
        var targetLeft = e.clientX - startX + origX;
        var targetTop = e.clientY - startY + origY;
        var minLeft = 0;
        var minTop = 0;
        var maxLeft = (document.documentElement.clientWidth || document.body.clientWidth) - elementToDrag.offsetWidth;
        var maxTop = (document.documentElement.clientHeight || document.body.clientHeight) - elementToDrag.offsetHeight;
        targetLeft = targetLeft > maxLeft ? maxLeft : (targetLeft < minLeft ? minLeft : targetLeft);
        targetTop = targetTop > maxTop ? maxTop : (targetTop < minTop ? minTop : targetTop);
        elementToDrag.style.left = targetLeft + "px";
        elementToDrag.style.top = targetTop + "px";
        if (e.stopPropagation) e.stopPropagation(); //标准
        else e.cancelBubble = true; //IE
    }
    /**
     *这是捕获在拖动结束时发生的最终mouseup事件的处理程序
     **/
    function upHandler(e) {
        if (!e) e = window.event; //IE事件模型
        //注销捕获事件处理程序
        if (document.removeEventListener) { //DOM事件模型
            document.removeEventListener("mouseup", upHandler, true);
            document.removeEventListener("mousemove", moveHandler, true);
        } else if (document.detachEvent) { //IE 5+事件模型
            elementToDrag.detachEvent("onlosecapture", upHandler);
            elementToDrag.detachEvent("onmouseup", upHandler);
            elementToDrag.detachEvent("onmousemove", moveHandler);
            elementToDrag.releaseCapture();
        }
        //并且不让事件进一步传播
        if (e.stopPropagation) e.stopPropagation(); //标准模型
        else e.cancelBubble = true; //IE
    }
}
```
**[⬆ back to top](#readme)**
## 在谷歌地图上显示地理位置信息
```js
//获取当前位置然后通过Google地图显示
//如果当前浏览器不支持地理位置API，则抛出一个错误
function getmap() { //检查是否支持地理位置API
    if (!navigator.geolocation) throw "Geolocation not supported"; //开始请求地理位置信息，
    navigator.geolocation.getCurrentPosition(setMapURL);
    function setMapURL(pos) { //从参数对象（pos）中获取位置信息
        var latitude = pos.coords.latitude; //经度
        var longitude = pos.coords.longitude; //纬度
        var accuracy = pos.coords.accuracy; //米
        var scale = 10; //比例
        //构造一个URL，用于跳转到Google地图
        var url = "https://www.google.com/maps/@" + latitude + "," + longitude + "," + scale + "z"; //设置一个大致的缩放级别
        location = url;
    }
}
```
**[⬆ back to top](#readme)**
## 使用所有地理位置特性
```js
//异步的获取我的位置，并在指定的元素中展示出来
function whereami(elt) { //将此对象作为第三个参数传递给getCurrentPosition()方法
    var options = { //设置为true，表示如果可以的话
        //获取高精度的位置信息（例如，通过GPS获取）
        //但是，要注意的是，这会影响电池寿命
        enableHighAccuracy: false, //可以近似：这是默认值
        //如果获取缓存过的位置信息就足够的话，可以设置此属性
        //默认值为0,表示强制检查新的位置信息
        maximumAge: 300000, //5分钟左后
        //愿意等待多长时间来获取位置信息？
        //默认值为无限长 [2] ，getCurrentPosition()方法永不超时
        timeout: 15000 //不要超过15秒
    };
    if (navigator.geolocation) //如果支持的话，就获取位置信息
        navigator.geolocation.getCurrentPosition(success, error, options);
    else
        elt.innerHTMl = "Geolocation not supported in this browser"; //当获取位置信息失败的时候，会调用此函数

    function error(e) { //error对象包含一些数字编码和文本消息，如下所示：
        //1:用户不允许分享他/她的位置信息
        //2:浏览器无法确定位置
        //3:发生超时
        elt.innerHTML = "Geolocation error" + e.code + ":" + e.message;
    }
    //当获取位置信息成功的时候，会调用此函数
    function success(pos) { //总是可以获取如下这些字段
        //但是要注意的是时间戳信息在outer对象中，而不在inner、coords对象中
        var msg = "时间是" +
            new Date(pos.timestamp).toLocaleString() + "地理位置是" +
            pos.coords.accuracy + "米范围内经度是" +
            pos.coords.latitude + "纬度是" +
            pos.coords.longitude + "."; //如果设备还返回了海拔信息，则将其添加进去
        if (pos.coords.altitude) {
            msg += "海拔是" + pos.coords.altitude + "±" +
                pos.coords.altitudeAccuracy + "千米.";
        }
        //如果设备还返回了速度和航向信息，也将它们添加进去
        if (pos.coords.speed) {
            msg += "速度是" +
                pos.coords.speed + "m/s方向是" +
                pos.coords.heading + ".";
        }
        elt.innerHTML = msg; //显示所有的位置信息
    }
}
```
**[⬆ back to top](#readme)**
## 优雅的图片翻转实现
```js
/**
 *优雅的图片翻转实现方式
 *
 *要创建图片翻转效果，将此模块引入到HTML文件中
 *然后在任意＜img＞元素上使用data-rollover属性来指定翻转图片的URL即可
 *如下所示:
 *
 *<img src="normal_image.png "data-rollover="rollover_image.png">
 *
 */
function changeImage() { //所有处理逻辑都在一个匿名函数中:不定义任何符号
    //遍历所有的图片，查找data-rollover属性
    for (var i = 0; i < document.images.length; i++) {
        var img = document.images[i];
        var rollover = img.getAttribute("data-rollover");
        if (!rollover) continue; //跳过没有data-rollover属性的图片
        //确保将翻转的图片缓存起来
        (new Image()).src = rollover; //定义一个属性来标识默认的图片URL
        img.setAttribute("data-rollout", img.src); //注册事件处理函数来创建翻转效果
        img.onmouseover = function () {
            this.src = this.getAttribute("data-rollover");
        };
        img.onmouseout = function () {
            this.src = this.getAttribute("data-rollout");
        };
    }
}
```
**[⬆ back to top](#readme)**
## 使用canvas绘制多边形
```js
//定义一个以(x,y)为中心，半径为r的规则n边形,c可以通过调用画布getContext()方法得到
//每个顶点都是均匀分布在圆周上
//将第一个顶点放置在最上面，或者指定一定角度
//除非最后一个参数是true，否则顺时针旋转
function polygon(c, n, x, y, r, angle, counterclockwise) {
    angle = angle || 0;
    counterclockwise = counterclockwise || false;
    c.moveTo(x + r * Math.sin(angle), //从第一个顶点开始一条新的子路径
        y - r * Math.cos(angle)); //使用三角法计算位置
    var delta = 2 * Math.PI / n; //两个顶点之间的夹角
    for (var i = 1; i < n; i++) { //循环剩余的每个顶点
        angle += counterclockwise ? -delta : delta; //调整角度
        c.lineTo(x + r * Math.sin(angle), //以下个顶点为端点添加线段
            y - r * Math.cos(angle));
    }
    c.closePath(); //将最后一个顶点和起点连接起来
}
```
**[⬆ back to top](#readme)**
## 使用canvas绘制雪花
```js
var deg = Math.PI / 180; //用于角度制到弧度制的转换
//在画布的上下文c中，以左下角的点(x,y)和边长len，绘制一个n级别的科赫雪花分形
function snowflake(c, n, x, y, len) {
    c.save(); //保存当前变换
    c.translate(x, y); //变换原点为起始点
    c.moveTo(0, 0); //从新的原点开始一条新的子路径
    leg(n); //绘制雪花的第一条边
    c.rotate(-120 * deg); //现在沿着逆时针方向旋转120 o
    leg(n); //绘制第二条边
    c.rotate(-120 * deg); //再次旋转
    leg(n); //画最后一条边
    c.closePath(); //闭合子路径
    c.restore(); //恢复初始的变换
    //绘制n级别的科赫雪花的一条边
    //此函数在画完一条边的时候就离开当前点，
    //然后通过坐标系变换将当前点又转换成(0,0,)
    //这意味着画完一条边之后可以很简单地调用rotate()进行旋转
    function leg(n) {
        c.save(); //保存当前坐标系变换
        if (n == 0) { //不需要递归的情况下:
            c.lineTo(len, 0); //就绘制一条水平线段
        } else { //递归情况下：绘制4条子边，类似这个样子： - \/ -
            c.scale(1 / 3, 1 / 3); //子边长度为原边长的1/3
            leg(n - 1); //递归第一条子边
            c.rotate(60 * deg); //顺时针旋转60 o
            leg(n - 1); //第二条子边
            c.rotate(-120 * deg); //逆时针旋转120 o
            leg(n - 1); //第三条子边
            c.rotate(60 * deg); //通过旋转回到初始状态
            leg(n - 1); //最后一条边
        }
        c.restore(); //恢复坐标系变换
        c.translate(len, 0); //但是通过转换使得边的结束点为(0,0)
    }
}
```
**[⬆ back to top](#readme)**
