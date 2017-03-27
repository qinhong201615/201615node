##node 第二周（周六3.25）
![Alt text](./1490408322119.png)
###一.require
require('jw-very-handsome');
//直接使用require可以将第三方模块进行引用

//先找自己的node_module,没有向上找，找到根目录结束,没有报错
console.log(module.paths);
//在node_modules中查找文件，并且名字是jw-very-handsome,在找package.json文件，找main对应的名字 进行执行/*

>* 文件模块 引用方式
 * 第三方模块 需要下载 node_modules  module.paths npm install
 * 内置模块 核心模块 node自带的


```
var util = require('util'); `//工具包  继承`
function Child() {}
function Parent() {this.smoke = '抽烟'}
Parent.prototype.eat = function () {console.log('吃');};
util.inherits(Child,Parent); `//util模块实现继承 原型继承 只继承公有方法`
var child = new Child();
child.eat();
console.log(child.smoke);
```


//Child.prototype = Parent.prototype;//不能这样继承
//1.Child.prototype = new Parent();//会继承私有和公有的方法
//2.Parent.call(this); 让父类的方法执行 并且this是儿子的实例
###继承公有的
//1)Child.prototype.__proto__ = Parent.prototype;
//2)Object.create()
//3)es6继承 Object.setProtoTypeOf(Child.prototype,Parent.prototype)
/*function create(p) {
 var Fn = function () {};
 Fn.prototype = p; //让Fn原型指向父类的原型
 return new Fn(); //new的Fn只有Parent的公有属性
 }
Child.prototype=create(Parent.prototype);*/

###二,global
- 1.Buffer是global上的属性 new/Buffer.  全局对象
 + 1.二进制 逢二进一 16进制 逢16进1  Buffer展现给我们的是16进制
  + 2.utf8一个汉字 代表3个字节
  + 3.一个字节由八个位组成Bit

```
 var buffer =new Buffer("珠")
console.log(buffer);//<Buffer e7 8f a0>
```

- 2转换公式 当前位的值* 进制^(当前所在位-1) 累加

```
var sum = 0;
for(var i = 1; i<=8;i++){
    // sum+= 1* 2^(i-1);
    sum+= 1*Math.pow(2,i-1)
}
console.log(sum); //255 最大255 -> 转换成16进制是多少 ff
//15*16^1+15*16^0
```
####1.创建buffer 三种方式(固定大小) 非常像数组 会将buffer转换成字符串
- 1） 长度创建 字符串创建 数组创建

```
var buffer = new Buffer(3); //6个字节
buffer.fill(0); //对256取模 0-255是256个数
console.log(buffer);

var buffer = new Buffer('珠峰');
console.log(buffer);

var buffer = new Buffer([0xfe, 0xff, 0x16]);//给的10进制 0x开头默认是16进制
console.log(buffer);
```
#### 2/buffer相当于数组里存的都是内存地址

```
var buffer = new Buffer([1, 2, 3]);
var newBuffer = buffer.slice(0, 1);
newBuffer[0] = 2;
console.log(buffer);
```
####3.write方法

```
var buffer = new Buffer(12);
//string,往里面写的字符串 offset,偏移量 length,写入的长度 encoding 默认是utf8
buffer.write('珠峰');
buffer.write('培训',6); //写多次必须制定偏移量
console.log(buffer.toString());（16进制）
console.log(buffer[0]);//只要单独拿取这个buffer的单独一项，得到的是10进制

forEach（循环这个buffer他单独的每一项都是10进制）
buffer.forEach(function (item) {
    console.log(item);//得到的是10进制
});

```

####4.拷贝方法 将小buffer拷贝到大buffer上

```
Buffer.prototype.copy = function(targetBuffer, targetStart, sourceStart, sourceEnd) {
};//targetBuffer,目标buffer targetStart,目标的开始 sourceStart,源的开始 sourceEnd源的结束
var buffer = new Buffer(12);
 var buf1 = new Buffer('文档');
 var buf2 = new Buffer('培训');
 
 buf1.copy(buffer,0);
 buf2.copy(buffer,6);
 console.log(buffer.toString());//文档培训
 
