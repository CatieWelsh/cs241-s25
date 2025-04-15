---
title: The Priority Queue Class
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



## The Priority Queue Class

Java has a built-in priority queue class called `PriorityQueue`, but it is not intuitive to use.  I have built a hopefully easier one that you may use for this project, called `PriQueue`.  This class represents a priority queue that can store any data type, and the priorities may be integers, doubles, or any other numeric object.

This class can prioritize either low priorities or high priorities by specifying a boolean in the constructor. `true` priorities *low* priorities, and `false` prioritizes high ones.

**Using the `PriQueue` class**

Here are the methods of the `PriQueue` class.  All of these except the constructor must be called on a `PriQueue` object (e.g., `pq.add("new item", 10);` )

- To create a new priority queue, you must specify the data type of the objects stored inside, and of the priorities themselves (usually some kind of number type, like Integer or Double).  For instance, if you are implementing a priority queue of people waiting to see a doctor, and the receptionist assigns them all an integer priority according to how urgent their situation is, then the *first* type parameter should represent the person (e.g., `Person` or `String`), and the *second* type parameter is `Integer` (because their priorities are integers.)

  In all of these methods, type `E` represents the data type in the queue, and `P` represents the type of the priority.

  The constructor takes a boolean parameter, which controls whether this priority queue represents a min-queue or a max-queue.  `true` priorities *low* priorities, and `false` prioritizes high ones. 

  Example: Create a min-priority queue of Strings, prioritied by integers:
  
  `PriQueue<String, Integer> pq = new PriQueue<String, Integer>(true)`;

- Add a new item to the priority queue:

  `void add(E item, P priority)`

- Remove and return the "highest priority" item from the queue -- meaning the greatest priority item for a max-queue, and least priority item for a min-queue:
  
  `E remove()`

- Change the priority of an item already in the queue:

  `void changePriority(E item, P newPriority)`

- Also contains the standard methods `clear()`, `isEmpty()`, `contains()`, and `size()`.

### A Full Example

```java
PriQueue<String, Integer> pq = new PriQueue<String, Integer>(true);
pq.add("a", 5);
pq.add("b", 6);
pq.add("c", 4);
pq.add("d", 8);
pq.add("e", 1);
pq.changePriority("c", 100);

while (!pq.isEmpty())
{
    String s = pq.remove();
    System.out.println(s);
}
```

Prints: `e a b d c`
