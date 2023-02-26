---
---

- 탭 대신 스페이스 4칸
- 79문자 이하 라인
```python
import requests

from sqlalchemy.orm import Session

from user.home import custom  # import문 순서

EXAMPLE_CONSTANT = "Hello world"

class FirstPythonExample:  # capitalized camel case
    _sample_dict = {'a': 'b'}  # 변수 대입, 딕셔너리 공백, private
    def first_python_example_function(self):
        sample_type_hint: list  # 타입 표기
	    return 1
# 한 줄
    def __second_python_example_function(self):  # private method
        return 2
# 두 줄

class SecondPythonExample:
    @classmethod
    def foo(cls):  # 클래스 메소드의 첫 번째 인자는 무조건 클래스
        return 0


def bar(cls):
    pass

# 함수, 변수, 애트리뷰트는 소문자 스네이크 케이스
```
- if a is not b O / if not a is b X (긍정적인 식을 부정하지 말고, 부정을 내부에 넣어라)
- if len(sample_list) == 0: X / if not sample_list: O
- 한 줄에 쓰기 어려운 코드는 (),\\n,\\t 등을 사용