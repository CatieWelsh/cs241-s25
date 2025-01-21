---
title: Project 1
parent: Projects
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# CS 241 Project 1: Large Integers
{: .no_toc }

1. TOC
{:toc}

**Reminder:** Everything in this project must be your own work.  If you include any code that was not invented by you, you must give a citation for 
it.

## Getting Started

1.  Download the starter code  [here](largeint-starter.zip). This zip file will unzip into a  `src`  folder.
2.  Make a new Intellij project. Then copy the contents of this  `src`  folder exactly into the  `src`  folder in the new project.

## Introduction
There are four different built-in (primitive) Java data types that can hold integers.  They are `byte`, `short`, `int`, and `long`, and take up 1, 
2, 4, and 8 bytes of storage, respectively.  Because each of these used a fixed number of bytes, there are only a certain number of possible 
integers each type can represent.  For instance, the smallest integer an `int` can represent is $$-2^{31}$$, or -2,147,483,648, whereas the largest is 
$$2^{31}-1$$, or 2,147,483,647.

When you exceed the limits of what an `int` can handle, Java will not realize it.  Consider the following code:
```java
int x = 2147483646;  
System.out.println(x);  
x++;  
System.out.println(x);  
x++;  
System.out.println(x);  
x++;  
System.out.println(x);
```
When this code runs, it prints the following:
```
2147483646
2147483647
-2147483648
-2147483647
```
What is happening is that when `x` becomes equal to 2147483647, adding 1 to the number causes an [overflow 
error](http://en.wikipedia.org/wiki/Integer_overflow), which, from the user's standpoint, causes the value of the variable to wrap around to the 
most negative number possible in the range.  This will happen with any of the four primitive integer data types.

You can imagine situations in which it would be useful to have an integer data type that *never* overflows (or at least not until we exceed the size 
of the computer's memory).  One way to accomplish this is to not represent an integer by a single variable, but by multiple variables.  Our strategy 
will be to use an `ArrayList` of integers to represent an integer with any number of digits: we store each digit of the integer in each position in 
the `ArrayList`.  We will built a class called `LargeInt` to accomplish this.

## The `LargeInt` class

Your LargeInt class will use an `ArrayList` of integers to represent a non-negative integer.  Though it sounds weird, you should store the digits in 
**backwards** order (it will make your later algorithms easier). For instance, the integer 6805 would be represented as an `ArrayList` of ints as in 
the diagram below.
```
position:    [0]   [1]   [2]   [3]
           -------------------------
value:     |  5  |  0  |  8  |  6  |
           -------------------------
```
### Why backwards?
Eventually, you will have to write an algorithm to add two LargeInts together. Your algorithm will simulate the elementary-school addition 
algorithm, which aligns integers to the right so the place-values match up between them:
```
  6805
 +  32
======
  6837
```
If you make your `ArrayLists` store the digits in "normal" order, you get the following:
```
position:    [0]   [1]   [2]   [3]
           -------------------------
value:     |  6  |  8  |  0  |  5  |
           -------------------------
           -------------
value:     |  3  |  2  |
           -------------
```
In this representation, the digits don't line up right. You can still write the addition algorithm, but the logic is trickier because the positions 
don't match up between the two vectors. However, if you store the digits in "backwards" order, everything lines up:
```
position:    [0]   [1]   [2]   [3]
           -------------------------
value:     |  5  |  0  |  8  |  6  |
           -------------------------
           -------------
value:     |  2  |  3  |
           -------------
```
This is a very good exercise in how the internal representation (what the class designer sees) of a class does not *necessarily* have to match its 
external representation (what the user of the class sees).

## A look at the code
- `LargeInt.java`: This is where you will write your LargeInt class.
- `LargeIntTester.java`: This is where you will write test cases for your LargeInt class.  You will notice the `main()` method is just a bunch of 
calls to other testing functions, all commented out.  You will write these functions later, and you can comment/uncomment the calls as you complete 
the project.
- `LargeIntDemo.java`: This is where you will write your final demonstration of the LargeInt class.
- `OverflowDemo.java`: This is a demonstration of integer overflow, which you can run if you want to see it happen.  There is nothing here you need 
to modify.

## Part A: Writing the LargeInt class
You will need to add a number of methods to the LargeInt class, as right now you will notice it only has a single instance variable for the digits 
of the number, and nothing else.

Here are the methods you will need to add.  You can write them in (almost) any order, though some make more sense before others.  For each method 
you **must** write a set of corresponding test cases in `LargeIntTester.java`.

### Constructors

You must write a default constructor.  The signature for this constructor should be
```
public LargeInt()
```
This constructor should initialize the LargeInt to represent the integer zero.  You can choose how to represent zero in the `digits` ArrayList; this 
might be with a completely empty ArrayList or an ArrayList with a single `0` in it.
<hr>

You must write a constructor that initializes a LargeInt from a `String`.  The signature for this constructor is
```
public LargeInt(String str)
```
This constructor may assume the `String` passed in contains a valid nonnegative integer, and the code should initialize the LargeInt to represent 
the same integer.
<hr>
You must write a constructor that initializes a LargeInt from an `int`.  The signature for this constructor is

```
public LargeInt(int i)
```
This constructor may assume the `int` passed in contains a nonnegative integer, and the code should initialize the LargeInt to represent the same 
integer.
<hr>

### `toString()`
You must write a `toString()` method.  The signature for this method should be

```
public String toString()
```
The `toString()` method should simply return a `String` with the "normal" representation of the integer.  Note that since you are storing the digits 
in "reversed" order, you will of course need to iterate through the array backwards.

- **Testing:**

  You must write a `testConstructors()` method in `LargeIntTester.java` that tests the three constructors and the `toString()` method.  You should 
write enough tests to convince yourself these constructors and methods work.  Do not delete the tests when you're done.  You can just comment out 
the call to `testConstructors()`.

### Calculating the number of digits
You must write a method to return the number of digits in an integer.    The signature for this method should be:
```
public int numDigits()
```
- **Testing**: You may write a new test method or integrate these tests with the constructors/toString().

### Testing two integers for equality
You must write a method to test if two integers are the same.  The method signature is 
```
public boolean equals(Object other)
```
This is a method inherited from the `Object` class in Java, which is why the parameter is specified to be `Object`.  Writing an `equals()` method is 
fairly straightforward.  You can follow the pattern here:
https://www.baeldung.com/java-comparing-objects#equals-instance

- **Testing:** Fill in the `testEquals()` method.  You may want to wait to really test this until you write the `add()` method, which will give you 
some more test cases.

### Adding two LargeInts
**Note: This method requires writing pseudocode first!**
You must write a method to add two integers together and return their sum as a new integer.  The method signature is
```
public LargeInt add(LargeInt other)
```
You should follow the "elementary school" algorithm for adding two integers together.  You will use a loop to iterate over the digits from 
right-most digit to left-most, adding pairs of digits as you go. If you get a sum less than ten, that becomes a digit in your answer. If you get a 
sum greater than or equal to ten, only the one's digit goes into your sum, and you will have to "carry" the one to the following column.

**Plan your pseudocode out on paper first (or in a text editor) and work through some examples.** You will turn this in as part of your final 
submission, so don't throw it out.

Special considerations:

-   What do you do if the two numbers have differing numbers of digits?
-   How do you handle the carry at the end of the loop if your answer will have more digits than either of the starting numbers?
-   Good examples to try: 1 + 1, 5 + 5, 99 + 1, 1 + 99, 1234 + 4321, 123456789 + 987654321.

- **Testing**: Fill in `testAdd()` and use a lot of examples to make sure this works! 

### Multiplying two LargeInts
Write a method to multiply two LargeInts and return the product.  The method signature is
```
public LargeInt multiply(LargeInt other)
```

You do not have to simulate the elementary-school multiplication algorithm here (though you can if you want). Instead, just make a for loop that 
does repeated addition. In other words, to multiply 9 by 5, make a new LargeInt, set it to zero, then add 9 to the new LargeInt five times. Yes this 
is slow, but it works.

**Be careful with implementing multiplication; this can really slow down your code if you do it wrong:**
 
Say I want to multiply 100 by 3. I can calculate this as 100 + 100 + 100, or 3 + 3 + 3 +...+ 3 (100 total additions). It is much faster to do the 
first calculation (3 additions) rather than the second calculation (100 additions). Make sure your multiplication function is doing the faster 
version.

- **Testing**: Fill in `testMultiply()` and use a lot of examples to make sure this works! 

### Comparing LargeInts
You will write a method to compare two LargeInts to see which one is bigger.  You will do this using a standard Java interface called `Comparable`.  
You can read the documentation for it here: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Comparable.html

First, you should change the beginning of the LargeInt class to read:
```
public class LargeInt implements Comparable<LargeInt>
```
This tells Java we are implementing the `Comparable` interface, and our LargeInts should be comparable against other LargeInts.  (Usually the part 
inside the angle brackets with `Comparable` will match the name of the class that implements the interface, though it doesn't have to match.)

To implement `Comparable`, add a method to the class:
```
public int compareTo(LargeInt other)
```
This method should return a negative integer, zero, or a positive integer if this LargeInt is less than, equal to, or greater than the "`other`" 
LargeInt.  The specific values of the negative or positive integers don't matter; you can use -1, 0, and +1 if you want.

To implement this method, invent an algorithm for examining two integers to see which one is bigger, and return an appropriate integer.

- **Testing**: Test your code in the `testCompareTo()` method.  You can also use the `testSorting()` method that I wrote for you --- this method 
illustrates what you can do with classes that implement `Comparable`!

## Part B: Using the LargeInt class
Now, you will use the LargeInt class to solve a (semi-)real world problem: computing factorials for large numbers.  Recall that the factorial 
function is defined as:
```
fact(0) = 1
fact(n) = 1 * 2 * ... * n, for n > 0
```

This function grows very quickly, so quickly, in fact, that computing the factorial of any integer past about 25 quickly exceeds the capacity of a 
regular `long` integer in Java.  You will use the LargeInt class to compute the factorials of all integers between 0 and 100.

First, go to LargeIntDemo.java, and write a (static) function called `LargeInt factorial(LargeInt li)` that computes the factorial of the LargeInt 
parameter `li` and returns it.  Then, in `main()`, write a loop to print out the factorials of 0 through 100, **along with the number of digits in 
each factorial** (this last part is good for testing/debugging).

Hint: do not try to write this function recursively, because you will need a subtraction method for LargeInts and you don't have one! 

## Output
Your output should match mine:
```
Factorials from 0 to 100:
0 factorial is 1 and has 1 digits.
1 factorial is 1 and has 1 digits.
2 factorial is 2 and has 1 digits.
3 factorial is 6 and has 1 digits.
4 factorial is 24 and has 2 digits.
5 factorial is 120 and has 3 digits.
6 factorial is 720 and has 3 digits.
7 factorial is 5040 and has 4 digits.
8 factorial is 40320 and has 5 digits.
9 factorial is 362880 and has 6 digits.
10 factorial is 3628800 and has 7 digits.
11 factorial is 39916800 and has 8 digits.
12 factorial is 479001600 and has 9 digits.
13 factorial is 6227020800 and has 10 digits.
14 factorial is 87178291200 and has 11 digits.
15 factorial is 1307674368000 and has 13 digits.
16 factorial is 20922789888000 and has 14 digits.
17 factorial is 355687428096000 and has 15 digits.
18 factorial is 6402373705728000 and has 16 digits.
19 factorial is 121645100408832000 and has 18 digits.
20 factorial is 2432902008176640000 and has 19 digits.
21 factorial is 51090942171709440000 and has 20 digits.
22 factorial is 1124000727777607680000 and has 22 digits.
23 factorial is 25852016738884976640000 and has 23 digits.
24 factorial is 620448401733239439360000 and has 24 digits.
25 factorial is 15511210043330985984000000 and has 26 digits.
26 factorial is 403291461126605635584000000 and has 27 digits.
27 factorial is 10888869450418352160768000000 and has 29 digits.
28 factorial is 304888344611713860501504000000 and has 30 digits.
29 factorial is 8841761993739701954543616000000 and has 31 digits.
30 factorial is 265252859812191058636308480000000 and has 33 digits.
31 factorial is 8222838654177922817725562880000000 and has 34 digits.
32 factorial is 263130836933693530167218012160000000 and has 36 digits.
33 factorial is 8683317618811886495518194401280000000 and has 37 digits.
34 factorial is 295232799039604140847618609643520000000 and has 39 digits.
35 factorial is 10333147966386144929666651337523200000000 and has 41 digits.
36 factorial is 371993326789901217467999448150835200000000 and has 42 digits.
37 factorial is 13763753091226345046315979581580902400000000 and has 44 digits.
38 factorial is 523022617466601111760007224100074291200000000 and has 45 digits.
39 factorial is 20397882081197443358640281739902897356800000000 and has 47 digits.
40 factorial is 815915283247897734345611269596115894272000000000 and has 48 digits.
41 factorial is 33452526613163807108170062053440751665152000000000 and has 50 digits.
42 factorial is 1405006117752879898543142606244511569936384000000000 and has 52 digits.
43 factorial is 60415263063373835637355132068513997507264512000000000 and has 53 digits.
44 factorial is 2658271574788448768043625811014615890319638528000000000 and has 55 digits.
45 factorial is 119622220865480194561963161495657715064383733760000000000 and has 57 digits.
46 factorial is 5502622159812088949850305428800254892961651752960000000000 and has 58 digits.
47 factorial is 258623241511168180642964355153611979969197632389120000000000 and has 60 digits.
48 factorial is 12413915592536072670862289047373375038521486354677760000000000 and has 62 digits.
49 factorial is 608281864034267560872252163321295376887552831379210240000000000 and has 63 digits.
50 factorial is 30414093201713378043612608166064768844377641568960512000000000000 and has 65 digits.
51 factorial is 1551118753287382280224243016469303211063259720016986112000000000000 and has 67 digits.
52 factorial is 80658175170943878571660636856403766975289505440883277824000000000000 and has 68 digits.
53 factorial is 4274883284060025564298013753389399649690343788366813724672000000000000 and has 70 digits.
54 factorial is 230843697339241380472092742683027581083278564571807941132288000000000000 and has 72 digits.
55 factorial is 12696403353658275925965100847566516959580321051449436762275840000000000000 and has 74 digits.
56 factorial is 710998587804863451854045647463724949736497978881168458687447040000000000000 and has 75 digits.
57 factorial is 40526919504877216755680601905432322134980384796226602145184481280000000000000 and has 77 digits.
58 factorial is 2350561331282878571829474910515074683828862318181142924420699914240000000000000 and has 79 digits.
59 factorial is 138683118545689835737939019720389406345902876772687432540821294940160000000000000 and has 81 digits.
60 factorial is 8320987112741390144276341183223364380754172606361245952449277696409600000000000000 and has 82 digits.
61 factorial is 507580213877224798800856812176625227226004528988036003099405939480985600000000000000 and has 84 digits.
62 factorial is 31469973260387937525653122354950764088012280797258232192163168247821107200000000000000 and has 86 digits.
63 factorial is 1982608315404440064116146708361898137544773690227268628106279599612729753600000000000000 and has 88 digits.
64 factorial is 126886932185884164103433389335161480802865516174545192198801894375214704230400000000000000 and has 90 digits.
65 factorial is 8247650592082470666723170306785496252186258551345437492922123134388955774976000000000000000 and has 91 digits.
66 factorial is 544344939077443064003729240247842752644293064388798874532860126869671081148416000000000000000 and has 93 digits.
67 factorial is 36471110918188685288249859096605464427167635314049524593701628500267962436943872000000000000000 and has 95 digits.
68 factorial is 2480035542436830599600990418569171581047399201355367672371710738018221445712183296000000000000000 and has 97 digits.
69 factorial is 171122452428141311372468338881272839092270544893520369393648040923257279754140647424000000000000000 and has 99 digits.
70 factorial is 11978571669969891796072783721689098736458938142546425857555362864628009582789845319680000000000000000 and has 101 digits.
71 factorial is 850478588567862317521167644239926010288584608120796235886430763388588680378079017697280000000000000000 and has 102 digits.
72 factorial is 61234458376886086861524070385274672740778091784697328983823014963978384987221689274204160000000000000000 and has 104 digits.
73 factorial is 4470115461512684340891257138125051110076800700282905015819080092370422104067183317016903680000000000000000 and has 106 digits.
74 factorial is 330788544151938641225953028221253782145683251820934971170611926835411235700971565459250872320000000000000000 and has 108 digits.
75 factorial is 24809140811395398091946477116594033660926243886570122837795894512655842677572867409443815424000000000000000000 and has 110 digits.
76 factorial is 1885494701666050254987932260861146558230394535379329335672487982961844043495537923117729972224000000000000000000 and has 112 digits.
77 factorial is 145183092028285869634070784086308284983740379224208358846781574688061991349156420080065207861248000000000000000000 and has 114 
digits.
78 factorial is 11324281178206297831457521158732046228731749579488251990048962825668835325234200766245086213177344000000000000000000 and has 116 
digits.
79 factorial is 894618213078297528685144171539831652069808216779571907213868063227837990693501860533361810841010176000000000000000000 and has 117 
digits.
80 factorial is 71569457046263802294811533723186532165584657342365752577109445058227039255480148842668944867280814080000000000000000000 and has 119 
digits.
81 factorial is 5797126020747367985879734231578109105412357244731625958745865049716390179693892056256184534249745940480000000000000000000 and has 
121 digits.
82 factorial is 475364333701284174842138206989404946643813294067993328617160934076743994734899148613007131808479167119360000000000000000000 and has 
123 digits.
83 factorial is 39455239697206586511897471180120610571436503407643446275224357528369751562996629334879591940103770870906880000000000000000000 and 
has 125 digits.
84 factorial is 3314240134565353266999387579130131288000666286242049487118846032383059131291716864129885722968716753156177920000000000000000000 and 
has 127 digits.
85 factorial is 281710411438055027694947944226061159480056634330574206405101912752560026159795933451040286452340924018275123200000000000000000000 
and has 129 digits.
86 factorial is 24227095383672732381765523203441259715284870552429381750838764496720162249742450276789464634901319465571660595200000000000000000000 
and has 131 digits.
87 factorial is 
2107757298379527717213600518699389595229783738061356212322972511214654115727593174080683423236414793504734471782400000000000000000000 and has 133 
digits.
88 factorial is 
185482642257398439114796845645546284380220968949399346684421580986889562184028199319100141244804501828416633516851200000000000000000000 and has 135 
digits.
89 factorial is 
16507955160908461081216919262453619309839666236496541854913520707833171034378509739399912570787600662729080382999756800000000000000000000 and has 
137 digits.
90 factorial is 
1485715964481761497309522733620825737885569961284688766942216863704985393094065876545992131370884059645617234469978112000000000000000000000 and has 
139 digits.
91 factorial is 
135200152767840296255166568759495142147586866476906677791741734597153670771559994765685283954750449427751168336768008192000000000000000000000 and 
has 141 digits.
92 factorial is 
12438414054641307255475324325873553077577991715875414356840239582938137710983519518443046123837041347353107486982656753664000000000000000000000 and 
has 143 digits.
93 factorial is 
1156772507081641574759205162306240436214753229576413535186142281213246807121467315215203289516844845303838996289387078090752000000000000000000000 
and has 145 digits.
94 factorial is 
108736615665674308027365285256786601004186803580182872307497374434045199869417927630229109214583415458560865651202385340530688000000000000000000000 
and has 147 digits.
95 factorial is 
10329978488239059262599702099394727095397746340117372869212250571234293987594703124871765375385424468563282236864226607350415360000000000000000000000 
and has 149 digits.
96 factorial is 
991677934870949689209571401541893801158183648651267795444376054838492222809091499987689476037000748982075094738965754305639874560000000000000000000000 
and has 150 digits.
97 factorial is 
96192759682482119853328425949563698712343813919172976158104477319333745612481875498805879175589072651261284189679678167647067832320000000000000000000000 
and has 152 digits.
98 factorial is 
9426890448883247745626185743057242473809693764078951663494238777294707070023223798882976159207729119823605850588608460429412647567360000000000000000000000 
and has 154 digits.
99 factorial is 
933262154439441526816992388562667004907159682643816214685929638952175999932299156089414639761565182862536979208272237582511852109168640000000000000000000000 
and has 156 digits.
100 factorial is 
93326215443944152681699238856266700490715968264381621468592963895217599993229915608941463976156518286253697920827223758251185210916864000000000000000000000000 
and has 158 digits.
```

## At the end of the project

-   As you are preparing to submit the project, please prepare a text file (`.txt`, pdf, or Word doc is fine) answering the following questions:
    1.  What bugs and conceptual difficulties did you encounter? How did you overcome them? What did you learn?
    2.  Describe whatever help (if any) that you received. Don’t include readings, lectures, and exercises, but do include any help from other 
sources, such as websites or people (including classmates and friends) and attribute them by name.
    3.  Describe any serious problems you encountered while writing the program.
    4.  Did you do any of the challenges (see below)? If so, explain what you did.
    5.  List any other feedback you have. Feel free to provide any feedback on how much you learned from doing the assignment, and whether you 
enjoyed doing it.
-   Please also add a comment at the top of your program stating your name and a pledge that you have followed the honor code and collaboration 
policy for this project. This can be as simple as writing “**I have neither given nor received unauthorized aid on this program.**” You can find the 
collaboration policy on the syllabus.
-   Remember, projects will be graded not only on correctness, but also appropriateness and efficiency of the algorithms you choose, and on coding 
style.

## Grading
This project will be graded on the correctness of the output, efficiency of the algorithms, and on coding style, including comments.

## Challenge Problems
From time to time, I will offer “challenge problems” on assignments. These problems are designed to have little (but some) impact on your grade 
whether you do them or not. You should think of these problems as opportunities to work on something interesting and optional, rather than a way to 
raise your grade through “extra credit.”

-   Add other math operators to the LargeInt class: subtraction, division, remainder (%), etc.
-   Make the multiplication algorithm work like the elementary school method, rather than repeated addition (which is slow).


## Submitting
To submit, upload all of your `.java` files to canvas, along with your answers to the post-project questions, and your pseudocode/pictures/notes for 
how you constructed your addition algorithm.  This doesn't have to be formal, and if you just have notes on a page, you can turn in photos.
