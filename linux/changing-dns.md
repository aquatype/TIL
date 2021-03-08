# Changing DNS

`Temporary failure in name resolution` 에러 발생 시 해결 방법

* `/etc/resolv.conf` 파일에 네임서버 추가

```
nameserver 1.1.1.1
nameserver 8.8.8.8
```

* `/etc/systemd/resolved.conf` 파일에 네임서버 추가
```
[Resolve]
DNS=1.1.1.1
FallbackDNS=8.8.8.8
...
```
