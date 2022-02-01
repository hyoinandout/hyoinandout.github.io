---
title: "코딩테스트를 준비할 때 알면 좋은 것들"
date: 2021-08-31
permalink: /posts/2021/08/blog-post-7/
tags:
  - algorithm
  - python
---

1. 파이썬 기준으로 시간 제한이 1초일 때

- N의 범위가 500 : N^3
- N의 범위가 2000 : N^2
- N의 범위가 100,000 : NlogN
- N의 범위가 10,000,000 : N

2. 파이썬 기준으로 공간:

- int a[1000] : 4KB
- int a[1000000] : 4MB
- int a[2000][2000] : 16MB

3. 특정한 프로그램의 수행시간 측정:

```
import time
start_time = time.time()

#프로그램 소스 코드
end_time = time.time()
print("Time : ", end_time - start_time)
```
