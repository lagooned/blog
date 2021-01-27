---
title: "topological sort of a directed acyclic graph"
date: "2021-01-25"
---

hey yall! my interview prep journey knows no bounds, and good thing because it's infinite content. [**depth-first search**](https://en.wikipedia.org/wiki/Depth-first_search) (commonly shortified as *dfs*) is a pervasive concept in computer science and offers good insight and introduction into the analysis of graphs. one particular application of depth-first search of a [**directed acyclic graph**](https://en.wikipedia.org/wiki/Directed_acyclic_graph) (or *dag* for short) caught my interest, that being **topological sort**.

# topsort

the topological sort of a dag is an ordering of all its vertex labels which satisfies the following statement:

`for all vertex v in any dag, there exists an ordering in which v occurs *after* the set of vertices that v directs to`

so in the following graph:

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="100%" height="100%" viewBox="0.00 0.00 206.00 188.00"><g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 184)"><title>G</title>
<polygon fill="#000000" stroke="transparent" points="-4,4 -4,-184 202,-184 202,4 -4,4"/>
<g id="node1" class="node"><ellipse fill="none" stroke="#FFFFFF" cx="171" cy="-162" rx="27" ry="18"/><text text-anchor="middle" x="171" y="-157.8" font-size="14.00" fill="#FFFFFF">3</text></g>
<g id="node2" class="node"><ellipse fill="none" stroke="#FFFFFF" cx="107" cy="-90" rx="27" ry="18"/><text text-anchor="middle" x="107" y="-85.8" font-size="14.00" fill="#FFFFFF">8</text></g>
<g id="edge1" class="edge"><path fill="none" stroke="#FFFFFF" d="M157.113,-146.3771C148.4747,-136.659 137.2107,-123.987 127.5503,-113.1191"/><polygon fill="#FFFFFF" stroke="#FFFFFF" points="130.1196,-110.7413 120.8599,-105.5924 124.8877,-115.3918 130.1196,-110.7413"/></g>
<g id="node3" class="node"><ellipse fill="none" stroke="#FFFFFF" cx="171" cy="-18" rx="27" ry="18"/><text text-anchor="middle" x="171" y="-13.8" font-size="14.00" fill="#FFFFFF">10</text></g>
<g id="edge2" class="edge"><path fill="none" stroke="#FFFFFF" d="M171,-143.7623C171,-119.201 171,-75.2474 171,-46.3541"/><polygon fill="#FFFFFF" stroke="#FFFFFF" points="174.5001,-46.0896 171,-36.0896 167.5001,-46.0897 174.5001,-46.0896"/></g>
<g id="node7" class="node"><ellipse fill="none" stroke="#FFFFFF" cx="99" cy="-18" rx="27" ry="18"/><text text-anchor="middle" x="99" y="-13.8" font-size="14.00" fill="#FFFFFF">9</text></g>
<g id="edge6" class="edge"><path fill="none" stroke="#FFFFFF" d="M104.9813,-71.8314C104.1257,-64.131 103.1083,-54.9743 102.1574,-46.4166"/><polygon fill="#FFFFFF" stroke="#FFFFFF" points="105.6289,-45.9656 101.0459,-36.4133 98.6717,-46.7386 105.6289,-45.9656"/></g>
<g id="node4" class="node"><ellipse fill="none" stroke="#FFFFFF" cx="27" cy="-162" rx="27" ry="18"/><text text-anchor="middle" x="27" y="-157.8" font-size="14.00" fill="#FFFFFF">5</text></g>
<g id="node5" class="node"><ellipse fill="none" stroke="#FFFFFF" cx="35" cy="-90" rx="27" ry="18"/><text text-anchor="middle" x="35" y="-85.8" font-size="14.00" fill="#FFFFFF">11</text></g>
<g id="edge3" class="edge"><path fill="none" stroke="#FFFFFF" d="M29.0187,-143.8314C29.8743,-136.131 30.8917,-126.9743 31.8426,-118.4166"/><polygon fill="#FFFFFF" stroke="#FFFFFF" points="35.3283,-118.7386 32.9541,-108.4133 28.3711,-117.9656 35.3283,-118.7386"/></g>
<g id="edge9" class="edge"><path fill="none" stroke="#FFFFFF" d="M56.25,-78.75C78.8722,-66.7735 114.8739,-47.7138 140.516,-34.1386"/><polygon fill="#FFFFFF" stroke="#FFFFFF" points="142.2501,-37.1808 149.4504,-29.4086 138.9749,-30.9943 142.2501,-37.1808"/></g>
<g id="edge8" class="edge"><path fill="none" stroke="#FFFFFF" d="M48.887,-74.3771C57.5253,-64.659 68.7893,-51.987 78.4497,-41.1191"/><polygon fill="#FFFFFF" stroke="#FFFFFF" points="81.1123,-43.3918 85.1401,-33.5924 75.8804,-38.7413 81.1123,-43.3918"/></g>
<g id="node8" class="node"><ellipse fill="none" stroke="#FFFFFF" cx="27" cy="-18" rx="27" ry="18"/><text text-anchor="middle" x="27" y="-13.8" font-size="14.00" fill="#FFFFFF">2</text></g>
<g id="edge7" class="edge"><path fill="none" stroke="#FFFFFF" d="M32.9813,-71.8314C32.1257,-64.131 31.1083,-54.9743 30.1574,-46.4166"/><polygon fill="#FFFFFF" stroke="#FFFFFF" points="33.6289,-45.9656 29.0459,-36.4133 26.6717,-46.7386 33.6289,-45.9656"/></g>
<g id="node6" class="node"><ellipse fill="none" stroke="#FFFFFF" cx="99" cy="-162" rx="27" ry="18"/><text text-anchor="middle" x="99" y="-157.8" font-size="14.00" fill="#FFFFFF">7</text></g>
<g id="edge4" class="edge"><path fill="none" stroke="#FFFFFF" d="M101.0187,-143.8314C101.8743,-136.131 102.8917,-126.9743 103.8426,-118.4166"/><polygon fill="#FFFFFF" stroke="#FFFFFF" points="107.3283,-118.7386 104.9541,-108.4133 100.3711,-117.9656 107.3283,-118.7386"/></g>
<g id="edge5" class="edge"><path fill="none" stroke="#FFFFFF" d="M85.113,-146.3771C76.4747,-136.659 65.2107,-123.987 55.5503,-113.1191"/><polygon fill="#FFFFFF" stroke="#FFFFFF" points="58.1196,-110.7413 48.8599,-105.5924 52.8877,-115.3918 58.1196,-110.7413"/></g>

