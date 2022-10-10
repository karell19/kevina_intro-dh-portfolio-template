---
layout: page
title: Python Basics II, Crash Course Exercises Ch. 03
description: This notebook contains my attempts of Chapter 03's Python exercises.
---

### Chapter 3 - Introducing Lists: Practice Exercises

Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Professor Saxton

#### What is a List?
* List = Collection of items in a particular order.
    * Indicated by ([]), with individual elements seperated by commas.

#### Accessing Elements in a List
* List elements are accessed through 0-based indices.
    * list[-1] = Accesses list's last element.
    * list[-2] = Accesses list's second-to-last element.
    * ... etc.
    
#### Using Individual Values from a List
* Deriving elements from lists using indices treats them like a variable, and we can then use them in f-strings for printing their value.

#### Exercise 3-1. Names:


```python
names = ['aleX', 'KathY', 'JASMIn', 'gEnEvieve', 'ViViana']

for name in names:
    print(name.title())
```

    Alex
    Kathy
    Jasmin
    Genevieve
    Viviana


#### Exercise 3-2. Greetings:


```python
for name in names:
    print(f'You, {name.title()}, are my best friend.')
```

    You, Alex, are my best friend.
    You, Kathy, are my best friend.
    You, Jasmin, are my best friend.
    You, Genevieve, are my best friend.
    You, Viviana, are my best friend.


#### Exercise 3-3. Your Own List:


```python
train_lines = ['LA Metro', 'MTA', 'MBTA', 'L']

for line in train_lines:
    print(f'Sorry, I gotta catch the {line}!')
```

    Sorry, I gotta catch the LA Metro!
    Sorry, I gotta catch the MTA!
    Sorry, I gotta catch the MBTA!
    Sorry, I gotta catch the L!


#### Changing, Adding, and Removing Elements
* Most lists will be dynamic, with the addition and/or removal of elements throughout program's course.

#### Modifying Elements in a List
* To change element:

  list[i] = ...
  
#### Appending Elements to the End of a List
* list.append('...')

* To make an empty list:

    list = []

#### Inserting Elements into a List
* list.insert(index, '...')

    Places element at such index, then moves each element to the right of it, one more index to the right.
    
#### Removing an Item Using the del Statement
* del list[index]

    For when position in list is known.

#### Removing an Item Using the pop( ) Method
* list.pop()

    Removes, yet allows user to continue utilizing, the last element of a list.

* list.pop(index)

    Removes, yet allows user to continue utilizing, elements from a list.
    
#### Removing an Item by Value
* list.remove('...')

    For when index of value to-be removed is not known, so we remove by value.

    Deletes only first occurence of value.
    
#### 3-4. Guest List:


```python
invitees = ['Taylor Swift', 'Beyonce', 'Shakira']

for invitee in invitees:
    print(f'Dear {invitee.title()}, you are cordially invited to dinner!')
    
print(f'---\n{invitees[0].title()} cannot make it to dinner...')
```

    Dear Taylor Swift, you are cordially invited to dinner!
    Dear Beyonce, you are cordially invited to dinner!
    Dear Shakira, you are cordially invited to dinner!
    ---
    Taylor Swift cannot make it to dinner...


#### 3-5. Changing Guest List:


```python
invitees.remove(invitees[0])
invitees.insert(0, 'Whitney Houston')

for invitee in invitees:
    print(f'Dear {invitee.title()}, you are cordially invited to dinner')
    
print('---\nGreat news, we found a bigger dinner table!')
```

    Dear Whitney Houston, you are cordially invited to dinner
    Dear Beyonce, you are cordially invited to dinner
    Dear Shakira, you are cordially invited to dinner
    ---
    Great news, we found a bigger dinner table!


#### 3-6. More Guests:


```python
waitlist = ['Adele', 'Rihanna', 'Lady Gaga']

invitees.insert(0, waitlist[0])
invitees.insert((int(len(waitlist) / 2)), waitlist[1])
invitees.append(waitlist[2])
```


