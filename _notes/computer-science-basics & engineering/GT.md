---
title: "Graph Theory"
---
# 그래프 이론 중간~

- Topics
    - **Tutte’s 1-factor Theorem**
        
        [Notes_211211_162635.pdf](/assets/GT/Notes_211211_162635.pdf)
        
        응용 : Any 3-regular graph that does not have a 1-factor?
        
        ![Untitled](/assets/GT/Untitled.png)
        
        For 3-regular graph without a cut-edge there is a 1-factor, say M
        
        ![Untitled](/assets/GT/Untitled%201.png)
        
    - **vertex coloring**
        - A k-(vertex) coloring of graph is a vertex labeling (or coloring) f:V(G) → S, where |S| = k. A k-coloring is proper if no two adjacent vertices have the same color.
        - k-colorable if there is a proper k-coloring
        - chromatic number of G, χ(G), is the minimum k such that G is k-colorable
        - In a proper k- coloring of G, the vertice in the same color → independent set
            - 2-colorable ↔ bipartite
            - k-colorable ↔ k-partite
        - graph with loops → uncolorable!
        - $K_(n_1,n_2,n_3, ... ,_k)$ : complete k-partite graph
        - k-chromatic ↔ χ(G)=k
        - H≤G ⇒ χ(H) ≤ χ(G)
        - color-critical : χ(H) < χ(G)
        - χ($K_n$) = n → χ(G) ≥ $w$(G) ($w$(G): clique number = maximum order of a complete graph contained in G)
        
        ![Untitled](/assets/GT/Untitled%202.png)
        
        ![Untitled](/assets/GT/Untitled%203.png)
        
        maximum number of independent set
        
    - **various bounds for chromatic number**
        - Lower bounds
            - H≤G ⇒ χ(H) ≤ χ(G)
            - χ(G) ≥ $w$(G)
            
            ![Untitled](/assets/GT/Untitled%204.png)
            
            ![Untitled](/assets/GT/Untitled%205.png)
            
            - χ(G+H) = max{χ(G),χ(H)} , χ(G V H) = χ(G) + χ(H)
            - χ(G ㅁ H) ≥ max{χ(G),χ(H)}
            
            ![Untitled](/assets/GT/Untitled%206.png)
            
        
        **greedy algorithm for coloring**
        
        ![Untitled](/assets/GT/Untitled%207.png)
        
        - Upper bounds
            1. χ(G) ≤ 🔺(G) + 1 ) counterexample : $K_n,_n$
            
            ![Untitled](/assets/GT/Untitled%208.png)
            
            1. If a graph G has degree sequence $d_1>= ... >= d_n$, 
            then χ(G)≤ 1+$max_i$min{$d_i,i-1$}
            
            ![Untitled](/assets/GT/Untitled%209.png)
            
            ![Untitled](/assets/GT/Untitled%2010.png)
            
        - Interval graphs
            
            χ(G) = $w$(G)
            
            ![Untitled](/assets/GT/Untitled%2011.png)
            
        - Brook's Theorem
            
            For a connected graph G, if χ(G) = 🔺(G) + 1 then G is either complete graph or an odd cycle (🔺 = k)
            
            ![Untitled](/assets/GT/Untitled%2012.png)
            
            - cases
                
                k≥3 because G is complete when k≤1, and G is an odd cycle or is bipartite when k=2
                
                🎯 Order the vertices so that each has at most k-1 lower indexed neighbors; greedy coloring
                
                1. G is not k-regular
                    
                    Choose a vertex of degree less than k as $v_n$. Since G is connected, draw a spanning tree from $v_n$, assigning indices in decreasing order as we reach vertices. Each vertex other than $v_n$ in the resulting ordering $v_1$ ... $v_n$ has a higher-indexed neighbor along the path to $v_n$ in the tree. Hence each vertex has at most k-1 lower indexed neighbors, and the greedy coloring uses at most k colors.
                    
                2. G is k-regular
                    - G has a cut-vertex
                    
                    ![Untitled](/assets/GT/Untitled%2013.png)
                    
                    - G does not have a cut-vertex
                    
                    ![Untitled](/assets/GT/Untitled%2014.png)
                    
                    ![Untitled](/assets/GT/Untitled%2015.png)
                    
                    ![Untitled](/assets/GT/Untitled%2016.png)
                    
        
    - **Mycielski’s construction**
        
        ![Untitled](/assets/GT/Untitled%2017.png)
        
        ![Untitled](/assets/GT/Untitled%2018.png)
        
        Let G' be the graph obtained by Mycielski's construction from G. The following properties hold.
        
        - If G is triangle-free, then G' is triangle free
        - If χ(G) = k, then χ(G') = k + 1
        
        triangle-free:
        
        ![Untitled](/assets/GT/Untitled%2019.png)
        
        χ(G') = k + 1:
        
        $f(u_i)=f(v_i)$ and $f(w) =k+1$ → χ(G') ≤ χ(G) + 1
        
        Show χ(G) < χ(G') :
        
        ![Untitled](/assets/GT/Untitled%2020.png)
        
        ![Untitled](/assets/GT/Untitled%2021.png)
        
    - **Turan graph**
        
        ![Untitled](/assets/GT/Untitled%2022.png)
        
    - **Turan’s theorem**
        
        Q. What is the minimum number of the edges that a k-chromatic graph with N vertices can have?
        
        what if the graph above is connected? $k(k-1)/2 + N - k$
        
        What about the maximum?
        
        ![Untitled](/assets/GT/Untitled%2023.png)
        
        ![Untitled](/assets/GT/Untitled%2024.png)
        
        ![Untitled](/assets/GT/Untitled%2025.png)
        
        ![Untitled](/assets/GT/Untitled%2026.png)
        
        prove by induction on r:
        
        ![Untitled](/assets/GT/Untitled%2027.png)
        
        r = 1
        
        true for r -1
        
        ![Untitled](/assets/GT/Untitled%2028.png)
        
        ![Untitled](/assets/GT/Untitled%2029.png)
        
        ![Untitled](/assets/GT/Untitled%2030.png)
        
        ![Untitled](/assets/GT/Untitled%2031.png)
        
        ![Untitled](/assets/GT/Untitled%2032.png)
        
        Tripartite-turan graph (T6,3)
        
        ![Untitled](/assets/GT/Untitled%2033.png)
        
        pf) Claim: G has no $K_4$ ((3+1) - clique)
        
        Suppose not : then we can find a quadrangle 
        
        1. convex
        2. nonconvex
        
        above both case has pair more than distance>1 so contradict
        
    - **chromatic polynomial**
        
        1.$k^n$
        
        2.$k(k-1)(k-2)...(k-n+1)$
        
        3.k(k-1)^(n-1)
        
        ![Untitled](/assets/GT/Untitled%2034.png)
        
        ![Untitled](/assets/GT/Untitled%2035.png)
        
        χ(G;k) + χ(G.e;k) = χ(G-e;k)
        
        예제) χ($C_4$;k)=??
        
        ![Untitled](/assets/GT/Untitled%2036.png)
        
        ![Untitled](/assets/GT/Untitled%2037.png)
        
        예제) χ($C_4$;k)=??
        
        ![Untitled](/assets/GT/Untitled%2038.png)
        
    - **Planar graph**
        
        A graph is **planar** if it has a drawing without crossings.
        
        Such a drawing is a **planar embedding** of G. A **plane graph** is a particular planar embedding of a planar graph
        
        The **faces** of a plane graph are the maximal regions of the plane that contain no point used in the embedding
        
        - A finite planar graph has one unbounded face
        
        dual graph of a planar G = G*
        
        ![Untitled](/assets/GT/Untitled%2039.png)
        
        ![Untitled](/assets/GT/Untitled%2040.png)
        
        ![Untitled](/assets/GT/Untitled%2041.png)
        
        A graph is **outerplanar** if it has an embedding with every vertex on the boundary of the unbounded face
        
    - **Euler’s formula**
        
        If a connected plane graph G has exactly n vertices, e edges and f faces, then n-e+f = 2
        
        pf) Induction on n ≥ 1
        
        base: n = 1
        
        ![Untitled](/assets/GT/Untitled%2042.png)
        
        Induction step: n≥2:
        
        ![Untitled](/assets/GT/Untitled%2043.png)
        
        2e ≥ 3f (every face boundary in a simple graph contains at least three edges) , if bipartite 2e ≥ 4f
        
        ![Untitled](/assets/GT/Untitled%2044.png)
        
        ![Untitled](/assets/GT/Untitled%2045.png)
        
    - **coloring of planar graphs**
        
        ![Untitled](/assets/GT/Untitled%2046.png)
        
    - **Five color theorem**
        
        ![Untitled](/assets/GT/Untitled%2047.png)
        
        not five colorable → contradiction (allocate colors on neighbor of v, 1,2,3,4,5 and then there exists crossing)
        
    - **spectrum of a graph**
        
        spec$K_n$ ?
        
        ![Untitled](/assets/GT/Untitled%2048.png)
        
        spec$K_m,_n$?
        
        ![Untitled](/assets/GT/Untitled%2049.png)
        
        ![Untitled](/assets/GT/Untitled%2050.png)
        
          
        
    - **eigenvalues of a graph**
        
        ![Untitled](/assets/GT/Untitled%2051.png)
        
        ![Untitled](/assets/GT/Untitled%2052.png)
        
        ![Untitled](/assets/GT/Untitled%2053.png)
        
        ![Untitled](/assets/GT/Untitled%2054.png)
        
        ![Untitled](/assets/GT/Untitled%2055.png)
        
        ![Untitled](/assets/GT/Untitled%2056.png)
        
        ![Untitled](/assets/GT/Untitled%2057.png)
        
        다 달라야 deserve 하니까
        
        ![Untitled](/assets/GT/Untitled%2058.png)
        
        ![Untitled](/assets/GT/Untitled%2059.png)
        
        ![Untitled](/assets/GT/Untitled%2060.png)
        
        ![Untitled](/assets/GT/Untitled%2061.png)
        
        ![Untitled](/assets/GT/Untitled%2062.png)
        
        ![Untitled](/assets/GT/Untitled%2063.png)
        
        ![Untitled](/assets/GT/Untitled%2064.png)
        
        ![Untitled](/assets/GT/Untitled%2065.png)
        
        ![Untitled](/assets/GT/Untitled%2066.png)
        
        ![Untitled](/assets/GT/Untitled%2067.png)
        
        ![Untitled](/assets/GT/Untitled%2068.png)
        
- Problems
    - Explain why the proof of Five-Color Theorem cannot be applied to prove FourColor
    Theorem.
    - Construct a planar graph G with χ(G) = 4.
    - Use the Four Color Theorem to prove that every planar graph decomposes into
    two bipartite graphs
    - Set problem
        
        Prove that every simple planar graph with at least four vertices has at least
        four vertices with degree less than 6.
        
        For each even value of n with n ≥ 8, construct and n-vertex simple planar
        graph G that has exactly four vertices with degree less than 6.