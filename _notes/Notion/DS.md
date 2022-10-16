---
title: "DS"
---
# 자료구조

- map vs unordered_map
    
    
    |  | map | unordered_map |
    | --- | --- | --- |
    | key 접근 time complexity | O(logn) | O(1) |
    | 구현 방법 | bst | hash table |

for(auto &i : d)

cout<<i.first<<”-”<<i.second<<endl;

![/assets/DS/unorderedmap_vs_map.PNG](%E1%84%8C%E1%85%A1%E1%84%85%E1%85%AD%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9%20edee07a51aeb4cf68b0fdf527b47ee11/unorderedmap_vs_map.png)

make_heap & priority_queue

priority_queue<int,vector<int>,greater<int>> → minheap

greater<int> 안쓰면 max heap