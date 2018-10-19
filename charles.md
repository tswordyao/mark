# charles和weinre手机代理的使用

## charles

### 电脑安装证书
> 打开Charles->help->ssl proxying->install Charles Root Certificate

### mac钥匙串管理
> 接受安装->搜索Charles->双击->信任->使用此证书始终信任

### 手机设置代理
> 点开wifi详细信息->选择代理>手动>输入电脑局域网ip,端口8888

### 手机下载证书
> 打开chls.pro/ssl 

### IOS手机安装证书
> 在手机设置 -> 通用 -> 关于本机 -> 证书信任设置 中打开信任

### 安卓手机装证书
> 设置---更多设置---系统安全---从存储设备安装--选择文件

### 手机通过电脑Charles代理访问
> 电脑上弹出Charles对话框,点allow

(注意：菜单Proxy -> SSL Proxying中配置可开启Enable SSL Proxying并配置需要抓包的域名。 add > host:*, port:443)

<br>
<br>

## weinre

### 安装
> npm i weinre -g

### 启动
> weinre --httpPort 10024 --boundHost -all-

### 插入脚本到要调试的html
> 这里可能不方便改动服务器上的html文件, 配合Charles代理为本地html
```
<script src="http://127.0.0.1:10024/target/target-script-min.js#anonymous" type="text/javascript"></script>
```

### 调试
- 进入weinre控制主页: http://127.0.0.1:10024/
- 点击Access Points>http://127.0.0.1:10024/client/#anonymous(即第一个link)
- client/#anonymous页面中会显示Targets, 绿色为当前目标
- 点第二个绿色Elements页签, 可以查看此时手机view里的dom元素
- console貌似看不到

(提示：注意手机不要锁屏，不然调试会断开！)





