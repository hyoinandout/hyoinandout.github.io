---
title: "PS with Python basic"
date: 2021-08-31
tags:
  - algorithm
  - python
---

1. 1 sec time limit
- N 500 : N^3
- N 2000 : N^2
- N 100,000 : NlogN
- N 10,000,000 : N
2. Space limit:
- int a[1000] : 4KB
- int a[1000000] : 4MB
- int a[2000][2000] : 16MB
3. How to get time in python:
```python
	import time
	start_time = time.time()

	# ...
	
	end_time = time.time()
	print("Time : ", end_time - start_time)
```
