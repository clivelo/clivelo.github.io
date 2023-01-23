---
title: 5 Obscure but handy string methods in Python
tags: [Coding, Python]
style: fill
color: info
description: Here are five string methods that I personally have found incredibly useful but most people have likely overlooked.
---

As of Python 3.11, there are 47 built-in string methods. For most programmers, a lot of these methods are rarely or never used. In this post, I will go through five string methods that I personally have found incredibly useful but most people have likely overlooked. Perhaps you already knew all these methods, but for those who don't, I hope you could learn something new.

## 1. str.join
```
str.join(iterable)

iterable: Required. Any iterable object where all the elements are strings
```
One of the most useful string methods, the `join` method returns a string that is the **concatenation of the iterable**. The separator between each element of the iterable is the string that is calling the method.

Let's say we have a list of words and we would like to join them together to form a sentence.

```python
s = ["I", "enjoy", "coding", "in", "Python", "very", "much"]

# Join the list of words with a space
print(" ".join(s))
```
```
I enjoy coding in Python very much
```

This method is particularly useful in cases where you used the `split` method to split the string into a list, then after some processing you want to join them back again. It is also useful for creating CSV files when you want to join columns of data with a delimiter.

```python
data = [["Tim", "62", "68", "82", "75"],
        ["Jack", "87", "88", "62", "85"],
        ["Ada", "78", "69", "96", "94"]]

s = ""
# Join each row with a comma-separated delimiter
for row in data:
    s += ",".join(row)
    s += "\n"

print(s)
```
```
Tim,62,68,82,75
Jack,87,88,62,85
Ada,78,69,96,94
```

## 2. str.ljust
```
str.ljust(width, [fillchar])

width: Required. The length of the returned string
fillchar: Optional. A character to fill the missing space. Default is space
```
The `ljust` method is extremely useful for formatting your output and then printing it to your console, especially when you are printing a table-like structure. Essentially, it **left-justifies your string** and **pads a number of characters on the right**. The `width` parameter specifies the total width **including the string** (e.g., if the `len()` of your string is 5 and your `width` is 10, it will pad 5 characters after your string). The `fillchar` parameter is optional which specifies what character to use as the padding, and default as a space character.

```python
header = ["Name", "Country", "1st Score", "2nd Score"]
data = [["Tim", "Canada", "62", "68"],
        ["Jack", "Malaysia", "87", "88"],
        ["Ada", "Peru", "78", "69"]]
wid = 12

# Print each element with an ljust gap
print(f"{header[0].ljust(wid)}{header[1].ljust(wid)}{header[2].ljust(wid)}{header[3].ljust(wid)}")
for row in data:
    print(f"{row[0].ljust(wid)}{row[1].ljust(wid)}{row[2].ljust(wid)}{row[3].ljust(wid)}")
```
```
Name        Country     1st Score   2nd Score   
Tim         Canada      62          68          
Jack        Malaysia    87          88          
Ada         Peru        78          69    
```

What I've done above is obviously a little too messy, but you should be able to clearly see what is happening with the `ljust` method. A better way may be to form a string using a loop and the `join` method and then print the whole string.

```python
header = ["Name", "Country", "1st Score", "2nd Score"]
data = [["Tim", "Canada", "62", "68"],
        ["Jack", "Malaysia", "87", "88"],
        ["Ada", "Peru", "78", "69"]]
wid = 12

# Forming the whole line at once by inserting the gaps for each element in the list
# Then joining the list of ljust strings
print("".join([h.ljust(wid) for h in header]))
for row in data:
    print("".join([r.ljust(wid) for r in row]))
```
```
Name        Country     1st Score   2nd Score   
Tim         Canada      62          68          
Jack        Malaysia    87          88          
Ada         Peru        78          69    
```
I highly recommend you to play around with this method. For a challenge, try to dynamically determine the optimal gap for each column and print the table with the computed gap widths.

## 3. str.zfill
```
str.zfill(width)

width: Required. The length of the returned string
```
The `zfill` method is a great way to **add leading zeros to your string**. The `width` parameter determines the total width of the string including the leading zeros. If your string has a length of 3 and you specify the `width` to be 5, it would add 2 zeros in front. Note that if your string has a minus sign in front, it will include the minus sign for the width, and the zeroes will be pad *after* the minus sign.

I find this method useful when I want a consistent length of file names and I'm exporting loads of files to my filesystem (e.g., 00001.txt, 00002.txt, 00003.txt, etc). Or if I need consistent length in a column of my data.

```python
for i in range(2000):
    # Convert the integer i into a string, then pad zeros in front of it.
    fname = str(i).zfill(5) + ".txt"
    print(fname)
```
```
00000.txt
00001.txt
00002.txt
00003.txt
00004.txt
…
```

## 4. str.maketrans
```
static str.maketrans(x, [y, [z]])

x: Required. If only one parameter is specified, this has to be a dictionary. If two or more parameters are specified, this parameter has to be a string
y: Optional. A string with the same length as parameter x
z: Optional. A string describing which characters to remove from the original string
```
The `maketrans` method is a static method that has one required parameter and two optional parameters. This method returns an ASCII **mapping table (dictionary) of what character(s) should be replaced with what character(s)**.

If there is only one parameter, it should be a dictionary with key/value pair(s) that specifies what character(s) should be replaced with what character(s). If there are two or more parameters, `x` should be a string that specifies what character(s) should be replaced, whereas `y` should be a string with the same length as `x` that specifies what each character in `x` should be replaced with. `z` is another optional parameter that describes what character(s) to completely remove from the string.

```python
txt = "Lay Amongst US"

# Replace "a" with "i"; replace "y" with "e"
x = "ay"
y = "ie"

# Remove "s" and "t"
z = "st"

# Create a mapping table
map_table = str.maketrans(x, y, z)
print(map_table)
```
```
{97: 105, 121: 101, 115: None, 116: None}
```

This creates a mapping table of ASCII values. ASCII of 97 ("a") is replaced by ASCII of 105 ("i"); ASCII of 121 ("y") is replaced by ASCII of 101 ("e"); ASCII of 115 ("s") is replaced by nothing (i.e., removed); and ASCII of 116 ("t") is replaced by nothing (i.e., removed).

To actually do the replacement on the string based on the mapping table, we will need the `str.translate` method next on the list.

## 5. str.translate
```
str.translate(table)

table: Required. A dictionary or mapping table of characters to be replaced
```
The `translate` method takes a dictionary or a mapping table that specifies what characters are to be replaced from the string. The easiest way to create this mapping table is to use the `maketrans` method detailed above. This method returns **a new string in which each character has been mapped through the table**.

```python
txt = "Lay Amongst US"

# Replace "a" with "i"; replace "y" with "e"
x = "ay"
y = "ie"

# Remove "s" and "t"
z = "st"

# Create a mapping table
map_table = str.maketrans(x, y, z)
print(txt)
print(txt.translate(map_table))
```
```
Lay Amongst US
Lie Among US
```

One common usage of the `maketrans` and the `translate` methods that I have encountered is to remove all punctuations from a string.

```python
# Import string module to get a string of punctuations
import string

txt = "Lorem ipsum dolor sit amet, consectetur adipiscing elit."

# Replace nothing with nothing, and remove all punctuations
map_table = str.maketrans("", "", string.punctuation)
print(string.punctuation)
print(txt)
print(txt.translate(map_table))
```
```
!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Lorem ipsum dolor sit amet consectetur adipiscing elit
```

## Conclusion
Hope you learned some new string methods from this post. Happy coding!
