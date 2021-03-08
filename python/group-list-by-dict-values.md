# Group List by Dict Values

다음과 같은 데이터 리스트가 있다.

```python
items = [
    {'name': 'Apple', 'category': 'fruit', },
    {'name': 'Tomato', 'category': 'vegetable', },
    {'name': 'Banana', 'category': 'fruit', },
    ...
]
```

이 데이터를 `category`를 기준으로 그루핑하고 싶으면 이렇게 한다.

```python
grouped_items = dict()

for item in items:
    grouped_items.setdefault(item['category'], []).append(item)
```
