---
title: The elusive lambda function in Python
tags: [Coding, Python]
style: fill
color: info
description: Let's talk about lambda functions—one of the more elusive features of Python.
---

Let's talk about **lambda functions**—one of the more elusive features of Python. It's so elusive that I didn't fully understand/never really used it in my first 4 years of Python coding.

## What is a lambda function?
A lambda function is an **anonymous function**. Anonymous function just means a function that doesn't have a name. Typically, we define a function like below in Python. This function named `square()` takes in a number and returns the squared value of that number.

```python
def square(num):
    return num ** 2

print(square(5))
print(square(7))
```
```
25
49
```

We can rewrite this function as a lambda function as follows. The structure of a lambda function begins with the `lambda` keyword, some amount of arguments, a colon, and what it returns.

```python
square = lambda num: num ** 2

print(square(5))
print(square(7))
```
```
25
49
```
The above lambda function takes in one argument `num` and returns `num ** 2`. Here is another example with more arguments.

```python
add_three = lambda a, b, c: a + b + c

print(add_three(1, 4, 5))
```
```
10
```

## Why do we need it?
I'm sure you have some questions about this. Why do we need another way of defining a function when they're basically the same? And how is this anonymous when we are assigning the lambda function to a variable?

Lambda functions are particularly useful when you are **passing a function into another function**. Below is an example where we have an `add_one` function that adds one to the result of an operation. The function accepts two numbers and a function. It executes the passed-in function with the two parameters, adds one to the result, and returns the value. Let's see what happens when we run the `add_one` function with three different lambda functions.

```python
def add_one(num1, num2, func):
    val = func(num1, num2)
    return val + 1

print(add_one(2, 5, lambda x, y: x + y))
print(add_one(2, 5, lambda x, y: x - y))
print(add_one(2, 5, lambda x, y: x * y))
```
```
8
-2
11
```

Using lambda functions here is useful because we don't want to create one separate function for each of the add, subtract, and multiply functions like below. This just makes the code messy and difficult to follow when your program has a bajillion lines.

```python
# Probably shouldn't do this
def add(num1, num2):
    return num1 + num2

def subtract(num1, num2):
    return num1 - num2

def multiply(num1, num2):
    return num1 * num2

def add_one(num1, num2, func):
    val = func(num1, num2)
    return val + 1

print(add_one(2, 5, add))
print(add_one(2, 5, subtract))
print(add_one(2, 5, multiply))
```

Obviously, the above example wasn't very practical. The real practical use case for lambda functions is when you are using some **built-in functions or functions from other modules where they take a function as an argument**. Do note that this is only coming from my perspective as a data guy. If you ask people from other fields, they may provide other important use cases. Anyway, let us go through a few lambda function use cases that I have personally found useful.

## Use case
#### The max and min functions
The `max` and `min` functions in Python find the maximum and minimum values in a list of values. Now, what if we want to find the maximum or minimum of a list of tuples based on its second index? We would need to make use of the `key=` parameter. Here, we can use a lambda function to determine what we are basing the maximum or minimum on.

```python
val = [("Jack", 5), ("Mark", 8), ("Tim", 2)]
print(max(val, key=lambda x: x[1]))
print(min(val, key=lambda x: x[1]))
```
```
("Mark", 8)
("Tim", 2)
```

We are evaluating the maximum and minimum based on the second index, and importantly, we are returning the whole tuple as the result.

#### The sorted function
The `sorted` function in Python returns a sorted list of the iterable. If we were sorting a list of integers, this function would be pretty straightforward. But sometimes, our variable may have more dimensions or it's a more complicated data structure, so we may wish to sort everything in a particular way. In this case, we can use the `key=` parameter and specify a lambda function that states how we wish to sort this iterable.

```python
val = [("Jack", 5), ("Mark", 8), ("Tim", 2)]
print(sorted(val, key=lambda x: x[1]))
```
```
[('Tim', 2), ('Jack', 5), ('Mark', 8)]
```

#### The map function
The `map` function in Python takes in two parameters, a function and an iterable. It applies the passed-in function to each value in the iterable and returns a map object. Here, we are applying the `x * 2` function to each of the values in the list.

```python
val = [1, 2, 3, 4, 5, 6, 7]
print(list(map(lambda x: x * 2, val)))
```
```
[2, 4, 6, 8, 10, 12, 14]
```

#### The filter function
The `filter` function in Python takes in two parameters, a function and an iterable. The function evaluates a boolean expression that determines whether each value in the iterable should be kept or filtered. Here, we are only keeping values in the list that are larger than 4.

```python
val = [1, 2, 3, 4, 5, 6, 7]
print(list(filter(lambda x: x > 4, val)))
```
```
[5, 6, 7]
```

#### The reduce function in functools
The `reduce` function in the `functools` module is another great opportunity to use a lambda function. The `reduce` method takes in a lambda function and an iterator. The lambda function should take in two arguments and return what happens to those two arguments. You can think of the `reduce` function as a recursion. The first two values in the iterator are first put into the lambda function, the returned result and the third value are put into the lambda function again, the returned result from that and the fourth value are put into the lambda function, and so on and so forth.

```python
from functools import reduce

val = [1, 2, 3, 4, 5, 6, 7]
print(reduce(lambda x, y: x + y, val))
```
```
28
```

## Conclusion
Hope this demystifies the elusive lambda function. It can be confusing to understand at first, but when you encounter those particular cases, it becomes incredibly useful.
