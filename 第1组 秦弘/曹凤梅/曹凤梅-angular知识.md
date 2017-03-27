##angular
1、sce
```
$sce.trustAsHtml()//将页面的数据转换成了html内容
ng-bind-html='' 这里只能放字符串类型的,将数据html编译成$sce.trustAsHtml(html);
```
2、指令
```
1）指令分为两类：自带指令和自定义指令
  自定义指令：
    - 装饰型指令 赋予标签一些功能  link函数
    - 组件式 把标签 替换成一个组件 template
2）指令分为四种格式：
    - Element E
      <my-drag></my-drag>
    - Attributes A
    <div my-drag></div>
    - Class C
    <div class="my-drag"></div>
    - Comment M 前后必须用空格，不建议使用
    <!-- directive:myDrag -->
3）模块名.directive('指令名',function () {
        return {//返回一个对象
            replace:true, //将原指令标签替换掉，要求模板只能有一个根节点
            restrict:'EA', //限制使用范围 默认范围是EA,范围的标识必须大写
            template:`<div>
                       <div>Hello</div>
                        <div>zfpx</div>
                      </div>` //模板的根节点一般只有一个，当replace为true时，必须只能是一个根节点；
     //link链接函数 链接视图和作用域的
            link:function (scope,element,attrs) {//形参 ,在link函数中可以操作DOM元素
                // 1.代表的是当前指令所在的作用域
                // 2.element代表当前指令所在的标签，是一个jquery对象
                PS：angular.element == $（转化为JQ对象）             angular.element(document).on('click',function () {
                    alert('click')
                });
                // 3.代表当前指令上所有的属性集合
                console.log(attrs);
            }                 
                      
        }
```
3、拖拽
```
<script src="node_modules/jquery/dist/jquery.js"></script>
<!--angular会判断jquery是否存在，存在的话不加载自己的js，所以先引入jQuery再引入angular-->
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.directive('drag',function () {
        return {
            restrict:'A',
            link:function (scope,element,attrs) {
                element.on('mousedown',function (e) {
                    //盒子距离鼠标的距离
                    var disX = e.pageX - $(this).offset().left;
                    var disY = e.pageY - $(this).offset().top;
                    //防止脱离焦点
                    $(document).on('mousemove',function (e) {
                        element.css({ //操作距离在不引入jQuery时要加单位
                            top:e.pageY-disY,
                            left:e.pageX-disX
                        });
                    });
                    $(document).on('mouseup',function () {
                        $(document).off(); //关闭抬起和移动事件
                    });
                    e.preventDefault();
                });
            }
        }
    });
</script>

```
4、简单组件化
```
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <panel st="success" title="文章1">
                这是文章内容 angularjs
            </panel>
            <panel st="danger" title="文章2">
                这是第二篇文章文章内容 angularjs
            </panel>
            <panel st="warning" title="文章3">
                这是第三篇文章文章内容 angularjs
            </panel>
        </div>
    </div>
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.directive('panel',function () {
        return {
            restrict:'E',
            templateUrl:`tmpl/panel.html`,//ajax获取过来的
            link:function (scope,element,attrs) {
                //自己家没有 根作用域，如果自己家有 就是自己家的作用域
                scope.name = attrs.st;
                scope.title = attrs.title;
            },
            scope:{}, //给当前指令产生一个作用域 {}  默认值是false
            // {} 不能获取父作用域的属性 相当于开辟了一个和$rootScope平级的作用域
            transclude:true,//会产生一个作用域,会将指令中夹着的部分 插入到带有ng-transclude的标签内
        };
    });
tmpl/panel.html文件内容：    
<div class="panel panel-{{name}}">
    <div class="panel-heading">
        {{title}}
    </div>
    <div class="panel-body" ng-transclude>

    </div>
    <div class="panel-footer">
      
    </div>
</div>

```
5、@符
```
<body ng-controller="myCtrl">
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <panel st="success" article="t">
                这是文章内容 angularjs
            </panel>
        </div>
    </div>
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.controller('myCtrl',function ($scope) {
        $scope.t = '文章1'
    });
    app.directive('panel',function () {
        return {
            restrict:'E',
         templateUrl:`tmpl/panel.html`,
         //如果通过属性传递的值 可以再scope:{} 里快速获取到,如下：
            scope:{ //@符引用的是字符串
                name:'@st',
                title:'@' //名字相同可以去掉@后面的，
               // =取的是变量
                title:'=article' 
            }, 
            transclude:true,
        };
    });
</script>
```
6、&符
```
//&符取的是函数方法，直接用&符号取到
 <panel st="success" article="t" s="say(abc,b)">
                这是文章内容 angularjs
            </panel>
scope:{
                name:'@st',
                title:'=article',
                say:'&s' 
            },
PS：使用& 传入的参数必须是一个对象，名字和形参名相同
        <button ng-click="say({abc:title（变量）,b:'hello'})">点我say</button>
```
7、控制div的显示和隐藏，学指令和指令之间的交互
```
ng-class（动态）和class不冲突（静态) ng-class的几种写法：
1) {true:'block',false:'none'}[flag]
2) {block:flag,none:!flag}
3）ng-show/ng-hide ng-if
    频繁切换用show/hide  操作的是样式
    一次就确定下来的内容用ng-if 如果值为false内部代码不执行 操作的dom, repeat经常和if连用  if会产生一个作用域
```
8、指令与指令之间的关联
```
<body>
<girl>
    <a eat train>小美丽</a>
</girl>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.directive('girl',function () {
        return {
            restrict:'E',
            controller:function ($scope) { //当前指令所在的作用域,不会产生作用域
                $scope.arr = [];
                this.collect = function (kinds) {
                    $scope.arr.push(kinds);
                }
            },
            link:function (scope,element) {
                element.on('click',function () {
                    alert(scope.arr)
                })
            }
        }
    });
    app.directive('train',function () {
        return {
            restrict:'A',
            //^自己身上找找不到向上找
            require:'^?girl',//在当前指令上找girl，找到则会在link函数的第四个参数增加girl控制器的实例
            link:function (scope,element,attrs,gCtrl) { //如果没有依赖到使用的是? 则得到的值是null，否则报错
                console.log(gCtrl);
                gCtrl.collect('火车');
            }
        }
    });

    app.directive('eat',function () {
        return {
            restrict:'A',
            require:'^girl',//在当前指令上找girl，找到则会在link函数的第四个参数增加girl控制器的实例
            link:function (scope,element,attrs,gCtrl) {
               gCtrl.collect('吃');
            }
        }
    })
</script>
</body>
```
9、$watch
```
<script>
    1) 脏值检测 对比两次的变化，触发对应的回调函数,将所有数据放在一个数组中，有一个变化,所有$watchers执行一遍
    2) vue 自带的defineProperty set get + 观察者
  
    3） ng-change="change()"触发的事件
    4） 实时监控
        $rootScope.$watch(function () {
            return $rootScope.name;
        },function (newVal,oldVal) {
            console.log(newVal,oldVal);
        })
   5）放入的是作用域上的一个变量字符串
      $rootScope.$watch('name',function (newVal,oldVal) {
            console.log(newVal,oldVal);
        })
    })
```
10、满百包邮的例子
```
<body>
<div ng-controller="myCtrl">
    名称：{{product.name}} <br>
    价格：{{product.price}} <br>
    数量:<input type="text" ng-model="product.count"> <br>
    邮费：{{product.free}} <br>
    总价：{{sum()+product.free}}
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.controller('myCtrl',function ($scope) {
        $scope.product = {name:'电脑',price:20,count:2,free:30};
        //监控总价 如果总价值大于100  邮费=0;
        $scope.sum = function () {
            return $scope.product.price*$scope.product.count
        };
        $scope.$watch($scope.sum,function (newVal) {
            $scope.product.free = newVal>=100?0:30;
        });
    });
</script>
</body>
```
11、$apply:通知视图刷新
```
<body ng-controller="myCtrl">
123123
<button ng-click="show()" ng-disabled="!flag">{{text}}</button>
{{d | date:'yyyy-dd hh:mm:ss'}}
<script src="node_modules/angular/angular.js"></script>
<script>
    //点击按钮 获取验证码， -> 等待(5s) 禁用状态->  获取验证码
    var app = angular.module('appModule',[]);
    app.controller('myCtrl',function ($scope,$interval) {
        $scope.text = "发送验证码";
        $scope.flag = true;
        $scope.show = function () {
            $scope.text = 5;
            $scope.flag = false;
            var timer = $interval(function () {
                $scope.text = $scope.text-1;
                if($scope.text<=0){
                    $interval.cancel(timer);//取消定时器
                    $scope.flag = true;
                    $scope.text = "发送验证码";
                }
            },1000);
        }
    });

```
12、$http(模拟百度搜索)
```
<body ng-controller="searchCtrl">
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <h1 class="h1 text-center">搜索框</h1>
            <input type="text" class="form-control" ng-model="query" ng-change="search()" ng-keyup="changeIndex($event)" ng-keydown="prevent($event)"/>
            <ul class="list-group">
                <li class="list-group-item" ng-class="{active:index==$index}"  ng-repeat="a in arrs track by $index">{{a}}</li>
            </ul>
        </div>
    </div>
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.controller('searchCtrl',function ($scope,$http,$sce) { //$http 用法同jquery
        $scope.index = -1; //默认哪个都不选中
        $scope.prevent = function (e) { //光标向前是 按下导致的
            if(e.keyCode == 38 ){ //阻止按下上时的事件
                e.preventDefault();
            }
        };
        $scope.changeIndex = function (e) {
            var code = e.keyCode;
            if(code == 38){
                $scope.index -=1;
                if($scope.index<=-1){
                    $scope.index = $scope.arrs.length-1;
                }
                $scope.query = $scope.arrs[$scope.index]; //上下实现赋值
            }else if(code == 40){
                $scope.index +=1;
                if($scope.index == $scope.arrs.length){
                    $scope.index = 0;
                }
                $scope.query = $scope.arrs[$scope.index]; //上下实现赋值
            }else if(code == 13){
                window.open('https://www.baidu.com/s?wd='+$scope.query); //打开新页面
            }
        };
        $scope.search = function () { //输入时触发
            //作为一个可信任的路径$sce.trustAsResourceUrl()
            //百度jsonp接口 callback名字得是cb
            //$http 返回都是promise对象 对象中有一个then方法，方法中有两个参数，第一个是成功的回调第二个是失败的回掉
            $http.jsonp(//以下是固定写法
                    $sce.trustAsResourceUrl('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd='+$scope.query)
                    ,{jsonpCallbackParam: 'cb'}
            ).then(function (res) { //成功
                $scope.arrs = res.data.s;
            },function (err) { //失败
                console.log(err);
            });
        }
    });
</script>
</body>
```
13、angular中的服务
```
1.服务是单例的 ，方法都是公用的;
2.实现复用代码的有 $rootScope, localStorage；
3.angular提供了五种服务provider factory service constant value，其中前两种比较常用；factory服务的第二个参数相当于provider的$get
4.provider是最大的服务，可以进行配置,其他4中不能配置，factory是基于provider的，service是基于factory的
5. app.config(function (CalcProvider) { //在要配置的服务后面 增加Provider后缀
        //console.log(CalcProvider); //是provider的一个实例
        CalcProvider.currency = '£'
    });
    //CalcProvider 配置时这个名字是服务名+provider，固定写法；
6. app.provider('Calc',function () { //当我们将Storage放到控制器中，会自动调用$get方法
        this.currency = '$'; //在实例上增加了属性
        this.$get = function () {
            //var that = this; //谁调用的$get(实例)
            return {
                sum: (a,b) => this.currency +(a+b)
            }
        };
    });//provider默认就会被实例化
7.app.controller('oneCtrl',function ($scope,$http,Calc) {  //sum()
        console.log(Calc.sum(1,2));
    });
    app.controller('twoCtrl',function ($scope,$http，Calc) {
    console.log(Calc.sum(1,2));
    });
8. 运行顺序 Provider > config > $get   
```
14、实现控制器之间的交互有：$rootscope, 服务，事件
15、事件
```
<div ng-controller="parent">
    <input type="text" ng-model="total">
    <div ng-controller="child">
        玩具名称  {{toy.name}} <br>
        数量 <input type="text" ng-model="toy.count"> <br>
        价格 {{toy.price}}
    </div>
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    // $scope.$broadcast
    var app = angular.module('appModule',[]);
    app.controller('parent',function ($scope) {
        $scope.total = 10;
        //e代表的是事件源 自带的
        $scope.$on('买车',function (e,data,a) {$scope.total = data;});
    });
    app.controller('child',function ($scope) {
        $scope.toy = {name:'玩具车',count:1,price:10};
        $scope.$watch('toy.count',function (newVal) {
            $scope.$emit('买车',newVal*$scope.toy.price);
        });
    });
</script>
</body>

```