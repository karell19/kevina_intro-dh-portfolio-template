---
layout: page
title: Week 3 Python Basics I, Crash Course Exercises
description: This notebook contains my attempts of Chapter 2's Python exercises.
---

## Week 3: Python Basics I, Crash Course Exercises

Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Professor Saxton

#### Exercise 2-1. Simple Message:


```python
message1 = "Hello, world!"
print(message1)
```

    Hello, world!


#### Exercise 2-2. Simple Messages:


```python
message2 = "I am very excited to see what I learn in CLS-0161."
print(message2)

message2 = "I wonder what the Digital Humanities are all about..."
print(message2)
```

    I am very excited to see what I learn in CLS-0161.
    I wonder what the Digital Humanities are all about...


#### Exercise 2-3. Personal Message:


```python
f_name = "ElIzAbEtH"
print("Hello " + f_name.title() + ", would you like to learn some Python today?")
```

    Hello Elizabeth, would you like to learn some Python today?


#### Exercise 2-4. Name Cases:


```python
f_name = "GeOrGe"
l_name = "WaShInGtOn"
full_name = f_name + " " + l_name

print("President " + full_name.lower())
print("President " + full_name.upper())
print("President " + full_name.title())
```

    President george washington
    President GEORGE WASHINGTON
    President George Washington


#### Exercise 2-5. Famous Quote:


```python
print('Eleanor Roosevelt once said, "The future belongs to those who believe in the beauty of their dreams."')
```

    Eleanor Roosevelt once said, "The future belongs to those who believe in the beauty of their dreams."


#### Exercise 2-6. Famous Quote 2:


```python
famous_person = "ElEaNor RoOsEvElT"
message = "The future belongs to those who believe in the beauty of their dreams"

print(famous_person.title() + ' once said, "' + message + '."')
```

    Eleanor Roosevelt once said, "The future belongs to those who believe in the beauty of their dreams."


#### Exercise 2-7. Stripping Names:


```python
person = "\tKeViN\n"
person = person.title()

print(person)
print(person.lstrip())
print(person.rstrip())
print(person.strip())
```

    	Kevin
    
    Kevin
    
    	Kevin
    Kevin


#### Exercise 2-8. Number Eight:
* Is 24 / 3 = 8.0 to be expected?
* How can the .0 be omitted?


```python
print(4 + 4)
print(100 - 92)
print(8 * 1)
print(24 / 3)
```

    8
    8
    8
    8.0


#### Exercise 2-9. Favorite Number:


```python
fav_num = 19

print("Hey, did you know my favorite number is " + str(fav_num) + "?")
```

    Hey, did you know my favorite number is 19?


#### Exercise 2-10. Adding Comments:
* Commenting on Exercise 2-7's Code.


```python
# Initialization of variable of string type, holding a person's name.
person = "\tKeViN\n"

# Ensures that the input person's name is formatted in titular format.
person = person.title()

# Printing of person's name, with whitespace omitted in different ways.
print(person)
print(person.lstrip())
print(person.rstrip())
print(person.strip())

# Program takes in a person's name, prints it in titular format, removing whitespace through different methods.
```

    	Kevin
    
    Kevin
    
    	Kevin
    Kevin

