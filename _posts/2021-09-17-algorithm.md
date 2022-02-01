---
title: "코딩테스트를 준비할 때 알면 좋은것들 6 - 2020 카카오 코테 : 실패율"
date: 2021-09-17
permalink: /posts/2021/09/blog-post-8/
tags:
  - algorithm
  - python
  - kakao
---

20분정도 소요해야하는 문제인데 custom하여 sort하는 방법과 손코딩을 안하고 그냥 냅다 풀어버리니까 논리가 부족했다(모든 케이스들에 대하여 다루지 못했다.) 반례가 생각이 안나서 결국 질문하기를 보고 반례를 알게 되었는데 이 포스트에서는 내가 이번 문제로 알게된 파이썬 함수/문법과 나의 코드와 예시 답안을 비교하며 반성하는 시간을 가지고자 한다.

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/42889)

## 새로 알게된 지식

- 파이썬의 정렬
  sort (return x), sorted (return,모든 iterable 가능)
  - key
    sort, sorted 모두 비교하기 전에 각 리스트 요소에 대해 호출할 함수를 지정하는 key 매개변수를 가지고 있다.
    ```py
      sorted("This is a test string from Alfred".split(), key=str.lower)
      #['a','Alfred','from','is','string','test','This']
    ```
  - lambda 표현식
    lambda 인자리스트: 표현식
    ```py
      #ex
      lambda x,y : x+y
    ```
    - key + lambda
      ```py
        sorted(answer,key = lambda t : t[1],reverse=True)
      ```
  - 정렬 안정성과 복잡한 정렬
    정렬은 순서가 유지된다.
- enumerate
  인덱스 번호와 컬렉션의 원소를 tuple 형태로 반환

  ```py
  for p in enumerate(t):
    print(p)

  #(0,1)
  #(1,5)
  #(2,52) ...
  ```

- heapq
  정렬 -> 우선순위 큐 생각나서 쓰려 했었음
  ```py
    import heapq
  ```
  list 원소 추가 O(1), 삭제 O(n)
  heapq 원소 추가 O(logn), 삭제 O(logn)
  사용법
  ```py
    q = []
    heapq.heappush(q,a)
    heapq.heappop(q)
  ```

## 부족했던 논리

1. 자료구조의 형식을 통일시키지 않아 런타임 에러가 발생했었다.
2. 손코딩을 안하고 푸니 특정한 경우에 대하여 실패했다. + 반례를 생각하지 못했다.

   - divisor가 0이 될 때 런타임 에러가 나오는 것은 디버깅을 통해 알 수 있었고 예외처리까지 해주는 것은 좋았는데 그 예외처리를 할 때 break를 쓰면서 논리가 깨지는 것을 파악하지 못했다. 하나의 확일한 로직 안에서 푼 것이 아니라 그냥 닥치는대로 코드를 짜니까 이런 경험을 한 거 같다. 아무리 쉬운 문제라도 손코딩을 하고 pseudo로 대략적으로 변환한 뒤에 풀어보자.

참조:
docs.python.org
devpouch.tistory.com
wikidocs.net
daleseo.com/python-heapq
