# Interview-questions
· Day1  js

1.使用发布订阅模式，实现vue得$on和$emit、$off 方法


const eventList = {}
 
判断当前事件名称是否存在，如果不存在则创建一个key值为事件名称 value为一个数组
将callback push到数组中
如果事件名称存在则直接添加到数组中去

const $on = (eventName,callback)=>{
    if(!eventList[eventName]){
        eventList[eventName] = [];
    }
    eventList[eventName].push(callback)
}
 
 
 
判断当前事件名称是否存在，如果存在则遍历数组，得到所有的函数，并执行。然后将params当做实参传递到函数中去


const $emit = (eventName,params)=>{
    if(eventList[eventName]){
        let arr = eventList[eventName];
        arr.map((cb)=>{
            cb(params);
        })
    }
 
}

判断当前事件名称是否存在，如果存在继续判断第二个参数是否存在，如果存在则找到相对于的下标 然后将函数在数组中移除
如果不存在则将整个数组清空

const $off = (eventName,callback)=>{
    if(eventList[eventName]){
        if(callback){
            let index = eventList[eventName].indexOf(callback);
            eventList[eventName].splice(index,1);
        }else{
            eventList[eventName].length = 0;
        }
    }
}
 
// 测试
 
function fn1(val){
    console.log(111,val)
}
 
function fn2(val){
    console.log(222,val)
}
 
function fn3(val) { 
    console.log(333,val)
 }
 
 
 
 $on("handle",fn1)
 $on("handle",fn2)
 $on("handle",fn3)
 $off("handle",fn1)
 $emit("handle","abc")



2.实现instanceOf方法

function _instanceof(left, right) {

    //如果是基本数据类型，返回false  
    // typeof null 是object ,要处理这种情况 
    if (typeof (left) !== "object" || left == undefined) {
        return false;
    }

    // 获取原型对象
    // 举个例子:Object.getPrototypeOf([])===[].__proto__  true
    let prototype = Object.getPrototypeOf(left);

    while (true) {

        //一直按着原型链查找  找到顶层还找不到，返回null

        if (prototype === null) {
            return false;
        }

        // 如果left right 原型对象一样 返回true
        if (prototype == right.prototype) {
            return true;
        }

        // 不满足条件的  继续查找
        //一直到   Object.getPrototypeOf(Object.getPrototypeOf({})) null
        prototype = Object.getPrototypeOf(prototype);


    }

}






3.红绿灯问题，绿灯3秒，红灯2秒，黄灯1秒，每隔一秒打印一条记录 这样循环，要求：可以在控制台可以运行的代码，并且正确输出
// 绿灯 3
// 绿灯 2
// 绿灯 1
// 红灯 2
// 红灯 1
// 黄灯 1
// 绿灯 3

// function red() {
//     console.log('red')
// }

// function green() {
//     console.log('green')
// }

// function yellow() {
//     console.log('yellow')
// }

// function genPromise(func, timeout) {
//     return () => {
//         func();
//         return new Promise((resolve) => setTimeout(resolve, timeout))
//     }
// }

// var redPromise = genPromise(red, 2000),
//     greenPromise = genPromise(green, 3000),
//     yellowPromise = genPromise(yellow, 1000);

// function step() {
//     greenPromise().then(() => yellowPromise()).then(() => redPromise()).then(() => step())
// }

// // 启动
// step();


· Day2  js

1.请封装一个数组reduce方法

let arr = [10, 11,12,13,14]

