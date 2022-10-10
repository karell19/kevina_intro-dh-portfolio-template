---
layout: page
title: Python Basics II, Crash Course Exercises Ch. 04
description: This notebook contains my attempts of Chapter 04's Python exercises.
---

### Chapter 4 - Working with Lists: Practice Exercises

Kevin Arellano

CLS-0161 -- Introduction to Digital Humanities

Professor Saxton

#### Looping Through an Entire List
* for loop.

#### A Closer Look at Looping
* for cat in cats:

    singular, plural is generally ideal.
    
#### Avoiding Indentation Errors
* Indentation used to determine relation between a series of lines of code to each-other, and to the program overall.

#### Forgetting to Indent
* When a for loop iterates through a list, when the for loop ends, the initialized iterator will hold a final value derived with the for loop's final iteration.
    * Thus, we can still access the iterator's value, however, it will be the lastly iterated value.

#### Indenting Unnecessarily
* Unneeded indents give a line of code relation to other lines of code where they do not exist.

#### Forgetting the Colon
* Colons tell Python to interpret the following indented line as part of a for loop.

#### 4-1. Pizzas:


```python
pizzas = ['cheese', 'pepperoni', 'buffalo-chicken']
```


```python
for pizza in pizzas:
    print(f'I like {pizza} pizza!')
    
print('I just love pizza so much...')
```

    I like cheese pizza!
    I like pepperoni pizza!
    I like buffalo-chicken pizza!
    I just love pizza so much...


#### 4-2. Animals:


```python
mammals = ['human', 'monkey', 'dolphin']
```


```python
for mammal in mammals:
    print(f'We learned about {mammal} anatomy in class today.')

print('All of these animals are mammals and vertebrates!')
```

    We learned about human anatomy in class today.
    We learned about monkey anatomy in class today.
    We learned about dolphin anatomy in class today.
    All of these animals are mammals and vertebrates!


#### Making Numerical Lists
* Python has many tools for working with lists of numbers.
#### Using the range( ) Function
* Use range() to print series of numbers:

    ##### # Prints 1-4, excluding 5.
    for value in range(1, 5):
        print(value)

* Use range() to begin to sequence numbers from 0:

    ##### # Prints 0-5, beginning to sequence from 0-5, excluding 6.
    for value in range(6):
        print(value)

#### Using range( ) to Make a List of Numbers
* numbers = list(range(1, 6))

    Composes a list, titled numbers, of numbers from 1-5.
    
    
