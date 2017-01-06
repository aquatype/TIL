# `*args` and `**kwargs`

```python
class Foo(object):
	def __init__(self, value1, value2):
		# do something with the values
		print value1, value2

class MyFoo(Foo):
	def __init__(self, *args, **kwargs):
		# do something else, don't care about the args
		print 'myfoo'
		super(MyFoo, self).__init__(*args, **kwargs)
```
