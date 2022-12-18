---
layout: page
title: Python Basics V, Crash Course Exercises Ch. 08
description: This notebook contains my attempts of Chapter 08's Python exercises.
---

### Chapter 8 - Dictionaries
Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Dr. Saxton

#### 8-1. Message:


```python
def display_message():
    """Summarizes Chapter 8's topic-of-focus."""
    print("I am learning about the use of functions in Python. Yay!")
    
display_message()
```

    I am learning about the use of functions in Python. Yay!


#### 8-2. Favorite Book:


```python
def favorite_book(title):
    """Shares my favorite book."""
    print(f'One of my favorite books is "{title.title()}"')
    
favorite_book("jane eyre")
```

    One of my favorite books is "Jane Eyre"


#### 8-3. T-Shirt:


```python
def make_shirt(size, message):
    """Details a shirt's specification."""
    print(f'This shirt must be size {size.title()}, and it should have "{message}" printed on it')
```


```python
make_shirt("large", "Hello, Jumbo!")
make_shirt(size="small", message="I <3 Tufts!")
```

    This shirt must be size Large, and it should have "Hello, Jumbo!" printed on it
    This shirt must be size Small, and it should have "I <3 Tufts!" printed on it


#### 8-4. Large Shirts:


```python
def make_shirt(size="large", message="I love Python"):
    """Details a shirt's specification."""
    print(f'This shirt must be size {size.title()}, and it should have "{message}" printed on it')
```


```python
make_shirt()
make_shirt(size="medium")
make_shirt(message="I <3 Digital Humanities!")
```

    This shirt must be size Large, and it should have "I love Python" printed on it
    This shirt must be size Medium, and it should have "I love Python" printed on it
    This shirt must be size Large, and it should have "I <3 Digital Humanities!" printed on it


#### 8-5. Cities:


```python
def describe_city(city, country="Mexico"):
    print(f"{city.title()} is in {country.title()}")
```


```python
describe_city("PUERTO VALLARTA")
describe_city(city="puebla")
describe_city(city="los ANGELES", country="the united states of america")
```

    Puerto Vallarta is in Mexico
    Puebla is in Mexico
    Los Angeles is in The United States Of America


#### 8-6. City Names:


```python
def city_country(city, country):
    location = f"{city.title()}, {country.title()}"
    return location
```


```python
print(city_country("new york city", "the united states of america"))
print(city_country(country="paris", city="FRANCE"))

print("")

city_name = input("City Name: ")
country_name = input("Country Name: ")
print(city_country(city_name, country_name))
```

    New York City, The United States Of America
    France, Paris
    
    City Name: santiago
    Country Name: chile
    Santiago, Chile


#### 8-7. Album:


```python
def make_album(artist_name, album_title, num_songs=None):
    album = {"artist":artist_name.title(), "album":album_title.title()}
    
    if (num_songs):
        album["song_count"] = num_songs
        
    return album
```


```python
print(make_album("Michael jackson", "thriller"))
print(make_album(album_title="21", artist_name="adele"))
print(make_album("abbey ROAD", "the BEATLES"))

print(make_album("spice girls", "spice", 10))
```

    {'artist': 'Michael Jackson', 'album': 'Thriller'}
    {'artist': 'Adele', 'album': '21'}
    {'artist': 'Abbey Road', 'album': 'The Beatles'}
    {'artist': 'Spice Girls', 'album': 'Spice', 'song_count': 10}


#### 8-8. User Albums:


```python
while (True):
    artist_name = input('Artist Name ("q" to Quit): ')
    if (artist_name == "q"):
        print("QUIT")
        break
    
    album_title = input('Album Title ("q" to Quit): ')
    if (album_title == "q"):
        print("QUIT")
        break
    
    print(make_album(artist_name, album_title))
```

    Artist Name ("q" to Quit): Taylor Swift
    Album Title ("q" to Quit): Red
    {'artist': 'Taylor Swift', 'album': 'Red'}
    Artist Name ("q" to Quit): Beyonce
    Album Title ("q" to Quit): Lemonade
    {'artist': 'Beyonce', 'album': 'Lemonade'}
    Artist Name ("q" to Quit): Mozart
    Album Title ("q" to Quit): q
    QUIT


#### 8-9. Messages:


```python
def show_messages(list_messages):
    for message in list_messages:
        print(message)
```


```python
messages = ["Hello, Kevin, it was nice to meet you!",
            "How are you doing?",
            "I hope to see you soon :)"]

show_messages(messages)
```

    Hello, Kevin, it was nice to meet you!
    How are you doing?
    I hope to see you soon :)


#### 8-10. Sending Messages:


```python
def send_messages(list_messages, list_sent):
    while list_messages:
        to_move = list_messages.pop(0)
        print(to_move)
        sent_messages.append(to_move)
```


```python
messages = ["Hello, Kevin, it was nice to meet you!",
            "How are you doing?",
            "I hope to see you soon :)"]
sent_messages = []

send_messages(messages, sent_messages)
```

    Hello, Kevin, it was nice to meet you!
    How are you doing?
    I hope to see you soon :)



```python
messages
```




    []




```python
sent_messages
```




    ['Hello, Kevin, it was nice to meet you!',
     'How are you doing?',
     'I hope to see you soon :)']



#### 8-11. Archived Messages:


```python
messages = ["Hello, Kevin, it was nice to meet you!",
            "How are you doing?",
            "I hope to see you soon :)"]
sent_messages = []

send_messages(messages[:], sent_messages)
```

    Hello, Kevin, it was nice to meet you!
    How are you doing?
    I hope to see you soon :)



```python
messages
```




    ['Hello, Kevin, it was nice to meet you!',
     'How are you doing?',
     'I hope to see you soon :)']




```python
sent_messages
```




    ['Hello, Kevin, it was nice to meet you!',
     'How are you doing?',
     'I hope to see you soon :)']



#### 8-12. Sandwiches:


```python
def make_sandwich(*list_items):
    print("Making a sandich with:")
    for item in list_items:
        print(f"- {item.title()}")
```


```python
make_sandwich("Whole WHEAT bread", "turkey breast", "american CHEESE")
print("")

make_sandwich("Whole wheat bread", "bacon", "lettuce", "tomato")
print("")
make_sandwich("Whole wheat bread")
```

    Making a sandich with:
    - Whole Wheat Bread
    - Turkey Breast
    - American Cheese
    
    Making a sandich with:
    - Whole Wheat Bread
    - Bacon
    - Lettuce
    - Tomato
    
    Making a sandich with:
    - Whole Wheat Bread


#### 8-13. User Profile:


```python
def build_profile(first, last, **user_info):
    user_info['first_name'] = first
    user_info['last_name'] = last
    return user_info
```


```python
build_profile("kevin", "arellano", school="tufts university", class_year=2025, major="computer science")
```




    {'school': 'tufts university',
     'class_year': 2025,
     'major': 'computer science',
     'first_name': 'kevin',
     'last_name': 'arellano'}



#### 8-14. Cars:


```python
def make_car(manufacturer, model, **kwargs):
    kwargs['manufacturer'] = manufacturer
    kwargs['model'] = model
    return kwargs
```


```python
car = make_car('subaru', 'outback', color='blue', tow_package=True)
car
```




    {'color': 'blue',
     'tow_package': True,
     'manufacturer': 'subaru',
     'model': 'outback'}


