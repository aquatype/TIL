# Collection Reverse Search


콜렉션에서 특정 값을 찾는 것은 `in`을 이용해서 쉽게 할 수 있다.

```python
haystack = ('apple', 'banana', 'cherry')
needle = 'apple'

if needle in haystack:
  do_something()
```

반대로, 특정 값에서 콜렉션 중 하나가 존재하는지의 여부를 확인해야 할 경우에는 `any()`를 사용하여 구현한다.

```python
haystack = 'there are 3 apples.'
needles = ('apple', 'banana', 'cherry')

if any(needle in haystack for needle in needles):
  do_something()
```
