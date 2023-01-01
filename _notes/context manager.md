---
---

literal하게 context를 manage 한다: 그 코드가 실행되는 맥락에 맞춰 리소스를 할당/해제 한다는 것!

class /  generator 형태로 만들 수 있고, \_\_enter\_\_, \_\_exit\_\_ 과 같은 함수를 통해 implement 가능. exit에는 type, value, trace_back 과 같은 인자도 넘길 수 있다. 아래 두 코드는 같은 의미이다.

```python
with open('some.file','w') as f:
	f.write('asd')
```

```python
f = open('some.file','w')
try:
	f.write('asd')
finally:
	f.close()
```

또한 \_\_exit\_\_ 이 True로 반환하면 예외 발생 안한다.

generator 쓰려면 contextlib의 @contextmanager 활용하면 된다.