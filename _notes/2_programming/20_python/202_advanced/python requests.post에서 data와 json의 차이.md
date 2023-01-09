---
---

python request module의 post함수의 signature은 아래와 같다
post(url, data=None, json=None, \*\*kwargs)
data와 json의 차이가 헷갈릴 수 있다

pydoc에서는 다음과 같이 서술한다
```python
:param data: (optional) Dictionary, list of tuples, bytes, or file-like
        object to send in the body of the :class:`Request`.
:param json: (optional) json data to send in the body of the :class:`Request`.
```
data=dict 는 dict를 form-encoded하게 해서 key=value 식으로 변환된다.
반며에 json같은 경우 value를 json data로 encode 해버리고 헤더에 Content-Type=application/json 식으로 추가된다.

그렇다면 post(url,data=json.dumps(foo))와 post(url,json=foo)의 차이점은 무엇일까?
json key에 data를 넣을 경우 헤더가 지정되지만 data key에 data를 넣는경우 헤더가 지정이 안돼서 manual하게 정해주어야한다.