console.log(arr.reduce((per , cur , index , arr)=>{
    console.warn(per , cur , index , arr)
    return per + cur
} , 0)

//60



2.请使用冒泡和快速两种方式实现数组排序
冒泡排序：
　　随便从数组中拿一位数和后一位比较，如果是想从小到大排序，那么就把小的那一位放到前面，大的放在后面，简单来说就是交换它们的位置，如此反复的交换位置就可以得到排序的效果。
function sortA(arr){
    for(var i=0;i<arr.length-1;i++){
        for(var j=i+1;j<arr.length;j++){
                      //获取第一个值和后一个值比较
            var cur = arr[i];
            if(cur>arr[j]){
                      // 因为需要交换值，所以会把后一个值替换，我们要先保存下来
                var index = arr[j];
                        // 交换值
                arr[j] = cur;
                arr[i] = index;
            }
        }
    }
    return arr;
}

快速排序：
　　　从数组的中间拿一个值，然后通过这个值挨个和数组里面的值进行比较，如果大于的放一边，小于的放一边，然后把这些合并，再进行比较，如此反复即可。
function sortA(arr){
    // 如果只有一位，就没有必要比较
    if(arr.length<=1){
        return arr;
    }
    // 获取中间值的索引
    var len = Math.floor(arr.length/2);
    // 截取中间值
    var cur = arr.splice(len,1);
    // 小于中间值放这里面
    var left = [];
    // 大于的放着里面
    var right = [];
    for(var i=0;i<arr.length;i++){
        // 判断是否大于
        if(cur>arr[i]){
            left.push(arr[i]);
        }else{
            right.push(arr[i]);
        }
    }
    // 通过递归，上一轮比较好的数组合并，并且再次进行比较。
    return sortA(left).concat(cur,sortA(right));
}






3.封装Storage对象
Storage.set('name', 哈哈哈') // 设置 name 字段存储的值为'哈哈哈'。
Storage.set('age', 2, 30);
Storage.set('people', ['Oli', 'Aman', 'Dante'],  60)
Storage.get('name')    // ‘前端一万小时’
Storage.get('age')     //  如果不超过 30 秒，返回数字类型的 2；如果超过 30 秒，返回 undefined，并且 localStorage 里清除 age 字段。
Storage.get('people')  // 如果不超过 60 秒，返回数组； 如果超过 60 秒，返回 undefined。








4.可以将数字转换成中文大写的表示，处理到万级别，例如 toChineseNum(12345)，返回 一万二千三百四十五

Const toChineseNum=(num)={
const unit = ['', '十', '百', '千']
const counts = ['零', '一', '二', '三', '四', '五', '六', '七', '八', '九']
const pre = Math.floor(num / 10000)
const next = num % 10000
let getfour = (mynum, flag = false) => {
 if(!mynum){return ''}
let i = 0, str = ''
 while(flag ? i < 4 : mynum > 0 ) {
count = mynum % 10
mynum = Math.floor(mynum / 10)
str = (count ? counts[count] + unit[i] : str[0] == '零' ? '' : str.length && i ? '零' : '') + str
i++
}
return str
}
 return pre ? getfour(pre) + '万' + getfour(next, true) : getfour(num)
}

}







·Day3  js

1.接受一个仅包含数字的 多维数组 ，返回一个迭代器，可以遍历得到它拍平以后的结果
const numbers = flatten2([1, [[2], 3, 4], 5]) 
cosnt res = 	flatten2.flat(infinity)
numbers.next().value // => 1 
numbers.next().value // => 2 
numbers.next().value // => 3 
numbers.next().value // => 4 
numbers.next().value // => 5

补齐flatten2 这个函数

2.复习数组方法 封装实现 数组 map、filter、every 方法

//map
Array.prototype.myMap= function(cb, context) {
    let newArr = [];
    let self = this;
    //this指向调用的数组
    for (let i=0; i<self.length; i++) {
    //把每一项cb返回值push到newArr
      newArr.push(cb.call(context, self[i], i, self))
    }
    return newArr;
  }
  let arr = [1, 2, 3,4, 5];
  let result = arr.myMap(function(item) {
   console.log(this)
    //[1, 2, 3]
    return item*2;
  }, [1, 2, 3])

//filter

Array.prototype.myFilter = function(cb, context) {
    let newArr = [];
    let self = this;
    //this指向调用的数组
    for (let i=0; i<self.length; i++) {
      //把cb结果为true的值push到newArr
      let result = cb.call(context, self[i], i, self);
      if (result) {
        newArr.push(arr[i])
      }
    }
    return newArr;
  }
//例
  let arr = [1, 2, 3,4, 5];
  let result = arr.myFilter(function(item) {
    console.log(this)
    //[1, 2, 3]
    return item > 2;
  }, [1, 2, 3])
 
// 结果为[3, 4, 5]

//every
Array.prototype.myEvery= function(cb, context) {
    let self = this;
    //this指向调用的数组
    for (let i=0; i<self.length; i++) {
    //如果有一个cb值为false返回false否则返回true
    //some改成有一个为true返回true，否则返回false即可
      let result = cb.call(context, self[i], i, self)
      if (!result) {
        return false
      } 
    }
    return true;
  }
  let arr = [1, 2, 3,4, 5];
  let result = arr.myEvery(function(item) {
    console.log(this)
    //【1， 2， 3】
    return item > 1;
  }, [1, 2, 3])







·Day4  js

1.用自己的话描述js垃圾回收机制

现在各大浏览器通常用采用的垃圾回收有两种方法：标记清除、引用计数。 
(1、标记清除 
这是javascript中最常用的垃圾回收方式。当变量进入执行环境是，就标记这个变量为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到他们。当变量离开环境时，则将其标记为“离开环境”。
垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记。然后，它会去掉环境中的变量以及被环境中的变量引用的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后。垃圾收集器完成内存清除工作，销毁那些带标记的值，并回收他们所占用的内存空间。 

(2、引用计数 
另一种不太常见的垃圾回收策略是引用计数。引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型赋值给该变量时，则这个值的引用次数就是1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数就减1。当这个引用次数变成0时，则说明没有办法再访问这个值了，因而就可以将其所占的内存空间给收回来。这样，垃圾收集器下次再运行时，它就会释放那些引用次数为0的值所占的内存。 



3.深入理解this执行
function Foo() {
    getName = function() { alert(1); }
    return this
}
Foo.getName = function() { alert(2); }
Foo.prototype.getName = function() { alert(3); }
var getName = function () { alert(4); }
function getName() { alert(5); }

// 输出值？ 请写出原因
Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName()
new Foo().getName()
new new Foo().getName()


4. 重绘和回流（重排）的区别和关系 用自己的话描述
重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘
回流：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流
回流必将引起重绘，而重绘不一定会引起回流



·Day5 js

1.总结http协议 、同源策略、跨域问题
1.HTTP是一种属于应用层通信协议，它允许将超文本标记语言(HTML)文档从Web服务器传送到客户端的浏览器。
2.同源策略（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。要同时满足三个要求,协议相同,域名相同,端口号相同
3.跨域，指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器
施加的安全限制。所谓同源是指，域名，协议，端口均相同，只要有一个不同，就是跨域
jsonp是前端解决跨域最实用的方法script的src属性可以随意引入不同源的js文件









2.中间件模式（middleware）是一种很常见、也很强大的模式，被广泛应用在 Express、Koa、Redux 等类库和框架当中。模拟一个中间件模式。

const app = {
  target: null,
  middleWares: [],
  index: -1,
  
  callback(ctx) {
    console.log(ctx)
  },
  use(fn) {
    this.middleWares.push(fn)
  },
  go(ctx) {
    this.target = ctx;
    this.index = -1;
    const execute = () => {
      this.index += 1;
      if (this.index >= this.middleWares.length) {
        this.callback(this.target)
      } else {
        this.middleWares[this.index](this.target, execute)
      }
    };
    execute()
  }
};

·Day 6 css
1.请实现未知宽高的盒子在页面中水平垂直居中
1.定位
#box{
 width:100px; 
height:100px;
 position:absolute; 
top:50%; 
left:50%;
 margin-top:-50px;
 margin-left:-50px;
 }
2.弹性盒  
父盒子{
       display: flex;
       align-items: center; /*定义body的元素垂直居中*/
       justify-content: center; /*定义body的里的元素水平居中*/
     }
#box{
    height:200px;
    width:200px;
    background-color:red;
}

2.简述盒模型、怎么修改盒模型
IE6盒子模型与W3C盒子模型。
文档中的每个元素被描绘为矩形盒子。盒子有四个边界：外边距边界margin, 边框边界border, 内边距边界padding与内容边界content。
CSS3中有个box-sizing属性可以控制盒子的计算方式，
content-box：padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和。（W3C盒子模型）
border-box：padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值
修改盒模型: box-sizing: border-box;

3.src和href的区别
1.href：Hypertext Reference的缩写，超文本引用，它指向一些网络资源，建立和当前元素或者说是本文档的链接关系。在加载它的时候，不会停止对当前文档的处理，浏览器会继续往下走。常用在a、link等标签。
2.src：source的所写，表示的是对资源的引用，它指向的内容会嵌入到当前标签所在的位置。由于src的内容是页面必不可少的一部分，因此浏览器在解析src时会停下来对后续文档的处理，直到src的内容加载完毕。常用在script、img、iframe标签中，我们建议js文件放在HTML文档的最后面。如果js文件放在了head标签中，可以使用window.onload实现js的最后加载。
 总结：href用于建立当前页面与引用资源之间的关系（链接），而src则会替换当前标签。遇到href，页面会并行加载后续内容；而src则不同，浏览器需要加载完毕src的内容才会继续往下走。



4.简述8种position属性值
常用的四种
1.static
positon定位的默认值，没有定位
2.relative
生成相对定位的元素，相对于其正常位置进行定位，一般在子元素设置absoute定位时，给父元素设置relative
元素的位置通过top、right、bottom、left  控制，其值的定位起点都是是父元素左上角(这点和absoute、fixed不一样)
3.absoute
生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位
元素的位置通过top、right、bottom、left  控制，top、left的定位起点是包含块左上角，right、bottom的定位起点是包含块右下角
4.fixed  （ie6不兼容）
生成绝对定位的元素，相对于浏览器窗口进行定位，和absoute的区别是fixed不会跟随屏幕滚动(常见的各种贴屏广告)
元素的位置通过top、right、bottom、left  控制，top、left的定位起点是包含块左上角，right、bottom的定位起点是包含块右下角
不常用的四种
1.inherit
规定应该从父元素继承 position 属性的值
inherit 关键字可用于任何 HTML 元素上的任何 CSS 属性
兼容：ie7及以下版本不支持此属性
2.initial 
设置positon的值为默认值(static)
兼容：ie不支持此属性
3.unset
设置positon的值为不设置：
如果该属性的默认属性是 继承属性(例如字体相关的默认属性基本都是继承)，该值等同于 inherit
如果该属性的默认属性 不是继承属性(例如pisition的默认属性为static)，该值等同于 initial
兼容：ie不支持此属性
4.sticky
css3新属性，它的表现就像position:relative和position:fixed的合体:
5.简述css hack



6.简述px、rem、em 的区别 怎么实现适配
px特点：设备是固定的，那么这个设备的css像素也就固定了大小。我们在布局的时候，（比如：width：200px ；height：200px）这样已经写死了盒子的大小，我们在通过调试的时候回发现，无论你如何改变屏幕尺寸，该盒子的宽高永远不会变的。在pc端页面可能没有什么问题，但是手机的屏幕尺寸很多，使用px进行页面布局明显就不合适了，所以在移动端的页面大多采用em布局或者rem布局。

em布局：根据em单位（当前元素的字体）的大小，进行页面布局，可以实现宽高自适应。但是，由于该属性子元素可以继承，如果改变元素的字体大小，该元素的布局也会发生改变！所以，移动端使用rem布局的最多。

rem布局：根据根rem单位元素的字体大小（就是html的字体大小）进行页面布局，由于该属性不会被继承，所以实现宽高自适应只需要根据屏幕大小的变化动态的改变html的字体大小即可。


rem等比例适配所有屏幕：
(function(){
    var currClientWidth,
        fontValue,
        originWidth;
    originWidth = 750;//ui设计稿的宽度，一般750或640
    __resize();

    window.addEventListener('resize', __resize, false);

    function __resize() {
        currClientWidth = document.documentElement.clientWidth;
        if (currClientWidth > 768){
            currClientWidth = 768;
        } 
        if (currClientWidth < 320){
            currClientWidth = 320;
        } 
        fontValue = ((625 * currClientWidth) / originWidth).toFixed(2);
        document.documentElement.style.fontSize = fontValue + '%';
    }
})();


·Day 7 webpack
1.简述webpack模块打包原理
原理
webpack整体是一个事件驱动架构，所有的功能都以Plugin(插件)的方式集成在构建流程中， 通过发布订阅事件来触发各个插件执行。webpack核心使用tapable来实现Plugin(插件)的注册和调用,Tapable是一个事件发布(tap)订阅(call)库。
webpack的流程
初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。



2.简述webpack loader
loader是一种打包的方案，webpack默认只识别js结尾的文件，当遇到其他格式的文件后，webpack并不知道如何去处理。此时，我们可以定义一种规则，告诉webpack当他遇到某种格式的文件后，去求助于相应的loader。




3.Webpack一些常用的自定义配置
一、Tree shaking 只支持 ES Module模块引入方式（import ）。
二、代码分割和webpack无关
1、同步代码： 在webpack.common.js配置optimization
2、异步代码无需操作，会自动分割打包。
三、webpack中，多写异步代码，有助于提升性能
 提高打包速度：
1、合理使用插件，尽量删除多余插件
2、及时升级版本
3、降低loader作用范围，合理使用exclude
4、第三方插件只需打包一次，借助DllPlugin、DllReferencePlugin





4.开发和生产环境的构建配置差异，如何优化webpack构建速度

（1）在项目开发的时候，我们通常会将程序分为开发环境和生产环境（或者叫线上环境），开发环境通常指的是我们正在开发的这个阶段所需要的一些环境配置，也就是方便我们开发人员调试开发的一种环境；生产环境通常指的是我们将程序开发完成经过测试之后无明显异常准备发布上线的环境，也可以理解为用户可以正常使用的就是生产环境；
当然开发环境和生产环境在配置方面的需求是不一样的，但是有共同点：
开发环境的需求：
 模块热更新  （本地开启服务，实时更新）
 sourceMap(方便打包调试)
 接口代理(配置proxyTable解决开发环境中的跨域问题)
代码规范检查(代码规范检查工具)
 生产环境的需求：
 提取公共代码
 压缩混淆(压缩混淆代码，清除代码空格，注释等信息使其变得难以阅读)
 文件压缩 / base64编码(压缩代码，减少线上环境文件包的大小)
 去除无用的代码
 开发环境和生产环境的共同需求：
 同样的入口
 同样的代码处理(loader处理)
 同样的解析配置
(2)优化webpack构建速度
 webpack在启动时会从配置的Entry出发，解析出文件中的导入语句，再递归解析。
 对于导入语句Webpack会做出以下操作：

 根据导入语句寻找对应的要导入的文件；
 在根据要导入的文件后缀，使用配置中的Loader去处理文件（如使用ES6需要使用babel - loader处理）
 针对这两点可以优化查找途径
 优化Loader配置

 Loader处理文件的转换操作是很耗时的，所以需要让尽可能少的文件被Loader处理
 {
     test: /\.js$/,
         use: [
            'babel-loader?cacheDirectory',//开启转换结果缓存
         ],
             include: path.resolve(__dirname, 'src'),//只对src目录中文件采用babel-loader
                 exclude: path.resolve(__dirname, ' ./node_modules'),//排除node_modules目录下的文件
},
优化resolve.modules配置
resolve.modules用于配置webpack去哪些目录下寻找第三方模块，默认是['node_modules']，但是，它会先去当前目录的./ node_modules查找，没有的话再去../ node_modules最后到根目录；
所以当安装的第三方模块都放在项目根目录时，就没有必要安默认的一层一层的查找，直接指明存放的绝对位置
 resolve: {
     modules: [path.resolve(__dirname, 'node_modules')],
     }
优化resolve.extensions配置
在导入没带文件后缀的路径时，webpack会自动带上后缀去尝试询问文件是否存在，而resolve.extensions用于配置尝试后缀列表；默认为extensions: ['js', 'json'];
及当遇到require('./data')时webpack会先尝试寻找data.js，没有再去找data.json；如果列表越长，或者正确的后缀越往后，尝试的次数就会越多；
所以在配置时为提升构建优化需遵守：
频率出现高的文件后缀优先放在前面；
列表尽可能的小；
书写导入语句时，尽量写上后缀名
因为项目中用的jsx较多，所以配置extensions: [".jsx", ".js"]

·Day 10 react&vue
1.简述react和vue的区别
1.监听数据变化的实现原理不同
Vue通过 getter/setter以及一些函数的劫持，能精确知道数据变化。
React默认是通过比较引用的方式（diff）进行的，如果不优化可能导致大量不必要的VDOM的重新渲染。为什么React不精确监听数据变化呢？这是因为Vue和React设计理念上的区别，Vue使用的是可变数据，而React更强调数据的不可变，两者没有好坏之分，Vue更加简单，而React构建大型应用的时候更加鲁棒。
2.数据流的不同
Vue1.0中可以实现两种双向绑定：父子组件之间，props可以双向绑定；组件与DOM之间可以通过v-model双向绑定。Vue2.x中去掉了第一种，也就是父子组件之间不能双向绑定了（但是提供了一个语法糖自动帮你通过事件的方式修改），并且Vue2.x已经不鼓励组件对自己的 props进行任何修改了。

React一直不支持双向绑定，提倡的是单向数据流，称之为onChange/setState()模式。不过由于我们一般都会用Vuex以及Redux等单向数据流的状态管理框架，因此很多时候我们感受不到这一点的区别了。

3.HoC和mixins
Vue组合不同功能的方式是通过mixin，Vue中组件是一个被包装的函数，并不简单的就是我们定义组件的时候传入的对象或者函数。比如我们定义的模板怎么被编译的？比如声明的props怎么接收到的？这些都是vue创建组件实例的时候隐式干的事。由于vue默默帮我们做了这么多事，所以我们自己如果直接把组件的声明包装一下，返回一个HoC，那么这个被包装的组件就无法正常工作了。

React组合不同功能的方式是通过HoC(高阶组件）。React最早也是使用mixins的，不过后来他们觉得这种方式对组件侵入太强会导致很多问题，就弃用了mixinx转而使用HoC。高阶组件本质就是高阶函数，React的组件是一个纯粹的函数，所以高阶函数对React来说非常简单。

4.组件通信的区别
Vue中有三种方式可以实现组件通信：父组件通过props向子组件传递数据或者回调，虽然可以传递回调，但是我们一般只传数据；子组件通过事件向父组件发送消息；通过V2.2.0中新增的provide/inject来实现父组件向子组件注入数据，可以跨越多个层级。
React中也有对应的三种方式：父组件通过props可以向子组件传递数据或者回调；可以通过 context 进行跨层级的通信，这其实和 provide/inject 起到的作用差不多。React 本身并不支持自定义事件，而Vue中子组件向父组件传递消息有两种方式：事件和回调函数，但Vue更倾向于使用事件。在React中我们都是使用回调函数的，这可能是他们二者最大的区别。

5.模板渲染方式的不同
在表层上，模板的语法不同，React是通过JSX渲染模板。而Vue是通过一种拓展的HTML语法进行渲染，但其实这只是表面现象，毕竟React并不必须依赖JSX。
在深层上，模板的原理不同，这才是他们的本质区别：React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的，更加纯粹更加原生。而Vue是在和组件JS代码分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现对这一点，这样的做法显得有些独特，会把HTML弄得很乱。
举个例子，说明React的好处：react中render函数是支持闭包特性的，所以我们import的组件在render中可以直接调用。但是在Vue中，由于模板中使用的数据都必须挂在 this 上进行一次中转，所以我们import 一个组件完了之后，还需要在 components 中再声明下，这样显然是很奇怪但又不得不这样的做法。

6.渲染过程不同
Vue可以更快地计算出Virtual DOM的差异，这是由于它在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。
React在应用的状态被改变时，全部子组件都会重新渲染。通过shouldComponentUpdate这个生命周期方法可以进行控制，但Vue将此视为默认的优化。
如果应用中交互复杂，需要处理大量的UI变化，那么使用Virtual DOM是一个好主意。如果更新元素并不频繁，那么Virtual DOM并不一定适用，性能很可能还不如直接操控DOM。

7.框架本质不同
Vue本质是MVVM框架，由MVC发展而来；
React是前端组件化框架，由后端组件化发展而来。

8.Vuex和Redux的区别
从表面上来说，store注入和使用方式有一些区别。在Vuex中，$store被直接注入到了组件实例中，因此可以比较灵活的使用：使用dispatch、commit提交更新，通过mapState或者直接通过this.$store来读取数据。在Redux中，我们每一个组件都需要显示的用connect把需要的props和dispatch连接起来。另外，Vuex更加灵活一些，组件中既可以dispatch action，也可以commit updates，而Redux中只能进行dispatch，不能直接调用reducer进行修改。

从实现原理上来说，最大的区别是两点：Redux使用的是不可变数据，而Vuex的数据是可变的，因此，Redux每次都是用新state替换旧state，而Vuex是直接修改。Redux在检测数据变化的时候，是通过diff的方式比较差异的，而Vuex其实和Vue的原理一样，是通过getter/setter来比较的，这两点的区别，也是因为React和Vue的设计理念不同。React更偏向于构建稳定大型的应用，非常的科班化。相比之下，Vue更偏向于简单迅速的解决问题，更灵活，不那么严格遵循条条框框。因此也会给人一种大型项目用React，小型项目用Vue的感觉。

2.简述spa，spa实现原理
Spa是单页 Web 应用 (single-page application 简称为 SPA)，简单理解为：仅仅在web页面初始化时加载相应的HTML、JavaScript、CSS，一旦页面加载完成了，SPA不会因为用户的操作而进行页面的重新加载或跳转，而是利用 JavaScript 动态的变换HTML的内（采用的是div切换显示和隐藏），从而实现UI与用户的交互。
