---
---

파이썬에서 None이 특별한 의미를 지니기 때문에 False와 동등한 반환 값으로 취급해서 잘못 해석할 수 있다.
```python
def careful_divide(a, b):
	try:
		return a / b
	except ZeroDivisionError:
		return None

x, y = 0, 5
result = careful_divide(x, y)

if not result:  # if result is None이 더 정확하다.
	print('잘못된 입력') # 이 코드가 실행되는데, 사실 이 코드가 실행되면 안된다!
```

### if not A의 뜻
A가 False, [], None, '', 0 이면~

해결법
1. 반환값을 분리
	 - ```def careful_divide(a, b):
			try:
				return True, a / b
			except ZeroDivisionError:
				return False, None```
2. 특별한 경우에 결코 None 반환하지 않는 것 
	- ```def careful_divide(a, b):
		 try:
			  return a/b
		 except ZeroDivisonError as e:
			  raise ValueError('잘못된 입력')
	- ```x, y = 5, 2
		try:
			result = careful_divide(x, y)
		except ValueError:
			print('잘못된 입력')
		else:
			print('결과는 %.1f 입니다' % result)```

	- 검증에서 제외되지만, type annotation을 사용하는 코드에도 적용 가능 -> 문서에 기록