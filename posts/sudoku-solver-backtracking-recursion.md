---
title: "recursion & backtracking: sudoku solver"
date: "2021-04-03"
---

hey y'all. i was recently looking up more problems to increase my computer science knowledge and came across a very interesting one: *brute forcing sudoku using recursion and backtracking*. learning how to do this has always been a goal of mine, yet the solution never quite came to me so i was forced to research it. luckily there are quite a lot of resources online which can help you build intuition on how this solution works; i hope this article can become one of them.

if you've never heard of sudoku before, it goes like so:

`given a 9x9 grid of numbers and some initial correct squares, mutate the grid of numbers as follows: each of 9 3x3 adjacent grids, 9 rows, and 9 columns, contain the numbers 1-9 with no repeats`

creating this grid constitutes a solved soduku and a job well done. these puzzles are very easy to find in books, online, and in your daily newspaper. amazingly, due to the relatively minor constraints, it makes it a very good topic to start exploring brute-force algorithms with, given that the [total number of sodukus](https://en.wikipedia.org/wiki/Mathematics_of_Sudoku#Validity_preserving_transformations) can be reduced to only 5,472,730,538 after defining automorphisms according to the game's rules. given the minimum of 17 initial values to solve any unique game, this reduces the problem space to something that brute forcable in a short time. i am here to give you a brief introduction on how to accomplish this.

lets define some terms:

- *grid*
  - the 9x9 grid of numbers, easily enough
- *clue*
  - one of the 17 or more initial values given for a unique grid, less for a mapping to multiple solutions
- *recursion*
  - the act of a algorithm dispatching itself given a new context
- *backtracking*
  - the act of an algorithm, for each subproblem of a macroproblem:
    1. assume a solution
    2. continue solving until disproving this assumption
    3. assume another solution and continue solving to eventually arrive at the whole solution

# the subproblem

in soduku, the identity of the subproblem is somewhat intuitive: `can we put a given number in a particualar square?`

we easily can write a subroutine that answers the question, given the current state of the board; its java implementation is as follows:

```java
```

# the backtracking & recursion

if we just so happen to choose the right number for each square the first time, then we've solved the puzzle and we get to go home. however, ultimately this is not so simple and we must reformulate this in the realm of reality; any guess we make is an **assumption** and subsequent action based on it could be incorrect. this is where backtracking will eventually come in:

`is it valid to put a particular number in a given square? if so then put it there and keep going until it's not valid, if not try another possibility`.
