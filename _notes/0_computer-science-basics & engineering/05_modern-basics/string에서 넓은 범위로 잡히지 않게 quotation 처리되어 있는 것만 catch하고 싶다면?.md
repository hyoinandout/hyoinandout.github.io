---
---

ex) "asdzcvzcxv asdzxc 'qwe' zxc 'asd'"
```python
import re
regex = r"'([^\']+)'"
for match in re.compile(regex).findall(ex)
	print(match)

# qwe
# asd
```

예외가 하나 있다.
\-\- 로 시작하는 주석에서 don't와 같이 하나의 quotation만 쓰이는 경우가 나올때이다.
그래서 앞에
(?!n)을 붙여주었더니 해결됐다.
하지만 또다른 예외가 발생했다.