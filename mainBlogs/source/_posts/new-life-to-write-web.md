---
title: new life to write web
date: 2016-12-08 10:18:29
tags:
---
# Node.js 的征服


## 写javascript另我非常苦恼
      以前我一直写C/C++/C#或者Java，这些语言有完善的包管理（import or include）,
      很好的debug&build工具而且deploy也非常方便。
      第一次写html和javascript，大概是十年前，给我的感觉就是非常的痛苦。
      在html里面嵌入javascript，实现一些特效。调试，加载javascript都是意见很痛苦的事情，还有浏览器。
      这么多年一直没接触web的开发，内心也不太愿意接触，直到前年要做一个demo，
      我接触了YUI，jquery和bootstrap，那真是太爽了。
      但是也有问题
        1. 引用很多js文件，依赖关系有问题
        2. 使用很多jquery plugin，各种版本的差异
        3. 这样<script>的tag语法，对js的引用顺序令人抓狂
        4. 修改html和js后，手动刷新浏览器，有的时候明明修改了代码，可是显示就有问题
        
## Node.js改变一切了

    Nodejs及其构建起来的生态，简直是前端的福音。
    
## 给我太多的震撼
先来说说如何安装nodejs及npm(还是省掉吧)

这么多天，我一直在npm，bowser，gulp，seajs，webpack，livereload还有browser-sync这些全新的名词里面苦恼。

另外还有commonjs，AMD和CMD。这里省去...很多很多的优秀开源项目

### 原来这些在nodejs基础上的工具，解决我以前对于写前端所有的不适应。

* 包管理 npm bowser等 —— 我主动放弃了bowser，用npm一统天下吧
* JS的规范CommonJS，AMD还有CMD，解决了如何引用javascript模块的问题。（终于科研期待像import一样引用javascript库）。对比了这三个规范的实现，nodejs，requirejs和seajs，暂时我先使用了CommonJS的语法，简单的使用nodejs的。这里又有很多新东西，再研究......
* webpack等解决了如何打包js，css（less&saas）的问题。举个例子，如该模块里面只使用了js库里面的少部分内容，webpack可以打包用到的js，实在是方便。当然Browserify也可以。
* 还缺少一个构建工具，来完成webpack的工作，js缩小，编译css和压缩图片等等工作。它就是gulp。
* gulp还有一个watch的功能，可以监视哪些文件变化，然后执行相应的任务
* 最后如何文件变化，需要浏览器自动刷新，用browser-sync神器。（舍弃了livereload）


# 是不是感觉世界很神奇？
到底如何来做呢？
#具体的例子

#### *安装


安装主件到系统（OS）层级，直接用command可以调用
    
    sudo npm install -g gulp

    sudo npm install -g webpack

安装project层级
    
    npm install gulp --save-dev

    npm install webpack --save-dev

    npm install gulp-webpack --save-dev

    npm install browser-sync --save-dev

安装需要的lib
    npm install jquery amazeui --save
#### *构建工程


    mkdir projectname
    cd projectname
    npm init
 以下在工程根目录新建文件
#### * cats.js 自己的一个js库
```javascript
    var cats = ['dave ', 'henry ', 'martha '];
    module.exports = cats;
```

#### *app.js 整个工程的js入口，引用了cats.js,还有类库
```javascript
cats = require('./cats.js');
console.log(cats);

var $ = require('jquery');
var AMUI = require('amazeui');

$('#ct').html(cats);

$( "#div1" ).html( "<span class='red'>Hello <b>Sonic from honeywell</b></span>" );
```

#### index.html
```html
 <!DOCTYPE html>
 <html>
     <head>
         <meta charset="utf-8">
     </head>
     <body>
         <div id="chnage">This is my change from sonic</div>
        <div id="ct"></div>
        <div id="div1"></div>
         <script src="./dist/app.bundle.js" charset="utf-8"></script>
     </body>
 </html>
```
#### webpack.config.js webpack的配置文件，将app.js打包后生成新的js文件
```javascript
 module.exports = {
     entry: './app.js',
     output: {
         // path: './bin',
         filename: 'app.bundle.js'
     }
 };
```

#### gulpfile.js 构建的文件，里面调到webpack和browserSync
```javascript
var gulp = require('gulp');
var webpack = require('gulp-webpack');
var browserSync = require('browser-sync').create();


//webpack task
gulp.task('webpack', function() {

return gulp.src('./app.js')
    .pipe(webpack(require('./webpack.config.js')))
    .pipe(gulp.dest('dist/'));

});


// create a task that ensures the `js` task is complete before
// reloading browsers
gulp.task('js-watch', ['webpack'], function (done) {
    browserSync.reload();
    done();
});


gulp.task('html-watch', function (done) {
    browserSync.reload();
    done();
});


gulp.task('css-watch', function (done) {
    browserSync.reload();
    done();
});



// use default task to launch Browsersync and watch JS files
gulp.task('watch', function () {

    // Serve files from the root of this project
    browserSync.init({
        server: {
            baseDir: "./"
        }
    });

    // add browserSync.reload to the tasks array to make
    // all browsers reload after tasks are complete.
    gulp.watch("./*.js", ['js-watch']);
    gulp.watch("./*.html", ['html-watch']);

});


// //watch task
// gulp.task('watch', function() {

//   // Watch .js files
//   gulp.watch('./*.js', ['webpack']);

//   // // Watch .scss files
//   // gulp.watch('src/scss/*.scss', ['sass']);

//   // // Watch image files
//   // gulp.watch('src/images/**/*', ['images']);

// });

//examle of default

// gulp.task('default', function() {
//   // place code for your default task here
// });


// Default Task
gulp.task('default', ['webpack','watch']);
```
# 最有在你的工程目录下，执行gulp命令，查看结果。修改某个html或者html文件，浏览器立马更新了。
