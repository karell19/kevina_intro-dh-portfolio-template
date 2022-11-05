---
layout: page
title: Python Basics III, Crash Course Exercises Ch. 05
description: This notebook contains my attempts of Chapter 05's Python exercises.
---

### Chapter 5 - If Statements
Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanitites

Professor Saxton

#### 5-1. Conditional Tests:


```python
print('Test 1: Does 1 + 1 == 2? I predict True.')
print(f'Test 1 Result: {(1 + 1) == 2}')

print('---')

kevins_college = 'Tufts University'

print('Test 2: Does kevins_college == "UCLA"? I predict False.')
print(f'Test 2 Result: {kevins_college == "UCLA"}')

print('---')

print('Test 3: Does kevins_college != "UCLA"? I predict True.')
print(f'Test 3 Result: {kevins_college != "UCLA"}')

print('---')

curr_year = 2022

print('Test 4: Is curr_year == 2202? I predict False.')
print(f'Test 4 Result: {curr_year == 2202}')

print('---')

print('Test 5: Is curr_year == 2022? I predict True.')
print(f'Test 5 Result: {curr_year == 2022}')

print('---')

programming_lang = 'Python'

print('Test 6: Is programming_lang == "C++"? I predict False.')
print(f'Test 6 Result: {programming_lang == "C++"}')

print('---')

print('Test 7: Does 11 * 11 == 121? I predict True.')
print(f'Test 7 Result: {(11 * 11) == 121}')

print('---')

us_states = ['California', 'Massachusetts', 'New York', 'Washington', 'Florida']

print('Test 8: Is "Washington D.C." in us_states? I predict False.')
print(f'Test 8 Result: {"Washington D.C." in us_states}')

print('---')

operation_performed = 'subtraction'

print('Test 9: Does 1 - 2 == -1, and is operation_performed == "subtraction"? I predict True.')
print(f'Test 9 Result: {((1 - 2) == -1) and (operation_performed == "subtraction")}')

print('---')

president = 'George Washington'
dem_presidents = ['Barack Obama', 'Theodore Roosevelt', 'Joe Biden']

print('Test 10: Is president in dem_presidents? I predict False.')
print(f'Test 10 Result: {president in dem_presidents}')
```

    Test 1: Does 1 + 1 == 2? I predict True.
    Test 1 Result: True
    ---
    Test 2: Does kevins_college == "UCLA"? I predict False.
    Test 2 Result: False
    ---
    Test 3: Does kevins_college != "UCLA"? I predict True.
    Test 3 Result: True
    ---
    Test 4: Is curr_year == 2202? I predict False.
    Test 4 Result: False
    ---
    Test 5: Is curr_year == 2022? I predict True.
    Test 5 Result: True
    ---
    Test 6: Is programming_lang == "C++"? I predict False.
    Test 6 Result: False
    ---
    Test 7: Does 11 * 11 == 121? I predict True.
    Test 7 Result: True
    ---
    Test 8: Is "Washington D.C." in us_states? I predict False.
    Test 8 Result: False
    ---
    Test 9: Does 1 - 2 == -1, and is operation_performed == "subtraction"? I predict True.
    Test 9 Result: True
    ---
    Test 10: Is president in dem_presidents? I predict False.
    Test 10 Result: False


#### 5-2. More Conditional Tests:


```python
print('Test 11: Is programming_lang == "Python"? I predict True.')
print(f'Test 11 Result: {programming_lang == "Python"}')

print('---')

print('Test 12: Does kevins_college != "Tufts University"? I predict False.')
print(f'Test 12: Result: {kevins_college != "Tufts University"}')

print('---')

queen = 'Queen Elizabeth'
unknown = 'qUeEn EliZabEth'

print('Test 13: Does queen == unknown in who they refer to? I predict True.')
print(f'Test 13 Result: {queen.lower() == unknown.lower()}')

print('---')

books = ['Jane Eyre', 'The Great Gatsby', "Bonesetter's Daughter"]

print('Test 14: Is "harry potter" not in books? I predict True.')

for book in books:
    book = book.lower()
    
print(f'Test 14 Result: {"harry potter" not in books}')
        
print('---')

planets_2005 = ['Earth', 'Mars', 'Saturn', 'Pluto']

print('Test 15: Is "Pluto" not in planets_2005? I predict False.')
print(f'Test 15 Result: {"Pluto" not in planets_2005}')

print('---')

height_1 = 180
height_2 = 152
print('Test 16: Are height_1 or height_2 < 152cm? I predict False.')
print(f'Test 16 Result: {(height_1 < 152) or (height_2 < 152)}')
```

    Test 11: Is programming_lang == "Python"? I predict True.
    Test 11 Result: True
    ---
    Test 12: Does kevins_college != "Tufts University"? I predict False.
    Test 12: Result: False
    ---
    Test 13: Does queen == unknown in who they refer to? I predict True.
    Test 13 Result: True
    ---
    Test 14: Is "harry potter" not in books? I predict True.
    Test 14 Result: True
    ---
    Test 15: Is "Pluto" not in planets_2005? I predict False.
    Test 15 Result: False
    ---
    Test 16: Are height_1 or height_2 < 152cm? I predict False.
    Test 16 Result: False


