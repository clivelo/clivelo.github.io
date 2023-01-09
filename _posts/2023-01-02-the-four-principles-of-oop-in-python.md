---
title: The four principles of Object-Oriented Programming in Python
tags: [Coding, Python]
style: fill
color: info
description: Object-oriented programming (OOP) is a programming design based on the idea of objects in real-life, which includes four critical principles, which are Inheritance, Polymorphism, Encapsulation, and Abstraction.
---

Object-oriented programming (OOP) is a programming design based on the idea of objects in real-life. Objects have properties (known as **attributes**) and functions (known as **methods**). This paradigm allows programmers to think about codes as working with real-life objects. The main idea of OOP includes four critical principles, which will be explained in the following post using Python.

This post requires the basic knowledge of writing OOP in Python. Please first read about [the fundmentals of OOP in Python](https://clivelo.me/blog/the-fundamentals-of-oop-in-python) if you have not learned about classes and object yet.

## OOP
We will be using the following code as the basis for all the examples below.

```python
class Human:
    def __init__(self, age, job=None):
        self.age = age
        self.job = job

class Cat:
    def __init__(self, age):
        self.age = age

h = Human(25, "Software Developer")
print("Age:", h.age)

c = Cat(7)
print("Age:", c.age)
```
Output:
```
Age: 25
Age: 7
```

{% capture list_items %}
Inheritance
Polymorphism
Encapsulation
Abstraction
{% endcapture %}
{% include elements/list.html title="Four Principles of OOP" type="toc" %}

## Inheritance
Inheritance is when one class inherits the attributes and methods of another class, the two classes are also known as child class (or derived class) and parent class (or base class) respectively.

In Java, we would use the `extends` keyword, but in Python, we would simply put the parent class in parentheses when declaring the child class.

Here we observe that the `Human` class and the `Cat` class both share similar attributes and methods. We can therefore create an `Animal` parent class (yes, humans are animals) that defines the `age` variable. We would then add the parent class when defining the child class by putting the parent class in parentheses, like `class Human(Animal)`. To make sure when instantiating a child object, the parent class `__init__()` is run, we need to call `super().__init__()`. The keyword `super()` indicate the parent class, whereas `.__init__()` calls the `__init__()` method of the parent class.

```python
# Define parent class
class Animal:
    def __init__(self, age):
        self.age = age

# Inheriting parent class
class Human(Animal):
    def __init__(self, age, job=None):
        # Calling __init__() of parent class, passing age as argument
        super().__init__(age)
        self.job = job

# Inheriting parent class
class Cat(Animal):
    def __init__(self, age):
        # Calling __init__() of parent class, passing age as argument
        super().__init__(age)

h = Human(25, "Software Developer")
print("Age:", h.age)

c = Cat(7)
print("Age:", c.age)
```
Output:
```
Age: 25
Age: 7
```

## Polymorphism
Polymorphism simply means "many forms". In OOP, it is used to describe that one method name can have multiple implementations.

Here we are implementing a `speak()` method for both the `Human` and `Cat` classes. We see that they both share the same method name, but they act differently depending on whether we are calling `h.speak()` or `c.speak()`.

```python
class Animal:
    def __init__(self, age):
        self.age = age

class Human(Animal):
    def __init__(self, age, job=None):
        super().__init__(age)
        self.job = job

    # Defining a speak() method for Human
    def speak(self):
        print("Hi")

class Cat(Animal):
    def __init__(self, age):
        super().__init__(age)

    # Defining a speak() method for Cat
    def speak(self):
        print("Meow")

h = Human(25, "Software Developer")
# Calling the speak() method
h.speak()

c = Cat(7)
# Calling the speak() method
c.speak()
```
Output:
```
Hi
Meow
```

This is only the most basic example of polymorphism. Let us go through two other common usages of polymorphism, **Method Overloading** and **Method Overriding**.

#### Method Overloading
Method overloading is a type of polymorphism where methods have the same name but different parameters. By passing a specific amount and/or data type of arguments, we can invoke the method with the correct configuration.

However, Python does not support method overloading. There is a way around it though, but it is not the same as other languages such as Java or C++. We shall first look at how method overloading is done in Java to understand its concept, then we will look at how we can partially get around it in Python.

Here, we see that we have three methods all named `add`, but they all have different arguments. The first method takes in *two integers* as arguments, the second method takes in *three integers* as arguments, and the third method takes in *two doubles* as arguments. When calling the `add` method, depending on the arguments passed, it will call a specific method matching the argument specifications.

```java
public class overloading {

    static int add(int a, int b) { return a + b; }
    static int add(int a, int b, int c) { return a + b + c; }
    static double add(double a, double b) { return a + b; }

    public static void main(String[] args) {
        System.out.println(add(1, 2));
        System.out.println(add(1, 2, 3));
        System.out.println(add(1.1, 2.2));
    }
}
```
Output:
```
3
6
3.3
```

But as we mentioned, Python does not support method overloading. As of Python 3.4, the way around it is to import `singledispatch` from the `functools` module. If you are working with class methods (which is what this post is about, but it gets a little confusing so we will explain in terms of normal functions), as of Python 3.8, you should import `singledispatchmethod` instead. **Note that this way of overloading only validates the FIRST argument (or the argument right after `self` or `cls` if using `singledispatchmethod`).**  

The `@singledispatch` decorator is placed on the first method which defines what happens if the first argument data type does not match any of the methods. All subsequent methods require a `methodName.register(type)` decorator and the method name can simply be an underscore `_`. This defines what happens when the method name is called with the first argument being the specified type.

```python
from functools import singledispatch

# Case if the method is called with none of the data types specified
@singledispatch
def add(a, b):
    raise NotImplementedError(f"Type {type(a)} + Type {type(b)} is not implemented")

# Case if the method is called with the first argument being an integer
@add.register(int)
def _(a, b):
    print("Running int method: ", end="")
    return a + b

# Case if the method is called with the first argument being a float
@add.register(float)
def _(a, b):
    print("Running float method: ", end="")
    return a + b

print(add(1, 2))  # int, int
print(add(1.1, 2.2))  # float, float
print(add([1, 2], [3, 4]))  # list, list
```
Output:
```
Running int method: 3
Running float method: 3.3
NotImplementedError: Type <class 'list'> + Type <class 'list'> is not implemented
```

Not only does this way of method overloading only validates the first argument, but it also **does not work with a varying number of arguments.** If you really need to work with a varying number of arguments, the best option is likely to just use `*args`.

For more information about using `@singledispatch`, you may take a look at its [documentation](https://docs.python.org/3/library/functools.html#functools.singledispatch).

#### Method Overriding
Method overriding is another type of polymorphism where a child class can override a method of the parent class with the same name.

Going back to our OOP example, a lot of animals can speak, so let us add the `speak()` method to the parent class. Let's also add another class for `Sloth`, which makes no sound. The `speak()` method in the parent class will be the default behavior when the animal cannot speak. The `Human` and `Cat` classes will have their own `speak()` method that overrides the parent method.

```python
class Animal:
    def __init__(self, age):
        self.age = age

    # Defining a speak() method at the parent class
    def speak(self):
        print("This animal can't speak")

class Human(Animal):
    def __init__(self, age, job=None):
        super().__init__(age)
        self.job = job

    # Overriding the method in the parent class
    def speak(self):
        print("Hi")

class Cat(Animal):
    def __init__(self, age):
        super().__init__(age)

    # Overriding the method in the parent class
    def speak(self):
        print("Meow")

# Defining an animal that cannot speak
class Sloth(Animal):
    def __init__(self, age):
        super().__init__(age)

h = Human(25, "Software Developer")
h.speak()

c = Cat(7)
c.speak()

s = Sloth(13)
s.speak()
```
Output:
```
Hi
Meow
This animal can't speak
```

## Encapsulation
Encapsulation is a way to restrict access to some aspects of a class from the outside. To put it simply, properties of a class may only be accessed by the class itself and sometimes its child classes, but not in the main program.

There are three different types of access: **public**, **protected**, and **private**.

In Java and C++, specifying the access scope of a class property is straightforward. You simply use the `public`, `protected`, or `private` keywords when declaring a variable or method. In Python, however, you would use underscores `_` in front to indicate the access scope of a class property. One underscore for protected (e.g., `_age`) and two underscores for private (e.g., `__dna`).

#### Protected Member
A protected member is declared with one underscore in front of the variable name (e.g., `_age`). It can only be accessed within its class and all of its sub-classes (child classes), but not outside the class. However, it is not enforced, that is to say, you will not get an error if you try accessing the protected variable outside.

```python
class Animal:
    def __init__(self, age):
        # Declare the age attribute to be protected
        self._age = age
        # Able to access within the class
        print("Inside the class:", self._age)

class Human(Animal):
    def __init__(self, age, job=None):
        super().__init__(age)
        self.job = job
        # Able to access within the sub-class
        print("Inside the sub-class:", self._age)

h = Human(25, "Software Developer")
# Able to (but not supposed to) access outside
print("Outside (not supposed to be accessed, but not enforced):", h._age)
```
Output:
```
Inside the class: 25
Inside the sub-class: 25
Outside (not supposed to be accessed, but not enforced): 25
```

#### Private Member
A private member is declared with two underscores in front of the variable name (e.g., `__dna`). It can only be accessed within its class, but not any sub-classes or outside the class. Python does enforce this by throwing an `AttributeError` when trying to access a private member outside of its class.

```python
class Animal:
    def __init__(self, age, dna):
        self._age = age
        # Declare the dna attribute to be private
        self.__dna = dna
        # Able to access within the class
        print("Inside the class:", self.__dna)

class Human(Animal):
    def __init__(self, age, dna, job=None):
        super().__init__(age, dna)
        self.job = job
        # Error when accessing within the sub-class
        print("Inside the sub-class:", self.__dna)

h = Human(25, "ATCGGTACTA", "Software Developer")
# Error when accessing outside
print("Outside:", h.__dna)
```
Output:
```
Inside the class: ATCGGTACTA
AttributeError: 'Human' object has no attribute '_Human__dna'
AttributeError: 'Human' object has no attribute '__dna'
```

Note that when the program experienced an error inside the sub-class, it did not keep running the script, but the above output is simply to illustrate what would happen if it kept running. And as you can see, the private attribute is not able to be accessed in a sub-class or on the outside.

The proper way to access private (or protected) attributes outside their restricted scope would be to create a public getter method that returns the value of the attributes, and a public setter method if you wish to change the values of these restricted attributes. For example:

```python
# ...Skipping the Animal class declaration
    def get_dna(self):
        return self.__dna

print("Outside:" h.get_dna())
```
Output:
```
Outside: ATCGGTACTA
```

Another way to access private attributes outside of their restricted scope, which I don't necessarily recommend as it defeats the purpose of encapsulation, is to use name mangling. You can access private attributes directly by calling `_className__attributeName`, one underscore in front of the class name, followed by two underscores and the attribute name.

```python
print("Outside:", h._Animal__dna)
```
Output:
```
Outside: ATCGGTACTA
```

## Abstraction
Abstraction is probably the OOP principle that is the most difficult to understand, as its concept is quite abstract (pun intended). Abstraction is when we declare the functionality of a class, but we do not define and implement it. An analogy would be the functionality of a washing machine. We know that its function is to wash your garment clean, but how it accomplishes that, the intricacies of different washing procedures, are unknown to the user. It is up to the manufacturer to define how it accomplishes the function of "washing garments", but abstractly we all understand its purpose.

In Java and C++, we have a keyword for declaring abstract methods. We would use `abstract` in Java and we would use `virtual` in C++. However, Python does not have native capabilities for doing abstraction, as such, we will have to import the `ABC` and `abstractmethod` from the `abc` module. The `abc` stands for Abstract Base Classes.

With our abstract `WashingMachine` class, we need to inherit the `ABC` class to declare that we will be using the attributes and methods from the `ABC` class. On the abstract method `wash`, we need to put an `@abstractmethod` decorator to indicate that this method is an abstract method. Then, we define two child classes of `WashingMachine` that have their own unique implementation of the `wash` method. It is important to note that when we define a child class of an abstract parent class, all abstract methods must be defined and implemented in the child class, or a `TypeError` will be thrown. This will be illustrated in the example below with a third child class.

Let us implement the washing machine example.

```python
from abc import ABC, abstractmethod

# Define the abstract class
class WashingMachine(ABC):
    # Declare the abstract method
    @abstractmethod
    def wash(self):
        pass

# Inherit the abstract parent class
class Samsung(WashingMachine):
    # Define the abstract method
    def wash(self):
        print("1. Flush with 100 degrees F water\n"
              "2. Add detergents with 160 degrees F water\n"
              "3. Bleach\n"
              "4. Rinse\n")

# Inherit the abstract parent class
class LG(WashingMachine):
    # Define the abstract method
    def wash(self):
        print("1. Add detergents with 140 degrees F water\n"
              "2. Soak for 15 minutes with 160 degrees F water\n"
              "3. Rinse\n"
              "4. Soak for 10 minutes with 100 degrees F water\n"
              "5. Rinse\n")

# Inherit the abstract parent class
class Whirlpool(WashingMachine):
    # Not defining the abstract method
    # This will lead to an error when instantiated
    pass

w1 = Samsung()
w1.wash()

w2 = LG()
w2.wash()

w3 = Whirlpool()  # Error when instantiated
```
Output:
```
1. Flush with 100 degrees F water
2. Add detergents with 160 degrees F water
3. Bleach
4. Rinse

1. Add detergents with 140 degrees F water
2. Soak for 15 minutes with 160 degrees F water
3. Rinse
4. Soak for 10 minutes with 100 degrees F water
5. Rinse

TypeError: Can't instantiate abstract class Whirlpool with abstract method wash
```

To summarize abstraction, an abstract method is a method that is declared but not defined. When a class has an abstract method, it is considered an abstract class. When you define a child class that inherits an abstract parent class, you must define all the abstract methods or a `TypeError` will be thrown.
