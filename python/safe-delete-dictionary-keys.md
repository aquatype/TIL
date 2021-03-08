# Safe-delete dictionary keys


딕셔너리를 이터레이트하며 키를 조작할 때, 아래처럼 하면 런타임 에러가 발생한다.

```python
for key in users.keys():
    if key == 'key_to_remove':
        del users[key]

>>> RuntimeError: dictionary changed size during iteration
```

아래처럼 간단하게 해결 가능하다.

```python
for key in users.copy().keys():
  if key == 'key_to_remove':
      del users[key]
```
