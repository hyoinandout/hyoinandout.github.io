---
---

ex) "asdzcvzcxv asdzxc 'qwe' zxc 'asd'"
```python
import re
regex = r"'([^\']*)'"
for match in re.compile(regex).findall(ex)
	print(match)

# qwe
# asd
```

예외가 하나 있다.
\-\- 로 시작하는 주석에서 don't와 같이 하나의 quotation만 쓰이는 경우가 나올때이다.
그래서 앞에
(?!n)을 붙여주었더니 해결됐다.
하지만 또다른 예외가 발생했다. '' 와 같은 경우가 있을 수 있다.
때문에
```
regex = r"'([^\']+)'"
```
가 아닌
```
regex = r"'([^\']*)'"
```
가 되어야 한다.

++
string이 아닌 단어(chunk, single quote)를 잡고자 함이기 때문에 개행문자까지 negated set 안에 넣어주면 금상첨화
```
regex = r"'([^\'\n]*)'"
```