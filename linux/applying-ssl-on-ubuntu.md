# Applying SSL on Ubuntu

좀 더 정확하게 제목을 달자면 "Ubuntu에서 apache + mod_wsgi로 동작 중인 django 프로젝트에 SSL을 적용하는 방법"이다.  
어느 카테고리로 올려야 할 지 선택장애가 와서 그냥 리눅스로..  

기본적으로 아파치 및 WSGI 설정이 되어 있는 것을 전제한다.  

### 1. SSL활성화

```
$ sudo a2enmod ssl
$ sudo a2ensite default-ssl.conf
$ sudo service apache2 restart
```

### 2. Certbot 설치
[Certbot]은 [Let's Encrypt]의 인증서를 서버에 쉽게 발급/설치할 수 있게 도와주는 어-썸한 헬퍼 툴이다.  
렛츠인크립트는 점유율 0.7% 미만의 인증서 발급기관이지만 순수하게 웹 시큐리티를 위해 논프로핏으로 운영되는 곳이며, 따라서 인증서 발급비가 **무료**이다. 안 쓸 이유가 없다.  

`/usr/local` 아래에 Certbot 클라이언트를 설치한다.

```
$ cd /usr/local && sudo git clone https://github.com/certbot/certbot
```

### 3. 인증서 설치
Certbot 스크립트를 실행한다.

```
$ sudo ./certbot/certbot-auto --apache -d domain.com -d www.domain.com
```

아파치 설정을 읽어와서 해당 도메인을 검증(HTTP-based DCV)하고 인증서 발급 및 연결 설정 등의 과정을 자동으로 진행한다. 중간에 선택지가 나오는데, 필요에 따라 HTTP 리퀘스트를 전부 HTTPS로 리다이렉트하도록 선택하면 아파치 rewrite 설정까지 자동으로 작성해 준다.

설치에 성공하면 완료 메세지를 볼 수 있으며, 발급받은 인증서 및 설정 파일은 `/etc/letsencrypt/`에 저장된다.

### 4. 검증
https://www.ssllabs.com/ssltest/analyze.html?d={도메인명} 으로 접속해서 인증서 상태를 테스트한다.

### 5. 인증서 갱신 및 백업
렛츠인크립트의 인증서 유효기간은 3개월이다. 코모도 등의 다른 업체처럼 리뉴받아서 재설치하는 과정 필요없이 그냥 스크립트에 `certonly` 옵션을 추가해 다시 한 번 돌려주면 되는 간단한 일이지만... 역시 그조차도 귀찮으므로 크론을 통해 자동으로 갱신되도록 설정해 둔다.

```
$ sudo crontab -e
```

`certbot renew` 옵션으로 실행하면 인증서 상태를 체크한 다음, 만료일이 30일 이내라면 자동으로 최초 인증서 생성 시의 크레덴셜을 이용하여 갱신을 시도한다. 매 주 스크립트가 실행되게 스케줄을 추가.

```
0 3 * * 6 /usr/local/certbot/certbot-auto renew
5 3 * * 6 sudo service apache2 reload
```

혹시 모르니 갱신된 인증서도 잘 백업해 두자.


[Certbot]:https://github.com/certbot/certbot/
[Let's Encrypt]:https://letsencrypt.org/
