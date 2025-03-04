---
title: Project 4
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


# CS 241 Project 4: Sentiment Analysis
{: .no_toc }

1. TOC
{:toc}

**Reminder:** Everything in this project must be your own work.  If you include any code that was not invented by you, you must give a 
citation for 
it.

## Getting started

- Download the starter code 💾 [here](sentiment-starter.zip).
- Make a new Intellij project. Then copy the contents of this  `src`  folder exactly into the  `src`  folder in the new project. Do not 
"nest" the `src` folders inside each other!


***Sentiment analysis***, in natural language processing or computational linguistics, is a set of techniques used to extract subjective information 
from text, usually in the sense of figuring out whether a piece of text is expressing a positive, neutral, or negative opinion about something. In 
this project, you will write a simple sentiment analysis program to try to figure out what sort of opinion is being expressed in a collection of movie 
reviews. [More about sentiment analysis.](https://en.wikipedia.org/wiki/Sentiment_analysis)

The sentiment analysis algorithm for this project is very simple. You are given a collection of movie reviews in text format. You will use a maps 
(explained below) to keep track of how often certain words appear in the reviews. That is, your map will store an association between words (the keys, 
as Strings), and the frequencies with which they appear in the movie reviews (the values, as integers).

You are provided with a text file containing movie reviews written by people who have seen various movies. Each person has rated the movie on a scale 
from zero to four stars, where 2 is neutral (so the person gives the movie a rating of 0 when they absolutely hate it, 1 when they dislike it, 2 when 
they’re indifferent, 3 when they like it, and 4 when they love it). Each person also has written a text review of the movie as well.

You can see in the text files that each review is contained on one line of the file. The first item on each line is the rating score from 0-4, and 
then the rest of the line is the review. Notice that all punctuation is removed, and all words are lowercase (this makes your life easier).

You will write a program that reads this file and stores all the words in **two** maps, implemented as binary search trees (the code for which you 
will also write). There is one map that simply counts the number of times each word appears in all the movie reviews, and there is a second map that 
stores the total score (the sum) of all the movie reviews in which that word appears. These two pieces of information will be needed in our sentiment 
analysis algorithm. 

After you read in all the movie reviews and create the two maps from the words (and print out some statistics about the BSTs), you will read in a file 
of what are called *stop words*. In natural language processing, stop words are words that are filtered out before any text processing happens, 
usually because these words carry very little meaning. Usually stop words are just a large collection of common English words, like “the,” “is,” “a,” 
etc. These words are in the file `stopwords.txt`, one per line.

You will create a Set of stop words from `stopwords.txt`, and you will them remove the stop words from your binary search trees of word frequencies 
and word scores.  Then, after printing a bunch of statistics again, you will have the user type in new movie reviews, until they type the word “quit”. 
For each review, you will calculate a number between 0-4 indicating the sentiment of the review, where 2 is neutral (0-1 are negative; 3-4 are 
positive).

## Important classes for this project

This project is all about using the Map and Set abstract data types.  In particular, you will be implementing your own Map type using a binary search 
tree, but you will use a built-in Java type for the Set.

Here we describe how to use these are used:

- **BSTMap**: You will be writing this class yourself, but here's how it's intended to be used, which mirrors the way built-in Java Maps are used.
  - **Create** a new map: `BSTMap<KeyType, ValueType> mapVariable = new BSTMap<KeyType, ValueType>();` This creates a new map that associates keys of 
`KeyType` with values of `ValueType`.  For instance, if we wanted to make a map where we could look up someone's bank account balance (a `double`), 
given their account number (an `int)`, then we could do: `BSTMap<Integer, Double> accounts = new BSTMap<Integer, Double>();`
  - **Put** a key-value pair into the map: `mapVariable.put(newKey, newValue)`
  - **Get** a value out of the map, given its key: `mapVariable.get(key)`
  - **Test** whether a key exists in a map: `mapVariable.containsKey(key)`
  - **Remove** a key-value pair from the map, given the key: `mapVariable.remove(key)`
  - **Get the size** of the map (the total number of key-value pairs): `mapVariable.size()`
- **TreeSet**: This is a built-in Java class that implements the Set ADT with a specific kind of BST behind the scenes called a red-black tree.  (We 
will cover red-black trees later in the course.)  You will not be implementing this class; the information here is simply so you can use this class in 
this project.
  - **Create** a new set: `Set<Type> setVariable = new TreeSet<Type>();` This creates a new set of the data type given by `Type`.
  - **Add** an item into the set: `setVariable.add(newItem)`
  - **Test** whether an element exists in the set: `setVariable.contains(item)`
  - Remove an item from the set: `setVariable.remove(item)`
  - **Get the size** of the set (the total number of items): `setVariable.size()`

As you can see, these two classes are used in very similar ways.  Recall that Maps and Sets are very similar ADTs, in that they are both intended to 
be used to keep track of a collection of items that need to be searched very quickly.  Maps are searched by their keys (and return the corresponding 
value), whereas Sets are searched by the item itself (and just return true or false, indicating whether the item is there or not).

Their implementations are often very similar; for instance both can be implemented using binary search trees.  The only difference is that each node 
in a BST for a Map stores **two** pieces of information (the key and the value), whereas each node in a BST for a Set stores only the item.  In fact, 
you can think of any Set as just a Map where the value field is ignored entirely.

## Part A

Part A is writing the BSTMap class, defined in `BSTMap.java`.  This class represents a Map that stores an association between a key (type `K`) and a 
value (`type V`).  Note that this Map class is general enough to store any kind of association, and can be used beyond this specific project.

**Important**: Remember that in BSTs, all the searching/inserting/removing happens based on comparisons done with the **keys**, and the values are 
just along for the ride (they don’t factor into the BST algorithms, except as extra information carried along in each node). 

Furthermore, because the BSTMap class can store any data type as a key, you must use the `compareTo` function, rather than `<` or `>` (similar to what 
you did in the SortedList project).  Here's a refresher:

```java
REGULAR WAY         WITH COMPARABLES
------------------------------------
if (a < b)          if (a.compareTo(b) < 0)
if (a > b)          if (a.compareTo(b) > 0)
if (a <= b)         if (a.compareTo(b) <= 0)
if (a >= b)         if (a.compareTo(b) >= 0)
if (a == b)         if (a.compareTo(b) == 0)   // or (a.equals(b))
```

So you will be calling this `compareTo` function frequently on your keys.  Be careful to not use `==` to compare keys with each other!  This will 
compare them based on whether they have the same location in memory, not whether they are equal.  (This is, for instance, why `String`s in Java must 
be compared with `.equals()`, rather than `==`).

The BSTMap class defines a Node class towards the end of the file; it holds a key of type `K` and a value of type `V`, along with pointers to the left 
and right child nodes of the BST.

You must implement the following functions, which you should feel free to base on the BST code given in class.

- `put`: Put a new key-value pair into the BST.  This function must use the recursive algorithm from class.  Note that there is a `public` version of 
this function (that a user will call), and a `private` version that will actually do the work.  Two versions are needed because the recursive 
`private` function needs a Node parameter, but the public-facing version of `put` does not.

  **Important note:** To be consistent with Java, our version of put, if given a key-value pair where the key already exists in the tree, should 
**overwrite** the old value with the new one.  Note that this is not the same as removing the old pair and adding the new one, as this might change 
its location in the tree.  Be sure to write tests for this functionality.

- `get`/`containsKey`: These are really almost the same function; in fact, you could write containsKey in terms of `get` if you wanted to.  These 
functions must be recursive, and should be based off the algorithm and code discussed in class.  `containsKey` is the equivalent of the `contains` 
function from the code handout, and `get` uses the exact same idea, except it returns the corresponding value for the searchKey, rather than true or 
false.  Hint: you will need to add `private` helper functions.

- `remove`: This is the equivalent of the deletion algorithm discussed in class.  This function does not have to be recursive.  Please use the code 
given in class to implement this.

- `size`: This function returns the number of key-value pairs in the tree.  This function should be implemented recursively.  (Yes, this function 
could be implemented by adding a `size` field to the class, but I want you to get practice with writing recursive functions.)

- `height`: This function returns the height of the tree (see the BST homework for more information).

- `preorder/inorder/postorderKeys`: These three functions should return `List`s of the keys in the tree, given in the specified orders.  Each one will 
be recursive, and will require its own `private` helper function.  Note: These functions should not print anything!  Instead, make an empty ArrayList 
at the start of each (`public`) function, and as you traverse the tree, append each key as you visit each node.  Hint: You may pass the ArrayList as a 
variable.

You should also implement tests in BSTTester.  Be sure to test your functions rigorously.

## Part B

In Part B, you will write the sentiment analyzer, which is in `SentimentAnalysis.java`.  We will first go over the sentiment analysis algorithm.

### Sentiment analysis algorithm

Recall that we are given a collection of movie reviews.  Each review has a star rating, between 0 and 4, inclusive.  Our goal is, given a *new* movie 
review, predict the star rating. 

The algorithm is extremely simple.  Each movie review is a list of words.  Given a new movie review, for each word in that review, we calculate the 
word's *average sentiment*, which is the *average score of all the movie reviews that contain that word*.  The idea is that this sentiment will be 
higher for words that frequently appear in reviews for highly-rated movies, and lower for words that frequently appear in reviews with low ratings.  
To get the overall sentiment for a review, each word's average sentiment is itself then averaged.  (See the full example at the end.)

### How the program works

You will notice large chunks of `main()` are already written for you.  You must fill in the missing pieces.  Here are some important variables you 
will use:

- There is a boolean constant defined at the top of main called `PRINT_TREES`.  This boolean constant is used to print extra debugging information.  
Keep it turned on as you write the program, as it will help you debug.  You can turn it off as you start working with more movie reviews, as it the 
output will become too long.
- `BSTMap<String, Integer> wordFreqs`: This map stores the frequency of each word seen in all the movie reviews.  Whenever you see a word in a review, 
you will call `put` on this map and increment the frequency by 1.
- `BSTMap<String, Integer> wordTotalScores`: This map stores, for every word, the (running) total of the ratings (scores) for all the movies 
containing that word.
- `Set<String> stopwords`: This set stores the stop words.  You will remove these words from the movie reviews (both the reviews you read from the 
files and the new ones typed in at the keyboard.)

Steps for writing the program:

- You should write `processFile` first.  This function takes the name of a file of movie reviews, the map of word frequencies, and the map of each 
word's total score.  This function opens the file of movie reviews, reads them in, and updates the two maps so that at the end, we have the frequency 
of every word and its total score.

  Code is already written here to open the file and loop over it.  You must write code that processes each word in a line, and for every word, updates 
the two maps.

  Guide to writing this function: Remember that the `words` array contains a single line from the file, so the first "word" is the rating score, and 
the remaining words are the movie review itself.  In Java, you can turn a `String` containing an integer (like `"2"`) into the corresponding `int` by 
calling `Integer.parseInt()` on the string.

  You will need to check if each word you see is already in the maps or not (use `containsKey`).  To update the `wordFreq` map, look up the old 
frequency and increase it by 1 if the word already exists, or just `put` a 1 in the map if the word is new.  To update `wordTotalScores`, look up the 
old score and increase it by this movie's rating if the word already exists, or, if the word is new, then the total score so far is just the current 
movie's rating.

  There is no special handling if a word appears multiple times in the same review.  You will update each map multiple times, and that's OK.

- Next, write `printFreqsAndScores`.  This function simply prints a table of all the words in the two maps, along with their frequencies and total 
scores.  The order of the words is based on an inorder traversal of the BST, which you can obtain by calling `inorderKeys()` on either map.  This will 
give you a list of keys in the appropriate order, and you can then loop over them and call `get()` on each map.

- Next, write `removeStopWords`.  This function opens the "stopwords.txt" file and adds all the words found inside into the `stopwords` map.  At the 
same time, it removes those words from the two BSTMaps.

  Fill in the code; you are literally just adding 3 lines inside the loop where it says to.

- Lastly, fill in the code in `main()` that processes new movie reviews.  Your code should loop over the `words[]` array and calculate the average 
sentiment for each word, ignoring any stop words.  Remember, the "average sentiment for a word" means the average movie rating for every movie review 
that contains that word.  You have two maps that will give you the exact pieces of information you need to calculate this.  

  **Important:** If a new word appears in a movie review --- a word that isn't contained in any other movie review --- you should also ignore it.

  Print out each word as it is seen, with its average sentiment score (or whether it's being skipped because it's a stop word or a new word).

  You then average all the average sentiments for each word, and this gives you your overall sentiment score.

## Hints, tips, and guidelines

- Test your BSTMap class thoroughly in Part A, before proceeding to Part B.  The traversal methods will help you debug, treat those as the equivalent 
of `toInternalString` from previous projects.
- Your output should match mine to the best of your abilities. In particular, make sure to print the messages asked for as you calculate the sentiment 
score for a new movie review at the end of Part B.
- I highly recommend not trying to write your own algorithm for removing/deleting a node from a binary search tree. Instead, use the code given in 
class (check Canvas for it as well). You can also copy the code from the handout for `contains()` and `add()` as well.  (`contains` becomes 
`containsKey`, and `add` becomes `put`.)

## Sample output

- For `reviews-small.txt`: [output](reviews-small-output.txt) | [output with PRINT_TREES](reviews-small-output2.txt)
- For `reviews-big.txt`: [output](reviews-big-output.txt) | [output with PRINT_TREES](reviews-big-output2.txt)

## At the end of the project

-   As you are preparing to submit the project, please prepare a text file (`.txt`, pdf, or Word doc is fine) answering the following 
questions:
    1.  What bugs and conceptual difficulties did you encounter? How did you overcome them? What did you learn?
    2.  Describe whatever help (if any) that you received. Don’t include readings, lectures, and exercises, but do include any help 
from other sources, such as websites or 
people (including classmates and friends) and attribute them by name.
    3.  Describe any serious problems you encountered while writing the program.
    4.  Did you do any of the challenges (see below)? If so, explain what you did.
    5.  List any other feedback you have. Feel free to provide any feedback on how much you learned from doing the assignment, and 
whether you enjoyed doing it.
-   Please also add a comment at the top of your program stating your name and a pledge that you have followed the honor code and 
collaboration policy for this project. This 
can be as simple as writing “**I have neither given nor received unauthorized aid on this program.**” You can find the collaboration 
policy on the syllabus.
-   Remember, projects will be graded not only on correctness, but also appropriateness and efficiency of the algorithms you choose, 
and on coding style.

## Grading

Your project will be graded on 

- correctness of your output
- appropriateness of your algorithmic choices, including big-oh times: Your implementations should be as fast as is reasonably possible (in other 
words, do not use an $O(n^2)$ algorithm where an $O(n)$ will do.
- thoroughness of your testing code
- code style, including appropriate choices regarding comments, naming of variables, design and readability of your code, etc.

## Submitting

Upload 3 files to Canvas: BSTMap.java, BSTTester.java, and SentimentAnalysis.java.

## A full example

The `reviews-small.txt` file contains the following movie reviews: 

```
0 this movie is awful i hate it
2 this movie is ok i think its not bad 
4 this movie is the best thing ive ever seen i love it 
```

This means there are three reviews (one per line). The first person gave a score of zero, the second one a two, and the third one a four. 

After processing the first review (just line #1), the two maps (`wordFreqs` and `wordTotalScores`) should reflect the following:

```
word=this, freq=1, totalScore=0
word=movie, freq=1, totalScore=0
word=is, freq=1, totalScore=0
word=awful, freq=1, totalScore=0
word=i, freq=1, totalScore=0
word=hate, freq=1, totalScore=0
word=it, freq=1, totalScore=0
```

And the keys would be arranged in the BST like this:

```
                        this
                        /
                      movie
                       /
                      is
                    /    \
                 awful   it
                   \
                    i
                   /
                 hate
```

After processing the 2nd line, you'd have:

```
word=this, freq=2, totalScore=2
word=movie, freq=2, totalScore=2
word=is, freq=2, totalScore=2
word=awful, freq=1, totalScore=0
word=i, freq=2, totalScore=2
word=hate, freq=1, totalScore=0
word=ok, freq=1, totalScore=0
word=think, freq=1, totalScore=2
word=its, freq=1, totalScore=2
word=not, freq=1, totalScore=2
word=bad, freq=1, totalScore=2
```

Then you'd process the 3rd line.  At this point, you can follow along with the output for `reviews-small.txt`.
