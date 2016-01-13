title: 初识 Gulp
date: 2016-01-01 20:23:28
tags:
---
元旦放假想自己用 Angular 做一个简单的个人任务管理系统，之前 用 ng1 做过一个 TODO，实习公司里也是用 ng1 的，但是老大没让我写过什么功能模块，SaaS 里都是在改啊改 bug ，还是自己学学，写写小东西吧。正好用一下 Gulp ，本来想学下 webpack 的，感觉还不太成熟，也没太了解过，自己之前也就用过 grunt ，而且基本上都是别人配置好的。听过 Gulp 炒鸡容易上手，就把这次的使用情况记录下来吧。

首先全局安装下 gulp

``` javascript
npm install gulp -g
```

新建一个文件夹并初始化一个 `packge.json` 文件

``` javascript
mkdir sys && cd sys
```

项目的目录结构如下

``` javascript
app/   //存放静态文件
    components  /
    css/
    images/
    scripts/
    scss/
    index.html
dist/  //存放编译后的文件
node_modules/
gulpfile.js
package.json
```

新建一个 `gulpfile.js` 文件，并编辑

``` javascript
var gulp = require('gulp') 
//在项目中引入 gulp
```

由于我在项目中使用的是`sass`，所以我想要一个插件可以帮我把`.scss`编译成`css`文件

``` javascript
npm install gulp-sass --save-dev
//安装 gulp-sass 插件
//以下省略安装步骤

//js
var sass = require('gulp-sass');
//引入gulp-sass插件
```
<!--more-->

我们可以创建一个任务来使用 `gulp-sass` 插件， 想要创建一个任务非常简单

``` javascript
gulp.task('task-name', function() {
	doing...
	//so easy~
})

//我们来创建一个 'sass' 任务来编译 sass/scss 文件,注意gulp的链式结构
gulp.task('sass', function() {
	return gulp.src('app/scss/**/*.scss')  //指定scss来源
	    .pipe(sass())   //执行一次gulp-sass编译命令
	    .pipe(gulp.dest(app/css))   //指定编译后的文件目录
})
```

这样一来我就可以在修改`.scss`文件之后在iTerm里用`gulp sass`命令来编译`scss`文件了

但是每一次保存`.scss`之后都要手动编译一次，这样太没有效率，我们可以使用gulp的watch方法来监听我们指定的文件

``` javascript
gulp.task('watch', function() {
    gulp.watch('app/scss/**/*.scss', ['sass'])
    //先指定文件目录，然后添加要执行的方法
})
```

为了提高效率，我们还可以使用`browser-sync`来在指定文件发生更改之后来自动刷新一下页面

``` javascript
var browserSync = require('browser-sync');
//制定一下文件根目录
gulp.task('browserSync', function() {
    browserSync: ({
        server: {
            baseDir: 'app'
        }
    })
})

//稍微做一点修改就可以在编译scss文件之后自动刷新页面
gulp.task('sass', function() {
    ...
    .pipe(browserSync.reload({
        stream: true
    }))
})

//同时我们还要做一点修改，来让watch命令执行之前先执行一下browserSync和sass命令，相当于给watch命令添加了依赖吧
gulp.task('watch', ['browserSync', 'sass'], function() {
    ...
})
```

现在每次修改保存scss文件之后gulp都会自动编译成css文件并自动刷新页面了

现在我们添加其他需要自动刷新的文件

``` javascript
gulp.task('watch', ['browserSync', 'sass'], function() {
    gulp.watch('app/scss/**/*.scss', ['sass']);
    gulp.watch('app/components/**/*.html', browserSync.reload);
	gulp.watch('app/*.html', browserSync.reload);
	gulp.watch('app/scripts/**/*.js', browserSync.reload);
})
```