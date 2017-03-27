
##node(buffer,fs,event)
###buffer
* 1.Buffer是global上的属性 new/Buffer.  全局对象
* setTimeout setInterval process setImmediate console Buffer 形参(exports require module __dirname __filename)
* console.log(global);
```
1.二进制 逢二进一 16进制 逢16进1  Buffer展现给我们的是16进制
2.utf8一个汉字 代表3个字节
3.一个字节由八个位组成Bit

转换公式 当前位的值* 进制^(当前所在位-1) 累加
var sum = 0;
for(var i = 1; i<=8;i++){
    // sum+= 1* 2^(i-1);
    sum+= 1*Math.pow(2,i-1)
}
console.log(sum); //255 最大255 -> 转换成16进制是多少 ff
//15*16^1+15*16^0
```
* 1.创建buffer 三种方式(固定大小) 非常像数组 会将buffer转换成字符串
1） 长度创建 字符串创建 数组创建
```
var buffer =  new Buffer(106); //6个字节
buffer.fill(0); //对256取模 0-255是256个数
console.log(buffer);

var buffer = new Buffer('珠峰');
console.log(buffer);

var buffer = new Buffer([0xfe,0xff,0x16]);//给的10进制 0x开头默认是16进制
console.log(buffer);
```
* buffer像数组
```
/*var obj = {name:1};
var arr = [obj,1,2];
var newArr = arr.slice(0); //浅拷贝
obj.name = 2;
console.log(newArr);*/
//buffer相当于数组里存的都是内存地址
var buffer = new Buffer([1,2,3]);
var newBuffer = buffer.slice(0,1);
newBuffer[0] = 2;
console.log(buffer);
```
*  write方法
```
var buffer = new Buffer(12);
//string,往里面写的字符串 offset,偏移量 length,写入的长度 encoding 默认是utf8
buffer.write('珠峰');
buffer.write('培训',6); //写多次必须制定偏移量
console.log(buffer.toString());
console.log(buffer[0]);
//forEach
buffer.forEach(function (item) {
    console.log(item);//得到的是10进制
});
```

* 拷贝方法 将小buffer拷贝到大buffer上
```
var buffer = new Buffer(12);
var buf1 = new Buffer('珠峰');
var buf2 = new Buffer('培训');
//targetBuffer,目标buffer targetStart,目标的开始 sourceStart,源的开始 sourceEnd源的结束
buf1.copy(buffer,0);
buf2.copy(buffer,6);
console.log(buffer.toString());*/
```
* concat方法
```
var buf1 = new Buffer('珠峰');
var buf2 = new Buffer('培训');
//list, totalLength
```
//1.myConcat list totalLength
//2.判断长度是否传递，如果给了长度就构建一个buffer,将小buffer依次拷贝到大buffer上，过长则将多余的部分 截取掉slice()截取有效长度
//3.手动维护长度 在构建buffer，将小buffer依次拷贝到大buffer上 copy
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

*  进制转换
```
//parseInt  将任意进制 转换成10进制
//toString  将任意进制 转化为任意进制
//01010101
console.log(parseInt('11100011',2));
console.log((0x16).toString(10));
// base64 不是一个加密算法， 加密 md5 sha1 sha256

//将汉字转换成base64 '珠' => 54+g  3*8 = 6*4
//2进制装换成10进制不得大于64,得到的结果在可见编码中取值

//将汉字转换成2进制
var buffer = new Buffer('珠');
console.log(buffer); //0xe7 0x8f 0xa0
console.log((0xe7).toString(2)); //11100111
console.log((0x8f).toString(2)); //10001111
console.log((0xa0).toString(2)); //10100000
    //将2进制转换成10进制
console.log(parseInt('00111001',2)); //57
console.log(parseInt('00111000',2)); //56
console.log(parseInt('00111110',2)); //62
console.log(parseInt('00100000',2)); //32
var str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
str+= 'abcdefghijklmnopqrstuvwxyz';
str+='0123456789';
str+='+/';
console.log(str[57]+str[56]+str[62]+str[32]);
```

###fs
//核心模块 file system
const fs = require('fs'); //既有同步 又有异步
//能用异步 绝不用同步

//1.读取文件没有会报错
//2.读取默认格式是buffer类型  encoding:null
```
/*
var school = {};
var name = fs.readFileSync('./name.txt','utf8');
school.name = name;
var age = fs.readFileSync('./age.txt','utf8');
school.age = age;
console.log(school);*/
```
```
var school = {}; //计数器的原理
fs.readFile('./name.txt','utf8',function (e,data) { //error-first
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
// arrow_fn  templateString class export 解构
```
```
const fs = require('fs');
//1.如果写的文件不存在会创建文件
//2.默认写入格式是utf8格式
//3.文件有内容，清空文件内容
//fs.writeFileSync('./a.txt',new Buffer('珠峰'));
//如果放入的是buffer格式 会默认toString('utf8');
fs.writeFile('./a.txt',new Buffer('珠峰培训'),function (err) {
    console.log(err);
});

//拷贝的方法  同步 readFileSync+writeFileSync
//          异步 readFile + writeFile
```


