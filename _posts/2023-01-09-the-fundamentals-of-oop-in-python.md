---
title: The fundamentals of Object-Oriented Programming in Python
tags: [Coding, Python]
style: fill
color: info
description: Object-oriented programming (OOP) is a programming design based on the concepts of real-life objects. This paradigm allows programmers to think about software development akin to working with real-life objects.
---

Object-oriented programming (OOP) is a programming design based on the concepts of real-life objects. Objects have data (known as **attributes** or **properties**) and functions (known as **methods**). This paradigm allows programmers to think about software development akin to working with real-life objects.

For example, a mobile phone is an object that has **attributes** such as brand, model, color, volume, and size. It also has **functions** like powering on, powering off, increasing volume, and decreasing volume. All mobile phones will likely possess all of these attributes and functions, but they may differ in the state of these attributes and how these functions behave. Incidentally, each mobile phone object shares a common archetype, but they are unique from each other.

## Terminology
Before we start explaining how to use OOP in Python, let's first define some basic terminologies that will be used throughout this post.

#### Class
Classes are the **blueprint or structure** of the object. Mobile phone could be a class, person could be a class, cat could be a class. When we write a class, we are defining what properties and functions this object should possess and how it should behave.

#### Object Instance
An object instance is a **specific occurrence** of a class. My mobile phone and your mobile phone share the same attributes and functions (as in they both have a brand, a model, a color, a size, can be turned on, and can be turned off), but they are not the same phone. They are two different object instances.

#### Instantiation
Instantiation is when you create a new instance of an object.

#### Attribute
Attributes are data that represent different **features or properties** of a class or an object. A mobile phone has attributes such as brand, model, color, size, etc. A person has attributes such as name, age, height, weight, etc.

#### Method
Methods are **functions and behaviors** that an object can perform. A mobile phone can be turned on and turned off, whereas A person can walk, talk, and eat, just to name a few.

#### Declaration
Declaration of a class or a method means we are declaring that they exist, which includes declaring the **name** of the class or method and the **arguments** it accepts.

#### Definition
Definition of a class or a method means we are defining/writing the **logic and behavior**.

## Motivation
Why do we need OOP? What advantage does it have over **procedural programming**?

Let us assume we want to create a university that stores all the info about its students. Let's say the university has 500 students and we want to store the students' names, ages, majors, GPAs of each year, cumulative GPAs, and more.

```python
# Creating each student attribute in a list
names = ["Tom", "Anna", "James", ..., "Yvonne"]
ages = [21, 19, 23, ..., 21]
majors = ["Computer Science", "Physics", "English Literature", ..., "Physics"]
yr1_sem1_gpa = [2.7, 3.3, 3.2, ..., 3.8]
yr1_sem2_gpa = [3.0, 3.7, 3.6, ..., 3.7]
yr2_sem1_gpa = [2.9, 3.3, None, ..., 3.9]
yr2_sem2_gpa = [3.3, 3.5, None, ..., None]
...
cgpa = [3.2, 3.5, 3.4, ..., 3.8]
```

This is looking very complicated and dangerous.

What if we wish to access a particular student's GPA? We will have to know what index that student belongs to and grab the correct element from the GPA list.

What if we want to delete a student because they dropped out of school? We have to delete every single entry of that student from all the lists, and make sure the index of that student is correct across all lists.

What if a student is only in their first year and they don't have a GPA for the second to fourth years yet? We must add a placeholder GPA for those semesters or the indexing for other students will be wrong.

What if a semester just ended and we have to update the GPA for all students? We have to carefully determine which semester the student is in, and update the GPA in that list. We also have to update the cGPA accordingly.

Storing data this way, there are just so many things that could go wrong if we are not careful. By using OOP, we create a blueprint of a student, then we can simply make 500 instances of students easily. Everything about a student is stored within a single object variable.

```python
# Assuming that we have declared the Student class
# The variables student1 to student500 are object instances
students = [student1, student2, student3, ..., student500]
```

## Creating a class
Let's finally declare a class that represents students in a university. To declare a class, we begin with the `class` keyword. Class names are usually title case by convention. For now, we will use `pass` to skip the definition of the class.

