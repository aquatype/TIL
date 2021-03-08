# Using AWS Lambda with Serverless Framework

[Serverless Framework] (이하 서버리스)를 이용하여 아마존 람다 위에서 동작하는 REST API 만들기 튜토리얼.

### 사전 준비

* 서버리스는 [Docker]에 의존하므로 미리 설치해둔다.

* 서버리스 설치
```
$ npm install -g serverless
```

* [Serverless Python Requirements] 플러그인 설치
```
$ serverless plugin install -n serverless-python-requirements
```

* 서버리스에서 람다에 디플로이할 때 이런저런 권한을 필요로 하므로, [필수 크레덴셜] 문서를 참고하여 해당 권한을 보유한 IAM 유저를 만들어둔다.
* CloudWatch Log를 이용할 수 있도록 CloudWatchLogsFullAccess 정책도 같이 주면 좋다.
* Store Manager를 이용한다면 SSM도..

### 설정

* 터미널에 `serverless` 를 입력해서 프로젝트를 생성한다.  
`AWS Node.js`, `AWS Python` 중 택일하여 만들 수 있고, `Others`를 선택하면 GCP 등의 타 플랫폼도 설정 가능하지만 우선은 AWS에서 파이썬을 쓸 거니까 그걸로 선택.

*


[Docker]:https://hub.docker.com/editions/community/docker-ce-desktop-mac
[Serverless Framework]:https://serverless.com/
[Serverless Python Requirements]:https://github.com/UnitedIncome/serverless-python-requirements/
[필수 크레덴셜]:https://gist.github.com/ServerlessBot/7618156b8671840a539f405dea2704c8
