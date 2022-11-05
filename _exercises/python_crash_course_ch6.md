---
layout: page
title: Python Basics IV, Crash Course Exercises Ch. 06
description: This notebook contains my attempts of Chapter 06's Python exercises.
---

### Chapter 6 - Dictionaries
Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Professor Saxton

#### 6-1. Person:


```python
person1 = {'first_name':'kevin', 'last_name':'arellano', 'age':18, 'city':'medford'}
```


```python
print(person1['first_name'])
print(person1['last_name'])
print(person1['age'])
print(person1['city'])
```

    kevin
    arellano
    18
    medford


#### 6-2. Favorite Numbers:


```python
fav_numbers = {'kevin':19, 'viv':7, 'kathy':18, 'alex':6, 'jasmin':17}
```


```python
print(f'Kevin: {fav_numbers["kevin"]}')
print(f'Viv: {fav_numbers["viv"]}')
print(f'Kathy: {fav_numbers["kathy"]}')
print(f'Alex: {fav_numbers["alex"]}')
print(f'Jasmin: {fav_numbers["jasmin"]}')
```

    Kevin: 19
    Viv: 7
    Kathy: 18
    Alex: 6
    Jasmin: 17


#### 6-3. Glossary:


```python
glossary = {
    'loop':'Repeated series of lines of code.',
    'variable':'A term representative of a data value.',
    'method':'Operation that may act upon a data value.',
    'list':'Collection of various data values',
    'tuple':'Collection of various data values that cannot be modified',
    }
```


```python
print(f'Loop: {glossary["loop"]}')
print(f'Variable: {glossary["variable"]}')
print(f'Method: {glossary["method"]}')
print(f'List: {glossary["list"]}')
print(f'Tuple: {glossary["tuple"]}')
```

    Loop: Repeated series of lines of code.
    Variable: A term representative of a data value.
    Method: Operation that may act upon a data value.
    List: Collection of various data values
    Tuple: Collection of various data values that cannot be modified


#### 6-4. Glossary #2:


```python
i = 1

for key, value in glossary.items():
    print(f'{i}) {key.title()}: {value}')
    i += 1
```

    1) Loop: Repeated series of lines of code.
    2) Variable: A term representative of a data value.
    3) Method: Operation that may act upon a data value.
    4) List: Collection of various data values
    5) Tuple: Collection of various data values that cannot be modified



```python
glossary['boolean'] = 'A value that is either True or False.'
glossary['conditional'] = 'An expression evaluating to either True or False.'
glossary['set'] = 'Collection of items, none of which repeat.'
glossary['dictionary'] = 'Collection of key-value pairs.'
glossary['slice'] = 'A specific group of items in a list.'
```


```python
i = 1

for key, value in glossary.items():
    print(f'{i}) {key.title()}: {value}')
    i += 1
```

    1) Loop: Repeated series of lines of code.
    2) Variable: A term representative of a data value.
    3) Method: Operation that may act upon a data value.
    4) List: Collection of various data values
    5) Tuple: Collection of various data values that cannot be modified
    6) Boolean: A value that is either True or False.
    7) Conditional: An expression evaluating to either True or False.
    8) Set: Collection of items, none of which repeat.
    9) Dictionary: Collection of key-value pairs.
    10) Slice: A specific group of items in a list.


#### 6-5. Rivers:


```python
rivers = {
    'amazon':'brazil',
    'mississippi':'united states of AMERICA',
    'huang he':'CHina',
    }
```


```python
for river, country in rivers.items():
    print(f'The {river.title()} river runs through {country.title()}.')
```

    The Amazon river runs through Brazil.
    The Mississippi river runs through United States Of America.
    The Huang He river runs through China.



```python
for river in rivers.keys():
    print(river.title())
```

    Amazon
    Mississippi
    Huang He



```python
for country in rivers.values():
    print(country.title())
```

    Brazil
    United States Of America
    China


#### 6-6. Polling:


```python
favorite_languages = {
    'jen':'python',
    'sarah':'c',
    'edward':'ruby',
    'phil':'python',
    }
```


```python
to_poll = ['jen', 'bob', 'billy', 'edward', 'bella', 'dan']
```


```python
i = 1

for name in to_poll:
    if (name in favorite_languages.keys()):
        print(f'{i}) {name.title()}, thank you for responding to this poll!')
    else:
        print(f'{i}) {name.title()}, please consider responding to this poll.')
    i += 1
```

    1) Jen, thank you for responding to this poll!
    2) Bob, please consider responding to this poll.
    3) Billy, please consider responding to this poll.
    4) Edward, thank you for responding to this poll!
    5) Bella, please consider responding to this poll.
    6) Dan, please consider responding to this poll.