Let us also instantiate three students in the main program. Right now, since we have yet to define what a student is like, these three students are very barebones, but they are still different object instances of the `Student` class.

``` python
# Declaring a class and skipping the implementation for now
class Student:
    pass

# Instantiating 3 student object instances
s1 = Student()
s2 = Student()
s3 = Student()
```

## Constructor
Now that we have declared a `Student` class and instantiated three students, we want the students to have the following attributes: `name`, `age`, and `major`. Importantly, we want these attributes to be initialized **from the beginning**. Here is where we would use a **constructor**.

A constructor is a method that gets called **automatically** right when an object is instantiated. In Python, to define the constructor, we would define the `__init__()` method inside the class.

To pass in the `name`, `age`, and `major` attributes right from the start, our `__init__()` method will need to accept these variables as arguments. Also, we have to include those variables when instantiating an object. Note below that we added a `self` variable as the first argument of the `__init__()` method, this will be explained in the next section. Now, the students finally have some attributes!

```python
class Student:
    # Constructor method that gets executed upon object instantiation
    def __init__(self, name, age, major):
        pass

# Passing arguments to constructor
s1 = Student("Tom", 21, "Computer Science")
s2 = Student("Anna", 19, "Physics")
s3 = Student("James", 23, "English Literature")
```

## The `self` keyword
We lied in our previous section, our `Student` class now does not have any attributes yet. When instantiating the students, we passed the name, age, and major into the class, but we have yet to assign those variables to the object. This is where the `self` keyword is crucial, and you will be seeing this keyword a lot when working with classes, so better get used to it!

The `self` keyword refers to the specific object instance itself. When we type, for example `self.name`, what we are referring to is the name of **this particular object instance** (fun fact, Java uses the `this` keyword instead). After passing the attributes into the `__init__()` method, we have to assign those arguments to itself.

Once we have those attributes, we can get the unique attributes from each student in the main program by doing `objectInstance.Attribute` (e.g., `s1.name` to get the name of object instance `s1`), we can even modify these attributes.

```python
class Student:
    def __init__(self, name, age, major):
        # Assigning arguments to object attributes
        self.name = name
        self.age = age
        self.major = major

s1 = Student("Tom", 21, "Computer Science")
s2 = Student("Anna", 19, "Physics")
s3 = Student("James", 23, "English Literature")

# Accessing object attributes outside the class
print(f"s1's name is {s1.name}, they are {s1.age} years old, they are majoring in {s1.major}.")
print(f"s2's name is {s2.name}, they are {s2.age} years old, they are majoring in {s2.major}.")
print(f"s3's name is {s3.name}, they are {s3.age} years old, they are majoring in {s3.major}.")

# Modifying attribute of student s2
s2.age = 20
print(f"s2's name is {s2.name}, they are {s2.age} years old, they are majoring in {s2.major}.")
```
Output:
```
s1's name is Tom, they are 21 years old, they are majoring in Computer Science.
s2's name is Anna, they are 19 years old, they are majoring in Physics.
s3's name is James, they are 23 years old, they are majoring in English Literature.
s2's name is Anna, they are 20 years old, they are majoring in Physics.
```

We want all students to be able to introduce themselves, so let's create an `introduce()` method in the student class that prints an introduction. Let us also create a `grow()` method that increases the age of the student by 1.

```python
class Student:
    def __init__(self, name, age, major):
        self.name = name
        self.age = age
        self.major = major

    # Defining a method inside the class similar to defining a function
    def introduce(self):
        print(f"My name is {self.name}, I am {self.age} years old, and I am majoring in {self.major}.")

    # Defining a method that modifies object attributes
    def grow(self):
        self.age += 1

s1 = Student("Tom", 21, "Computer Science")
s2 = Student("Anna", 19, "Physics")
s3 = Student("James", 23, "English Literature")

# Calling the methods
s1.introduce()
s2.introduce()
s3.introduce()
s2.grow()
s2.introduce()
```
Output:
```
My name is Tom, I am 21 years old, and I am majoring in Computer Science.
My name is Anna, I am 19 years old, and I am majoring in Physics.
My name is James, I am 23 years old, and I am majoring in English Literature.
My name is Anna, I am 20 years old, and I am majoring in Physics.
```

