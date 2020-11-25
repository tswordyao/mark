```
mysql

初始密码root@localhost: quuQEoaLu4;u

打开mysql
安装后，进入系统偏好设置 ，找到mysql的logo开启服务


 (1).进入/usr/local/mysql/bin,查看此目录下是否有mysql，
 (2).执行vim ~/.bash_profile 在该文件中添加mysql/bin的目录，
       PATH=$PATH:/usr/local/mysql/bin 
       添加完成后，按esc，然后输入:wq保存。 
       最后在命令行输入source ~/.bash_profile编译下



登录mysql：
mysql -uroot -p，输入初始密码

修改密码：
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('newpwd');

密码newpwd


链接：https://www.jianshu.com/p/fd3aae701db9

可以直接用open ~/.bash_profile文本编辑器打开



eggjs配合mysql
先配置密码
 ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'newpwd'
语句后要加;   加了分号再回车才是执行命令，否则只是换行继续输入

mysql> create database realworld
    ->....;

Query OK ,1 row ...表示成功

mysql> quit


 下面的命令好像路径在本机不对？
##启动MySQL服务
sudo /usr/local/MySQL/support-files/mysql.server start
 
##停止MySQL服务
sudo /usr/local/mysql/support-files/mysql.server stop
 
##重启MySQL服务
sudo /usr/local/mysql/support-files/mysql.server restart
```