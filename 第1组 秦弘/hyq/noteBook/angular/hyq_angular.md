####angular框架的好处：
    简化代码，提高开发效率
####MVVM模式：  双向绑定数据（model view viewModel）
    MVVM是真正将页面与数据逻辑分离的模式，它把数据绑定工作放到一个JS里去实现，而这个JS文件的主要功能是完成数据的绑定，即把model绑定到页面的元素上。
####angular自带命令：
   1. ng-app='模块名'
        一般写于HTML标签上，表示加载页面时，告诉浏览器执行angular，此时会产生一个根作用域
        
   2. ng-model='自定义变量名'
        绑定数据 
        ng-model-options="{debounce:200}"
        延迟刷新数据 数据确定下来 过两秒刷新  
        
   3. contenteditable="true/fasle"标签属性
        表示此标签元素变成可编辑标签元素跟texrare 效果一样、
        
   4. {{变量名}}===》ng-bind="变量名"
            支持三目运算 赋值运算 算数运算
             使用{{变量名}}页面属性 会有闪烁问题
                解决方案：
                    在拥有{{变量名}}这个属性的父元素上添加一个属性[ng-cloak]前提是自己需要写样式
                    [ng-clock]{display:none}当数据编译完成后，页面会自动移除这个属性以及样式
                    
   5.  声明模块
   
            var app = angular.model("模块名"，[]，function(){});
            第一参数：
                模块名
            第二参数：
                模块依赖
            第三参数:
                配置函数
                
   6.   取出页面中的参数值：
   
                要取出页面中的参数值：前提是页面中是参数值必须是存放在作用域中【根作用域($rootscope) 模块作用域($scope)】
                $scope.属性名(获取)  $scope.属性名 = value  (设置值)
                
   7.   在模块中获取event事件中的属性：
   
                要想获取event中的属性/方法，必须是在调用此方法时以参数的形式传入($event[angular中固定写法])
                在页面元素中以angular形式绑定：ng-事件函数='需要调用模块中的方法名'(ng-click="Fn($event)")
                
   8.   循环：
   
            A(自定义) in arry(数据对象) track by  $index （$index是angular提供）
                或
            (key,value) in arry(数据对象) track by  key
            必须跟 ng-repeat一起使用 否者不生效
       
   9. find()[ECMAScript 6 中方法 带有循环的作用]
   
            找出第一个符合条件的数组成员，
            它的参数是一个回调函数，所有数组成员依次执行该回调函数，
            直到找出第一个返回值为`true`的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`。
            eg：
                var arr = [{name:1,age:2},{name:2,age:3},{name:2,age:3,gender:1}];
                arr.find(function (item,index) {
                        return item.name == 2;
                    });
    10. 过滤器：
    
             把小写字母转换成大写
            {{"asdasd" | uppercase}}  ===》 ASDASD
            
             把大写字母转换成小写
            {{"AASDASD"| lowercase}}  ===》 asdasd
            
             货币过滤器 默认在最前面增加$符号 3代表 千分符
            {{"123123.123"| currency:'￥':3}}  ===》 ￥123,123.123 
            限制过滤器
            {{"你好吗" | limitTo:2}} ===》  你好 
            json过滤器
            <pre>{{ {name:1,age:4} | json:4 }}</pre>  ===》 {
                                                              "name": 1,
                                                              "age": 4
                                                            }
            number过滤器
            {{123123|number}} ===》  123,123 
            日期过滤器
            {{1489568089171 | date:"yyyy年MM月 hh:mm:ss"}}  ===》XXXX 年 XX 月 XX:XX:XX

   10. 自定义过滤器：
        
            .filter('multiFirstAlterUppercase',function(){}) 
            eg：
                {{"   welcome, to bei-jing   " | multiFirstAlterUppercase}}  ===>    Welcome, To Bei-jing   
                <script>
                    var app = angular.module('appModule',[]);
                        app.filter('multiFirstAlterUppercase',function(){
                            return function(input){
                                input = input.replace(/^\w|\s\w/g,function(){
                                //将匹配到的内容转换成大写
                                    return arguments[0].toUpperCase();
                                });
                                return input;//返回的是最终的结果
                            }
                        });
                    </script> 
                    
                    
   11.  map数组的方法
   
      "修改"映射,返回是一个新的数组
            var newArr = [1,2,3,2,2,2].map(function (item,index) {
                 return `<li>${item}</li>`;//返回什么内容，就会将这一项变成什么内容
            });
            console.log(newArr);
    
    12. orderby:
    
            第一个参数可以有三种类型，分别为：function,string,array:
            第一种：
                function函数会遍历待排序的数组，并将返回的结果作为angular库函数中comparator的参数，进行比较排序。
                eg:
                    <div>
                        {{[{'name':'nick','age':'34'},{'name':'bob','age':'23'}]] | orderBy: sortObjectType}}
                    </div>
                    $scope.sortObjectType = function(obj){
                       return obj['name']
                    }
                    排序结果为：
                    [{"name":"bob","age":"23"},{"name":"nick","age":"34"}]
            第二种：
                     <div>
                         {{[{'name':'nick','age':'34'},{'name':'bob','age':'23'}]] | orderBy: 'name'}}
                     </div>
                     排序结果为：
                     [{"name":"bob","age":"23"},{"name":"nick","age":"34"}].
            第三种：
                    <div>
                        {{[{'name':'nick','age':34},{'name':'nick','age':23},{'name':'bob','age':23}]] | orderBy:[ 'name','age']}}
                    </div>
                    排序结果为：
                    [{"name":"bob","age":23},{"name":"nick","age":23},{"name":"nick","age":34}]
                    
              E - Element元素名称: <my-directive></my-directive>
							A - Attribute属性: <div my-directive="exp"> </div>
							C - Class: <div class="my-directive: exp;"></div>
							M - Comment注解: <!-- directive: my-directive exp -->
							
    内置属性：
            $watch()可以时时监控数据的动态
            $apply()通知服务可以刷新页面数据
            $http() 用于数据交互相当于ajax
                $http 返回都是promise对象 对象中有一个then方法，方法中有两个参数，第一个是成功的回调第二个是失败的回掉
                作为一个可信任的路径$sce.trustAsResourceUrl()
                百度jsonp接口 callback名字得是cb
                eg:
                    $http({
                    method: 'JSONP',
                    url: '',
                    }).success(function (msg) {
                        console.log(msg);
                    });
                    或者：
                    $http
                    .jsonp( $sce.trustAsResourceUrl('URL'),{jsonpCallbackParam: 'cb'})
                    .success(function (msg){
                        console.log(msg);
                    }); 
                    
   服务：
        angular提供了五种服务  "provider factory" service constant value
                provide：
                        provider是最大的服务，可以进行配置,其他不能配置
                        创建服务：
                            .provider("name",function(){
                                this.$get = function(){}//服务入口
                             })
                        配置服务用.config("nameProvider")
                        
                factory: 基于provider的 后面的函数就是provider的$get
                        service 是基于factory的  value是基于 factory的
                    .factory('name',function () {
                            return {
                           
                            }; 
                        });
        事件：(观察者模式)
            $on  $emit  $broadcast
                eg;
                    <!DOCTYPE html>
                    <html lang="en" ng-app="appModule">
                    <head>
                        <meta charset="UTF-8">
                        <title></title>
                    </head>
                    <body>
                    <div ng-controller="parent">
                        <input type="text" ng-model="total" ng-change="tellSon()">
                        <div ng-controller="child">
                            玩具名称  {{toy.name}} <br>
                            数量 <input type="text" ng-model="toy.count "> {{toy.count | number:0}}<br>
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
                            $scope.tellSon = function () {
                                $scope.$broadcast('给你钱',$scope.total)
                            };
                            $scope.$on('买车',function (e,data,a) {$scope.total = data;});
                        });
                        app.controller('child',function ($scope) {
                            $scope.$on('给你钱',function (e,data) {
                                $scope.toy.count =data/ $scope.toy.price
                            });
                            $scope.toy = {name:'玩具车',count:1,price:10};
                            $scope.$watch('toy.count',function (newVal) {
                                $scope.$emit('买车',newVal*$scope.toy.price);
                            });
                        });
                    </script>
                    </body>
                    </html>
                        
   
                     