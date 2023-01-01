---
---

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

## Builtin markers
usefixtures, filterwarnings, skip, skipif, xfail, parameterize

mark has no effect on fixtures
pytest.ini or pyproject.toml (here pyproject.toml)
```
[tool.pytest.ini_options]
markers = [
	"fast": marks tests as fast (deselect with -m "not slow"),
	"serial"
]
```

## fixture
provides a defined, reliable and consistent context for the tests (environment or content)
[[Anatomy of a test]]

ex)
```python
import pytest

class Animal:
	def __ init__(self,name):
		self.name = name

	def __eq__(self,other):
		return self.name == other.name

@pytest.fixture
def my_animal():
	return Animal("lion")

@pytest.fixture
def animal_zoo():
	return [Animal("tiger"),my_animal]

def test_my_animal_in_zoo(my_animal, animal_zoo):
	assert my_animal in animal_zoo

```

fixture can use other fixtures as well

3 styles
[[xunit-style]]
[[unittest.TestCase]]
nose based

fixture errors: linear

sharing test data makes use of cache

