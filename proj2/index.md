# Project 2: RArrayList and Land of the Squirrels

In this project, you will design a more sophisticated version of the `RArrayList` class as we did in class, and then you will use this class to solve a problem in SquareWorld.  At the end of this project, you will have a fully-functional `RArrayList` class that works almost identically to the built-in `ArrayList` class in Java.

## Getting Started

Download the starter code [here](https://rhodes.box.com/s/84tquy2bnfwnc1kf2tzx8ymn0xnadwk6).  This zip file will unzip into a `src` folder.  You should copy the contents of this `src` folder exactly into the `src` folder in a new IntelliJ project.

**Reminder:** Everything in this project must be your own work.  If you include any code that was not invented by you, you must give a citation for it.

**Read all of this project description before beginning!**

## Part A: RArrayList

In this part of the project, you will add some methods and functionality to your `RArrayList` class that we started designing in class.  The files you will need for this part are `RList.java`, `RArrayList.java`, and `RArrayListTester.java`.  You can ignore everything else for the moment.

### Refamiliarizing yourself with the code

Open `RList.java`.  Recall that `RList` is a Java *interface* that contains five methods: `size()`, `get()`, `set()`, `append()`, and `prepend()`.  The only change we have made is that now the interface is *parameterized*, which means it takes a *type parameter* called `E`.  Notice the beginning of the interface now is 

```java
public interface RList<E>
```

What this `<E>` represents is that when we use `RList` in practice (or any subclass of `RList`, like `RArrayList`), we must fill in that `E` with a Java class name.  You will probably recognize this idea from using normal Java `ArrayList`s: you write `ArrayList<Integer> numbers = new ArrayList<Integer>()` to make a new list of `Integer`s, and `ArrayList<Double> numbers = new ArrayList<Double>()` to make a new list of `Double`s, etc.  Our `RList` interface, in order to allow our lists to hold objects of any type, will be parameterized similarly.

All you need to remember is that `<E>` stands for **the type of object stored in this list**.  Otherwise you can use `E` as a regular class name throughout your code (with a few caveats that won't factor into this project that much.)

### Writing the code

1. Open `RArrayList.java`.  Read through the entire file to get a sense of it.  You will notice that a few of the methods are already written for you.  The methods here you must fill in are (in suggested order of writing them):

   - `isEmpty()`
   - `clear()`
   - `expandToCapacity()` (you don't have to write this if you don't want to, but it's useful to have this to help write the multiple-item version of append/prepend)
   - `append()` (multiple item version)
   - `prepend()` (multiple item version)
   - `slice()` 
   - `remove()`


2. Each method that you must write has a comment above it that explains how it should work.  I tried to leave very little to interpretation, so it should be (hopefully) unambiguous what each method should do.  Nevertheless, here are some general guidelines for writing the code here:

   - Use `RArrayListTester.java` to test your code.  What I suggest doing is writing a new test function for every method you write.  Inside that method, put some code to test the method as rigorously as you can.  Then you can call `toInternalString()` and verify that the output matches what you think it should be.  When you're certain the method is working, you can then comment out the call in `main()`.   **Leave the test methods in the file when you're done.** Feel free to comment them out of the `main()` method if you'd like, but I want to see how you've tested your code when you submit the project.

   - The data array (`data`) will never *decrease* in capacity, it will only grow.  Even when using `remove()` or `clear()`, the data array will stay the same size (from the programmer's perspective).  From the *user's* perspective, it will certainly get smaller in size, but this can be achieved by manipulating the `size` variable.

   - The single-item versions of `append()` & `prepend()` will always grow the `data` array (if necessary) by `SIZE_INCREMENT` elements (which is set to 3).  The multiple-item versions will grow the array (if necessary) only as large to exactly fit the new number of items.

   - To help debug, you can use `toInternalString()` to print out the status of the `data` array. This will be helpful as you start writing the more complicated methods that must shift items around within the array.

   - Your code should be as efficient as possible.  This usually comes into play with the multi-item versions of `append()` and `prepend()`, and sometimes with `slice()`.  It is often not efficient to call the single-item versions of `append()` and `prepend()` in these situations, because those methods may call `expand()` multiple times, which unnecessarily copies the `data` array.  For example, imagine a user calls `append()`, passing in another `RArrayList` of ten items.  If you implement the multi-item version of `append()` by calling the single-item version of `append()` in a loop, adding ten new items will necessitate *three or four* calls to `expand()` (depending on space), whereas you should just expand the array once to make enough room the first time.

   - Similarly, when shifting items in the array, as will need to be done in `prepend()` and `remove()`, if an item must be shifted more than one spot in the array, do not repeatedly shift it by one spot to make it move farther.  This is slow and inefficient (usually turning what should be an O(n) operation into an O(n^2) operation).  Instead, move each item directly to its final position, without making it pass through all the array slots in between.

## Part B: SquirrelWorld

**You cannot complete Part B until Part A works, because it requires a working `RArrayList` implementation.**

Squirrels are running rampant on the Rhodes College campus! It’s so cold and they are searching for acorns. You will write a program to simulate the squirrels moving around campus as they collect acorns.

This simulation takes place in a SquareWorld environment, where objects all reside in a large grid or board. Here, a single squirrel will roam around the grid, collecting acorns from every tree they encounter, and stashing the acorns in their “home” tree (the tree in which they live).

### Demo of the basic world

SquirrelWorld works out of the box, though it doesn't do everything it should.  Open and run `SquirrelWorld.java`.  When it asks you for a file, type in `world1.txt`.  Step through the world, and see how the squirrel moves from tree to tree.  This will go on forever, so stop the program whenever you want.  Try it with `world2.txt`. 

### What is the squirrel doing?

The squirrel has one mission.  It must visit each tree and gather acorns from each one in turn.  The squirrel takes some acorns from each tree and stashes them in its "home" tree.  It always visits the trees in the same order, and keeps cycling through the trees over and over.  Your job is to program the squirrel to actually gather the acorns (you can see from the output the squirrel never actually takes any acorns from the trees), and deposit them in its "home" tree.

### The starter code

Let's walk through the starter code:

- Open `Tree.java`.  A `Tree` object is an `Actor` that never moves.  Instead, it has a private `RArrayList<Acorn>` called `acorns` that stores all of its `Acorn`s. Note that there is a method `getAcornList()` that allows you to get the list of `Acorn`s for a `Tree`.  Note that this does not modify the list in any way; this method simply allows you access to the list so you can examine what's inside or change it.

- Open `Acorn.java`.  An `Acorn` is not an `Actor`, but a very simple object that just has a weight, plus a getter for weight.

- Open `Squirrel.java`.  Take a look at the fields at the top.  Notice there is a list of `Acorn`s that the squirrel currently holds and a list of `Tree`s the squirrel knows about.  The first tree in the `trees` list (Tree #0) is always the squirrel's "home" tree, where they will stash the acorns it collects from the other trees.  

- The squirrel moves through the world by use of the `targetTreeIndex` variable.  This variable controls which tree (in the `trees` list) the squirrel is currently heading towards.  The squirrel also counts its steps using the `stepsTaken` variable.

- Take a look at `act()`, which is already written for you.  Try to get a sense of how it works.  (There are a few methods here that you don't need to worry about, but you might want to look at just to see how they work if you're interested: `turnAndMoveTowards()` and `isNextTo()`.)  The two functions you *will* need to worry about are `stashAcorns()` and `gatherAcornsFrom()`, because they are blank and you must write them.  You can write them in any order, but I suggest starting with `gatherAcornsFrom()` and then writing `stashAcorns()`.

### Writing `gatherAcornsFrom()` and `stashAcorns()`

- Notice that `gatherAcornsFrom` is automatically called when a squirrel is next to a tree that is not its home tree.  Squirrels are very polite, and they like to leave some acorns behind in each tree, at least initially.  Therefore, `gatherAcornsFrom()` should use `slice()` to remove the *first half* of the acorns from the tree in question.  Round up for an odd number of acorns.  The squirrel should then `append()` all of these acorns (all at once) to their own list of acorns.  That's all `gatherAcornsFrom()` needs to do.

- Notice that `stashAcorns()` is called automatically whenever a squirrel is next to its home tree (which is why the method doesn't take a `Tree` argument, since you can always access Tree #0 from the list of `Tree`s).  These squirrels are very peculiar, and like to keep the big acorns separate from the small acorns.  So the squirrel should iterate through its list of acorns (use a `for` loop) and place all the acorns with weights less than 10 on the left side of its nest (`prepend()` to the `Tree`'s list of acorns) and all the acorns with weights 10 or more on the right side of its nest (`append()` to the `Tree`'s list.)

- Lastly, a squirrel eventually has to stop moving! The squirrel knows when it is done when it visits its home tree and does not have any acorns to stash there.  So at the beginning of `stashAcorns()`, place a check to see if the squirrel isn't carrying any acorns, and if so, removes itself from the world entirely (call `removeSelfFromBoard()`).

## Testing your project

You should run your code on `world1.txt` and `world2.txt`, and verify that your output matches mine, in terms of which acorns are being gathered when, and how long it takes until the squirrel finishes.


| Output for world1.txt: [output](https://rhodes.box.com/s/yxiwi8bxdo205a9mk9wjq7wnatvgtpq7) | [movie](https://rhodes.box.com/s/oqq4kw6mwe3xamr5v388sd5353jtpcqt) |
|-------------------------------|-------|
| Output for world2.txt: [output](https://rhodes.box.com/s/fcjzjdz0kq5x8z6f5rp1txpxdeacwkwr) | [movie](https://rhodes.box.com/s/wu5guy7vot5o8ipm7pl1a4x4lrjadac5) |

## Submitting your project

Upload the following files to Canvas (please upload them one at a time, not in a zip file, as it makes my life easier to grade them.)

- `RArrayList.java`
- `RArrayListTester.java`
- `Squirrel.java`
- a text file with your answers to the post-project questions listed below.

## At the end of the project

-   As you are preparing to submit the project, please prepare a text file (`.txt`, pdf, or Word doc is fine) answering the following questions:
    1.  What bugs and conceptual difficulties did you encounter? How did you overcome them? What did you learn?
    2.  Describe whatever help (if any) that you received. Don’t include readings, lectures, and exercises, but do include any help from other 
sources, such as websites or people (including tutors, classmates, and friends) and attribute them by name.
    3.  Describe any serious problems you encountered while writing the program.
    4.  Did you do any of the challenges (see below)? If so, explain what you did.
    5.  List any other feedback you have. Feel free to provide any feedback on how much you learned from doing the assignment, and whether you 
enjoyed doing it.
-   Please also add a comment at the top of your program stating your name and a pledge that you have followed the honor code and collaboration 
policy for this project. This can be as simple as writing “**I have neither given nor received unauthorized aid on this program.**” You can find the 
collaboration policy on the syllabus.
-   Remember, projects will be graded not only on correctness, but also appropriateness and efficiency of the algorithms you choose, and on coding style.

## Grading
This project will be graded on the correctness of the output, efficiency of the algorithms, and on coding style, including comments.

## Challenge

From time to time, I will offer "challenge problems" on assignments. These problems are designed to have little (but some) impact on your grade whether you do them or not. You should think of these problems as opportunities to work on something interesting and optional, rather than a way to raise your grade through "extra credit."

Policy on challenge problems:

- Challenge problems will typically allow you to get 1-5% of additional credit on an assignment, yet they will typically be much more difficult than this credit amount suggests.
- **You should not attempt a challenge problem until you have finished the rest of an assignment.**
- The tutors will not provide help on challenge problems. The instructor will provide minimal assistance only, as these problems are optional and are designed to encourage independent thought.
- Challenge problems may be less carefully specified and less carefully calibrated for how difficult or time-consuming they are.

### Challenges for this assignment:

If you do a challenge, save your regular code separately before starting on the challenge.  At the end, turn in the challenge files separately as well.  For example, make a zip file of the challenge submission and upload that separately from the regular files, so that I can easily run the basic program and the challenge program.  Include a document of some sort that explains what you did for the challenge.

You may do any of these:

- Add additional methods to the `RArrayList` class, such as for inserting multiple items in the middle, determining if an item is present or not, etc.
- Add the ability to have multiple squirrels in the world who coordinate their actions to get all the acorns back to the "home" tree as quickly as possible.  Or maybe each squirrel has their own "home" tree and they are competing to see who can get the most acorns as quickly as possible.

