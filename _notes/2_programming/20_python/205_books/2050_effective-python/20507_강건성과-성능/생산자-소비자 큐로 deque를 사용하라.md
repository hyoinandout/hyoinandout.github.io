생산자-소비자 큐(대기열)의 많은 예시

큐가 도입되는 이유:
새로운 원소를 받아들이는 지연시간은 최소화 + 일정한 스루풋

```python
import collections

queue = []
queue.append()
queue.pop(0)  # O(n), 리스트의 모든 남은 원소들을 제자리로 해야하기 때문

deque = collections.deque()  # deque, 양방향 큐
deque.append
deque.popleft()  # O(1)
```