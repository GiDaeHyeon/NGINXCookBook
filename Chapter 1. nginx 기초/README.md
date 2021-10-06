# 정적 콘텐츠 서비스 & 무중단 설정 리로드

### 정적 콘텐츠 서비스

---

```
server {
    listen 80 default_server;
    server_name localhost

    location / {
        root /usr/share/nginx/html;
        # alias /usr/share/nginx/html;
        index index.html index.htm;
    }
}
```

- 위 세팅은 HTTP 프로토콜과 80포트를 이용해 /usr/share/nginx/html/경로에 저장된 정적 콘텐츠를 제공한다.
    - 1행. server 블록를 선언해 NGINX가 처리할 새 컨텍스트를 정의한다.
    - 2행. NGINX가 80포트로 들어오는 요청을 수신하게 하고, 80포트에 대한 기본 컨텍스트가 되도록 default_server 매개변수를 사용했다.
        - 여기서는 listen 지시자가 단일 포트만 사용하는데, 필요에 따라 포트를 범위로 지정해줄 수도 있다.
    - 3행. server_name 지시자에는 서버가 처리할 호스트명이나 도메인명을 지정한다.
        - 만약에 설정이 default_server 매개변수를 통해 기본 컨텍스트로 지정되지 않으면?
            - NGINX는 요청 호스트의 헤더값이 server_name 지시자에 지정된 값과 같을 때만 server 블록의 내용을 수행한다.
    - 4행. location 블록을 생성한다.
        - URL의 경로를 기반으로 한다.
    - 5행. root 지시자로 콘텐츠의 경로를 지정한다.
        - NGINX는 root 지시자에 정의된 경로에, 수신된 URI 값을 합쳐 요청된 파일을 찾는다.
        - location 지시자에 URI 접두어를 사용하면, 이 값도 root 지시자에 지정한 값과 합쳐진다.
        - alias 지시자를 사용하면 값이 합쳐지지 않는다.
    - 6행. index 지시자는 URI에 더 참고할 경로 정보가 없을 때, NGINX가 사용할 기본 파일 또는 확인할 파일 목록을 알려준다

### 무중단 설정 리로드

---

```powershell
$ nginx -s reload
```

- 위 명령은 동작 중인 NGINX Master Process에 Reload 신호를 보내 설정을 다시 읽어들이도록 한다.

서버 중지 없이 NGINX 설정을 리로드해 패킷 손실 없이 설정을 변경한다.

높은 가용성이 필요한 환경에서 부하분산 설정을 변경해야 할 일은 굉장이 빈번한데, NGINX는 로드밸런서로서 계속 작동하면서 동적으로 설정을 변경할 수 있다.

NGINX! NGINX!