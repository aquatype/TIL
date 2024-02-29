# Ubuntu EC2 Setup Walkthrough

새 EC2 인스턴스 생성 후 프로젝트 디플로이까지의 보편적 과정 정리   
우분투 18.04 기준

```
$ sudo apt update
```

pip3 및 디펜던시 설치
```
$ apt install python3-pip
# 가상환경 생성
$ pip3 install virtualenv
$ virtualenv -p /usr/bin/python3 ./dork
$ pip3 uninstall virtualenv

# 아파치 및 WSGI 설치
$ sudo apt install apache2 libapache2-mod-wsgi-py3
$ sudo a2dismod --force autoindex
$ sudo a2enmod rewrite

# Postgres 설치
$ sudo apt install postgresql postgresql-contrib libpq-dev

# 유저 설정
sudo -u postgres createuser --interactive
sudo -u postgres psql
psql > \password postgres
psql > \password app

sudo -u postgres createdb {{DBNAME}}

# 외부 접속 설정
$ vim /etc/postgresql/14/main/postgresql.conf
# listen_addresses 주석해제 후 접속할 IP로 변경, '*'
$ vim /etc/postgresql/14/main/pg_hba.conf
# IPv4 local connections 설정 찾아서 접속 IP 설정
host    all             all             0.0.0.0/0            md5 

# certbot
sudo a2enmod ssl
cd /usr/local && sudo git clone https://github.com/certbot/certbot

sudo ./certbot/certbot-auto --apache -d domain.com
```
