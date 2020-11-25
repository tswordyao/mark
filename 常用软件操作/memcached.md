```
memcached 💽

brew install memcached  安装完成后，使用如下命令启动：
$ sudo memcached -m 32 -p 11211 -d

--memberncached 命令行连接远程

$ telnet 127.0.0.1 11211
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.

set name 0 60 5
11111
STORED
get name 
VALUE name 0 5
11111
END
quit

// 根据用户id获取sessionId  返回的是一个含有sid的对象
shared_logon_vsck_v2_pre_1020201007pcwykt

// 根据用户id获取sessionInfo 一个含有yoocRoles,studyRoles,nickname的对象. membercaced直接取会乱码, 里面有javamap序列化
shared_MEMBER_SESSION_V2_1020201007

启动
memcached -d -p 11211 -u nobody -c 1024 -m 6

连接
telnet localhost 11211


set foo 0 300过期秒数 3字节数 
下一行再输入值 bar  


node中使用
const Memcached = require('memcached');
const memcached = new Memcached('192.168.1.144:11211');

/**
 * set 设置key-value
 */
memcached.set('asdf', JSON.stringify(json), 0, function (err) {
  console.log('ok');
});

/**
 * get 获取值
 */
memcached.get('abc_10001',(err,row)=>{
  if(err){
    console.log('error');
  }else {
    console.log(row);
    memcached.end();
  }
});
 ———————————————— 
* 用TreeSoft管理可视化redis 和 memecached。


```