ex) "asdzcvzcxv asdzxc 'qwe' zxc 'asd'"
```python
import re
regex = r"'([^\']+)'"
for match in re.compile(regex).findall(ex)
	print(match)

# qwe
# asd
```
