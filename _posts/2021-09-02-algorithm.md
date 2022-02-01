---
title: "코딩테스트를 준비할 때 알면 좋은것들 2"
date: 2021-09-02
permalink: /posts/2021/09/blog-post-2/
tags:
  - algorithm
  - python
---

## 자료

- INF = 1e9 / 1,000,000,000 / 987,654,321
- round(123.4567,3) = 123.457
- python list = linked-list(append, remove)
- 크기가 N인 1차원 리스트 initialize :

  ```
  n = 10
  a = [0] * n
  # a = [0,0,0,0,0,0,0,0,0,0]
  print(a)
  ```

- a[-1] : 가장 마지막 원소
- slicing :

  ```
  a = [1,2,3,4,5,6,7]
  # a = [2,3,4,5]
  print(a[1:5])
  ```

- list comprehension :

  ```
  # 0 부터 20까지 짝수만 포함하는 리스트
  a = [i for i in range(21) if i % 2 == 0]
  print(a)
  ```

  same as

  ```
  a = []
  for i in range(21):
    if i % 2 == 0:
      a.append(i)

  print(a)
  ```

  another example(without if)

  ```
  # 구구단 - 3단
  a = [i*3 for i in range(1,10)]
  print(a)
  ```

  **2차원 리스트 초기화**

  ```
  n = 3
  m = 4
  a = [[0]* m for _ in range(n)]
  print(a)
  ```

- list functions
  |name|use|ex|O(n)|
  |----|----|----|----|
  |append|||1|
  |sort|a.sort()||nlogn|
  ||a.sort(reverse=True)||nlogn|
  |reverse|||n|
  |insert|a.insert(index,value)||n|
  |count|a.count(value)|데이터 개수 셀때|n|
  |remove|a.remove(value)|값 가진 원소가 여러개면 하나만 제거|n|
- how to remove specific list value which appears more than once in the list:

  ```
  a = [1,2,3,4,4,4,4,5]
  remove_set = {2,4}

  result = [i for i in a if i not in remove_set] //Notice this part
  print(a)
  ```

- In string : "''" or '""' or "\"\""
- String +
  ```
  a = "GIST"
  b = "Student"
  print(a + " " + b)
  # GIST Student
  ```
- tuple : immutable
- dict : hash table

  ```
  a = dict()
  a['집'] = 'house'
  a['동물원'] = 'zoo'
  print(a) // {'집':'house','동물원':'zoo'}

  if '집' in a:
    print('key:집 in a') //  in 사용 가능, 검색, 수정 O(1)

  key_list = a.keys()
  value_list = a.values()
  for key in key_list:
    print(a[key]) // house'\n'zoo
  ```

- set : 특정한 데이터가 이미 등장한 적이 있는지 여부 체크할 때 효과적 - hash table로 구현됨

  ```
  a = set([1,2,3,4,4,5,6,6])
  print(a) // {1,2,3,4,5,6}
  b = {1,2,3,4,5,6,6,6,6,7}
  print(a) // {1,2,3,4,5,6,7}

  print(a|b) // b
  print(a&b) // a
  print(b-a) // {7}

  a.add(8) // 1
  a.update([8,9,10])
  a.remove(8) // 1
  ```