#### 6-7. People: 


```python
person2 = {'first_name':'phineas', 'last_name':' taylor barnum', 'age':80, 'city':'bridgeport'}
person3 = {'first_name':'charles', 'last_name':'tufts', 'age':95, 'city':'somerville'}

people = [person1, person2, person3]
```


```python
for person in people:
    print(person)
```

    {'first_name': 'kevin', 'last_name': 'arellano', 'age': 18, 'city': 'medford'}
    {'first_name': 'phineas', 'last_name': ' taylor barnum', 'age': 80, 'city': 'bridgeport'}
    {'first_name': 'charles', 'last_name': 'tufts', 'age': 95, 'city': 'somerville'}


#### 6-8. Pets:


```python
pet1 = {'species':'dog', 'owner':'kevin'}
pet2 = {'species':'sea snail', 'owner':'spongebob'}
pet3 = {'species':'eagle', 'owner':'george'}

pets = [pet1, pet2, pet3]
```


```python
for pet in pets:
    print(pet)
```

    {'species': 'dog', 'owner': 'kevin'}
    {'species': 'sea snail', 'owner': 'spongebob'}
    {'species': 'eagle', 'owner': 'george'}


#### 6-9. Favorite Places:


```python
favorite_places = {
    'kevin':{
        'place_1':'miner hall',
        'place_2':'eaton hall',
        'place_3':'stratton hall',
        },
    
    'alex':{
        'place_1':'dewick',
        'place_2':'tisch library',
        'place_3':'ginn library'
        },
    
    'viv':{
        'place_1':'kindlevan',
        'place_2':'joyce cummings center',
        'place_3':'campus center'
        },
}
```


```python
for person, place in favorite_places.items():
    print(f"{person.title()}'s favorite places on campus are: ", end = "")
    
    for place in favorite_places[person]:
        if (favorite_places[person][place] == favorite_places[person]['place_3']):
            print(f'and {favorite_places[person][place].title()}.')
        else:
            print(f'{favorite_places[person][place].title()}', end = ", ")
```

    Kevin's favorite places on campus are: Miner Hall, Eaton Hall, and Stratton Hall.
    Alex's favorite places on campus are: Dewick, Tisch Library, and Ginn Library.
    Viv's favorite places on campus are: Kindlevan, Joyce Cummings Center, and Campus Center.


#### 6-10. Favorite Numbers:


```python
fav_numbers = {
    'kevin':[19, 3, 21],
    'viv':[7, 20],
    'kathy':[18],
    'alex':[12, 6, 18],    
    'jasmin':[2, 17],
}
```


```python
for person, numbers in fav_numbers.items():
    print(f"{person.title()}'s favorite numbers are: ", end = "")
    
    for num in fav_numbers[person]:
        if (num == fav_numbers[person][-1]):
            print(f'and {num}.')
        else:
            print(f'{num}', end = ", ")
```

    Kevin's favorite numbers are: 19, 3, and 21.
    Viv's favorite numbers are: 7, and 20.
    Kathy's favorite numbers are: and 18.
    Alex's favorite numbers are: 12, 6, and 18.
    Jasmin's favorite numbers are: 2, and 17.


#### 6-11. Cities:


```python
cities = {
    'los angeles':{
        'country':'United States of America',
        'population':3973000,
        'fact':'Founded in 1781',
        },
    
    'mexico city':{
        'country':'Mexico',
        'population':8855000,
        'fact':'Founded in 1521',
        },
    
    'paris':{
        'country':'France',
        'population':2161000,
        'fact':'Founded in 259BC',
        },
    }
```


```python
for city, data in cities.items():
    print(f'{city.title()}:')
    print(f"\tCountry: {data['country']}")
    print(f"\tPopulation: {data['population']}")
    if (city == list(cities)[-1]):
        print(f"\tFact: {data['fact']}.")
    else:
        print(f"\tFact: {data['fact']}.\n")
```

    Los Angeles:
    	Country: United States of America
    	Population: 3973000
    	Fact: Founded in 1781.
    
    Mexico City:
    	Country: Mexico
    	Population: 8855000
    	Fact: Founded in 1521.
    
    Paris:
    	Country: France
    	Population: 2161000
    	Fact: Founded in 259BC.

