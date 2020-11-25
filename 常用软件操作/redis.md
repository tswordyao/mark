# redis

```
brew install redis
brew services start redis
brew services restart redis
brew services stop redis
brew uninstall redis

通过bin目录启动
cd /usr/local/Cellar/redis/6.0.5/bin
./redis-server

配置开机启动
ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents
```


## 用TreeSoft管理可视化redis 和 memecached。
```
通过命令行方式连接redis

1、首先安装redis客户端
yum/brew install redis

2、连接
redis-cli -h host -p port -a password
host:远程redis服务器host
port:远程redis服务端口(默认
password:远程redis服务密码（无密码的的话就不需要-a参数了）

3\ 常用命令
 set testxx 12321 EX 10     #这里和memcached不一样,需要加EX再接超时秒数

hmset user-001 name evan age 18 birth 1998  #这是设置hashMap的方式
expire user-0001 10  #对于hashmap的过期只能这样设置, 普通的keyval也可以使用

hset user-001 age 19  #hget等于hmget, 可以单个字段更新
hset user-001 sex 1  #也可以单个字段写入

SET not-exists-key "value" NX  #只能新建键值对

SET exists-key "value"
SET exists-key "value" XX   #只能更新键值对, 只有上一句创建过才能有这句



4, 其他数据结构和用法
自增,    INCR count  不存在count默认为0
自减decr    DECR count  不存在count默认为0
list
```
> 我mac用iRedis查看远程redis内容


## redis-server 命令启动

```
配置文件在 /usr/local/etc/redis.conf

使用配置文件启动 redis-server /usr/local/etc/redis.conf

开机启动redis命令 $ ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents

使用launchctl启动redis server $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist

停止redis server的自启动 $ launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.redis.plist

卸载redis和它的文件 
brew uninstall redis
 rm ~/Library/LaunchAgents/homebrew.mxcl.redis.plist


测试redis server是否启动 $ redis-cli ping

redis-cli  -h redis所在服务器ip

redis命令行详解：
-c：集群查找
-h：redis主机ip
-p：redis端口：默认6379
-a：如果redis加锁，需要传递redis密码
```
