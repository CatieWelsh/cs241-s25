---
title: The Java Collection Framework
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

# The Java Collection Framework

The Java Collections Framework is an entire hierarchy of data structures consisting of Lists, Sets, Maps, Stacks, Queues, and a few other data structures.  Here are the highlights, including a few we've used in past projects, and a few new ones for this project.

- **Lists** (all implement the [List](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/List.html) interface)
  - [**ArrayList**](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/ArrayList.html) - a List implemented with an array.
  - [**LinkedList**](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/LinkedList.html) - a List implemented with a doubly-linked list.
- **Sets** (all implement the [Set](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/Set.html) interface)
  - **[HashSet](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/HashSet.html)** - a Set implemented with a hash table (items must have a `hashCode()` function).
  - [**TreeSet**](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/TreeSet.html) - a Set implemented with a red-black tree, which is a variant on a binary search tree (items must implement the `Comparable` interface).
- **Maps** (all implement the [Map](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/Map.html) interface)
  - [**HashMap**](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/HashMap.html) - a Map implemented with a hash table (keys must have a `hashCode()` function).
  - [**TreeMap**](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/TreeMap.html) - a Map implemented with a hash table (keys must implement the `Comparable` interface).

Normally, you should use these built-in Java data structures; rarely will you implement your own in the real world.  These six classes --- and their relatives --- form the backbone of the data structures in most Java projects you will have to write.

## Common operations

Here are the most useful things you can do with these classes.

- HashSet and TreeSet (look at the APIs for the full set of methods and more thorough descriptions)
  - add(item): adds a new item to this set.
  - clear(): removes all the items from this set.
  - contains(item): tests if this set contains a given item.
  - isEmpty(): tests if this set is empty.
  - remove(item): removes an item from the set.
  - size(): returns the number of items in the set.
- HashMap and TreeMap (look at the APIs for the full set of methods and more thorough descriptions)
  - put(key, value): adds a new key-value pair to this map.
  - get(key): returns the value corresponding to this key, or `null` if the map doesn't contain this key.
  - containsKey(key): tests if this map contains a given key (with any value).
  - containsValue(value): tests if this map contains a given value (with any key(s)).
  - keySet(): returns a set of all the keys in the map.
  - remove(key): removes a key (and its associated value) if it exists in the map.
  - clear(), isEmpty(), size(): similar to the set methods above.

## Traversing these structures

Often times you will need to traverse these data structures.  All of these examples below will need to be modified to fit the specific data types your structures are holding.

### ArrayLists

```java
// Using indices:
// Assume we have an ArrayList of Strings, called list.
for (int x = 0; x < list.size(); x++) {
  // Access the current item with list.get(x) or list.set(x).
}

// Or using a (hidden) iterator:
for (String str : list) {
  // Do something with str.
  // If the type is mutable, changing the str variable through 
  // its methods will modify the contents of the list.
}
```

### LinkedLists

```java
// For linked lists, even though they have indices, it is inefficient to use them
// for iteration, since each call to get(index) must traverse from the head of the list.
// Therefore, using an iterator is the most common way.

// Assume we have a LinkedList of Strings, called list.
for (String str : list) {
  // Do something with str.
  // If the type is mutable, changing the str variable through 
  // its methods will modify the contents of the list.
}
```

### HashSets or TreeSets

```java
// Assume we have a HashSet or TreeSet of Strings called set.
for (String str : set) {
  // Do something with str.
}
```

### HashMaps or TreeMaps

Two ways:

```java
// Assume we have a HashMap or TreeMap of Strings to integers called map:
//   that is, the keys are Strings, vals are ints.
for (String key : map.keySet()) {
  // Can get the corresponding value with map.get(key).
}
```

Or:

```java
for (Map.Entry<String, Integer> entry : map.entrySet()) {
  String key = entry.getKey();
  int value = entry.getValue();
  // Do something with these...
}
```

