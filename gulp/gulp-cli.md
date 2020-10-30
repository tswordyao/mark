## gulp 命令行（CLI）文档

> 使用fish等非标准shell,可能会出现全局gulp无法找到的问题  

### 参数标记
* -v 或 --version 会显示全局和项目本地所安装的 gulp 版本号
* --require <module path> 将会在执行之前 reqiure 一个模块。这对于一些语言编译器或者需要其他应用的情况来说来说很有用。你可以使用多个--require
* --gulpfile <gulpfile path> 手动指定一个 gulpfile 的路径，这在你有很多个 gulpfile 的时候很有用。这也会将 CWD 设置到该 gulpfile 所在目录
* --cwd <dir path> 手动指定 CWD。定义 gulpfile 查找的位置，此外，所有的相应的依赖（require）会从这里开始计算相对路径
* -T 或 --tasks 会显示所指定 gulpfile 的 task 依赖树
* --tasks-simple 会以纯文本的方式显示所载入的 gulpfile 中的 task 列表
* --color 强制 gulp 和 gulp 插件显示颜色，即便没有颜色支持
* --no-color 强制不显示颜色，即便检测到有颜色支持
* --silent 禁止所有的 gulp 日志

  
# 

###  Tasks 
Task 可以通过 gulp <task1> <task2> 方式来执行。如果只运行 gulp 命令，则会执行所注册的名为 default 的 task，如果没有这个 task，那么 gulp 会报错。


