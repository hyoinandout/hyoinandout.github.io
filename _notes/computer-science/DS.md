---
title: "Data Structure"
---
# 자료구조

- map vs unordered_map
    
    
    |  | map | unordered_map |
    | --- | --- | --- |
    | key 접근 time complexity | O(logn) | O(1) |
    | 구현 방법 | bst | hash table |

for(auto &i : d)

cout<<i.first<<”-”<<i.second<<endl;

![unorderedmap_vs_map](/assets/DS/unorderedmap_vs_map.PNG)

make_heap & priority_queue

priority_queue<int,vector<int>,greater<int>> → minheap

greater<int> 안쓰면 max heap