# Translation

장고 자체에 내장된 i18n 모듈을 이용하여 멀티링구얼 앱을 만드는 과정을 간단하게 기록한다.

1. gettext 모듈 설치  
장고 모듈에서 GNU gettext tools 버전 0.15 이상을 요구하므로 설치해 준다.  
```
$ brew install gettext
$ echo 'export PATH="/usr/local/opt/gettext/bin:$PATH"' >> ~/.bash_profile
```

2. 설정 추가  
`settings.py` 파일에 다음과 같이 설정을 추가한다.  
```python
from django.utils.translation import ugettext_lazy as _

...

USE_I18N = True
USE_L10N = True

MIDDLEWARE = [
    ...
    'django.middleware.locale.LocaleMiddleware',
]


LOCALE_PATHS = [
    os.path.join(BASE_DIR, 'locale'), # 로케일 파일이 저장될 경로 지정
]

LANGUAGES = (
    ('ko-KR', _('Korean')),
    ('en', _('English')),
    ...
)
```

3. 번역이 필요한 텍스트를 설정  
```python
from django.utils.translation import ugettext_lazy as _

str = _("Hello.")
```

템플릿에서는 아래와 같이 한다.
```html
{% trans "Hello" %}

{% blocktrans %}I have {{ variable }} in translation text.{% endblocktrans %}
```

4. 로케일 파일 생성  
```
$ python manage.py makemessages -l ko_KR
```

위 명령어를 실행하면 프로젝트 파일들을 전부 훑어서 번역이 필요한 텍스트들을 설정한 경로 내에 PO 포맷으로 추출해준다.  
한참을 삽질했었는데, ISO 랭귀지 코드에 맞게 로케일을 생성하되 언어와 국가 코드 사이를 대쉬가 아닌 언더스코어로 써야 한다. 안 그러면 장고가 인식하지 못한다.  

5. 번역 및 컴파일  
```
$ python manage.py compilemessages
```

텍스트들을 적절히 번역해 준 다음 MO 파일로 컴파일한다.

6. 언어 스위칭

기본적으로는 장고에서 브라우저의 선호 언어 설정에 따라 자동으로 로컬라이즈해준다.  
각 로케일 별로 사이트를 분리하려면 `urls.py`에서 i18n 패턴 디텍터를 이용해 주소에 로케일 prefix를 붙이는 방법 등을 쓸 수 있지만, 굳이 명시적으로 분리할 필요가 없다면 내장된 스위처 뷰를 이용할 수도 있다.

```python
urlpatterns = [
    url(r'^i18n/', include('django.conf.urls.i18n')),
    ...
]
```

이제 `/i18n/setlang/` 으로 `language` 파라미터를 담아 HTTP POST 리퀘스트를 날리면 언어가 바뀐다.  
이것 하나 때문에 폼을 두기는 귀찮으므로 대충 야매로 처리했다.

```javascript
$.post('/i18n/setlang/', {
    'language': 'ko-KR',
    'csrfmiddlewaretoken': {{ csrf_token }}
}, function() {
    document.location.href = document.location.href;
});
```



### Reference:

 * [Django Docs]

[Django Docs]:https://docs.djangoproject.com/en/2.0/topics/i18n/translation/
