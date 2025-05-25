Data structure is the collection of data types arranged in a specific order. 
It is of two types i.e Linear and Non Linear data structures.

#### **Linear Data Structures**
- Array 
- Stack
- Queue
- Linked List

#### **Non Linear Data Structures**
- Graph based Structures
- Tree based Structures


![[Pasted image 20250520142024.png]]

### Asymptotic Analysis

The study of change in performance of the algorithm with the change in the order of the input size is defined as asymptotic analysis.

Asymptotic notations are the mathematical notations used to describe the running time of an algorithm when the input tends towards a particular value or a limiting value.

There are mainly three asymptotic notations:
-   Big-O notation
-   Omega notation
-   Theta notation

Big-O notation represents the upper bound of the running time of an algorithm. Thus, it gives the worst-case complexity of an algorithm.

Omega notation represents the lower bound of the running time of an algorithm. Thus, it provides the best case complexity of an algorithm.

Theta notation encloses the function from above and below. Since it represents the upper and the lower bound of the running time of an algorithm, it is used for analyzing the average-case complexity of an algorithm.

O(g(n)) = { f(n): there exist positive constants c and n0 such that 0 ≤ f(n) ≤ cg(n) for all n ≥                    n0 }
For any value of `n`, the running time of an algorithm does not cross the time provided by `O(g(n))`.