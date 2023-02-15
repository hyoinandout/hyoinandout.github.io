문자열 데이터의 시퀀스
1. bytes
	1. 부호가 없는 8 바이트 데이터 
	2. 직접 대응하는 텍스트 엔코딩이 없음 -> str.encode()
2. str
	1. 유니코드 코드 포인트
	2. 직접 대응하는 이진 인코딩이 없음 -> bytes.decode()

헬퍼 함수를 통해 입력 값이 코드가 원하는 값과 일치하는지 확인하자.
```python
def to_str(bytes_or_str):
	if isinstance(bytes_or_str, bytes):
		value = bytes_or_str.decode('utf-8')
	else:
		value = bytes_or_str
return value # str 인스턴스
```

기억해야 할 두가지 문제점:
1. bytes와 str은 서로 호환되지 않음 -> 전달 중인 문자 시퀀스가 어떤 타입인지 알아야 한다.
2. file handle 관련 연산들이 default로 유니코드 문자열을 요구하고, 이진 바이트 문자열을 쓰려면 모드 뒤에 b를 붙여줘야 한다.