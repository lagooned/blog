---
title: "interview prep: topological sort"
date: "2021-01-25"
---

hey yall! my interview prep journey knows no bounds, and good thing because it's infinte content. [**depth-first search**](https://en.wikipedia.org/wiki/Depth-first_search) (commonly shortified as *dfs*) is a pervasive concept in computer science and offers good insight and introduction into the analysis of graphs. one particular application of depth-first search of a [**directed acyclic graph**](https://en.wikipedia.org/wiki/Directed_acyclic_graph) (or *dag* for short) caught my interest, that being **topological sort**.

# topsort

the topological sort of a dag is an ordering of all its vertex labels which satisfies the following statement:

`for all vertex v in any dag, there exists an ordering in which v occurs after the set of vertices that v directs to`
