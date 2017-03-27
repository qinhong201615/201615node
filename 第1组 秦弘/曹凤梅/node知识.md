##node知识
####buffer
```
1.Buffer是global上的属性;  
全局对象:
setTimeout setInterval process setImmediate console Buffer 形参(exports require module __dirname __filename)
1.二进制 逢二进一 16进制 逢16进1  Buffer展现给我们的是16进制
2.utf8一个汉字 代表3个字节
3.一个字节由八个位(Bit)组成;
4.进制转换公式 当前位的值* 进制^(当前所在位-1)， 然后累加
5.创建buffer 三种方式(固定大小) 非常像数组 会将buffer转换成字符串
1） 长度创建 
var buffer =  new Buffer(6); //6个字节
buffer.fill(0); //将buffer内容清0
,对256取模 0-255是256个数
2）字符串创建
var buffer = new Buffer('珠峰')
3）数组创建
var buffer = new Buffer([0xfe,0xff,0x16]);//给的10进制 0x开头默认是16进制
6.buffer相当于数组里存的都是内存地址，例：
var buffer = new Buffer([1,2,3]);
var newBuffer = buffer.slice(0,1);
newBuffer[0] = 2;
console.log(buffer);//[2,2,3]
7.write方法
var buffer = new Buffer(12);
buffer.write('珠峰');//第一个参数：string,往里面写的字符串；第二个参数：offset,偏移量；第三个参数：length,写入的长度；第四个参数：encoding 默认是utf8；偏移量为0时可以省略；写入的长度不写默认为全部的总长度；utf8可以省略；
buffer.write('培训',6); //写多次必须制定偏移量
8.buffer中的forEach
var buffer = new Buffer(12);
buffer.forEach(function (item) {
    console.log(item);//得到的是10进制
});
9.copy：拷贝方法 将小buffer拷贝到大buffer上
var buffer = new Buffer(12);
var buf1 = new Buffer('曹凤梅');
var buf2 = new Buffer('真好');
//参数：targetBuffer目标buffer； targetStart,目标的开始； sourceStart,源的开始 sourceEnd源的结束；
buf1.copy(buffer,0);
buf2.copy(buffer,6);
console.log(buffer.toString());
10.concat方法
var buf1 = new Buffer('曹凤梅');
var buf2 = new Buffer('真好');
console.log(Buffer.concat([buf1,buf2]).toString());
```
####fs内置模块
1、fs.readFileSync和fs.readFile
- fs.readFileSync():参数：1）文件路径及名称；2）utf8，可以省略，如果想字符串格式则要写上；
- fs.readFile():参数：1）文件路径及名称；2）utf8，可以省略，如果想字符串格式则要写上；3)回调函数，回调函数的参数是e和数据，PS：readFile，会淹没可用内存，不能读取比内存大的文件
```
const fs = require('fs'); //既有同步 又有异步
1.读取文件时，文件没有会报错；
2.读取默认格式是buffer类型  encoding:null
同步例子：
var school = {};
var name = fs.readFileSync('./name.txt','utf8');
school.name = name;
var age = fs.readFileSync('./age.txt','utf8');
school.age = age;
console.log(school);
异步例子：
var school = {}; //计数器的原理
fs.readFile('./name.txt','utf8',function (e,data) { //e：error-first
    if(e)console.log(e);
    school.name = data;
    out();
});
fs.readFile('./age.txt','utf8',function (e,data) { //error-first
    if(e)console.log(e);
    school.age = data;
    out();
});
function out() {
    if(Object.keys(school).length == 2){
        console.log(school); //{name:'珠峰培训',age:8}
    }
}
```
2、fs.writeFileSync和fs.writeFile
```
1.如果写的文件不存在会创建文件;
2.默认写入格式是utf8格式
3.文件有内容，清空文件内容(覆盖式写法)
PS：fs.writeFileSync('./a.txt',new Buffer('珠峰'));
//如果放入的是buffer格式 会默认toString('utf8');
fs.writeFile('./a.txt',new Buffer('珠峰培训'),function (err) {
    console.log(err);
});
```
3、fs.exists
```
fs.exists():参数 1）目录名 2）函数（函数中有一个参数，是判断此目录名是否已经存在，布尔值）
```
4、fs.createReadStream 创建可读流
```
1.读取时保证文件必须存在
2.默认返回的是可读流对象
3.highWaterMark: 65536, 64k 一次读多少4.encoding 是null 默认读出的内容是buffer
var rs = fs.createReadStream('./1.txt',{highWaterMark:1});
可读流的方法：
on('data') ; on('end') ; on('error')监听错误信息 ; pause()暂停 ; resume()重新执行on（'data'）方法；
```
5、fs.createWriteStream 创建可写流
```
1.写的默认编码 utf8
2.文件不存在会创建文件，会清空原有内容
3.ws代表的是可写流
4.highWaterMark  16384 16*1024
var ws =fs.createWriteStream('./2.txt',{highWaterMark:4});
//可写流的方法：写入 write()；； 写完 end()；on('drain',function(){}）表示写的抽干了，可以继续写了
var flag = ws.write('abcd1',function () { //write的第一个参数的格式：buffer or string
    console.log('成功');
});
//flag表示是否可以继续写入
ws.end('再放一个');// 表示结束时，强制将没有写完的内容写完，再关闭；PS：end可以传递参数，表示在结束时 再写点东西
```
6、pipe:把可读流写入可写流 rs.pipe(ws)
```
var fs = require('fs');
function pipe(source,target) {
    var rs = fs.createReadStream(source);
    var ws = fs.createWriteStream(target);
    rs.pipe(ws);//想看到文件中的内容，pipe的好处 异步，不会淹没可用内存
}
pipe('1.txt','3.txt');
```
5、fs中常用方法：
```
// copySync('./name.txt','./name1.txt');
// readFileSync readFile
// writeFile  writeFileSync
// appendFile appendFileSync
// mkdirSync mkdir 创建目录
// rmdirSync rmdir 删除目录
// unlinkSync unlink 删除文件
// readdirSync readdir 读取目录
// existsSync exists 是否存在
// statSync  stat 判断文件状态
```
####path
```
var path  = require('path'); //核心模块 resolve  join
console.log(path.resolve('1.js'));//以当前路径解析出一个相对路径
console.log(path.join(__dirname,'1.js'));//相当于resolve
常用方法：
// path.resolve() 
//path.join() 给一个相对路径 解析一个绝对路径
```
####连续创建目录的方法例子：递归思想   用mkdir + exists  
```
function makep(p) {
    var paths = p.split('/');
    var index = 1;
    function makeOne(subpath) {
        if(paths.length == index-1){
            return;
        }
        fs.exists(subpath,function (flag) {
            var temp = paths.slice(0,++index).join('/');
            if(!flag){ //不存在的时候创建
                fs.mkdir(subpath,function () {
                    makeOne(temp);// a/b
                });
            }else{ //存在的时候继续下一次
                makeOne(temp);
            }
        });
    }
    makeOne(paths[index-1]);
}
makep('a/b/c/d/e/f');
```
####util内置模块
```
var util = require('util');
util.inherits(Man,EventEmitter);
//参数:1)子 2）父；子的实例继承父的公有方法；
```
####events内置模块
```
//使用node自带的事件模块
var EventEmitter = require('events');
var util = require('util');
function Man() {}
util.inherits(Man,EventEmitter);
var man = new Man(); //让man拥有EventEmitter上的公有方法
function learnBad(any) {
    console.log('learnBad'+any);
}
function buyCar(any) {
    console.log('buyCar'+any);
}
man.once('有钱',learnBad); //{'有钱':[learnBad,buyCar]}
man.removeAllListeners('有钱');
//man.removeListener('有钱',learnBad);
man.emit('有钱','xxx');
man.emit('有钱','xxx');
man.emit('有钱','xxx');
//on=addListener once removeListener emit removeAllListeners
```
####事件:发布订阅模式例子
```
//发布订阅  一对多的关系  {"买饭":["看帅哥","看美女"],"学习":["学数学","学英语"]}  {"有钱":["买车","买房","买包"]}
//异步

function Man() {
    this._events = {};
}
Man.prototype.on = function (eventName,callback) {
    if(this._events[eventName]){//这是第二次 {“有钱”:[买车,买房]}
        this._events[eventName].push(callback);
    }else{ //这是第一次 {“有钱”:[买车]}
        this._events[eventName] = [callback];
    }
};
Man.prototype.removeListener = function (eventName,callback) {
    if(this._events[eventName]){
        this._events[eventName] = this._events[eventName].filter(function (cb) {
            //返回true表示 这一项留下 false表示这一项删除
            return cb!=callback;
        })
    }
};
Man.prototype.emit = function (eventName,...args) {
    //console.log([].slice.call(arguments,1));
    //console.log(Array.from(arguments).slice(1));
    if(this._events[eventName]){ //如果有 ，才执行
        this._events[eventName].forEach( callback =>{
            callback.apply(this,args);//执行数组中的函数
        });
    }
    //this.removeListener(eventName,this.del);
};//{有钱:['买车','买房']}
Man.prototype.once = function (eventName,callback) {
    //先绑定事件，当emit触发后移除绑定的函数
    this.on(eventName,one);
    //this.del = callback;
    function one() {
        callback.apply(this,arguments);
        this.removeListener(eventName,one);
    }
};
var man = new Man();
function buyCar(who) {console.log(`给${who}买车`)}
function buyHouse(who) {console.log(`给${who}买车房`)}
man.once('有钱',buyCar); //只执行一次,触发一次后，在数组中删除买车的事件
man.on('有钱',buyHouse); //{"有钱":["买车"]}
man.removeListener('有钱',buyCar);//{"有钱":[one]} 无法删除
man.removeListener('有钱',buyHouse);
man.emit('有钱','妹子');
man.emit('有钱','妹子');
man.emit('有钱','妹子');
man.emit('有钱','妹子');
```
####在github中找到idoc，然后可以通过npm安装，看笔记；
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