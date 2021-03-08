# Unescaped unicode JSON output


내장 JSON 모듈을 이용하여 덤프를 하면 유니코드 문자의 경우 `r\u016f\u017ee` 같은 식으로 이스케이프되어 저장된다.

아래처럼 덤프 옵션에 `ensure_ascii=False` 옵션을 주면 해결된다.

```python
with open('file.json', 'w+') as file:
    json.dump(data, file, indent=4, ensure_ascii=False)
```
