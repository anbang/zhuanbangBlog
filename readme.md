#this is my blog  by node 【我的 node博客项目】

## express 用的是老版本的；
  - 开始是用 npm install -g express-generator来安装用的；但是每次用的时候，出现不是可用的命令行；折腾半天没有解决；我就用老版本了
  - 我用了；npm install -g express@3.5.0  这个版本来操作的；
  - 后来解决好了，可以用npm install -g express-generator了

# 1、初始化一个仓库

 - express -e zhuanbangBlog
 - cd zhuanbangBlog && npm install
 - 设置环境变量并启动服务器,在命令行中执行下列命令
 - SET DEBUG=zhuanbangBlog:* & npm start
 - 然后会显示zhuanbangBlog:server Listening on port 3000 +0ms
 - 在浏览器里访问 http://localhost:3000 就可以显示欢迎页面
    - Express
    - Welcome to Express
 - 再命令行输入 SET PORT=5000 可以把默认的3000端口改为5000
 - 通过命令行简历一个忽略文件。touch .gitignore 忽略
    - node_modules
    - .idea
 - 上面就是用express生成器生成了一个使用ejs模板的示例工程。

### 提交本地仓库
- git init 初始化git仓库
- git add -A 把所有的文件添加到暂存区
- git commit -m"初始化博客" 把所有的修改添加到历史区

### git 仓库关联github
- git remote add origin XXXXX.git 添加远程仓库的关联
- git push -u origin master 把本地的仓库推送到远程服务器上去

#2、生成文件说明
- app.js：express的主配置文件
- package.json：存储着工程的信息及模块依赖，当在 dependencies 中添加依赖的模块时，运行 npm install，npm 会检查当前目录下的 package.json，并自动安装所有指定的模块
- node_modules：存放 package.json 中安装的模块，当你在 package.json 添加依赖的模块并安装后，存放在这个文件夹下
- public：存放 image、css、js 等文件
- routes：存放路由文件
- views：存放视图文件或者说模版文件
- bin：可执行文件，可以从此启动服务器的

#3、功能分析
- 搭建一个简单的具有多人注册、登录、发表文章、登出功能的博客。

##设计目标
- 未登录：主页导航显示 首页、注册、登陆，下面显示已发表的文章、发表日期及作者。
- 登陆后：主页导航显示 首页、发表文章、退出，下面显示已发表的文章、发表日期及作者。
- 用户登录、注册、发表成功以及登出后都返回到主页。

# 4、路由规划
我们已经把设计的构想图贴出来了，接下来的任务就是完成路由规划了。路由规划，或者说控制器规划是整个网站的骨架部分，因为它处于整个架构的枢纽位置，相当于各个接口之间的粘合剂，所以应该优先考虑。

- / ：首页
- /users/login ：用户登录
- /users/reg ：用户注册
- /articles/post ：发表文章
- /articles/logout ：登出

app.js中有用的核心代码

- app.use('/', routes);//根目录的路由
- app.use('/users', users);//用户的路由的目录文件用user.js来控制
- app.use('/articles', articles);//视图中的articles文件夹用articles来控制；

http://localhost:3000/users/reg

![](http://i.imgur.com/Hounhie.png)

http://localhost:3000/users/login

![](http://i.imgur.com/dnWDZm7.png)

http://localhost:3000/articles/add

![](http://i.imgur.com/LGEzIus.png)

# 5、开发准备工作 ,把首页，登录页，注册页，发表文章页面做好 [git版本存档](https://github.com/Broszhu/zhuanbangBlog/commit/3338397799c6452535c26c49101fb667d996aaaf)
MD5加密算法

- var crypto = require('crypto');
- var content = 'password'
- var md5 = crypto.createHash('md5');
- md5.update(content);
- var d = md5.digest('hex'); 

SHA1加密例程

- var crypto = require('crypto');
- var content = 'password'
- var shasum = crypto.createHash('sha1');
- shasum.update(content);
- var d = shasum.digest('hex');


### 配置bower
 执行bower init

一路回车就可以生产一个bower.json 文件；然后再通过 

 - touch  .bowerrc

创建一个.bowerrc文件；里面的内容输入

- {"directory":"./public/lib"}

这表示以后bower安装的模块都安装在./public/lib下面。

然后通过 bower install bootstrap --save  安装bootstrap文件；因为家了--save。不但会安装bootstrap,还会安装依赖的jquery；

然后通过拼接文档的方式来拼index.ejs；内容如下

- <% include include/header.ejs%>
- < div class="container">
-   这是主页的内容哦！
- < /div>
- <% include include/footer.ejs%>

下面是这里的样子；

![](http://i.imgur.com/ltL6U3w.png)

注册页面如下

![](http://i.imgur.com/RxRrA5Z.png)

登录页面如下
![](http://i.imgur.com/pHfkHm1.png)

发表文章页面如下
![](http://i.imgur.com/LmpeCKk.png)

# 6，博客注册功能完善，链接mongodb数据库

### 链接数据库

安装mongodb模块到node_modules下面并把此配置添加到package.json文件中

- npm install mongoose --save

mogodb的安装启用；在D:\mongodb\data文件夹下

- 命令窗体中输入 mongod --dbpath=D:\Mongodb\data 按回车键
注： --dbpath后的值表示数据库文件的存储路径 而且后面的路径必须存在否则服务开启失败 
借助mongoose工具mongoVUE来管理；

- 在根目录下新建一个db文件夹，并且新建一个index.js文件；
 

    	var mongoose=require('mongoose');
    	mongoose.connect('mongodb://127.0.0.1:27017/zhuanbangblog');
    	mongoose.model('User',new mongoose.Schema({
	    	username:String,
	    	password:String,
	    	email:String
    	}));
		global.Model=function(modName){
		    return mongoose.model(modName);//一个参数是获取model值；并且放在global用；可以直接在users.js里用
		}

//model2个是定义，一个是取值；

global.Model=function(modName){
    return mongoose.model(modName);//一个参数是获取model值；并且放在global用；可以直接在users.js里用
}
 

 一个参数是获取model值；并且放在global用；可以直接在users.js里用；model2个值是定义，一个是取值；

在app.js里require('./db'); 引入一下db里面的index.js;./db和./db/index.db是一样的效果的；

###user.js里的登录如下；
    router.post('/reg', function (req, res) {
    var user=req.body;//获取用户提交过来的注册表单
    new Model('User')(user).save(function(err,user){
    if(err){
    res.redirect('/users/reg')
    }else{
    res.redirect('/users/login')
    }
    })
    });


###mongoVUE中的数据如图

![](http://i.imgur.com/f9LFaeF.png)
