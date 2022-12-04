Start with
```
pytest tests
```

## Mixing pytest features into unittest.TestCase subclasses using marks
```python
# conftest.py
import pytest

@pytest.fixture(scope="class")
def db_class(request):
	class DummyDB:
		pass
	request.cls.db = DummyDB()
```

```python
# test_unittest_db.py
import unittest

import pytest

@pytest.mark.usefixtures("db_class")
class MyTest(unittest.TestCase):
	def test_method1(self):
		assert hasattr(self,"db")
		assert 0, self.db # fail
	def test_method2(self):
		assert 0, self.db # fail
```

the two test methods share the same self.db instance (intended when we defined fixture above)

##  Using autouse fixtures and accessing other fixtures
```python
# test_unittest_cleandir.py
import unittest

import pytest

class MyTest(unittest.TestCase):
	@pytest.fixture(autouse=True)
	def initdir(self,tmp_path,monkeypatch):
		monkeypatch.chdir(tmp_path)
		tmp_path.joinpath("samplefile.ini").write_text("# testdata")
	def test_method(self):
		with open("samplefile.ini") as f:
			s = f.read()
		assert "testdata" in s
```

Due to autouse flag in initdir, it will be used for all methods of the class

## Note
unittest.TestCase cannot use direct fixture -> use usefixtures and autouse to mix