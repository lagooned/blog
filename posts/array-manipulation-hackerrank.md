---
title: "array manipulation"
date: "2021-01-13"
---

as part of my attempt to stay up-to-date on programming, as well as increase my confidence for interviews, i've been doing the hackerrank [interview prep kit](https://www.hackerrank.com/interview/interview-preparation-kit) series. one problem in particular caught my eye, and after solving it sub-optimally, i researched the best solution of the problem and learned some cool things along the way and connected it to my functional programming knowledge.

# the problem

[array manipulation](https://www.hackerrank.com/challenges/crush/problem)'s problem statement is as such:

1. take a zeroed integer array of size n
2. process a list of queries which transform the array
3. then find the maximum value within the array

the list of queries are of the form:

```
a0 b0 k0
a1 b1 k1
a2 b2 k2
...
```

where a is the start index, b is the end index, and k is the number to add to the range of integers in the zerod array.

# the naive solution

when i initially approached this problem, it seemed easy enough; i implemented the solution and passed the initial test cases.

```java
static long arrayManipulation(int n, int[][] q) {
    long[] arr = new long[n];
    long max = -1;
    for (int i = 0; i < q.length; i++) {
        for (int j = q[i][0]; j < q[i][1]; j++) {
            long val = js[j] + q[j][2];
            if (val > max) max = val;
            js[j] = val;
        }
    }
    return max;
}
```

now, come on... this isn't a terrible solution, it's actually pretty elegant; i'm even calculating the maximum value whilst performing the array manipulation. this implementation has a big problem, however. it falls over when the size of the array and queries gets huge, as the nested for loop makes the worst-case complexity **O( n * m )** where **n** is the number of queries, and **m** is the maximum value of **b<sub>n</sub> - a<sub>n</sub>**.

# the optimal solution

...
