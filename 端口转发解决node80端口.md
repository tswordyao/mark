## 使用端口转发解决node在80端口上的监听权限问题

> 由于linux的系统限制，普通用户是无法打开1024以下端口的，这里面就包括http的默认端口80，这就使得很多用户使用root权限来执行node，这带来了不可预计的安全问题，所以这并不是一个好办法。

其实我们可以使用iptables的端口转发功能来解决这个问题：

```
1，首先将node的主程序绑定到高于1024端口，比如8090，这样普通用户就可以启动这个http server了，只不过不是在默认的80端口上监听；

2，配置iptables将80端口转发到8090上，如下命令：
1. #iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8090  

该命令的意思就是在iptable中添加一条端口转发规则，如果删除该规则，重新绑定，则先查找出：
1. #iptables --line-numbers --list PREROUTING -t nat  

然后使用行号删除：
1. #iptables -t nat -D PREROUTING 行号  

3，记得添加8090端口到iptables的INPUT ACCEPT规则中
1. #iptables -I INPUT -p tcp --dport 8090 -j ACCEPT  

4，第2步和第3步添加的这些规则，都是临时性的，重启服务器之后就无效了，所以需要保存起来
1. #/sbin/service iptables save  
```
端口转发配置完成。

### 补充：

经过上面的设置后，远程使用默认80端口访问网站没有问题，但是在本机访问就要包Connect Refused的错误，如：

```￼￼
1. [use@host ~]$ wget http://localhost  
2. --2014-04-17 11:32:41--  http://localhost/  
3. Connecting to localhost:80... failed: Connection refused.  

这是因为本地连接的端口转发与远程连接的不一样，所以我们还要做如下设置：

1. #iptables -t nat -A OUTPUT -p tcp -d 127.0.0.1 --dport 80 -j DNAT --to 127.0.0.1:8090  
2. #iptables -t nat -A OUTPUT -p tcp -d 本机IP --dport 80 -j DNAT --to 本机IP:8090  
3. #/sbin/service iptables save  
```

这样设置之后，本机就可以使用默认端口了。
