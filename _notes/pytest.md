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

pytest -> test_*.py or *_test.py
-k: keyword
to run specific model within a module: pytest test_mod.py::test_func
-m: marker ex) pytest -m slow -> tests decorated with @pytest.mark.slow
-p: plugin / with "no:" disable plugin

pytest vs python -m pytest: same but calling via python will add current directory to sys.path

