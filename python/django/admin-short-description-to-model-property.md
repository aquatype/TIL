# [Admin] `short_description` to Model Property

특정 모델의 필드값에 정형화된 처리가 반복적으로 필요할 때 보통 커스텀 메소드를 사용해서 구현하는데, 장고 어드민에도 기본적으로 모델의 메소드를 출력하는 기능을 지원한다.  
기본적으로 해당 메소드의 이름을 라벨명으로 출력하기 때문에, 가독성을 위해 보통 `short_description` 속성을 지정해주곤 한다.


```python
class Person(models.Model):
    first_name = models.CharField()
    middle_name = models.CharField()
    last_name = models.CharField()

    def full_name(self):
        return '{} {} {}'.format(first_name, middle_name, last_name)
    full_name.short_description = 'Full name'
```

문제는 이 메소드를 `@property` 데코레이터로 감쌀 경우 `short_description` 속성을 할당할 수가 없다는 것이다. 그래서 아래처럼 먼저 속성을 준 다음, 명시적으로 프로퍼티 선언을 하는 식으로 구현했었다.

```python
def full_name(self):
    pass
full_name.short_description = 'Full name'
full_name = property(full_name)
```

하지만 찾아보니 더 간단한 방법이 있었는데, 프로퍼티의 `fget` 함수를 이용하여 구현하는 것이다.

```python
@property
def full_name(self):
    pass
full_name.fget.short_description = 'Full name'
```

라벨 출력부를 따로 오버라이드하지 않아도 그냥 동작하는 이유가 궁금해서 찾아봤는데, 장고 자체에서 이미 '프로퍼티에서 라벨을 추출하는 경우'를 상정하여 구현을 해놨기 때문이다. 단지 문서화가 안 되어 있을 뿐..

```python
def label_for_field(name, model, model_admin=None, return_attr=False, form=None):
	...

    if hasattr(attr, "short_description"):
        label = attr.short_description
    elif (isinstance(attr, property) and
          hasattr(attr, "fget") and
          hasattr(attr.fget, "short_description")):
        label = attr.fget.short_description
    elif callable(attr):

	...
```



### Reference:

 * [Django source]

[Django source]:https://github.com/django/django/blob/master/django/contrib/admin/utils.py
