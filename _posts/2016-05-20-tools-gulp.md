---
layout: post
title: gulp
excerpt: 
category: tools
---

#### 新建项目
到 workspace 中建一个文件夹，源码和项目会放在里面。然后在文件夹上右键开命令后窗口。
```
$ cnpm install gulp -g	全局安装gulp
$ gulp --help		检查本地没有安装过
$ cnpm i gulp		在本目录安装gulp
$ npm init	  初始化，回车...最后yes，然后ctrl+c结束。
```

此时文件夹中会出现 package.json 配置文件

将以下代码复制到package.json里的对应位置。然后$cnpm install，快捷安装需要的组件。
```
 "devDependencies": {
    "gulp": "^3.9.1",
    "gulp-minify-css": "^1.2.4",
    "gulp-sass": "^2.3.1",
    "gulp-uglify": "^1.5.3",
    "gulp-webpack": "^1.5.0",
    "gulp-webserver": "^0.9.1",
    "imports-loader": "^0.6.5",
    "vinyl-named": "^1.1.0"
  },
```
在node_modules同级目录下创建 gulpfile.js 和 index.html

#### gulp 配置
gulpfile.js 文件如下，相关src文件夹要自己创建，只更改项目根目录下的 index.html，会同步到项目文件中，所以，js，css引人的路径要用项目中index需要的路径，例如：`prd/scripts/app.js`
```
//1、引入gulp
var gulp = require('gulp');
//2、引入gulp-webserver
var webserver = require('gulp-webserver');
//4、引入gulp-sass
var sass = require('gulp-sass');
//5、引入css压缩
var minifyCSS = require('gulp-minify-css');

//6、引入js相关
var webpack = require('gulp-webpack');
var named = require('vinyl-named');
var uglify = require('gulp-uglify');

//7、mock数据--到webserver中配置
var fs = require('fs');
var url = require('url');

//1-----index.html拷贝到webapp中
gulp.task('copy-index',function(){
	//建一个任务，copy文件。
	//src----source,把src的内容放到管道里。dest表示放到webapp文件夹里。
	//用git的命令$ gulp copy-index
	gulp.src('./index.html')
	.pipe(gulp.dest('./webapp/'));
});

//2-----修改gulp命令执行的内容
gulp.task('default',['watch','webserver']);

gulp.task('webserver',function(){
	gulp.src('./')
	.pipe(webserver({
		host:'localhost',//定义域名、端口号
		port:80, //浏览器中localhost的默认页面，显示目录
		directoryListing:{
			enable:true,
			path:'./'
		},
		//保存时自动同步
		livereload:true,
		//7-----mock数据
		middleware:function(req,res,next){
			var urlObj = url.parse(req.url,true);
			var method = req.method;
			//告诉浏览器返回json串,json文件路径
			switch(urlObj.pathname){
				case '/api/homeList.php':
					res.setHeader('Content-Type','application/json');
					fs.readFile('./mock/home.json','utf-8',function(err,data){
						res.end(data);
					});
					return;
				case '/api/xxxxxx.php'://.....其他端口
					return;
			}
			next();//node的语法，让上面的代码执行
		}
	}));
});

//3-----同步到项目文件夹 webapp
gulp.task('watch',function(){
	gulp.watch('./index.html',['copy-index']);
	//styles/**/*代表styles目录下所有文件
	gulp.watch('./src/styles/**/*',['scss','css']);
	gulp.watch('./src/scripts/**/*',['packjs']);
})

//4-----引入gulp-sass
//定义入口文件,然后到这个路径下去建文件
var cssFIles = [
	'./src/styles/app.scss'
];
// 5-----拿到文件，放到管道中编译，然后放到dest中的目录里
// 压缩加在这里，重启服务，修改scss文件有触发压缩
gulp.task('scss',function(){
	return gulp.src(cssFIles)
	.pipe(sass().on('error',sass.logError))
	.pipe(minifyCSS())
	.pipe(gulp.dest('./webapp/prd/styles/'));

});
//css文件压缩、发布
gulp.task('css',function(){
	return gulp.src('./webapp/styles/**/*.css')
	.pipe(minifyCSS())
	.pipe(gulp.dest('./webapp/prd/styles/'));
});


//6、js打包，同步要加上watch
var jsFiles = [
	'./src/scripts/app.js'
];
gulp.task('packjs',function(){
	gulp.src(jsFiles)
	.pipe(named())
	.pipe(webpack({
		output:{//输出时的文件名
			filename:'[name].js'
		},//.js正则转义,要安装loader
		module:{
			loaders:[{
				test:/\.js$/,
				loader:'imports?define=>false'
			}]
		},
		resolve:{//别名，用zepto这种简短的名字代替长文件名
			alias:{
				'./a/b/c/zepto.js':'zepto'
			}
		},
		devtool:'#eval-source-map'
	})).pipe(uglify().on('errer',function(e){
		//回调函数，返回错误的行号，错误信息
		console.log('\x07',e.lineNumber,e.message);
	})).pipe(gulp.dest('./webapp/prd/scripts/'));
});
//8、控制版本号，安装gulp-rev，gulp-rev-collector
var rev = require('gulp-rev');
var revCollector = require('gulp-rev-collector');

//dist---distribute发布
var cssDistFiles = [
	'./webapp/prd/styles/app.css'
];
var jsDistFiles = [
	'./webapp/prd/scripts/app.js'
];
//manifest()使css/js文件生成json文件，放到ver里专门给后端用
gulp.task('ver',function(){
	gulp.src(cssDistFiles)
	.pipe(rev())
	.pipe(gulp.dest('./webapp/prd/styles'))
	.pipe(rev.manifest())
	.pipe(gulp.dest('./webapp/ver/styles'));

	gulp.src(jsDistFiles)
	.pipe(rev())
	.pipe(gulp.dest('./webapp/prd/scripts'))
	.pipe(rev.manifest())
	.pipe(gulp.dest('./webapp/ver/scripts'));
});

//自动改index.html中的版本号
gulp.task('html',function(){
	gulp.src(['./webapp/ver/**/*.json','./webapp/*.html'])
	.pipe(revCollector())
	.pipe(gulp.dest('./webapp'))
})

//用gulp min 命令执行版本号相关操作
gulp.task('min',['ver','html']);//开发完才min一层，所以不用放到default中
```


