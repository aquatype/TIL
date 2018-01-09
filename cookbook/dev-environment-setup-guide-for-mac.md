# Dev Environment Setup Guide (for Mac)

작업환경 클론용 셋업 가이드  
맥OS 10.13 High Sierra 기준.

1. [Homebrew] 설치
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. Python 2.7 삭제
```
$ sudo rm -rf "/Library/Frameworks/Python.framework/Versions/2.7"
$ sudo rm -rf "/Applications/Python 2.7"
$ brew doctor
$ brew prune
```

3. Python 3, virtualenv 설치
```
$ brew install python3
$ pip3 install virtualenv
$ virtualenv ~/venv/
```

4. .bash_profile alias 설정
```
alias py='python3'
alias python='python3'
alias pip='pip3'
alias venv='. ~/venv/bin/activate'
alias run='python3 manage.py runserver 0.0.0.0:8000'
```

5. [Postgres.app] 설치  
맥에서 개발용으로 쓰기엔 패키지보다는 이 쪽이 훨씬 연동이 간편함

6. [Github Desktop] 설치, 리포 클론, 디펜던시 설치  
프로젝트 내 requirements.txt 참조
```
pip install --upgrade pip3
pip install -r requirements.txt
```

7. 태스크 자동화용 nodejs, gruntjs, 기타 디펜던시 설치  
프로젝트 내 package.json 참조
```
$ brew install node
$ npm install -g grunt-cli
$ gem install compass grunt-contrib-clean grunt-contrib-concat grunt-contrib-cssmin grunt-contrib-jshint grunt-contrib-uglify grunt-contrib-watch
```

8. [Atom] 및 패키지 설치
```
$ apm install emmet minimap sass-autocompile linter linter-pycodestyle linter-tidy intentions atom-clock
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
