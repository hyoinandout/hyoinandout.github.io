---
---

형식화: 미리 정의된 문자열에 데이터 값을 끼워 넣어서 사람이 보기 좋은 문자열로 저장하는 과정
형식 문자열: % 형식화 연산자를 사용한 텍스트 템플릿 왼쪽에 들어가는 것

C 스타일 형식 문자열

문제점
1. 형식화 식 오른쪽 tuple 순서를 바꾸거나 타입을 바꾸면 타입 변환 불가능 하므로 오류 발생
2. 가독성
3. 같은 값을 여러 번 사용하고 싶다면 튜플에서 같은 값을 반복
4. 세 번째 문제를 해결하기 위해 딕셔너리를 사용해 형식화 한다면 번잡스러워 진다

str.format 으로 1,3 번째 문제점 해결 가능: 위치 지정자와 같은 위치 인덱스를 여러 번 사용함으로써

f-문자열(인터폴레이션을 통한 형식 문자열)로 모든 문제 해결 가능

```python
key = 'my_var'

value = 1.234

formatted = f'{key!r:<10} = {value:.2f}'
f_string = f'{key:<10} = {value:.2f}'
c_tuple = '%-10s = %.2f' % (key, value)
str_args = '{:<10} = {:.2f}'.format(key, value)
str_kw = '{key:<10} = {value:.2f}'.format(key=key,value=value)
c_dict = '%(key)-10s = %(value).2f' % {'key': key,'value': value}
```