---
title: The Graph Class
parent: Project 6
---
<script>
MathJax = {
  tex: {
    inlineMath: [['$', '$'], ['\\(', '\\)']]
  },
  svg: {
    fontCache: 'global'
  }
};
</script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js">
</script>

## The Graph Class

This graph class represents a directed, weighted graph.  It can be used as an undirected graph simply by adding each edge in both directions.

The graph represents vertices as Strings, and Edges are represented by a separate `Edge` class.  The `Edge` class is only used in a few circumstances, but we will begin with it:

**The Edge Class**

You will never have to manually create an instance of this class, the `Graph` class does this for you.  The `Edge` class is only used when traversing edges in the graph.

The `Edge` class represents a directed edge in a graph, starting at one vertex (called vertex 1), and ending at another vertex (called vertex 2).

The class has three methods you will use:  (feel free to look at `Edge.java`)

- `getVertex1()`: returns the `String` representing the starting vertex of this edge.
- `getVertex2()`: returns the `String` representing the ending vertex of this edge.
- `getWeight()`: returns an `int` representing the weight of this edge.

**Using the Graph class**

Here are the methods of the `Graph` class.  All of these except the constructor must be called on a `Graph` object (e.g., `g.addVertex("a");` )

- Creating a new graph:

  `Graph g = new Graph();`
  
- Adding a vertex (represented as a String):

  `void addVertex(String v)`
  
- Adding an edge:

  `void addEdge(String v1, String v2, int weight)`
  
- Get all the vertices in a graph:

  `Set<String> getVertices()`
  
- Get all the edges in a graph that point *away* from some vertex:

  `Set<Edge> getEdgesFrom(String v)`
  
- Get all the vertices in a graph connected to vertex `v` by an edge leading away from `v`:

  `Set<String> getAdjacentVerticesFrom(String v)`
  
- Get the weight of a specific edge:

  `int getWeight(String v1, String v2)`

**Iterating over parts of a graph**

- If you need to iterate over the vertices in a graph, use a loop with `getVertices()`.
- If you need to iterate over the vertices that are adjacent to another vertex, use a loop with `getAdjacentVerticesFrom()`.
- if you need to iterate over the edges (or some of the edges), use a loop with `getEdgesFrom()`.

### A Full Example

```java
Graph graph = new Graph();
graph.addVertex("a");
graph.addVertex("b");
graph.addVertex("c");
graph.addVertex("d");
graph.addVertex("e");

graph.addEdge("a", "b", 5);
graph.addEdge("a", "c", 10);
graph.addEdge("a", "d", 4);
graph.addEdge("b", "c", 6);
graph.addEdge("b", "e", 8);
graph.addEdge("c", "e", 1);
graph.addEdge("d", "a", 9);

System.out.println(graph);
System.out.println(graph.getVertices());
System.out.println(graph.getEdgesFrom("a"));
System.out.println(graph.getAdjacentVerticesFrom("a"));
System.out.println(graph.getWeight("a", "b"));
```

