---
title: Project 6
parent: Projects
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


# CS 241 Project 6: Dijkstra's Algorithm
{: .no_toc }

1. TOC
{:toc}

**Reminder:** Everything in this project must be your own work.  If you include any code that was not invented by you, you must give a 
citation for 
it.

### Getting started

- Download the starter code 💾 [here](proj6-starter.zip).
- Make a new Intellij project. Then copy the contents of this  `src`  folder exactly into the  `src`  folder in the new project. Do not 
"nest" the `src` folders inside each other!


In this project, we will put all of our pieces from this semester together.  Well, at least some of them.  You will write a program implementing a classic algorithm, picking the best data structures to use in the code.  You will also get experience using some built-in Java data structures we haven't used yet.

### Primer on the Java Collection Framework

For this project, you will be using a number of classes in the Java Collections Framework, which contains all of the basic data structures programmers commonly use in their programs, including a variety of Lists, Sets, and Maps.  You have already used ArrayLists, LinkedLists, and HashSets in previous projects; for this project, you will make use of the built-in Java Maps, known as HashMap (implemented with hash tables) and TreeMap (implemented with a variety of a BST).  If you are not already familiar with these, [you can read this description of how they work](java-collections).

### Project description

You will implement Dijkstra's algorithm for this project.  Your project will read in a description of a graph from a file, which may be directed or undirected, and then run Dijkstra's algorithm to compute the shortest path between two vertices specified in the file.

To make your life easier, the graph data structure and priority queue data structure are already written for you.  Read about the [graph](graph-class) and the [priority queue](pq-class) classes.

### Part A: File Reading

Each file describes a graph.  Take a look at the examples given in the `src` folder (graph1-graph4).  You should be able to see how the file structure works.  There are four possible commands you’ll see in these files: 

- The first line of the file will always be the word `directed` or `undirected` which specifies the type of graph. To make your life easier, your project will "fake" undirected graphs as directed graphs: you will simply represent an undirected edge as two directed edges, one in each direction.
- `vertex name`: this command adds a vertex called `name` to the graph. You may assume `name` is a string that hasn’t already been used as the name of a vertex.
- `edge name1 name2 weight`: this command adds an edge between the vertices `name1` and `name2` with the given integer weight. You may assume `name1` and `name2` have already been added to the graph, and there is not an existing edge between them already. 
- `dijkstra name1 name2`: this command will always be the last one in the file. When your program sees this command, your code should print out a list of all the vertices and edges in the graph (see the sample output for the formatting).  Then your code calls your Dijkstra’s algorithm function (see below) to find the shortest path from vertex `name1` to vertex `name2`. 

**Completing Part A**

To write Part A, fill in the `processFile` function.  Follow the model we've used in previous projects.  

A few hints, tips, and guidelines:

- Notice a `Graph g` is already created for you.
- I suggest using a series of `if`/`else if`/`else` tests to check which category from above `words[0]` falls into.  Then call `g.addVertex()` or `g.addEdge()` or `dijkstra()` as appropriate.  
- The `Integer.parseInt()` function will turn a `String` into an `int`.
- For `directed`/`undirected`, you'll need some way of saving this piece of information so you know whether to add two edges or just one.
- For now, since you haven't written the Dijkstra's algorithm function, when you read the `dijkstra` command from the file, you should print all the vertices and edges, and then call the `dijkstra()` function, but the function won't actually do anything yet.

**Debugging Part A**

- If you run into problems, use lots of print statements.  You can print the entire state of the graph with `System.out.println(g)`.  This is handy to see what your graph looks like.

**How do I know when Part A works?**

When your output from the list of vertices and list of edges for each graph matches mine (see the output section below), you should feel confident that you're ready to move on.

### Part B: Dijkstra's algorithm

