source

```
举例说明：

1.新建一个test.sh脚本，内容为:A=1

2.然后使其可执行chmod +x test.sh
3.运行sh test.sh后，echo $A，显示为空，因为A=1并未传回给当前shell
4.运行./test.sh后，也是一样的效果

5.运行source test.sh 或者 . test.sh，然后echo $A，则会显示1，说明A=1的变量在当前shell中


有时候总要:
source ~/.bash_profile
才能加载环境变量, echo $PATH可以查看当前终端的值
```