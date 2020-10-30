gulp5大命令

gulp.task(name, fn) 定义任务

gulp.run(tasks...) 尽可能多的并行运行多个task

gulp.watch(glob, fn) 当glob内容发生改变时，执行fn

gulp.src(glob) 返回一个可读的stream

gulp.dest(glob) 返回一个可写的stream