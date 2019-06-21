# String Formatting Cheatsheet

### 숫자 관련

```python
# 천 단위 구분자
>>> '{:,}'.format(1000)
'1,000'

# 헥스 변환
>>> '{:x}'.format(200)
'c8'
>>> '{:X}'.format(255)
'FF'
>>> '{:#x}'.format(255)
'0xff'

# 바이너리 변환
>>> '{:b}'.format(100)
'1100100'

# 익스포넨트
>>> '{:e}'.format(0.0000000001)
'1.000000e-10'

# 소수 자릿수 지정
>>> '{:.2}'.format(0.141592)
'3.14'
```

### 값 접근

```python
# 순차 접근
>>> '{0}, {2}, {1}'.format(1, 2, 3)
'1, 3, 2'
# 암묵적 순차 접근
>>> '{}, {}, {}'.format(1, 2, 3)
'1, 2, 3'

# 아규먼트 접근
>>> '{first} {middle}. {last}'.format(first='Chris', middle='P', last='Bacon')
'Chris P. Bacon'

# 인덱스 접근
>>> '{[1]}'.format(['first', 'second', 'third'])
'second'

# 엘리먼트 어트리뷰트 접근
>>> '{.name}'.format(sys.stdin)
'<stdin>'

# 키를 통한 접근
>>> '{[foo]}'.format({'foo': 'bar'})
'bar'
```

### 변환

```python
class Data(object):
    def __str__(self):
        return 'str'

    def __repr__(self):
        return 'repr'

# `str` 호출
>>> '{!s}'.format(Data())
'str'

# `repr` 호출
>>> '{!r}'.format(Data())
'repr'
```

### `__format__()` 오버라이드

```python
class HAL9000(object):
    def __format__(self, format):
        if format == 'open-the-pod-bay-doors':
            return "I'm afraid I can't do that."
        return 'HAL 9000'

>>> '{:open-the-pod-bay-door}'.format(HAL9000())
"I'm afraid I can't do that."
```
