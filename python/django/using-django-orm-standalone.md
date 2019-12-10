# Using Django ORM Standalone

장고 ORM에 익숙해지니 도저히 다른 걸 쓸 수가 없다. 대안으로 SQL알케미를 열심히 파봤으나 이건 도대체...  
그래서 '장고에서 디비 관리 / ORM 부분만 빼서 쓰면 참 좋지 않을까??'라고 생각하고 방법을 찾아보았다.

장고 2.2 기준.

#### 모델 생성
원하는 이름으로 디렉토리를 하나 만들고 그 안에 `models.py` 파일을 생성한다. 어차피 모델만 사용하므로 startapp 커맨드로 생성할 필요도 없다.  
여기서는 디렉토리명을 `db`로 정했음.
```python
from django.db import models

class User(models.Model):
    name = models.CharField()
    ...
```

#### settings.py
일반적으로는 프로젝트명 디렉토리 아래에 위치하는 파일이지만, 굳이 그 구조를 고수할 필요는 없으므로 루트로 옮긴다.  
구동에 필요한 최소 설정값은 아래의 4개이다.

```python
import os

BASE_DIR = os.path.dirname(os.path.abspath(__file__))
SECRET_KEY = 'sup3rs3cr3t'

DATABASES = {
    'default': {
        ...
    },
}

INSTALLED_APPS = (
    'db',  # 모델 디렉토리 명
)
```

#### manage.py
마이그레이션 관련 기능을 이용하기 위해 남겨두는 파일.  
`settings.py` 파일의 위치를 옮겼으므로 그걸 반영하여 내용을 살짝 변경해준다.
```python
import os
import sys

if __name__ == '__main__':
    from django.core.management import execute_from_command_line

    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'settings')
    execute_from_command_line(sys.argv)
```

#### 모델 임포트를 위한 처리
위까지 진행하면 CLI를 이용한 데이터베이스 마이그레이션 등이 가능해진다. 하지만 프로젝트 내에서 ORM을 사용하려면 당연하지만 내부에서 장고가 실행이 되어야 한다.  
필요할 때마다 장고를 임포트해서 실행해도 되지만, 뭔가 좀 코드가 난잡해지는 게 싫어서 `db/__init__.py` 안에 다음과 같이 호출부를 넣어두었다.

```python
import os
from django.conf import settings

if not settings.configured:
    import django

    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'settings')
    django.setup()
```

이제 ORM이 필요한 곳에서 모델 임포트만 하면 알아서 처리된다. 야 신난다!

```python
from db.models import User

users = Users.object.all()
```

물론 이렇게 구현하면 모델 임포트문을 처리할 때마다 매번 그 무거운 장고를 통째로 호출하는 식이 된다. 그러나, 넌 애초에 ORM 하나 써보겠다고 장고를 디펜던시에 넣는 미친 짓을 이미 감행했잖아? 하하핳ㅎㅎ하하

일단 가능하다는 것은 알았으니 최적화를 할 방법을 천천히 생각해보자.
