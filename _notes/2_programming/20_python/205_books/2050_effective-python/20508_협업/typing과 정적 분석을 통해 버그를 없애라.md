---
---

컴파일 시점 타입 검사를 통해 몇몇 유형의 버그를 없앨 수 있다.
타입 애너테이션: 정적 분석 도구로 버그가 생길 가능성이 높은 부분을 식별할 수 있다.

typing을 사용하는 정적 분석 도구 구현 여러가지가 있다.
mypy 사용

```python
# 제네릭 타입 애너테이션이 있는 코드

from typing import Callable, List, TypeVar
Value = TypeVar('Value')
Func = Callable[[Value, Value], Value]

def combine(func: Func[Value], values: List[Value]) -> Value:
	assert len(values) > 0
result = values[0]

for next_value in values[1:]:
	result = func(result, next_value)
	return result

Real = TypeVar('Real', int, float)

def add(x: Real, y: Real) -> Real:
	return x + y

inputs = [1, 2, 3, 4j] # 아이고!: 복소수를 넣었다
result = combine(add, inputs)

assert result == 10

# optional

def get_or_default(value: Optional[int],default: int) -> int:
	if value is not None:
		return value
	return value # 아이고!: "default"를 반환해야 하는데 "value"를 반환했다

```

예외는 typing에 포함되지 않음.

typing 모듈을 사용하다, 전방 참조를 처리할 때 함정에 빠질 수 있다.
```python
class FirstClass:
	def __init__(self, value: 'SecondClass') -> None:
		self.value = value

class SecondClass:
	def __init__(self, value: int) -> None:
		self.value = value
```

제일 좋은 방법은 타입 애너테이션 표현 시 문자열 사용하는 것

예전 : 파이썬의 버전을 알아두라
++ type hint 관련해서도, 파이썬 3.9부터는 내장되어 있지만 그 전에는
```
from __future__ import annotations
```
로 임포트 해주어야 함. [PEP 585](https://peps.python.org/pep-0585/)


타입 힌트를 사용할 때 모범적인 사용법
1. 처음부터 다 사용하려 하면 힘들다: 타입 X -> 테스트 -> 타입 O
2. 코드베이스의 경계에서 유용함
3. API 일부분이 아니더라도 오류 발생하기 쉬운 부분에 사용하면 좋긴 하지만, 모두 다 적용하는게 좋은 건 아님
4. CI에 mypy 포함
5. 타입 정보를 추가해 놓으면서 타입 검사기를 실행하는 것이 중요. (type.py)

항상 '비즈니스 밸류'를 따져서 사용하는게 올바를 것이라고 생각합니다.