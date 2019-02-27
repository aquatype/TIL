# Connect Amazon S3 with Django

장고에서 아마존 S3 버킷을 외부 저장소로 사용하도록 설정하는 방법에 대해 다룬다.

### 1. 디펜던시 설치

`boto3` 및 `django-storages` 두 가지 라이브러리가 필요하다.  
`boto3`는 아마존 공식 퍼스트파티 파이썬 SDK이고, `django-storages`는 장고 프로젝트에서 사용하는 스토리지 백엔드를 손쉽게 확장할 수 있게 해주는 오픈 소스 라이브러리이다. 드랍박스, MS 애주어 스토리지, 구글 클라우드, 심지어는 FTP마저도 지원하지만 일단 다 필요없고 S3를 지원하기 때문에 쓰는 것이다. 일단 깐다.

```
$ pip3 install boto3 django-storages
```


### 2. IAM 설정

이제 이 라이브러리를 사용할 IAM 유저를 생성해야 한다.

1. AWS 콘솔의 서비스 목록 중 IAM을 찾아 들어간 다음, 왼쪽의 **Users** 메뉴에서 `Add user` 버튼을 클릭해 새로운 유저를 생성한다. 유저 네임은 뭐든 상관없고, 아래의 Access type 설정을 `programmatic access`로 설정한다.

2. 다음 단계에서 권한 그룹을 설정해줘야 하는데, S3 같은 경우 이미 아마존에서 만들어둔 액세스 폴리시가 몇 종류 존재한다. `create group` 버튼을 누른 다음 `s3`로 폴리시를 검색하면 `AmazonS3FullAccess`나 `AmazonS3ReadOnlyAccess` 등이 뜨는데, 딱히 버킷이나 CRUD 권한을 엄격하게 나눠 설정할 게 아니라면 그냥 이거 그대로 가져다 쓰면 되고 아니라면 적당히 폴리시를 고쳐서 권한 설정을 직접 만들면 된다.

3. 입력한 내용을 리뷰한 다음 유저 생성을 누르면 IAM 액세스 키 및 시크릿 키가 나오는데 이는 뒤에 쓰이므로 기록해둔다.


### 3. S3 버킷 설정

장고에서 사용할 S3 버킷을 설정한다. 새로 생성하든 기존 걸 쓰든 상관없지만, 가급적 static / media를 구분해서 쓰면 좋다.


### 4. 장고 설정

`settings.py`의 `INSTALLED_APPS` 리스트에 아래와 같이 `django-storages` 라이브러리를 추가해준다.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    # ...
    'storages',
]
```

아래와 같이 `django-storages`에서 사용할 설정 상수들도 선언해준다.

```python
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
AWS_ACCESS_KEY_ID = '{{ IAM 액세스 키 }}'
AWS_SECRET_ACCESS_KEY = '{{ IAM 시크릿 키 }}'
AWS_QUERYSTRING_AUTH = False
AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}
```

마지막으로 아래와 같이 스태틱 파일 디렉토리 설정을 해주면 된다.

```python
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'  # DEFAULT_FILE_STORAGE 를 설정했다면 필수는 아님
STATIC_URL = 'https://{{ 버킷 주소 }}.s3.amazonaws.com/'
STATICFILES_DIRS = [
    # ...
]
```


### 5. 사용하기

평소처럼 `$ python3 manage.py collectstatic` 으로 스태틱 파일 디플로이를 실행하면, 알아서 파일들을 S3 버킷으로 업로드해준다. 웹서버 자체에서 디플로이하는 것보다는 당연히 좀 느리므로 인내심을 갖고 기다리도록 하자.

템플릿에서 사용할 때에는 아래처럼 내장 static 모듈을 사용하여 에셋을 불러오면 주소를 알아서 맞춰준다.

```html
{% load static %}

<img src='{% static "images/common/logo.png" %}'>
<!-- 렌더되는 주소: https://{{ 버킷 주소 }}.s3.amazonaws.com/images/common/logo.png -->
```


### 6. 스태틱 / 미디어 파일을 분리해서 관리하고자 하는 경우

[[내용 추가 예정]]

### Reference:

 * DK..
