Gulp 中异步任务的处理

1. 使用回调函数通知gulp任务完成
gulp.task("task",  next=>{  
    setTimeout(it =>{
           console.log("task2");  
           next();
     });
});


2.让任务返回流
如果你定义的gulp任务，返回的是流（通常来讲，gulp的插件返回的都是流对象，这样才可以级联起来），那么gulp也可以确定你的任务何时执行完成，从而确保任务的先后执行顺序。
返回的异步stream (不是promise), npm上搜end-of-stream,  可以用这个库手动设置其异步回调; 这时另需用这个库stream-consume来确保这种方式(没有通过下一个pipe)的readableStream能继续流动.



3. 让任务返回Promise
如果你定义的gulp任务，返回的是Promise对象，那么gulp也可以确定你的任务何时执行完成，从而确保执行顺序


总结：当gulp启动多个任务时，它默认是并发方式启动的。如果你希望各个任务以串行的方式执行，必须显式地用上面三种方式之一告诉gulp。
进一步参考：https://github.com/gulpjs/gulp/blob/master/docs/API.md#async-task-support http://www.gulpjs.com.cn/docs/api/




Gulp 中异步任务的处理 再深入

1.多任务的组合---sequence库
var runSequence = require('run-sequence');
gulp.task('default', function(next) {
    runSequence(
		'task1',
       		 ['task2-a', 'task2-b'], // 这里的任务并行, 而且都完成后, 进入下一个
        		'task3',
        		next
   );
});


2.单任务中多个异步Stream的合并----mergeStream库
var mergeStream = require('merge-stream');
gulp.task("minify", function() {
    var mincss = gulp.src('./styles/**/*.css',  { base: "./" })
        .pipe(cssmin())
        .pipe(gulp.dest('.'));

    var minjs = gulp.src('./scripts/**/*.js',  { base: "./" })
        .pipe(uglify())
        .pipe(gulp.dest("."));
    
    return mergeStream(mincss, minjs);
});


 3. gulp-awaitable-tasks库
 require('gulp-awaitable-tasks')(gulp);
 gulp.task('compile-css', function*() {
    yield gulp.src('./scss/**/*.scss',  { base: "./" })
        .pipe(sass())
        .pipe(gulp.dest('./styles'));

    yield gulp.src('./styles/**/*.css',  { base: "./" })
        .pipe(cssmin())
        .pipe(gulp.dest('.'));
 });








