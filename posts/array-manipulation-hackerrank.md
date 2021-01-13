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

in order to fully appreciate the optimal solution, there is a very cool concept at the core of functional programming that is at the core of this solution: **fold**.

## the fold

a fold is not some crazy dance move, it's a *function*. the whole idea of functional programming is that highly complex operations can be broken down into compositions of simple, named transformations. this concept is similar the purpose of object oriented design patterns; it gives programmers a **vocabulary**, which is the most important part of computer science as it lets you discuss solutions.

a fold is result of the following expression when built from a list, when done with the addition operator:

```
foldl (+) 0 [1,2,3,4,5] => (((((0+1)+2)+3)+4)+5) => 15
foldr (+) 0 [1,2,3,4,5] => (1+(2+(3+(4+(5+0))))) => 15
```

when the operator you use is communative, this has the effect of replacing the commas in a list definition with the operator. this is a more specific version of a **reduce** which is found in most langauges. here is an example of using both a left and right fold with a list of strings and the concatination operation:

```
foldl (++) "" ["1","2","3","4","5"] => "54321"
foldr (++) "" ["1","2","3","4","5"] => "12345"
```

as you can see using this can be very useful for concicely reducing a list to a single value in a particular order

here are the definitions in haskell:

```haskell
foldl f z [] = z
foldl f z (x:xs) = foldl f (f z x) xs

foldr f z [] = z
foldr f z (x:xs) = f x (foldr f z xs)
```

## from fold to scan

the reason i've introduced the fold is because it is a good segue into it's sister function, the **scan**. a scan is similar to a fold, but instead of reducing the list to a single value, it reduces it into a list of the successive values created by doing the folding operation:

```
scanl (+) 0 [1,2,3] => [0+1,((0+1)+2),(((0+1)+2)+3)] => [1,3,6]
scanl (+) 0 [1,2,3] => [0+1,(0+(1+2)),(0+(1+(2+3)))] => [1,3,6]
```

this function may look quite innoquous, but it is in fact the key to optimizing the array manipulation problem.

here's its haskell definition for completeness:

```haskell
scanl :: (b -> a -> b) -> b -> [a] -> [b]
scanl = scanlGo
  where
    scanlGo :: (b -> a -> b) -> b -> [a] -> [b]
    scanlGo f q ls = q : (case ls of
                          [] -> []
                          x:xs -> scanlGo f (f q x) xs)

scanr :: (a -> b -> b) -> b -> [a] -> [b]
scanr _ q0 [] = [q0]
scanr f q0 (x:xs) = f x q : qs
                    where qs@(q:_) = scanr f q0 xs
```

you don't really have to understand these, it's just helpful to explore how they are implemented. i found the implementations [here](https://hackage.haskell.org/package/base-4.14.1.0/docs/src/GHC.List.html).

# the array manipulation

the complexity of the array manipulation step of the original problem can be reduced by utilizing the scan via the following observation:

```
scanl (+) 0 [0,0,1,0,0,0,0,-1,0,0] => [0,0,1,1,1,1,1,1,0,0]
scanr (+) 0 [0,0,-1,0,0,0,0,1,0,0] => [0,0,1,1,1,1,1,1,0,0]
```

this is huge<sup>tm</sup>, as it shows that we can use this scan operation to remove the inner for loop from the naive solution. this is the final implementation in java:

```java
static long arrayManipulation(int n, int[][] queries) {
    long[] ns = new long[n+2];
    for (int i = 0; i < queries.length; i++) {
        int a = queries[i][0];
        int b = queries[i][1];
        int k = queries[i][2];
        ns[a-1] = ns[a-1]+k;
        ns[b] = ns[b]-k;
    }
    long max = -1l;
    for (int i = 0; i < n; i++) {
        ns[i+1] = ns[i]+ns[i+1];
        if (ns[i] > max) max = ns[i];
    }
    return max;
}
```
