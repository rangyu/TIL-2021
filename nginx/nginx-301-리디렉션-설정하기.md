# Nginx에서 301 리디렉션 설정하기

nginx에서 301 리디렉션을 설정하려면 `return 301 {url}`순으로 리디렉션할 url값을 반환하면 된다.

## nginx.conf

```
server {
    listen 80;
    server_name mydomain.com;

    return 301 https://$host$request_uri;
}
```
