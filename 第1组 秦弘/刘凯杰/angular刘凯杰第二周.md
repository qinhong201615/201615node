##angules第二 周整理
### yarn(yarn确实要比npm更快速，更简单)
具体语法如下：
开始一个新工程	

```
yarn init
```

添加一个依赖

```
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

更新一个依赖

```
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

移除一个依赖

```
yarn remove [package]
```

安装package.json中所有的依赖项

```
yarn
```

或者

```
yarn install
```

下载yarn
```
npm install yarn -g
```

## 初始化一个package.json
```
yarn init
```

## 安装包
```
yarn add angular
yarn add gulp --dev
yarn remvoe gulp --dev
```


----------------------------------------------------------------------------
---------------------------------------------------------------------------------

```
###1.ng-bind-html
```

这里只能放字符串类型的,将数据html编译成$sce.trustAsHtml(html)（课件第三天3-21/1）

```
<tr ng-repeat="score in scores track by $index">
                    <!--130=><font color="red">13</font>0-->
                    <td>{{score.name}}</td>
                    <td ng-bind-html="score.english | tableRed:query | asHtml"></td>
                    <td ng-bind-html="score.chinese | tableRed:query | asHtml"></td>
                    <td ng-bind-html="score.math | tableRed:query | asHtml"></td>
                </tr>
```

###2.指令（课件第三天3-21/2）

```
<!--指令分四种格式-->
<!--Element 元素  E-->
<hello></hello>
<!-- attribute A 属性-->
<div hello=""></div>
<!--className C     -->
<div class="hello"></div>
<!-- directive:commit M -->
<!--注释-->
```

指令分为两类 自带指令 自定义指令
自定义指令
   -  装饰型指令  赋予标签一些功能
   -  组件式 把标签 替换成一个组件 template
 ```

 return{
            replace:true,
            restrict:"",//EACM  Element 元素  attribute A className C directive:commit M
            // 属性
            template:"<div><div>你好</div><div>滚蛋</div></div>"
        }
       ## 需要注意 restrict:""里一定方式是对面的类型E，A，C，M ，
   
  ```
 ###3.组件化开发的好处，复用，管理方便统一管理（课件第三天3-21/5）
   scope:{},
给当前指令产生一个作用域 {}  
- 默认值是false是没有作用域 
- true是可以向上查找
- {} 不能获取父作用域的属性 相当于开辟了一个和$rootScope平级的作用

```
（课件第三天3-21/6）
/*link:function (scope,element,attrs) {
                scope.name = attrs.st;
                scope.title = attrs.title; //取不到属性对应的值的变量
            },*/
            上面代码等于下面代码（写法）
             scope:{ //@符引用的是字符串
                name:'@st',
                //title:'@' //名字相同可以去掉后面的
                // = 取的是变量
                title:'=article' //scope.title = $scope.t
            }, //如果通过属性传递的值 可以再scope:{} 里快速获取到


```
###4.@ ,= ，&的区分
- @符引用的是字符串
- = 取的是变量
- &取的是方法（自定义的方法函数），可以直接用&符号取到
###5.$watchers	
脏值检测 对比两次的变化，触发对应的回调函数,将所有数据放在一个数组中，有一个变化 所有$watchers执行一遍（用法课件第三天3-22/3和4）
###`6.$scope $apply()解决方式$interval, $timeout（只执行一次）`
手动刷新视图（用法课件第三天3-22/5和6,7）
###angular提供了五种服务  "provider factory" service  value constant
- 1.服务是单例的 ，方法都是公用的
- 2.实现复用代码 $rootScope, localStorage
- 3.provider是最大的服务，可以进行配置,其他4中不能配置
###分区

```
factory 不具有配置能力，因为$get方法在我们的config方法后执行 配置后会被覆盖掉
factory是基于provider的 后面的函数就是provider的$get
最大的是provider factory是基于provider的  service 是基于factory的  value是基于 factory的
（用法课件第三天3-22/8，9，10，例子10）
```
###`$on  $emit(发射)  $broadcast的理解`

>`$on、$emit和$broadcast使得event、data在controller之间的传递变的简单。`
>`$emit只能向parent controller传递event与data `
>`$broadcast只能向child controller传递event与data`
>`$on用于接收event与data `
> `只要$emit和$broadcast 发射，$on就可以立刻接受到的。`
> `不过用的是$scope 发射和接受。`
> `$emit(name,data)向父control发射的。`
> `而$broadcast(name,data)是向子control发射的。 `

