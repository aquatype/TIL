# Dev Environment Setup Guide (for Mac)

작업환경 셋업 가이드  
맥OS 10.14 Mojave 기준.

1. [Homebrew] 설치
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. Python 2.7 삭제
```
$ sudo rm -rf "/Library/Frameworks/Python.framework/Versions/2.7"
$ sudo rm -rf "/Applications/Python 2.7"
$ brew prune
```

3. Python 3, virtualenv 설치
```
$ brew install python
$ pip3 install virtualenv
$ virtualenv ~/venv/
```

(update) 파이썬이 버전업되면서 홈브류 포뮬라를 통해서는 3.7.x 버전밖에 설치할 수 없는데, 일단 임시로 아래처럼 해서 다운그레이드할 수 있다.

```
# 일단 디펜던시를 전부 포함한 최신 버전(3.7.x)의 파이썬을 설치
$ brew install python

# 다운그레이드를 위해 심볼릭 언링크
$ brew unlink python

# 원하는 버전을 디펜던시 제외하고 수동 설치 (아래는 3.6.5 포뮬라 주소임)
$ brew install --ignore-dependencies https://raw.githubusercontent.com/Homebrew/homebrew-core/f2a764ef944b1080be64bd88dca9a1d80130c558/Formula/python.rb
```

4. [Postgres.app] 설치  
맥에서 개발용으로 쓰기엔 패키지보다는 이 쪽이 훨씬 연동이 간편함

5. [Github Desktop] 설치, 리포 클론, 디펜던시 설치  
프로젝트 내 requirements.txt 참조
```
pip install --upgrade pip3
pip install -r requirements.txt
```

6. 태스크 자동화용 nodejs, npm 스크립트 용 디펜던시 설치  
프로젝트 내 package.json 참조
```
$ brew install node
$ npm install -g onchange concat node-sass uglifycss postcss-cli autoprefixer jshint
```

7. [Atom] 및 패키지 설치
```
$ apm install emmet minimap linter linter-pycodestyle linter-tidy intentions atom-clock
```

8. .bash_profile 설정  
환경변수 설정
```
export PATH="Library/Frameworks/Python.framework/3.6/bin:${PATH}"
export PATH="/Applications/Postgres.app/Contents/Versions/10/bin:${PATH}"
```
alias 설정
```
alias py='python3'
alias python='python3'
alias pip='pip3'
alias venv='. ~/venv/bin/activate'
alias run='python3 manage.py runserver 0.0.0.0:8000'
```
기타 편의 스크립트
```
conn() {
  if [ $1 == 'dev' ]; then
    ssh -i {PATH_TO_PEM}.pem {PATH_TO_SERVER}
  elif [ $1 == 'prod' ]; then
    ssh -i {PATH_TO_PEM}.pem {PATH_TO_SERVER}

    ...

  fi
}
```


9. 그 외 자주 쓰는 툴들
  * [pgAdmin]
  * [Cyberduck]

[Homebrew]:https://brew.sh/
[Postgres.app]:http://postgresapp.com/
[Github Desktop]:https://desktop.github.com/
[Atom]:https://atom.io/
[pgAdmin]:https://www.pgadmin.org/
[Cyberduck]:https://cyberduck.io/
