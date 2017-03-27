
线程：
    node主线程是单线程 进程中包含线程 在node中一个进程只能包含一个线程，但允许开子进程

同步：
    在js中 代码是从上往下执行的 但如果代码中存在同步跟异步方法时，则先执行同步方法在执行异步方法，异步不会阻塞主线程
    
 在node中 没有window属性node中的全局是global，用var申明的变量也不会存放在全局中，在node中 setTimeout等异步方法中的this都是指向方法本身，要改变方法中this指向只能使用bind
 
        eg：
            function Fn(who,val){
                console.log(who,val)===>me,name;
            };
            setTimeout(fn,2000,'me','name').bind()
        
  //process.pid 当前的进程id
  //process.kill()杀死某个进程
  //process.exit()退出自己
  //process.cwd() current working directory 可以更改
  //process.chdir() 改变目录
  
   如何定义模块    
      创建一个js文件，每个文件就是一个模块，多个模块可以组成一个包
      
      node中导出一个模块使用：module.exports = 模块名<方法名>
      在另外一个模块使用上一个模块的方法使用：var A = require('./模块的文件名'); //同步方法
       此时的A就是你要使用上一个模块的方法<对象>
       
   nrm源切换工具
   
        安装nrm：
             npm install nrm -g  ===> npm i nrm -g
             
        查看源路径：
            nrm ls
            
        切换源：
            nrm use 源名称
            
        使文件已PPT格式展示：
            安装一个插件：
             npm install nodeppt  -g
        使用命令创建一个外部server
            npm  install http-server -g
            http-server -p  端口号  此时外部就可以用服务创建的地址访问你本地的程序
            
  往npm官网上发送直接的模块：
        初始化文件：
            npm init -y
            
        注意：必须使用npm源 在初始化的.json 文件中 文件名必须是小写的 不能有特殊字符
        添加用户名：
            npm addUser
            此时会提示让你输入用户名、密码、邮箱 如果没有 则注册 有则登陆
            
            最后一步推送：
                npm publish
            撤回推送：
                npm unpublish  --force
                
                
        使用bower管理前端文件（可以指定安装文件的目录）
            安装：
                npm  install  bower  -g
            在文件中初始化
                bower init 
            指定文安装的目录
                必须新建一个文件 .bowerrc<文件名固定>
                    {"directory":"需要安装文件的路径"}
                默认安装到bower_component下
             
        发布博客：
            安装一插件：
                npm install hexo-cli -g
                在GitHub上创建一个仓库文件名字是：GitHub用户名+.github.io<固定格式>
                初始化文件
                    hexo init
                    hexo server 启动服务 在本地浏览
                发布到git上还需要一插件：
                    npm install hexo-deployer-git --save
                配置_config.yml
                deploy:
                  type: git
                  repo: https://username:password@github.com/zuyuan/zuyuan.github.io.git
                  branch: master
                  message: push
                  使文件生效：
                    hexo g
                  发布文件：
                    hexo deploy
                  此时博客创建成功
                  修改博客中的文章：
                    在文件中找到source中文件是以.md格式
                    
                
             
        
            
        