GIL로 인해(선점형 쓰레드가 없음) 멀티쓰레딩 연산이 좋지 않다 - 유명한 소수 찾기 예시
그럼에도 유용: 시스템 콜은 GIL 영향 범위가 아님. -> ex) 블로킹 IO와 계산을 동시에 할 수 있다.
```python
import select
import socket
import time
from threading import Thread

def slow_systemcall():
	select.select([socket.socket()], [], [], 0.1)

start = time.time()

threads = []
for _ in range(5):
	thread = Thread(target=slow_systemcall)  # 스레드 없었다면?
	thread.start()
	threads.append(thread)

def compute_helicopter_location(index):
	pass

for i in range(5):
	compute_helicopter_location(i)

for thread in threads:
	thread.join()

end = time.time()
delta = end - start
print(f'총 {delta:.3f} 초 걸림')
```
병렬성을 피하라...?