In this part, you will implement Dijkstra's algorithm, which we discussed in class.  [Here's the pseudocode](https://rhodes.app.box.com/s/k08g32b1stbec2mn93r7l0xoho3zbcmt).

**Completing Part B**

- First, you will need to pick your data structures.  There are three major data structures in the algorithm, namely `dist`, `prev`, and the priority queue.  The priority queue has already been created for you, but you need to pick the appropriate structures for `dist` and `prev`.  `dist` and `prev` are maps: `dist` keeps track of the best distance so far for each vertex, so it is a map from `String`s to `int`s.  `prev` keeps track of the best "previous" vertex for each vertex in the graph, so it is a map from `String`s to `String`s.  (Recall that vertices in our graph are represented as strings.)

  What you need to do is decide whether to use a `HashMap` or a `TreeMap` for each of these maps.  Which would be better here?  Using a hash table or a binary search tree?  (Either will work fine, but you should think about this.)

- Next, translate the pseudocode into Java code.  Unlike some previous projects, this code cannot be cut-and-pasted; you will need to think about how each line should be expressed in Java.  Refer back to the details of the [graph class](graph-class) and the [priority queue class](pq-class), and how [Java maps](java-collections) work.

- Your code *must* output each vertex as it is visited (removed from the priority queue), and it also *must* report all changes to the `dist` map.  I suggest, however, using additional print statements to help debug.  What I did was I made a `DEBUG` constant that triggers extra output when it's set to `true`.  See my sample output below for examples of this.

- At the end of the algorithm, your code *must* print four pieces of information:

  - The final shortest path.
  - The distance of that shortest path.
  - The contents of the final `dist` map.
  - The contents of the final `prev` map.
  
- Hint for printing the shortest path: Because when you follow the chain of vertices starting from `prev[finish]` back to the `start` vertex, they will come out backwards, you most likely will want to put them into some sort of data structure so that then you can print them in the correct (forwards) order.

**Debugging Part B**

- If you run into problems, use print statements to print every step of the algorithm as it is being completed.  See my sample debugging output for ideas.

**How do I know when Part B works?**

When your output matches mine (see the next section), you're done!

### ✅✅✅ Sample output

I will be checking your output against the "without debugging" output.  Everything should match, with the exception of:

- The order of the vertices and edges listed at the beginning (when you just print the contents of the graph).
- The order in which the *neighboring* vertices `v` of each vertex `u` are considered during each step of Dijkstra's algorithm (e.g., if vertex A is connected to vertices B and C, your algorithm may consider B before C, or C before B).  To be clear, the order of the `u` vertices (being removed from the priority queue) must match.  The order of the `v` vertices *within* each iteration of the main loop does not matter.
- The order of the vertices in the `dist` and `prev` maps printed at the very end.

**Files**

- `graph1.txt`: [without debugging](graph1-output.txt) | [with debugging](graph1-output-debug.txt)
- `graph2.txt`: [without debugging](graph2-output.txt) | [with debugging](graph2-output-debug.txt)
- `graph3.txt`: [without debugging](graph3-output.txt) | [with debugging](graph3-output-debug.txt)
- `graph4.txt`: [without debugging](graph4-output.txt) | [with debugging](graph4-output-debug.txt)

## At the end of the project

-   As you are preparing to submit the project, please prepare a text file (`.txt`, pdf, or Word doc is fine) answering the following 
questions:
    1.  What bugs and conceptual difficulties did you encounter? How did you overcome them? What did you learn?
    2.  Describe whatever help (if any) that you received. Don’t include readings, lectures, and exercises, but do include any help 
from other sources, such as websites or 
people (including classmates and friends) and attribute them by name.
    3.	Does your project still have errors in it? If so, describe them and how you tried to fix them. 
    4.  How did you apply thoroughness to this project? (related to your mid-semester reflection)
    5.  List any other feedback you have. Feel free to provide any feedback on how much you learned from doing the assignment, and 
whether you enjoyed doing it.
-   Please also add a comment at the top of your program stating your name and a pledge that you have followed the honor code and 
collaboration policy for this project. This 
can be as simple as writing “**I have neither given nor received unauthorized aid on this program.**” You can find the collaboration 
policy on the syllabus.
-   Remember, projects will be graded not only on correctness, but also appropriateness and efficiency of the algorithms you choose, 
and on coding style.


### Turning in the project

Upload `DijkstrasAlgorithm.java`, as well as your text file answering the required questions. No other files are needed.

