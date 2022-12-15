---
title: Python generator and yield statement are neat
tags: [Coding, Python]
style: fill
color: info
description: Generator is one of those things that most Python beginners and even intermediate programmers forget about, but when you encounter a situation that requires it, it becomes incredibly crucial.
---

Generator is one of those things that most Python beginners and even intermediate programmers forget about, but when you encounter a situation that requires it, it becomes incredibly crucial.

## Use Case
When you have a large set of data that you are iterating over, allocating memory over the whole dataset is computationally intensive for your machine or server. This is when generators are useful as it retrieves items one by one.

#### Generator Expression
Consider the following scenario:

```python
# Generating a large list using list comprehension
large_list = [i ** 2 for i in range(100000000000)]
```

You can imagine how much memory it would take to store the individual values of `i ** 2` from `i = 1` to `i = 100000000000` in a list. You would more than likely encounter a `MemoryError` if you did this.

This is when generators are useful, you simply have to replace the `[]` with `()` and you have created a generator.

```python
# Generating a large list using generator expression
large_list = (i ** 2 for i in range(100000000000))
```

With this expression, the values of `i ** 2` are not *generated* yet (i.e., the individual values are not stored in memory). When you need to iterate over `i`, you can extract the values one by one by either looping over the generator or using `next()`.

```python
# Option 1: Looping over the generator

for val in large_list:
    # You can do whatever operation with the value you want
    print(val)  # 0, 1, 4, 9, 16, etc.
```

```python
# Option 2: Using next()

print(next(large_list))  # 0

print(next(large_list))  # 1

print(next(large_list))  # 4

print(next(large_list))  # 9

print(next(large_list))  # 16
```

#### Generator Function
Realistically, you will encounter scenarios where you can’t fit your iterator in one line, here is where you have to use a generator function with the `yield` statement.

The `yield` statement is similar to the `return` statement, the difference is that `return` returns a value whereas `yield` returns a generator object.

Let’s unpack the generator above into a generator function.

```python
# Unpacking (i ** 2 for i in range(100000000000))

def gen_func():
    for i in range(100000000000):
        yield i ** 2
```

When you access the generator using `next()`, the generator function will run up to the `yield` statement, and return that state of the generator back to you.

Here is another example:

```python
def gen_func():
    i = 0 
    while True:
        yield i ** i
        i += 1
```

## Practical Scenarios

#### Reading CSV Files
When you are reading a CSV file, you may write something like this.

```python
file = open(“csv_file.csv”, “r”)
content = file.read().split(“\n”)
file.close()
```

But what if your CSV file is too large? The `file.read()` statement reads the whole file and stores it in a variable called `content` all at once. If your file is too large, it’s going to run into memory issues. This is where generator is useful, and Python’s `with` statement returns you with a generator.

```python
with open(“csv_file.csv”, “r”) as file:  # file is a generator
    for line in file:
        # Do your operations line by line
```

#### Machine Learning
This is a scenario that I came across where generator proved to be helpful. I was trying to train a model on TensorFlow that requires two inputs. Without going into too much depth, the input shapes of both inputs were huge and the output shape was also huge, and I also had a lot of data. My computer could not handle the amount of data if I simply throw the whole dataset into the model. Besides, I had to format the data in order to fit the model specifications. Long story short, I used generator to feed the data into the model batch by batch. Here is some very rough semi-pseudocode:

```python
def data_gen(ds1, ds2, batch=8):
    X1, X2, y = list(), list(), list()
    n = 0
    while True:
        # Format the data

        X1.append(...)  # Formatted first input
        X2.append(...)  # Formatted second input
        y.append(...)  # Formatted output
        n += 1

        if n % batch == 0:
            # Yield the batch of data and feed it into the model
            yield [[np.asarray(X1), np.asarray(X2)], np.asarray(y)]
            X1, X2, y = list(), list(), list()
```

## When should you NOT use generators
Although generators are powerful, there are situations where they are not appropriate.

#### 1. You need to freely access data by indexes
Generators do not allow you to access specific elements by indexes. For example, if you want to extract specific elements within the generator, you can’t do `generator[index]` as you would with a list `list[index]`. 

#### 2. When you need list operations
With generators, you cannot use list operations, such as `sum`, `min`, `max`, etc. You also can’t reverse the order of a generator or sort the elements of a generator (easily), it only goes in one direction.

#### 3. Dealing with small data
Generators are slow, especially if you are dealing with a small amount of data. The benefits of generators over standard list operations only become apparent when the large amount of data is impacting your machine’s speed. Generally speaking, only use generators when you run into problems with performance, then you make the switch.
