---
---

type hint 3.9 이상부터
PEP 585부터 built-in과 typing 중복돼서 빼놓음

그 전(3.7)에 type hint 쓰려면
```python
from __future__ import annotation
```
이렇게 해야함.
아니면 
```python
from typing import List
```
이런 식인데, 런타임 때 느려지는 것으로 판명나서 deprecated 판정받음

나중에 mypy 쓰면 더 도움이 될 것