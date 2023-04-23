---
---

우선순위 큐가 필요할 때

우선순위 큐 없이 리스트를 매번 삽입 때 마다 정렬한다면, n * n log(n) -> O(n^2log(n))

```python
import heapq

heap_list = []
heapq.heappush(heap_list,element)
heapq.heappop(heap_list)

original_list = list(range(10**5,0,-1))
heapq.heapify(orignal_list)
```
기본적으로 minheap

heapq를 통해 정렬하려면 원소들이 서로 비교 가능하고 원소 사이 자연스러운 정렬 순서 존재해야 한다.
```python
import functools

@functools.total_ordering
class Human:
    def __init__(self, gender, age):
        self.gender = gender
        self.age = age

    def __lt__(self,other):
        return self.age < other.age
```

단점: 구현에 따라 메모리 사용량이 최대크기보다 줄어들지 않는다.
스레드 안전한 다른 선택이 필요하다면, queue.PriorityQueue

아까 말씀드린 @total_ordering과 __lt__ 에 관해서,

@total_ordering 으로 데코레이팅 된 클래스는 __lt__(), __gt__(), __le__(), __ge__() 중 하나만 정의하고, __eq__() 만 정의하면 나머지 비교 연산자도 모두 사용 가능하다고 하네요.

제가 말씀드린 대로 __lt__만 구현해도 heapify가 가능하지만,
책에서 `~클래스에 비교 기능과 자연스러운 정렬 순서를 제공할 수 있다`는 맥락이므로, @functools.total_ordering까지 쓴 것 같습니다.
