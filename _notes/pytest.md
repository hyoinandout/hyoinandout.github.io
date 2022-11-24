pytest.raises() to write assertion about raised exceptions
```python
import pytest

def test_zero_division():
	with pytest.raises(ZeroDvisionError):
		1/0
```

-q : quiet mode

## Group multiple tests in a class
refer to convention:
1. test organization
2. sharing fixtures
3. applying marks at the class lvl
