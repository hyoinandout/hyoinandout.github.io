---
title: "코딩테스트를 준비할 때 알면 좋은것들 8 - 4가지 sorting algorithm"
date: 2021-10-02
permalink: /posts/2021/10/blog-post-1/
tags:
  - algorithm
  - python
  - sort
---

## - Insertion Sort

카드 정렬과 비슷: 진짜 카드 삽입하듯이
O(n^2)

```py
def insertionSort(array):
  for i in range(1,len(array)):
    key = array[i]
    j=i-1
    while j>=0 and array[j]>key:
      array[j+1]=array[j]
      j-=1
    array[j+1] = key
```

## - Merge Sort

다 분할 후 짝지어 비교하면서 합친다 (divide and conquer)
O(nlogn)

```py
def mergeSort(array):
  if len(array) > 1:
    mid = len(array) // 2
    L = [:mid]
    R = [mid:]
    mergeSort(L)
    mergeSort(R)
    i,j,k=0,0,0
    while i<len(L) and j<len(R):
      if L[i] < R[j]:
        arr[k] = L[i]
        i+=1
      else:
        arr[k] = R[j]
        j+=1
      k+=1
    while i<len(L):
      arr[k]=L[i]
      i+=1
      k+=1
    while j<len(R):
      arr[k]=R[j]
      j+=1
      k+=1
```

## - Selection Sort

말그대로 선택 정렬
1루프에서 가장 작은거 선택 스왑
2루프(1루프보다 길이 1 작음)에서 그다음으로 작은거 선택 스왑
O(n^2)

```py
def selectionSort(array):
  for i in range(len(array)):
    min_idx = i
    for j in range(i+1,len(array)):
      if array[j] < array[min_idx]:
        min_idx = j
    (array[i],array[min_idx]) =(array[min_idx],array[i])
```

## - Quick Sort

기준을 정하고 그 기준 왼쪽은 작게 오른쪽은 크게
그렇게 계속 분할
O(nlogn)

```py
#improved version
def partition(array,low,high):
  i = (low-1)
  pivot = array[high]
  for j in range(low,high):
    if array[j] <= pivot:
      i=i+1
      array[i],array[j] = array[j],array[i]
  array[i+1],array[high] = array[high],array[i+1]
  return (i+1)

def quickSort(array,low,high):
  if len(array)==1:
    return array
  if low < high:
    pi = partition(array,low,high)
    quickSort(array,low,pi-1)
    quickSort(array,pi+1,high)
```
