---
title: "코딩테스트를 준비할 때 알면 좋은것들 5"
date: 2021-09-16
permalink: /posts/2021/09/blog-post-7/
tags:
  - algorithm
  - python
---

- 배열 중간값(median) : (n-1)//2
- 함수 호출 과정에서 인자를 넘겨줄 때, 파라미터의 변수를 직접 지정해서 값을 넣어줄 수 있음

  ```
  def multipy(a,b):
    print("함수의 결과:", a*b)

  multiply(b=7,a=3)
  ```

- 함수 안에서 함수 밖의 데이터를 변경해야 하는 경우 : global
  global 키워드로 변수 지정할 시 해당 함수에서는 함수 바깥에 선언된 변수 참조

  ```py
  a = 0

  def f():
    global a
    a += 1

  for i in range(10):
    f()
  print(a)
  #10
  ```

- 람다 표현식

  ```py
    def divide(a,b):
      return a//b

    print(divide(6,2))
    # 3
    print((lambda a,b: a//b)(6,2))
    # 3
  ```

- 빠르게 입력받기
  ```py
  import sys
  sys.stdin.readline().rstrip()
  ```
- 각 변수를 콤마로 구분하여 출력할 때, 변수의 값 사이에 의도치 않은 공백이 삽입될 수 있다. 이럴때는
  1.  ```py
      print("정답은 " + str(answer) + "입니다")
      ```
  2.  ```py
       # or
       print(f"정답은 {answer}입니다")
      ```
