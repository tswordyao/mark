## nginx 基本操作
```
位置 /usr/sbin/nginx
测试 ./nginx -t
启动 ./nginx -c 配置文件地址
重启 ./nginx -s reload

停止
cat nginx.pid 查看进程号
kill -TERM pid  快速停止服务
kill -QUIT pid  平缓停止服务
kill -9 pid     强制停止服务
```

## nginx开启多个跨域白名单 cors
```
server {
        ... ...

        set $cors_origin "";
        if ($http_origin ~* "^http://127.0.0.1$") {
                set $cors_origin $http_origin;
        }
        if ($http_origin ~* "^http://localhost$") {
                set $cors_origin $http_origin;
        }
        add_header Access-Control-Allow-Origin $cors_origin;

        location / {
                if ($request_method = 'OPTIONS') {
                   add_header Access-Control-Allow-Origin $cors_origin;
                   add_header Access-Control-Allow-Methods GET,POST,PUT,DELETE,OPTIONS;
                        return 204;
                }
                 
                ... ...
        }
}
```