Notice that we have to include `self` as the first argument when declaring the methods (e.g., `def introduce(self)`), even though we are not passing any values (e.g., `s1.introduce()`). Essentially, the student itself is passed as the first argument. When we are doing `s1.introduce()`, the object instance `s1` is automatically passed into the `self` argument of the `introduce()` method.

## Class attributes
Class attributes are attributes that are not unique to any object instance, all classes regardless of their instance would share these attributes. You can think of it as a constant for the whole class.

Let's assume we wish the students to say their university name when introducing themselves. We could pass in the university name when instantiating each student, but that seems a little redundant when all students are from the same university. In this case, we should define the university name as a class attribute right after the class declaration.

```python
class Student:
    # Defining a class attribute
    school_name = "MIT"

    def __init__(self, name, age, major):
        self.name = name
        self.age = age
        self.major = major

    # Using the class attribute in the introduce method
    def introduce(self):
        print(f"My name is {self.name}, I am {self.age} years old, I am studying at {Student.school_name}, and I am majoring in {self.major}.")

s1 = Student("Tom", 21, "Computer Science")
s2 = Student("Anna", 19, "Physics")
s3 = Student("James", 23, "English Literature")

s1.introduce()
s2.introduce()
s3.introduce()
```
Output:
```
My name is Tom, I am 21 years old, I am studying at MIT, and I am majoring in Computer Science.
My name is Anna, I am 19 years old, I am studying at MIT, and I am majoring in Physics.
My name is James, I am 23 years old, I am studying at MIT, and I am majoring in English Literature.
```

Note that we are calling `Student.school_name` instead of `self.school_name` since the school name is not exclusive or unique to a student instance, using the class name would suffice. Though, doing `self.school_name` would also work.

## Static method and class method
Recall that our `introduce()` method needed to access object attributes through the `self` keyword, which is why we have to include `self` as an argument when declaring the method. But what if the method does not need to access or modify any object attributes?

A method that does not access or modify object or class attributes is called a **static method**. Let us implement a simple `say_hi()` method that just prints `hi` to the console. Since this method does not depend on any attributes, we can declare this method as a **static method** by adding an `@staticmethod` **decorator** above the method declaration, and we will not need to put `self` as an argument. Without going into too much detail, a decorator basically allows us to modify the behavior of a function.

```python
class Student:
    school_name = "MIT"

    def __init__(self, name, age, major):
        self.name = name
        self.age = age
        self.major = major

    def introduce(self):
        print(f"My name is {self.name}, I am {self.age} years old, I am studying at {Student.school_name}, and I am majoring in {self.major}.")

    # Declaring a static method
    @staticmethod
    def say_hi():
        print("hi")

s1 = Student("Tom", 21, "Computer Science")
s2 = Student("Anna", 19, "Physics")
s3 = Student("James", 23, "English Literature")

s1.say_hi()
```
Output:
```
hi
```

Now, what if we were to create a method that does not access or modify any object attributes, but needs to access class attributes only? Then, we would declare a **class method** with the `@classmethod` **decorator**. The class attributes will be passed into the method through the `cls` keyword.

```python
class Student:
    school_name = "MIT"

    def __init__(self, name, age, major):
        self.name = name
        self.age = age
        self.major = major

    def introduce(self):
        print(f"My name is {self.name}, I am {self.age} years old, I am studying at {Student.school_name}, and I am majoring in {self.major}.")

    @staticmethod
    def say_hi():
        print("hi")

    # Declaring a class method with cls as the argument
    @classmethod
    def say_school_name(cls):
        print(f"I am studying at {cls.school_name}.")

s1 = Student("Tom", 21, "Computer Science")
s2 = Student("Anna", 19, "Physics")
s3 = Student("James", 23, "English Literature")

# Calling the class method
# Do not need to pass in any arguments because the object class is automatically
# passed into the method as cls
s1.say_school_name()
```
Output:
```
I am studying at MIT.
```

## What's next?
Now that you have learned the basics of classes and objects, the next step would be to learn [the four principles of OOP](https://clivelo.me/blog/the-four-principles-of-oop-in-python). Happy coding!