#### 5-3. Alien Colors #1:


```python
alien_color = 'green'

if (alien_color == 'green'):
    print('Player has earned 5 points!')
```

    Player has earned 5 points!



```python
alien_color = 'yellow'

if (alien_color == 'green'):
    print('Player has earned 5 points!')
```

#### 5-4. Alien Colors #2:


```python
alien_color = 'green'

if (alien_color == 'green'):
    print('Alien was shot-down! Player has earned 5 points.')
else:
    print('Player has earned 10 points.')
```

    Alien was shot-down! Player has earned 5 points.



```python
alien_color = 'red'

if (alien_color == 'green'):
    print('Alien was shot-down! Player has earned 5 points.')
else:
    print('Player has earned 10 points.')
```

    Player has earned 10 points.


#### 5-5 Alien Colors #3:


```python
alien_color = 'green'

if (alien_color == 'green'):
    print('Player has earned 5 points!')
elif (alien_color == 'yellow'):
    print('Player has earned 10 points!')
else:
    print('Player has earned 15 points!')
```

    Player has earned 5 points!



```python
alien_color = 'yellow' 

if (alien_color == 'green'):
    print('Player has earned 5 points!')
elif (alien_color == 'yellow'):
    print('Player has earned 10 points!')
else:
    print('Player has earned 15 points!')
```

    Player has earned 10 points!



```python
alien_color = 'red'

if (alien_color == 'green'):
    print('Player has earned 5 points!')
elif (alien_color == 'yellow'):
    print('Player has earned 10 points!')
else:
    print('Player has earned 15 points!')
```

    Player has earned 15 points!


#### 5-6. Stages of Life: 


```python
age = 0

if (age < 0):
    quit()

if (age < 2):
    print('This person is a baby.')
elif (age < 4):
    print('This person is a toddler.')
elif (age < 13):
    print('This person is a kid.')
elif (age < 20):
    print('This person is a teenager.')
elif (age < 65):
    print('This person is an adult.')
else:
    print('This person is an elder.')
```

    This person is a baby.


#### 5-7. Favorite Fruit:


```python
favorite_fruits = ['mangos', 'watermelon', 'bananas']
```


```python
if ('apples' in favorite_fruits):
    print('You really like apples!')
if ('mangos' in favorite_fruits):
    print('You really like mangos!')
if ('strawberries' in favorite_fruits):
    print('You really like strawberries!')
if ('watermelon' in favorite_fruits):
    print('You really like watermelon!')
if ('bananas' in favorite_fruits):
    print('You really like bananas!')
```

    You really like mangos!
    You really like watermelon!
    You really like bananas!


#### 5-8. Hello Admin.


```python
usernames = ['bob', 'jumbo', 'bessie', 'admin', 'pikachu']
```


```python
for username in usernames:
    if (username == 'admin'):
        print(f'Hello {username}, would you like to see a status report?')
    else:
        print(f'Hello {username}, thank you for logging in again.')
```

    Hello bob, thank you for logging in again.
    Hello jumbo, thank you for logging in again.
    Hello bessie, thank you for logging in again.
    Hello admin, would you like to see a status report?
    Hello pikachu, thank you for logging in again.


#### 5-9. No Users:


```python
usernames = []
```


```python
if (usernames):
    for username in usernames:
        if (username == 'admin'):
            print(f'Hello {username}, would you like to see a status report?')
        else:
            print(f'Hello {username}, thank you for logging in again.')
else:
    print('We need to find some users!')
```

    We need to find some users!


#### 5-10. Checking Usernames:


```python
current_users = ['bob01', 'jumbo02', 'bessie03', 'admin04', 'pikachu05']
```


```python
new_users = ['bessie03', 'dewick01', 'kindlevan05', 'BOB01', 'patrick04']
```


```python
temp_users = current_users[:]
current_users = []

for user in temp_users:
    current_users.append(user.lower())
    
for user in new_users:
    if (user.lower() in current_users):
        print(f'{user} is already owned by someone else. Please enter a new username.')
    else:
        print(f'{user} is available.')
```

    bessie03 is already owned by someone else. Please enter a new username.
    dewick01 is available.
    kindlevan05 is available.
    BOB01 is already owned by someone else. Please enter a new username.
    patrick04 is available.


#### 5-11. Ordinal Numbers:


```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]
```


```python
for num in numbers:
    if (num == 1):
        print(f'{num}st')
    elif (num == 2):
        print(f'{num}nd')
    elif (num == 3):
        print(f'{num}rd')
    else:
        print(f'{num}th')
```

    1st
    2nd
    3rd
    4th
    5th
    6th
    7th
    8th
    9th

