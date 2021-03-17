# Serverless Django Using Zappa

Zappa + SQLite를 이용하여 AWS Lambda 위에 웹서버리스(?)를 띄우는 과정을 기록한다.

### Prerequisites

* 고정 서버 비용을 줄이기 위해 시작한 프로젝트이므로, AWS의 프리 티어를 최대한 우려먹을 수 있도록 RDS나 EC2 DB서버 대신 S3 위에 SQLite 데이터베이스를 셋업해서 사용한다.
* django 2.1 이상 버전을 사용할 경우 SQLite 3.8.3 이상 버전이 필요하다. 람다와 동일한 환경(=아마존 리눅스 베이스)에서 돌아가는 .so 파일을 직접 컴파일하려면 도커까지 써야 해서 상당히 귀찮으므로, 프리컴파일된 파일을 구해서 사용한다.
* Zappa는 파이썬 3.9를 아직 지원하지 않고, 3.6 / 3.7 / 3.8 중에서 골라 쓰면 된다. 프리컴파일 SQLite에 맞추어 3.8을 쓰는 것이 가장 나아 보인다.

### Steps

1. 디펜던시들을 설치하고, zappa를 이니셜라이즈한다. 미리 생성해둔 django 프로젝트가 있어야 한다.
```
$ pip3 install zappa django-s3-sqlite
$ cd /path/to/project
$ zappa init
```

2. 커스텀 도메인을 사용하기 위해 `zappa_settings.json` 파일에 아래 설정을 추가한다. 특이하게도 ACM 인증서는 반드시 `us-east-1` 레전에 존재해야 한다.
```json
{
    "dev": {
        "domain": "{{도메인 주소}}",
        "certificate_arn": "{{ACM 인증서 ARN}}",
         ...
    }
}
```

3. AWS 크레덴셜 IAM에 아래 폴리시를 추가한다.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudfront:UpdateDistribution",
                "route53:ListHostedZones",
                "route53:GetHostedZone",
                "route53:ChangeResourceRecordSets"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

4. 로컬에서 다음 명령을 실행하여 도메인을 인증하고 API Gateway 커스텀 도메인을 생성한다.
```
$ zappa certify {stage}
```

5. `settings.py` 파일을 열어 아래처럼 데이터베이스를 설정한다.
```python
INSTALLED_APPS = [
    # ...
    'django_s3_sqlite',
    # ...
]
DATABASES = {
    'default': {
        'ENGINE': 'django_s3_sqlite',
        'NAME': '{{DB파일 이름}}',
        'BUCKET': '{{S3 버킷 이름}}',
    }
}
```

6. 기존에 `python3 manage.py COMMAND`로 수행하던 모든 장고 명령어들은 아래처럼 로컬에서 호출할 수 있다.
```
$ zappa manage {stage} makemigrations
$ zappa manage {stage} migrate
```

7. 아래 명령어로 슈퍼유저를 생성한다.
```
$ zappa manage prod create_admin_user {id} {email} {password}
```

8. 기본적인 설정이 끝났다. 스태틱 파일을 서브하기 위해 `django-s3-storage`를 쓸 수 있고, 개별 API마다 CORS 세팅을 해주기 위해 `django-cors-headers` 같은 패키지를 쓸 수 있다.
