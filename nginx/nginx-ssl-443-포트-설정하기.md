# Nginx에서 SSL 인증서 설치 후 443 포트 설정하기


## nginx.conf
```
# HTTPS
server {
    listen 443 ssl;
    server_name mydomain.com;

    ssl_certificate "/etc/letsencrypt/live/mydomain.com/fullchain.pem"; # cert.pem
    ssl_certificate_key "/etc/letsencrypt/live/mydomain.com/privkey.pem"; # key.pem

    location / {
        root "/opt/bitnami/nginx/html";
        index index.php index.html index.htm;
    }
}
```
