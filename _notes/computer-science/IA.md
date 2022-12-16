---
---

# 알고리즘 개론 중간~

- Recurrence Relation
    - T(n) = T(n-1)+1
        
        O(n)
        
        ![Untitled](/assets/IA/Untitled.png)
        
        - Recursion Tree
            
            ![Untitled](/assets/IA/Untitled%201.png)
            
        - Substitution
            
            ![Untitled](/assets/IA/Untitled%202.png)
            
        - Masters Theorem
    - T(n) = T(n-1)+n
        
        O(n^2)
        
        ![Untitled](/assets/IA/Untitled%203.png)
        
        - Recursion Tree
            
            ![Untitled](/assets/IA/Untitled%204.png)
            
        - Substitution
            
            ![Untitled](/assets/IA/Untitled%205.png)
            
        - Masters Theorem
    - T(n) = T(n-1) + log n
        
        O(nlogn)
        
        ![Untitled](/assets/IA/Untitled%206.png)
        
        - Recursion Tree
            
            ![Untitled](/assets/IA/Untitled%207.png)
            
        - Substitution
            
            ![Untitled](/assets/IA/Untitled%208.png)
            
        - Masters Theorem
    
    You are just multiplying right side of the addition operand with n
    
    - T(n) = 2T(n-1)+1
        
        O(2^n)
        
        ![Untitled](/assets/IA/Untitled%209.png)
        
        - Recursion Tree
            
            ![Untitled](/assets/IA/Untitled%2010.png)
            
        - Substitution
            
            ![Untitled](/assets/IA/Untitled%2011.png)
            
        - Masters Theorem
    - T(n) = T(n/2)+1
        
        O(logn)
        
        ![Untitled](/assets/IA/Untitled%2012.png)
        
        - Recursion Tree
            
            ![Untitled](/assets/IA/Untitled%2013.png)
            
        - Substitution
            
            ![Untitled](/assets/IA/Untitled%2014.png)
            
        
        나누는 거면 T(1)까지
        
    - T(n) = 2*T(n/2)+n
        
        O(nlogn)
        
        ![Untitled](/assets/IA/Untitled%2015.png)
        
        - Recursion Tree
            
            ![Untitled](/assets/IA/Untitled%2016.png)
            
        - Substitution
            
            ![Untitled](/assets/IA/Untitled%2017.png)
            
    - Recursion Tree
    - Substitution
    - Masters Theorem
        - Masters Theorem in Algorithms for subtracting Functions
            
            T(n)=aT(n-b)+f(n)
            
            a>0 b>0 and f(n) = O(n^k) where k ≥ 0
            
            - case 1 : **if a = 1 → O(f(n)*n) ⇒ O(n^(k+1))**
                
                ![Untitled](/assets/IA/Untitled%2018.png)
                
            - case 2 : if a > 1 → O(n^k*a^(n/b))
                
                ![Untitled](/assets/IA/Untitled%2019.png)
                
            - case 3: if a< 1 → O(f(n)) ⇒ O(n^k)
        - Masters Theorem in Algorithms for Dividing Functions
            
            T(n)=aT(n/b)+f(n)
            
            a≥1 b>1 f(n)=O(n^k*log^p(n))
            
            1) $log_ba$
            
            2)k
            
            - case 1 : **if $log_ba >k$ then O(n^$log_ba$)**
                
                ![Untitled](/assets/IA/Untitled%2020.png)
                
            - case 2 : **if $log_ba$=k if p > - 1 O($n^klog$^(p+1)n) if p == -1 O($n^k$log logn) if p ≤ -1 O($n^k)$**
                
                ![Untitled](/assets/IA/Untitled%2021.png)
                
                ![Untitled](/assets/IA/Untitled%2022.png)
                
                ![Untitled](/assets/IA/Untitled%2023.png)
                
            - case 3: **if $log_ba$<k if p ≥0 O($n^klog$^(p)n) if p < 0 O($n^k$)**
                
                ![Untitled](/assets/IA/Untitled%2024.png)
                
                ![Untitled](/assets/IA/Untitled%2025.png)
                
    
- Greedy
    - Huffman Coding
        
        ![Untitled](/assets/IA/Untitled%2026.png)
        
        ![Untitled](/assets/IA/Untitled%2027.png)
        
        ![Untitled](/assets/IA/Untitled%2028.png)
        
- Heap
    
    full binary vs complete
    
    complete이 덜 완성(null없으면 complete, 이쁘면 full)
    
    heapify : heap 구성할 때 그 밑으로 heap인지 확인하면서