```

####5.Buffer.myConcat
1.myConcat list totalLength
//2.判断长度是否传递，如果给了长度就构建一个buffer,将小buffer依次拷贝到大buffer上，过长则将多余的部分 截取掉slice()截取有效长度
//3.手动维护长度 在构建buffer，将小buffer依次拷贝到大buffer上 copy



//concat方法
var buf1 = new Buffer('珠峰');
var buf2 = new Buffer('培训');
//list, totalLength

```
Buffer.myConcat = function (list,totalLength) {
    //1.判断长度是否传递
    if(typeof totalLength == "undefined"){
        totalLength = 0;
        list.forEach(function (item) {
            totalLength += item.length;
        });
    }
    var buffer = new Buffer(totalLength);//1000
    var index = 0;
    list.forEach(function (item) {
        item.copy(buffer,index);
        index+= item.length;
    });
    return buffer.slice(0,index);
};
console.log(Buffer.concat([buf1,buf2,buf1,buf2]).toString());
```

####6.转化进制（原理E:\js\201615node\node\6.Buffer）
parseInt  将任意进制 转换成10进制
toString  将任意进制 转化为任意进制

####7.文件操作命令

同步拷贝：copySync('./name.txt','./name1.txt');
异步拷贝 ：                copy（）
                   buf1.copy(buffer,0);
                   buf2.copy(buffer,6);
同步读取 ：readFileSync
 异步读取：readFile
 
异步写入writeFile 
同步写入 writeFileSync

```
const fs = require('fs');
//1.如果写的文件不存在会创建文件
//2.默认写入格式是utf8格式
//3.文件有内容，清空文件内容
//fs.writeFileSync('./a.txt',new Buffer('珠峰'));
//如果放入的是buffer格式 会默认toString('utf8');
fs.writeFile('./1.txt',new Buffer('珠峰培训'),function (err) {
    console.log(err);//珠峰培训
});
//拷贝的方法  同步 readFileSync+writeFileSync
//          异步 readFile + writeFile
```


appendFile ：该方法以异步的方式将 data 插入到文件里，如果文件不存在会自动创建。data可以是任意字符串或者缓存。
appendFileSync：该方法以异步的方式将 data 插入到文件里，如果文件不存在会自动创建。data可以是任意字符串或者缓存。

// path.resolve() path.join() 给一个相对路径 解析一个绝对路径
// mkdirSync mkdir 创建目录
// rmdirSync rmdir 删除目录
// unlinkSync unlink 删除文件
// readdirSync readdir 读取目录
// existsSync exists 是否存在
// statSync  stat 判断文件状态

--------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------
##node 第二周（周日3.26）
###fs.createReadStream
>//1.读取保证文件必须存在
>//2.默认返回的是可读流对象
>//3.highWaterMark: 65536, 64k 一次读多少
>//4.encoding 是null 默认读出的内容是buffer
默认的时候 非流动模式，监听了data 叫流动模式
### fs.createWriteStream
>>//2.文件不存在会创建文件，会清空原有内容
>>//3.ws代表的是可写流
>>//4.highWaterMark  16384 16*1024




####http内置模块
```
var http = require('http');//核心模块 提供的http服务
   http.createServer(function (req,res) { //监听函数,请求到来的时候执行
       //如果没有任何响应 状态就是pending
       //res.writeHead(200,{'Content-Type':'text/plain;charset="utf8"',a:1});还可以用setheader（）重写响应头；如：
       res.setHeader('Content-Type','text/plain;charset="utf8"');
       res.setHeader('a',1);
       res.write('你好'); //res是一个可写流，可以直接用可写流的方法；
       res.end();
   }).listen(80,function () {
       console.log('start 80');
   });
```
   ```
###在github中找到idoc，然后可以通过npm安装
```
npm install idoc -g
```

```
fandoc init 
```

```
fandoc build
```

```
fandoc server
```





