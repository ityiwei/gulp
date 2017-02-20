# gulp
Gulp使用入门介绍JS/CSS/IMG方法


Gulp使用入门操作十一步压缩JS
前提需要安装nodejs
一. 全局安装Gulp
npm install -g gulp
二、新建一个 gulpfile.js 文件
chapter2
└── gulpfile.js

三、在 gulpfile.js 中编写代码
// 获取 gulp
var gulp = require('gulp')
require() 是 node （CommonJS）中获取模块的语法。
在 gulp 中你只需要理解 require() 可以获取模块。
四、获取 gulp-uglify 组件
// 获取 uglify 模块（用于压缩 JS）
var uglify = require('gulp-uglify')

五、创建压缩任务
// 压缩 js 文件
// 在命令行使用 gulp script 启动此任务
gulp.task('script', function() {
// 1. 找到文件
gulp.src('js/*.js')
// 2. 压缩文件
.pipe(uglify())
// 3. 另存压缩后的文件
.pipe(gulp.dest('dist/js'))
})
gulp.task(name, fn) - 定义任务，第一个参数是任务名，第二个参数是任务内容。
gulp.src(path) - 选择文件，传入参数是文件路径。
gulp.dest(path) - 输出文件
gulp.pipe() - 管道，你可以暂时将 pipe 理解为将操作加入执行队列
六、跳转至 gulpfile.js 所在目录
打开命令行使用 cd 命令跳转至 gulpfile.js 文件所在目录。
例如我的 gulpfile.js 文件保存在 D:\wamp\www\BootsDataTable\gulpfile.js。
那么就需要在命令行输入
cd D:\wamp\www\BootsDataTable

七、使用命令行运行 script 任务
在控制台输入 gulp 任务名 可运行任务，此处我们输入 gulp script 回车。
注意：输入 gulp script 后命令行将会提示错误信息
[09:38:12] Local gulp not found in D:\wamp\www\BootsDataTable
[09:38:12] Try running: npm install gulp

