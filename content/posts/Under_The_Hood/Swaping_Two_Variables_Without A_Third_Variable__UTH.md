+++
title = "Under The Hood: Swaping Two Variables Without A Third Variable"
date = "2023-04-10T15:16:13+06:00"
author = "Gourob Dev"
authorTwitter = "" #do not include @
cover = ""
tags = ["Interesting Piece Of Code", "Beautiful Snippets", "Python", "C", "Beauty of Code", "Under the hood"]
keywords = ["", ""]
description = "Swapping the value of two variables without assigning a temporary third variable is a very simple task that we do pretty often. I used to do it without really understanding what's really happening under the hood..."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

Today when I was doom scrolling on YouTube I came accross [this](https://www.youtube.com/watch?v=Nusg5dUjR0A) video by The Primagen talking about the biggest takes on programming. There was a part where writing a program about swapping the values of two variables without a temporary variable was mentioned. I thought how it was done in C. After a quick googling I came accross [this article](https://www.javatpoint.com/c-program-to-swap-two-numbers-without-using-third-variable) and the code was kinda interesting.

I'm currently learning and using C because of school. But before that I used Python for most of my projects.
In python to swap the value of two variables without a third variable I use the following snippet.

```python
x = 3
y = 5

x, y = y, x

print(x) # 5
print(y) # 3
```
I used the exact statement `x, y = y, x;` in C but after compilation when I ran the code the output wasn't the same. The content of the variables didn't swap! But why?

## Tuple Unpacking in Python
To assign multiple values into a single variable in Python we can use Lists, Dictionaries or Tuples.
But the values which are stored in a tuple are unchangeable.

Tuples are assigned like this.
```python
t = (1, 2, 3)
```

By using the feature implemented in Python called tuple unpacking we can assign multiple variables buy splitting a tuple.

For example, suppose we have a tuple of two numbers. Now we will assign the first number to the variable x and the second number to the variable y.
```python
x, y = (1, 2)
```
Here the tuple `(1, 2)` is split in half and then one is a signed to x and 2 is assigned to y. normally to do this we would write two lines of code. But by using this feature we can do all that in a single line of code.

A tuple can also contain variables. 

```python
var1 = 6
var2 = 9

t = (var1, var2)

print(t)  # (6, 9)
```
Here Python first gets the actual value of the variables var1 and var2 and then assigning to the variable t.

So in the snippet `x, y = y, x` Python is first getting the value of `y, x` and then using tuple unpacking it assigns the value of x and y. 
the process is indeed very beautiful.

But, the feature doesn't exist in C. That is why it doesn't work there.

## Swapping The Contents of Two Variables in C
We will be discussing three methods on how we can swap the contents of two variables without assigning a temporary third variable in C.

### The Arithmetic Method
Suppose we have two variables x and y. Where x is assigned with the number 2 and why is assigned with the number 5.

Now if we add x and y we will get 7 (`5 + 2 = 7`) and then assign it to x.
```c
x = x + y   // x=7
```

Now if we subtract y from x we will get the previous value of x (`7 - 2 = 5`). Now by assigning it to y the previous value of x will now be assigned to y.

```c
x = x + y   // x=7
y = x - y   // y=2
```

Finally, by subtracting the new value of y from the new value of x we will get the original value of x (`7 - 5 = 2`) and by assigning it to the variable y the swapping process will be completed.

```c
x = x + y   // x=7
y = x - y   // y=2
x = x - y   // x=5
```
<br>
To sum it up the c implementation of the arithmetic method will be:

```c
#include <stdio.h>

int main(){
    int x = 2;
    int y = 5;

    x = x + y   // x=7
    y = x - y   // y=2
    x = x - y   // x=5
    
    return 0;
}
```

### The XOR(`^`) Method
XOR(exclusive or) is a bitwise operator that takes to binary bits and compares them. If the two bits match then it returns 0. but if the bits are different IT returns 1.

| A | B | A XOR B |
|---|---|---------|
| 0 | 0 |    0    |
|1|0|1|
|0|1|1|
|1|1|0|

Suppose we have two variables x and y. Where x is assigned with the number 2 and why is assigned with the number 5.

```c
int x = 2;
int y = 5;
```

Here 2 and 5 are decimal numbers. 
The binary form of 2 is `00000010` and 5 in binary form is `00000101`.
If we do the XOR operation on the variable x and y the result will be `00000111` or 7 in decimal.

```
Explanation:

    00000010     (x)
(^) 00000101     (y)
--------------
    00000111

```

Now if we assign the value of x XOR y to x then x's new value will be 7.

```c
x = x ^ y   // x = 7
```

Then if we do the XOR operation between y and the new value of x and store it into y then the number which was previously assigned to x will now be assigned to y.

```
Explanation:

    00000111     (new x)
(^) 00000101     (y)
--------------
    00000010     (Previous value of x)

```
```c
x = x ^ y   // x=7
y = x ^ y   // y=2
```

Finally if we do the XOR operation again between the new values of both x and y and then assigning the result to x. The old value of y will now be assigned to x and thus the swapping operation will be completed.

```
Explanation:

    00000111     (new x)
(^) 00000010     (new y)
--------------
    00000101     (Previous value of y)
```
```c
x = x ^ y   // x=7
y = x ^ y   // y=2
x = x ^ y   // x=5
```
<br>

To sum it up the c implementation of the XOR method will be:

```c
#include <stdio.h>

int main(){
    int x = 2;
    int y = 5;

    x = x ^ y   // x=7
    y = x ^ y   // y=2
    x = x ^ y   // x=5
    
    return 0;
}
```

### Comparing both methods
Both of these methods are very beautiful!

Although the arithmetic method is more readable and easier to understand it is actually slower.

In the Arithmetic Method three arithmetic operations are done to finish the process.

```c
x = x + y
y = x - y
x = x - y
```

On the other hand, in the XOR method three bitwise operations are done to finish the process.

```c
x = x ^ y
y = x ^ y
x = x ^ y
```

Bitwise operations work on the binary form of the values thus the operation is done at a lower level than an arithmetic operations.

That is the reason the XOR method is faster and more efficient than the Arithmetic method.

## Some Thoughts
Swapping the value of two variables without assigning a temporary third variable is a very simple task that we do pretty often. I used to do it without really understanding what's really happening under the hood.

I'm a 17 y/o boy who thought that he knew python pretty well but after doing the research for this blog post now I know that my current knowledge is basically at the surface level. But I can say that I have more knowledge than I had yesterday.

The reason for me making a blog is to learn more about stuff I already kind of know it or want to master it. Because, I try not to post something that is wrong or false.

As Richard Fynman once said,

> *"If you want to master something, teach it.
The more you teach, the better you learn.
Teaching is a powerful tool to learning."*
