##angular
>指令分为两类 自带指令  自定义指令
自定义指令
    装饰型指令 赋予标签一些功能  link函数
    组件式 把标签 替换成一个组件 template


```
app.directive('myDrag',function () {
        return {
            replace:true, //将原指令标签替换掉，要求模板只能有一个根节点
            restrict:'EACM', //限制使用范围 默认范围是EA,范围的标识必须大写
            //templateUrl:`tmpl/panel.html`,//ajax获取过来的
            template:`<div>
                        <div>Hello</div>
                        <div>zfpx</div>
                      </div>` //模板的根节点只能有一个
            link:function ($scope,element,attrs) {//形参 ,在link函数中可以操作DOM元素
                // 1.代表的是当前指令所在的作用域
                // 2.element代表当前指令所在的标签，是一个jquery对象
                element.css('outline','5px dashed yellowgreen');
                //angular.element == $
                angular.element(document).on('click',function () {
                    alert('click')
                });
                // 3.代表当前指令上所有的属性集合
                console.log(attrs);
            }
            //scope:{}, //给当前指令产生一个作用域 {}  默认值是false
            // {} 不能获取父作用域的属性 相当于开辟了一个和$rootScope平级的作用域
            scope:{ //@符引用的是字符串
                name:'@st',
                //title:'@' //名字相同可以去掉后面的
                // = 取的是变量
                title:'=article' //scope.title = $scope.t
            }, //如果通过属性传递的值 可以再scope:{} 里快速获取到
            
            transclude:true//会产生一个作用域,会将指令中夹着的部分 插入到带有ng-transclude的标签内
        }
    });
```

>控制div的显示和隐藏，学指令和指令之间的交互-->

- ng-class（动态）和class不冲突（静态）
- {true:'block',false:'none'}[flag]
- {block:flag,none:!flag}
```
    ng-show/ng-hide ng-if
    频繁切换用show/hide  操作的是样式
    一次就确定下来的内容 如果值为false内部代码不执行 操作的dom, repeat经常和if连用  if会产生一个作用域
```



## yarn
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









>directive

```
 app.directive('train',function () {
        return {
            restrict:'A',
            //^自己身上找找不到向上找
            require:'^?girl',//在当前指令上找girl，找到则会在link函数的第四个参数增加girl控制器的实例
            //require:'^girl',//在当前指令上找girl，找到则会在link函数的第四个参数增加girl控制器的实例
            link:function (scope,element,attrs,gCtrl) { //如果没有依赖到使用的是? 则得到的值是null，否则报错
                console.log(gCtrl);
                gCtrl.collect('火车');
            }
        }
    });
```

>$watch (脏值检测)
*  脏值检测 对比两次的变化，触发对应的回调函数,将所有数据放在一个数组中，有一个变化 所有$watchers执行一遍
     vue 自带的defineProperty set get + 观察者
```
 app.run(function ($rootScope) {
        /*  ng-change="change()"触发的事件
            $rootScope.change = function () {
                console.log($rootScope.name);
            };
        */
       /*  实时监控
        $rootScope.$watch(function () {
            return $rootScope.name;
        },function (newVal,oldVal) {
            console.log(newVal,oldVal);
        });*/

        //放入的是作用域上的一个变量字符串
        /*$rootScope.$watch('name',function (newVal,oldVal) {
            console.log(newVal,oldVal);
        })*/
    })
```

>$apply (手动刷新视图)
* {{d | date:'yyyy-dd hh:mm:ss'}}
```

app.controller('myCtrl',function ($scope,$interval,$timeout,$sce) { //服务
        var timer = $interval(function () { //不是angular自带的方法，数据更新不会影响视图
            //通知视图刷新
            $scope.d = Date.now();
            //$scope.$apply(); //手动刷新视图
        },1000);
        $interval.cancel(timer); //取消定时器
    });
```

>$http()
```
$scope.search = function () { //输入时触发
            //作为一个可信任的路径$sce.trustAsResourceUrl()
            //百度jsonp接口 callback名字得是cb
            //$http 返回都是promise对象 对象中有一个then方法，方法中有两个参数，第一个是成功的回调第二个是失败的回掉
            $http.jsonp(
                    $sce.trustAsResourceUrl('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd='+$scope.query)
                    ,{jsonpCallbackParam: 'cb'}
            ).then(function (res) { //成功
                $scope.arrs = res.data.s;
            },function (err) { //失败
                console.log(err);
            });
        }
```

>$server
```
    1.服务是单例的 ，方法都是公用的
    2.实现复用代码 $rootScope, localStorage
    3.angular提供了五种服务  "provider factory" service constant value
    4.provider是最大的服务，可以进行配置,其他4中不能配置
    //运行顺序 new Provider > config > $get
```
>factory
```
//factory 不具有配置能力，因为$get方法在我们的config方法后执行 配置后会被覆盖掉
    /* app.config(function (CalcProvider) { //CalcProvider  provider 的实例
        CalcProvider.currency = '%'; //未生效的
    });*/
    app.factory('Calc',function () { //factory是基于provider的 后面的函数就是provider的$get
        var currency = '$';
        return {
            "+" : (a,b)=>currency+(a+b)
        }; //这里返回的就是最后注入到控制的内容
    });
    // 最大的是provider factory是基于provider的  service 是基于factory的  value是基于 factory的
    //控制器间的交互 1.$rootScope, 服务， 事件(观察者模式) $on  $emit  $broadcast
    app.controller('myCtrl',function ($scope,Calc) {
        console.log(Calc["+"](1,2));
    });
```

》事件（$on,$emit,$broadcast）
```
$on:监听$emit或者$broadcast传递过来的事件
$emit:由下往上传递事件
$broadcast:由上往下传递事件

 $scope.$on('买车',function (e,data,a) {$scope.total = data;});
 $scope.$emit('买车',newVal*$scope.toy.price);
 
 $scope.$broadcast('给你钱',$scope.total)
 $scope.$on('给你钱',function (e,data) {
            $scope.toy.count =data/ $scope.toy.price
        });
```