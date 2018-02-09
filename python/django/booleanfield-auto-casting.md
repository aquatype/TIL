# BooleanField Auto Casting

폼에서 넘어온 데이터를 boolean 필드에 저장할 때, 굳이 값을 검증해서 `True` `False` 로 캐스팅해 줄 필요 없이 해당 폼 값 자체를 `'1'` `'0'`으로 두기만 해도 잘 동작한다.

```html
<input type='radio' name='are_you_nuts' value='1'>
<input type='radio' name='are_you_nuts' value='0'>
```

```python
# 예를 들어 이런 건 필요없음
# mymodel.is_nuts = True if request.POST['are_you_nuts'] == '1' else False
mymodel.is_nuts = request.POST['are_you_nuts']
```

일단 되긴 되는데 이게 대체 왜 되는 걸까? HTTP POST에서 데이터 타입을 보존해주는 마법같은 신기술이 나왔나? 같은 생각에 굉장히 혼란스러웠는데, 소스까지 뒤진 끝에 이유를 알았다. 장고의 BooleanField 모델 클래스에 특정 문자열을 bool 타입으로 캐스팅해 주는 메소드가 내장되어 있다.

```python
def to_python(self, value):
    if value in (True, False):
        # if value is 1 or 0 than it's equal to True or False, but we want
        # to return a true bool for semantic reasons.
        return bool(value)
    if value in ('t', 'True', '1'):
        return True
    if value in ('f', 'False', '0'):
        return False
    raise exceptions.ValidationError(
        self.error_messages['invalid'],
        code='invalid',
        params={'value': value},
    )
```

정작 공식 문서에도 단 한줄의 설명조차 없어서 여태껏 수동 캐스팅을 하는 비효율적인 짓을 하고 있었다..

### Reference:

 * [Django Source]
 * @ysb0124

[Django Source]:https://docs.djangoproject.com/en/2.0/_modules/django/db/models/fields/#BooleanField
