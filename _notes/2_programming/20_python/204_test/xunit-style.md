---
---

##  Module level setup/ teardown
multiple test fx, cls in single module
```python
def setup_module(module: Optional):
	"""setup any state specific to the execution of the given module"""

def teardown_module(module: Optional):
	"""teardown any state that was previously setup with a setup_module method"""
```

## Class level setup/ teardown
```python
@classmethod
def setup_class(cls):
	"""setup any state specific to the execution of the given class(which usually contains tests)"""

@classmethod
def teardown_class(cls):
	"""teardown any state that was previously setup with a setup_class method"""
```

## Method and function level setup/teardown
```python
def setup_method(self, method: Optional):
	"""setup any state tied to the execution of the given method in a class. setup_method is invoked for every test method of a class"""

def teardown_method(self, method: Optional):
	"""teardown any state that was previously setup with a setup_method call"""
```

```python
def setup_function(function: Optional):
	"""setup any state tied to the execution of the given function. setup_method is invoked for every test function in the module"""

def teardown_function(function: Optional):
	"""teardown any state that was previously setup with a setup_function call"""
```