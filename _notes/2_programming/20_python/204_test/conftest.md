#test

pytest에서 conftest.py를 통해, 초기 설정을 할 수 있다.
보통
```python
@pytest.fixture(scope="session")
```
이런 식으로 test session동안 생성되고 재사용되는 fixture를 DBFactory로 생성해서 테스트한다.
