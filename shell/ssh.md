-ssh
```
创建
ssh-keygen -t rsa -C "XXX@163.com"

指定私钥登录
ssh yaoyifeng@10.177.47.21 -p 1046 -i ./.ssh/h15192_20190729_rsa 

#添加私钥到conf  (最下方)
ssh-add -K 私钥地址(这步如果需要输入密码,就是创建时的密码,密码不要设置大小写)

#列出conf中的秘钥
ssh-add -L



ssh yaoyifeng@10.177.47.21 -p 1046



查看linux版本信息
cat /proc/version
在linux终端输入 getconf LONG_BIT 命令
如果是32位机器，则结果为32

ssh yao@10.155.125.888 -p 1046
logout 退出  exit也可

sudo -iu appops 可以切到appops身份及其用户目录下
logout 再退出
```


###  ssh复制远程机器文件到本地
scp root@192.168.1.100:/home/root/A  /home/B