```python
for invitee in invitees:
    print(f'Dear {invitee.title()}, you are cordially invited to dinner')
    
print('---\nOh no... I can only invite two people to dinner.')
```

    Dear Adele, you are cordially invited to dinner
    Dear Rihanna, you are cordially invited to dinner
    Dear Whitney Houston, you are cordially invited to dinner
    Dear Beyonce, you are cordially invited to dinner
    Dear Shakira, you are cordially invited to dinner
    Dear Lady Gaga, you are cordially invited to dinner
    ---
    Oh no... I can only invite two people to dinner.


#### 3-7. Shrinking Guest List:


```python
while (len(invitees) > 2):
    uninvited = invitees.pop()
    print(f'{uninvited}, I am so sorry, but I cannot invite you to dinner.')

print('---')
    
for invitee in invitees:
    print(f'{invitee.title()}, you are still invited!')
```

    Lady Gaga, I am so sorry, but I cannot invite you to dinner.
    Shakira, I am so sorry, but I cannot invite you to dinner.
    Beyonce, I am so sorry, but I cannot invite you to dinner.
    Whitney Houston, I am so sorry, but I cannot invite you to dinner.
    ---
    Adele, you are still invited!
    Rihanna, you are still invited!



```python
while (len(invitees) > 0):
    del invitees[0]

for invitee in invitees:
    print(f'{invitee.title()} is still on this list.')
```

#### Organizing a List
* Lists may be built in unpredictable orders.
* Methods for list organization in certain orders are possible.

#### Sorting a List Permanently with the sort( ) Method
* list.sort()

    Permanently sorts list alphabetically, cannot be reverted to original order.
    
* list.sort(reverse = True)

    sort() method, sorts reverse alphabetically, and permanently so.
    
#### Sorting a List Temporarily with the sorted( ) Function
* sorted(list)

    Allows for temporary display/use of a sorted list. Actual list not affected in any way.

* sorted(list, reverse = True)

    Temporary sorted() method, reverse alphabetically.

* Sorting capital and lower-case letters is a tad-bit more complicated, probably, perhaps, maybe, because of ASCII values?

#### Printing a list in Reverse Order
* list.reverse()

    Permanently reverses the order of a list, although a second use of reverse() would return it back to normal.
    
#### Finding the Length of a List
* len(list)

    Gives the length value of a list. 
    
#### 3-8. Seeing the World:


```python
to_visit = ['paris', 'mexico city', 'new york city', 'great barrier reef', 'london']
```


```python
to_visit
```




    ['paris', 'mexico city', 'new york city', 'great barrier reef', 'london']




```python
for place in sorted(to_visit):
    print(place)
```

    great barrier reef
    london
    mexico city
    new york city
    paris



```python
to_visit
```




    ['paris', 'mexico city', 'new york city', 'great barrier reef', 'london']




```python
for place in sorted(to_visit, reverse = True):
    print(place)
```

    paris
    new york city
    mexico city
    london
    great barrier reef



```python
to_visit
```




    ['paris', 'mexico city', 'new york city', 'great barrier reef', 'london']




```python
to_visit.reverse()
to_visit
```




    ['london', 'great barrier reef', 'new york city', 'mexico city', 'paris']




```python
to_visit.reverse()
to_visit
```




    ['paris', 'mexico city', 'new york city', 'great barrier reef', 'london']




```python
to_visit.sort()
to_visit
```




    ['great barrier reef', 'london', 'mexico city', 'new york city', 'paris']




```python
to_visit.sort(reverse = True)
to_visit
```




    ['paris', 'new york city', 'mexico city', 'london', 'great barrier reef']



#### 3-9. Dinner Guests:


```python
if (len(invitees) != 1):
    print(f'I am inviting {len(invitees)} guests to dinner.')
else:
    print(f'I am inviting {len(invitees)} guest to dinner.')
```

    I am inviting 0 guests to dinner.


#### Avoiding Index Errors When Working with Lists
* Attempting to acces non-existent indices.
* Requesting -1th item from an empty list

#### 3-11. Intentional Error:


```python
out_of_range = [1, 2]
# print(out_of_range[2])
print(out_of_range[1])
```

    2