* even_numbers = list(range(2, 11, x)

    Makes a list, titled even_numbers, incrementing by x while sequencing from 2 until it reaches/passes 11. Therefore, captures only even numbers.
    
    = [2, 4, 6, 8, 10]

#### Simple Statistics with a List of Numbers
* min(list_num)

    Gives minimum number value in a list.
    

* max(list_num)

    Gives maximum number value in a list.
    
    
* sum(list_num)
    
    Gives the sum of all values in a list.
    
    
* These statistic functions work rapidly, even on large lists of numbers.

#### List Comprehensions
* List Comprehensions combine for loops and creation of new elements into one line, automatically appending each new element.

    squares = [value**2 for value in range(1, 11)]

#### 4-3. Counting to Twenty:


```python
for value in range(1, 21):
    print(value)
```

    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    11
    12
    13
    14
    15
    16
    17
    18
    19
    20


#### 4-4. One Million:


```python
million_num = list(range(1, 1000001))
```


```python
for num in million_num:
    print(num)
```

    IOPub data rate exceeded.
    The notebook server will temporarily stop sending output
    to the client in order to avoid crashing it.
    To change this limit, set the config variable
    `--NotebookApp.iopub_data_rate_limit`.
    
    Current values:
    NotebookApp.iopub_data_rate_limit=1000000.0 (bytes/sec)
    NotebookApp.rate_limit_window=3.0 (secs)
    


#### 4-5. Summing a Million:


```python
print(f'Min: {min(million_num)}')
print(f'Max: {max(million_num)}')
```

    Min: 1
    Max: 1000000



```python
print(sum(million_num))
```

    500000500000


#### 4-6. Odd Numbers:


```python
odd_num = list(range(1, 21, 2))
```


```python
for num in odd_num:
    print(num)
```

    1
    3
    5
    7
    9
    11
    13
    15
    17
    19


#### 4-7. Threes:


```python
multiples_3 = list(range(3, 31, 3))
```


```python
for num in multiples_3:
    print(num)
```

    3
    6
    9
    12
    15
    18
    21
    24
    27
    30


#### 4-8. Cubes:


```python
cubes = []

for num in range(1, 11):
    cubes.append(num**3)
```


```python
for num in cubes:
    print(num)
```

    1
    8
    27
    64
    125
    216
    343
    512
    729
    1000


#### 4-9. Cube Comprehension:


```python
cubes_comprehension = [num**3 for num in range(1, 11)]
```


```python
for num in cubes_comprehension:
    print(num)
```

    1
    8
    27
    64
    125
    216
    343
    512
    729
    1000


#### Working with Part of a List
* Slice: A specific group of items in a list.

#### Slicing a List
* To slice, specify the index of the first and last elements you intend to work with:

    list[1:4]
    
    Slice with indices 1-3.
    
    
* list[:4]
    
    Instincitvelly begins slice at start of list when beginning index is not specified.
    
    
* list[2:]

    Instinctivelly ends slice at end of list when ending index is not specified.
    
    
* list[-3:]

    Slice including only last three elements of a list.
    

* A third value within the brackets for a slice indicates by how much to increment when sequencing a range of indices.

#### Looping Through a Slice
* for player in players[:3]:
    
    Loop through first three indices in a list.
    
#### Copying a List
* Can make a slice including the entire original list:

    list_1 = ...
    
    list_2 = list_1[:]
    

* Wrong!

    list_1 = ...
    
    list_2 = list_1
    
    Makes each variable point to the same list.
    
#### 4-10. Slices:


```python
print(f'The first three items in the list are: {odd_num[:3]}')
```

    The first three items in the list are: [1, 3, 5]



```python
mid_index = (len(odd_num) / 2)
mid_index = int(mid_index)

print(f'Three items from the middle of the list are: {odd_num[mid_index:(mid_index + 3)]}')
```

    Three items from the middle of the list are: [11, 13, 15]



```python
print(f'The last three items in the list are: {odd_num[-3:]}')
```

    The last three items in the list are: [15, 17, 19]


#### 4-11. My Pizzas, Your Pizzas:


```python
friend_pizzas = pizzas[:]

pizzas.append('hawaiian')
friend_pizzas.append('deep-dish')
```


```python
print('My favorite pizzas are: ', end = "")

for pizza in pizzas:
    if (pizza == pizzas[-1]):
        print(f'and {pizza}.')
    else:
        print(f'{pizza}, ', end = "")
```

    My favorite pizzas are: cheese, pepperoni, buffalo-chicken, and hawaiian.



```python
print("My friend's favorite pizzas are: ", end = "")

for pizza in friend_pizzas:
    if (pizza == friend_pizzas[-1]):
        print(f'and {pizza}.')
    else:
        print(f'{pizza}, ', end = "")
```

    My friend's favorite pizzas are: cheese, pepperoni, buffalo-chicken, and deep-dish.


#### 4-12. More Loops:


```python
my_foods = ['pizza', 'falafel', 'carrot cake']
friend_foods = my_foods[:]
```


```python
my_foods.append('cannoli')
friend_foods.append('ice cream')
```


```python
print('My favorite foods are: ', end = "")

for food in my_foods:
    if (food == my_foods[-1]):
        print(f'and {food}.')
    else:
        print(f'{food}, ', end = "")
```

    My favorite foods are: pizza, falafel, carrot cake, and cannoli.



```python
print("My friend's favorite foods are: ", end = "")

for food in friend_foods:
    if (food == friend_foods[-1]):
        print(f'and {food}.')
    else:
        print(f'{food}, ', end = "")
```

    My friend's favorite foods are: pizza, falafel, carrot cake, and ice cream.


#### Tuples
* Tuple: List of items that cannot and should not be modified, are immutable.

#### Defining a Tuple
* Tuples defined using parentheses and not square brackets.
* Tuple elements accessed like list elements.
* To define a tuple with one element, need presence of a comma as that defines tuples:

    my_tuple = (3,)
    
#### Looping Through All Values in a Tuple
* Can use loops to iterate through tuples, literally very similar to a list.

#### Writing over a Tuple
* While tuples cannot be modified, can assign new values to variables representing a tuple.

#### 4-13. Buffet:


```python
menu = ('scrambled eggs', 'hashbrowns', 'waffles', 'pancakes', 'bacon')
```


```python

```


```python
print('This restaurant offers: ', end = "")

for item in menu:
    if (item == menu[-1]):
        print(f'and {item}.')
    else:
        print(f'{item}, ', end = "")
```

    This restaurant offers: scrambled eggs, hashbrowns, waffles, pancakes, and bacon.



```python
menu[2] = 'turkey'
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Input In [34], in <cell line: 1>()
    ----> 1 menu[2] = 'turkey'


    TypeError: 'tuple' object does not support item assignment



```python
menu = ('scrambled eggs', 'potato triangles', 'waffles', 'pancakes', 'turkey')
```


```python
print('In November, the restaurant will instead be offering: ', end = "")

for item in menu:
    if (item == menu[-1]):
        print(f'and {item}.')
    else:
        print(f'{item}, ', end = "")
```

    In November, the restaurant will instead be offering: scrambled eggs, potato triangles, waffles, pancakes, and turkey.


#### Styling Your Code
* Important to write code that can be read!
* Comments limited to 72 characters per line.