八、安装 gulp-uglify 模块
因为我们并没有安装gulp gulp-uglify 模块到本地，所以找不到此模块。
使用 npm 安装 gulp gulp-uglify 到本地
npm install gulp
npm install gulp-uglify --save dev
安装成功后你会看到如下信息：
test@1.0.0 D:\wamp\www\BootsDataTable
`-- gulp-uglify@2.0.0
+-- gulplog@1.0.0
| `-- glogg@1.0.0
+-- has-gulplog@0.1.0
| `-- sparkles@1.0.0
+-- lodash@4.15.0
+-- make-error-cause@1.2.1
| `-- make-error@1.2.1
+-- through2@2.0.1
| +-- readable-stream@2.0.6
| | +-- core-util-is@1.0.2
| | +-- inherits@2.0.3
| | +-- isarray@1.0.0
| | +-- process-nextick-args@1.0.7
| | +-- string_decoder@0.10.31
| | `-- util-deprecate@1.0.2
| `-- xtend@4.0.1
+-- uglify-js@2.7.0
| +-- async@0.2.10
| +-- source-map@0.5.6
| +-- uglify-to-browserify@1.0.2
| `-- yargs@3.10.0
| +-- camelcase@1.2.1
| +-- cliui@2.1.0
| | +-- center-align@0.1.3
| | | +-- align-text@0.1.4
| | | | +-- kind-of@3.0.4
| | | | | `-- is-buffer@1.1.4
| | | | +-- longest@1.0.1
| | | | `-- repeat-string@1.5.4
| | | `-- lazy-cache@1.0.4
| | +-- right-align@0.1.3
| | `-- wordwrap@0.0.2
| +-- decamelize@1.2.0
| `-- window-size@0.1.0
+-- uglify-save-license@0.4.1
`-- vinyl-sourcemaps-apply@0.2.1
npm WARN test@1.0.0 No description
npm WARN test@1.0.0 No repository field.
在你的文件夹中会新增一个 node_modules 文件夹，这里面存放着 npm 安装的模块。
目录结构：
├── gulpfile.js
└── node_modules
└── gulp-uglify
接着输入 gulp script 执行任务
gulp script
[09:50:02] Using gulpfile D:\wamp\www\BootsDataTable\gulpfile.js
[09:50:02] Starting 'script'...
[09:50:02] Finished 'script' after 21 ms

九、编写 js 文件
我们发现 gulp 没有进行任何压缩操作。因为没有js这个目录，也没有 js 目录下的 .js 后缀文件。
创建 a.js 文件，并编写如下内容
// a.js
function demo (msg) {
alert('--------\r\n' + msg + '\r\n--------')
}
demo('Hi');
目录结构：
├── gulpfile.js
├── js
│ └── a.js
└── node_modules
└── gulp-uglify
接着在命令行输入 gulp script 执行任务
gulp 会在命令行当前目录下创建 dist/js/ 文件夹，并创建压缩后的 a.js 文件。
目录结构：
├── gulpfile.js
├── js
│ └── a.js
├── dist
│ └── js
│ └── a.js
└── node_modules
└── gulp-uglify
dist/js/a.js
function demo(n){alert("--------\r\n"+n+"\r\n--------")}demo("Hi");
十、检测代码修改自动执行任务
jjs/a.js一旦有修改 就必须重新在命令行输入 gulp script ，这很麻烦。
可以使用 gulp.watch(src, fn) 检测指定目录下文件的修改后执行任务。
在 gulpfile.js 中编写如下代码：
// 监听文件修改，当文件被修改则执行 script 任务
gulp.watch('js/*.js', ['script']);
但是没有命令可以运行 gulp.watch()，需要将 gulp.watch() 包含在一个任务中。
// 在命令行使用 gulp auto 启动此任务
gulp.task('auto', function () {
// 监听文件修改，当文件被修改则执行 script 任务
gulp.watch('js/*.js', ['script'])
})
接着在命令行输入 gulp auto，自动监听 js/*.js 文件的修改后压缩js。
D:\wamp\www\BootsDataTable>gulp auto
[10:09:04] Using gulpfile D:\wamp\www\BootsDataTable\gulpfile.js
[10:09:04] Starting 'script'...
[10:09:04] Finished 'script' after 16 ms
此时修改 js/a.js 中的代码并保存。命令行将会出现提示，表示检测到文件修改并压缩文件。
D:\wamp\www\BootsDataTable>gulp script
[10:23:09] Using gulpfile D:\wamp\www\BootsDataTable\gulpfile.js
[10:23:09] Starting 'script'...
[10:23:09] Finished 'script' after 64 ms
[10:23:31] Starting 'script'...
[10:23:31] Finished 'script' after 7.18 ms
至此，我们完成了 gulp 压缩 js 文件的自动化代码编写。
注意：使用 gulp.watch 后你的命令行会进入“运行”状态，此时你不可以在命令行进行其他操作。可通过 Ctrl + C 停止 gulp。
十一、使用 gulp.task('default', fn) 定义默认任务
增加如下代码
gulp.task('default', ['script', 'auto']);
此时你可以在命令行直接输入 gulp +回车，运行 script 和 auto 任务。
最终代码如下：
// 获取 gulp
var gulp = require('gulp')
// 获取 uglify 模块（用于压缩 JS）
var uglify = require('gulp-uglify')
// 压缩 js 文件
// 在命令行使用 gulp script 启动此任务
gulp.task('script', function() {
// 1. 找到文件
gulp.src('js/*.js')
// 2. 压缩文件
.pipe(uglify())
// 3. 另存压缩后的文件
.pipe(gulp.dest('dist/js'))
})
// 在命令行使用 gulp auto 启动此任务
gulp.task('auto', function () {
// 监听文件修改，当文件被修改则执行 script 任务
gulp.watch('js/*.js', ['script'])
})

// 使用 gulp.task('default') 定义默认任务
// 在命令行使用 gulp 启动 script 任务和 auto 任务
gulp.task('default', ['script', 'auto'])
去除注释后，你会发现只需要 11 行代码就可以让 gulp 自动监听 js 文件的修改后压缩代码。但是还有还有一些性能问题和缺少容错性.
只有不断努力，才有机会成功。
