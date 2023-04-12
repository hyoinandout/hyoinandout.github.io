bisect: 이진 탐색
```python
import bisect

data = list(range(10**5))

index_from_linear_search = data.index(91234)  # O(n)

index_from_bisection = bisect.bisect_left(data,91234) # O(logn)
index_from_bisection_left = bisect.bisect_left(data,91234.4)  # 91235
index_from_bisection_right = bisect.bisect_right(data,91234.4)  # 91235
```

upper, lower bound