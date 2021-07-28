# Nginx 서버에서 React 새로고침 시 404에러 없애기

React나 Vue에서 빌드한 파일을 서버에 올려서 사용할 경우 홈 화면 외의 다른 경로(예를 들면, `http://mydomain.com/mypage`와 같은)에서 새로고침을 하면 404 에러가 발생된다. 

왜냐하면 React로 빌드한 파일은 SPA(Single Page Application) 형식이기 때문에 여러 페이지를 사용하더라도 실제 파일은 index.html만 단 하나만 있기 때문인데, 만약 사용자가 홈화면을 거치지 않고 절대경로인 `http://mydomain.com/mypage`로 직접 들어오면 실제로는 해당 파일이 없기 때문에 404 에러가 나는것. 

이를 해결하기 위해서는 다음과 같이 nginx.conf에서 try_files 설정을 해주면 된다.

## nginx.conf
```
# HTTP
server {
    listen 80;
    
    root "/opt/bitnami/nginx/html";
    index index.php index.html index.htm;

    location / {
        try_files $uri /index.html;
    }
}
```
