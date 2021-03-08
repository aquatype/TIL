# DB Migrating from Sqlite

장고로 프로젝트를 생성하면 기본 디비 설정이 Sqlite로 되어 있는데... 이 안의 데이터를 PostgreSQL 등으로 마이그레이션하는 방법에 대하여 다룬다.  
**TL;DR**: 그냥 개발 초기 단계부터 제대로 디비 설정하고 시작해라. **그게 훨씬 덜 귀찮다.**  

---

[Pgloader] 같은 오픈소스 툴을 쓸 수도 있지만, 가장 쉬운 방법은 장고 자체의 관리 커맨드인 `dumpdata`를 이용하는 것이다.  
디비 백엔드가 Sqlite로 설정된 상태에서 아래 커맨드를 이용하여 디비를 덤프한다.

```
$ python3 manage.py dumpdata > db.json
```

이제 `settings.py` 에서 디비 백엔드를 원하는 것으로 바꾼 다음, 초기 마이그레이트를 실행하여 장고 기본 테이블 및 모델 구조 등을 반영한다.

```
$ python3 manage.py migrate
```

이제 덤프 뜬 데이터를 때려박...으면 될 것 같지만, 방금의 마이그레이션으로 Content Type 테이블이 이미 생성되었기 때문에 unique constraint 충돌이 일어난다. 파이썬 쉘을 연 다음 아래처럼 싹 비워준다.

```python
>>> from django.contrib.contenttypes.models import ContentType
>>> ContentType.objects.all().delete()
```

이제 덤프 뜬 데이터를 때려박는다.

```
$ python manage.py loaddata db.json
```

혹은 아래처럼 특정 앱의 특정 모델 데이터만 덤프를 할 수도 있다.
이 편이 Content Type을 날리지 않아도 되므로 좀 더 편하다.

```
$ python3 manage.py dumpdata myapp1 myapp2.my_model > db.json
```


[Pgloader]:https://pgloader.io
