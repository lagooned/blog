---
title: "recursion & backtracking: sudoku solver"
date: "2021-01-13"
---

hey y'all. i was recently looking up more problems to increase my computer science knowledge and came across a very interesting one: *brute forcing sudoku using recursion and backtracking*. learning how to do this has always been a goal of mine, yet the solution never quite came to me so i was forced to research it. luckily there are quite a lot of resources online which can help you build intuition on how this solution works; i hope this article can become one of them.

if you've never heard of sudoku before, it goes like so:

```given a 9x9 grid of numbers and some initial correct squares, mutate the grid of numbers as follows: each of 9 3x3 adjacent grids, 9 rows, and 9 columns, contain the numbers 1-9 with no repeats```

creating this grid constitutes a solved soduku and a job well done. these puzzles are very easy to find in books, online, and in your daily newspaper. amazingly, due to the relatively minor constraints, it makes it a very good topic to start exploring brute-force algorithms with, given that the [total number of sodukus](https://en.wikipedia.org/wiki/Mathematics_of_Sudoku#Validity_preserving_transformations) can be reduced to only 5,472,730,538 after defining automorphisms according to the game's rules. 

