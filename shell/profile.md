```
Ubuntu 进入系统文件/etc/profile修改内容
/etc/profile 默认权限为 -rw-r--r--
即只有root用户可以修改，其它用户只能读取。

要修改/etc/profile，先要使用root登录系统，再使用文本编辑软件打开/etc/profile进行编辑，最后保存退出即可。

使用vi或vim进行编辑，输入：vi /etc/profile  ,如果无法保存，则可能是权限问题，输入 sudo vim /etc/profile  进入系统文件进行修改和保存

图形界面下可用gedit进行编辑，输入：gedit /etc/profile

```