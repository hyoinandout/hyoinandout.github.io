with 문: 특별한 컨텍스트 안에서 실행되는 경우를 표현한다.

contextlib 내장 모듈을 통해 내가 만든 객체나 함수를 with문에서 쉽게 쓸 수 있다. -> contextmanager 데코레이터를 통해서
enter, exit magic method를 사용해 정의하는 방법보다 쉽다.

yield를 통해 구현하면 된다.

```python
@contextmanager
def log_level(level, name):
	logger = logging.getLogger(name)
	old_level = logger.getEffectiveLevel()
	logger.setLevel(level)
	try:
		yield logger
	finally:
		logger.setLevel(old_level)


with log_level(logging.DEBUG, 'other-log') as logger:
	logger.debug(f'대상: {logger.name}!')
	logging.debug('이 메시지는 출력되지 않습니다')
```