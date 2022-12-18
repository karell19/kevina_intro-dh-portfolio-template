---
layout: page
title: Variables, Strings, and Numbers
description: This notebook contains my expository engagement with Python, learning about variables, strings, and numbers in this programming language in addition to gaining a familiarity with Jupyter Notebook.
---
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5

Plain Text.
* item one
* item two

[This is a link to Google](www.google.com)

# TO DELETE
dd

# Variables


```python
greeting = "Hello, world!"
```


```python
print(greeting)
```

    Hello, world!



```python
greeting
```




    'Hello, world!'




```python
cls = "Introduction to Digital Humanities"
print(cls)
```

    Introduction to Digital Humanities



```python
cls
```




    'Introduction to Digital Humanities'




```python
cls = "Teaching me Python!"
print(cls)
```

    Teaching me Python!



```python
cls161 = "Introduction to DH"
print(greeting)
print(cls)
print(cls161)
```

    Hello, world!
    Teaching me Python!
    Introduction to DH



```python
ex_1 = "This is a string type variable."
print(ex_1)

ex_2 = 'This is also a string type variable! Whaaat!'
print(ex_2)
```

    This is a string type variable.
    This is also a string type variable! Whaaat!



```python
ex_3 = "Here is a string as well.'
print(ex_3)
```


      Input In [12]
        ex_3 = "Here is a string as well.'
                                          ^
    SyntaxError: EOL while scanning string literal




```python
ex_4 = 'I asked the question, "When should I use single quotes?"'
print(ex_4)
```

    I asked the question, "When should I use single quotes?"



```python
historian = "edward gibbon"
print(historian)
```

    edward gibbon



```python
print(historian.title())
```

    Edward Gibbon



```python
print(historian.title)
```

    <built-in method title of str object at 0x7fc2f0f3ec30>



```python
novelist = "Becky Chambers"
print(novelist.lower())
```

    becky chambers



```python
"becky" == "Becky"
```




    False




```python
"becky" == "becky"
```




    True




```python
print(novelist.upper())
```

    BECKY CHAMBERS



```python
first_name = "kevin"
last_name = "arellano flores"
```


```python
full_name = first_name + " " + last_name
print(full_name)
print(full_name.title())
print(full_name.lower())
print(full_name.upper())
```

    kevin arellano flores
    Kevin Arellano Flores
    kevin arellano flores
    KEVIN ARELLANO FLORES



```python
greeting = f" Hello, {first_name} {last_name}!"
print(greeting)
print(greeting.title())
```

     Hello, kevin arellano flores!
     Hello, Kevin Arellano Flores!


# Variables


```python
print("tab")
```

    tab



```python
print("\ttab")
```

    	tab



```python
print("Languages:\nGreek\nLatin\nEnglish\nSpanish")
```

    Languages:
    Greek
    Latin
    English
    Spanish



```python
str_rspace = "string       "
print(str_rspace)
```

    string       



```python
print(str_rspace.rstrip())
```

    string



```python
example = str_rspace + "."
print(example)
```

    string       .



```python
example_2 = str_rspace.rstrip() + "."
print(example_2)
```

    string.


# Numbers


```python
# integers
238 + 4834
```




    5072




```python
num = 45
num
```




    45




```python
num + 5
```




    50




```python
type(num)
```




    int




```python
30 / 10
```




    3.0




```python
f = 30 / 7
f
```




    4.285714285714286




```python
type(f)
```




    float




```python
3 ** 3
```




    27



# Lists


```python
subjects = ['classics', 'history', 'philosophy']
```


```python
print(subjects)
```

    ['classics', 'history', 'philosophy']



```python
print(subjects[0])
```

    classics



```python
print(subjects[2])
```

    philosophy



```python
# accessing the last one
print(subjects[-1])
```

    philosophy



```python
message = f"My most difficult subject is {subjects[0].title()}..."
print(message)
```

    My most difficult subject is Classics...



```python
# how to modify a list
subjects[0] = 'sociology'
print(subjects)
```

    ['sociology', 'history', 'philosophy']



```python
# append
subjects.append('classics')
print(subjects)
```

    ['sociology', 'history', 'philosophy', 'classics']



```python
subjects.append('calculus')
print(subjects)
```

    ['sociology', 'history', 'philosophy', 'classics', 'calculus']



```python
# insert
subjects.insert(0, 'spanish')
print(subjects)
```

    ['spanish', 'spanish', 'sociology', 'history', 'philosophy', 'classics', 'calculus']



```python
subjects.insert(2, 'community health')
print(subjects)
```

    ['spanish', 'spanish', 'community health', 'sociology', 'history', 'philosophy', 'classics', 'calculus']



```python
# delete item
del subjects[0]
print(subjects)
```

    ['spanish', 'community health', 'sociology', 'history', 'philosophy', 'classics', 'calculus']



```python
# pop method
popped_subject = subjects.pop(-1)
print(popped_subject)
print(subjects)

print(f"NUM IN QUESTION: {popped_subject} hahaha")
```

    philosophy
    ['spanish', 'community health', 'sociology', 'history']
    NUM IN QUESTION: philosophy hahaha



```python
# remove by name
subjects.remove('sociology')
print(subjects)
```

    ['spanish', 'community health', 'history']



```python

```
