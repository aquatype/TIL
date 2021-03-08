# Cleaning Up Kernels

디스크 용량 및 아이노드 확보하기

```
# 디스크 용량 현황 확인
$ df -h
# 아이노드 현황 확인
$ df -i

# 로컬 리포 정리
$ sudo apt autoclean
# 캐시까지 날리려면
$ sudo apt clean

# stale 커널들 삭제하기

# 설치된 데비안 패키지 확인
$ dpkg -l linux-headers-\* linux-image-\* | grep ^ii
# 디펜던시가 꼬여서 클린이 안 될 경우 하나씩 수동으로 삭제
$ sudo dpkg --remove linux-image-***-***-generic
# 패키지 재설정
$ sudo dpkg --configure -a
# 강제 재인스톨로 디펜던시 정렬
$ sudo apt -f install
```