there exist multiple orderings which satisfy a topological sort, one such being:

`[3, 5, 7, 8, 11, 2, 9, 10]`

which is the ordering which greedily picks the smallest vertex required to satisfy the sorting property. it is easy to see what this means when you arrange the graph as an adjacency list:

```
2 -> []
3 -> [8 10]
5 -> [11]
7 -> [8 11]
8 -> [9]
9 -> []
10 -> []
11 -> [9 10]
```

you can easily verify that in comparing to the provided ordering, each left value occurs in the ordering at a lower index than each of the right values.

using this interpretation, we can implement a test case to program against:

```java
public class TopologicalSortShould {

  TopologicalSort topologicalSort =
    new TopologicalSort();

  @Test
  void createCorrectTopologicalSort() {

    Map<Integer, List<Integer>> graph =
      Map.of(
        2, List.of(),
        3, List.of(8, 10),
        5, List.of(11),
        7, List.of(8, 11),
        8, List.of(9),
        9, List.of()
        10, List.of()
        11, List.of(9, 10));

    var actual =
      topologicalSort.topologicalSort(graph);

    for (var entry : graph.entrySet())
      for (var v : entry.getValue())
        assertThat(
          actual.indexOf(entry.getKey()),
          is(lessThan(actual.indexOf(v))));

  }

}
```

# the how

the best way to think about this is to interpret the graph as a group of tasks and the net of dependencies between. then the question becomes,

*in what order can we execute all tasks such that no dependency conditions are violated?*

the not-so-obvious answer is the use of *depth first search.* what makes dfs' applicable is that, when traversing a dage, it greedily traverses the graph to first finds and visits the most-dependent nodes, then backtracks to visit the second-most-dependent nodes, followed by visiting the third-most-dependent nodes, and so on. so if we record the **post-order** of the traversal and then reverse it, it will yield an order in which the least dependent vertices come first.

# the recursive impl

...
