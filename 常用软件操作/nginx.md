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