>const fs = require('fs');
* readFile，会淹没可用内存，不能读取比内存大的文件
* var path  = require('path'); //核心模块 resolve  join
* console.log(path.resolve('1.js'));//以当前路径解析出一个相对路径
* console.log(path.join(__dirname,'1.js'));//相当于resolve
###常用的方法
```
// copySync('./name.txt','./name1.txt');
// readFileSync readFile
// writeFile  writeFileSync
// appendFile appendFileSync
// path.resolve() path.join() 给一个相对路径 解析一个绝对路径
// mkdirSync mkdir 创建目录
// rmdirSync rmdir 删除目录
// unlinkSync unlink 删除文件
// readdirSync readdir 读取目录
// existsSync exists 是否存在
// statSync  stat 判断文件状态
```

###readstream
```
const fs = require('fs');
//1.读取保证文件必须存在
//2.默认返回的是可读流对象
//3.highWaterMark: 65536, 64k 一次读多少
//4.encoding 是null 默认读出的内容是buffer
var rs = fs.createReadStream('./1.txt',{highWaterMark:1});
//默认的时候 非流动模式，监听了data 叫流动模式
var arr = [];
rs.on('data',function (data) { //如果你监听了data事件，会不停的触发rs.emit("data",data)
    rs.pause();
    console.log('停一会');
    arr.push(data);
    setTimeout(function () {
        rs.resume(); //重新出发data事件
    },1000);
});
rs.on('end',function () { //结束的事件  rs.emit('end');如果没读完 不会触发
    console.log(Buffer.concat(arr).toString());
});
rs.on('error',function (err) { //监听错误信息的
    console.log(err);
});
// on('data') on('end') on('error') pause() resume()
```

###writestream
```
const fs = require('fs');
//1.写的默认编码 utf8
//2.文件不存在会创建文件，会清空原有内容
//3.ws代表的是可写流
//4.highWaterMark  16384 16*1024
var ws = fs.createWriteStream('./2.txt',{highWaterMark:4});
//写入 write() 写完 end()
var flag = ws.write('abcd1',function () { //buffer or string
    console.log('成功');
});
//flag表示是否可以继续写入
ws.end('在吃一口');// 表示合上嘴，强制将没有写完的内容写完，在关闭
//end可以传递参数，表示在合上嘴之前 在写点东西
console.log(flag);

```

###pipe
```
//1.10G 先读一次 64k ，开始写入 16k，如果写不进去了,暂停读取，等待写完后在读取，读取成功后，关闭写入
var fs = require('fs');
function pipe(source,target) {
    var rs = fs.createReadStream(source);
    var ws = fs.createWriteStream(target);
    rs.pipe(ws);//想看到文件中的内容，pipe的好处 异步，不会淹没可用内存
    //on('data') on('end') on('error') pause() resume() on('drain') write() end()
    //1.先监听读取事件rs.on('data');
    rs.on('data',function (data) {
        //2.先写入ws.write，如果返回值为false，rs.pause();
        var flag = ws.write(data);
        if(!flag){
            rs.pause();
        }
    });
    //3.ws.on('drain')可写流干了,在进行读取rs.resume();
    ws.on('drain',function () {
       rs.resume();
    });
    //4.rs.on('end') 读完了 ws.end()关闭写的文件
    rs.on('end',function () {
        ws.end();
    });
}
pipe('1.txt','3.txt');
```

###server
```
if(req.url == '/'){
        res.setHeader('Content-Type','text/html;charset="utf8"');
        // pipe管道，可以将可读流 导入到可写流中
        fs.createReadStream('./index.html').pipe(res);//会自动调用res中的write和end方法
    }
    else{
        //如果服务器有，我才返回
        fs.exists('.'+req.url,function (flag) {
           if(flag){ //存在读出来返回，第三方模块 mime,通过后缀名解析正确的content-type格式
               res.setHeader('Content-Type',mime.lookup(req.url)+';charset="utf8"');
               fs.createReadStream('.'+req.url).pipe(res);
           }else{ //不存在 返回404
               res.statusCode = 404; // 301 302 304
               var text = require('_http_server').STATUS_CODES[res.statusCode];
               res.end(text);
           }
        });
    }
```
















