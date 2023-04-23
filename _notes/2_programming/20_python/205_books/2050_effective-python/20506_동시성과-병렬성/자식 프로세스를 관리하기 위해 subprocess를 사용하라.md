---
---

자식 프로세스는 파이썬 인터프리터와 병렬로 실행되므로 CPU 코어를 최대로 쓸 수 있다.
```python
#run, poll, Popen, communicate-timeout
result = subprocess.run(['echo', '자식프로세스가 보내는 인사!'], capture_output=True, encoding='utf-8')
while proc.poll() is None:
	print('작업중...')
subprocess.Popen(
['openssl', 'dgst', '-whirlpool', '-binary'],
stdin=input_stdin,
stdout=subprocess.PIPE)
proc.communicate(timeout=0.1)
```