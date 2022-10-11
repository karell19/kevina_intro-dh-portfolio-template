---
layout: page
title: Finding Places in Text with the World Historical Gazeteer
description: This Programming Historian lesson has taught me about those resources available to digital historians
that allow them to learn about geographical locations mentioned throughout texts, and further, to visualize
such locations for reconciliation and geocoding purposes. 
---

### Finding Places in Text with the World Historical Gazeteer
Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Professor Saxton

#### Finding Places in Text with Python
* enumerate() method adds counter to iterable data, such as lists or tuples.
* find() searches matches for a specified sequence of characters along a text, returning location of first character in that sequence along a series of text.
    * Method only seeks exact matches of character sequences. Must consider lower/upper case spelling of terms and the possibility that such matches of character sequences may be found, but may not be representative of valid terms.


```python
text = "Siberia has many rivers."

# Using enumerate() method to locate characters along text, by index.
for index, char in enumerate(text):
    print(index, char)
```

    0 S
    1 i
    2 b
    3 e
    4 r
    5 i
    6 a
    7  
    8 h
    9 a
    10 s
    11  
    12 m
    13 a
    14 n
    15 y
    16  
    17 r
    18 i
    19 v
    20 e
    21 r
    22 s
    23 .



```python
# Using find() method to locate sequence of characters along text.
index = text.find("rivers")

print(f'"rivers" was located at index {index}.')
```

    "rivers" was located at index 17.



```python
# Using find() method to display emphasis on lower/upper case spelling of terms.
index = text.find("Rivers")

print(f'"Rivers" was located at index {index}.')
```

    "Rivers" was located at index -1.



```python
# Using find() method to display how it seeks only to identify exact matches for character sequences.
index = text.find("y riv")

print(f'"y riv" was located at index {index}.')
```

    "y riv" was located at index 15.


#### Natural Language Processing
* Tokenization: Splitting of text into meaningful parts, aka word tokens.
    * Efficacy depends on language of interest and its grammar/spelling rules.
    * Must work to distinguish words from spaces, punctuation as appropriate, with consideration of elided terms and other exceptions.
    
* NLP langauge objects can tokenize series of text, remove "stop words" and punctuation, and perform further text processing functions.


```python
# Installing spaCy NLP library.
pip install spacy
```

    Requirement already satisfied: spacy in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (3.4.1)
    Requirement already satisfied: pathy>=0.3.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (0.6.2)
    Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.0.8)
    Requirement already satisfied: typer<0.5.0,>=0.3.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (0.4.2)
    Requirement already satisfied: langcodes<4.0.0,>=3.2.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (3.3.0)
    Requirement already satisfied: preshed<3.1.0,>=3.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (3.0.7)
    Requirement already satisfied: spacy-legacy<3.1.0,>=3.0.9 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (3.0.10)
    Requirement already satisfied: requests<3.0.0,>=2.13.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.27.1)
    Requirement already satisfied: catalogue<2.1.0,>=2.0.6 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.0.8)
    Requirement already satisfied: jinja2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.11.3)
    Requirement already satisfied: srsly<3.0.0,>=2.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.4.4)
    Requirement already satisfied: cymem<2.1.0,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (2.0.6)
    Requirement already satisfied: pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.9.2)
    Requirement already satisfied: setuptools in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (61.2.0)
    Requirement already satisfied: tqdm<5.0.0,>=4.38.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (4.64.0)
    Requirement already satisfied: numpy>=1.15.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.21.5)
    Requirement already satisfied: wasabi<1.1.0,>=0.9.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (0.10.1)
    Requirement already satisfied: thinc<8.2.0,>=8.1.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (8.1.3)
    Requirement already satisfied: packaging>=20.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (21.3)
    Requirement already satisfied: spacy-loggers<2.0.0,>=1.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy) (1.0.3)
    Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from packaging>=20.0->spacy) (3.0.4)
    Requirement already satisfied: smart-open<6.0.0,>=5.2.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pathy>=0.3.5->spacy) (5.2.1)
    Requirement already satisfied: typing-extensions>=3.7.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4->spacy) (4.1.1)
    Requirement already satisfied: idna<4,>=2.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (3.3)
    Requirement already satisfied: charset-normalizer~=2.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (2.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (2021.10.8)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy) (1.26.9)
    Requirement already satisfied: confection<1.0.0,>=0.0.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy) (0.0.3)
    Requirement already satisfied: blis<0.8.0,>=0.7.8 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy) (0.7.8)
    Requirement already satisfied: click<9.0.0,>=7.1.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from typer<0.5.0,>=0.3.0->spacy) (8.0.4)
    Requirement already satisfied: MarkupSafe>=0.23 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from jinja2->spacy) (2.0.1)
    Note: you may need to restart the kernel to use updated packages.



```python
# Loading German language support from spaCy NLP library into a "language object" (nlp).
from spacy.lang.de import German
nlp = German()
```


```python
# Initialization of variable with text in German.
doc = nlp("Berlin ist eine Stadt in Deutschland.")

# Iteration of German text in effort to return token indices and associated text 
for token in doc:
    print(token.i, token.text)
```

    0 Berlin
    1 ist
    2 eine
    3 Stadt
    4 in
    5 Deutschland
    6 .


##### Not working appropriately, file not found.


```python
# Importing pathlib library.
from pathlib import Path

# Using pathlib to open a text file to-be read.
gazetteer = Path("gazetteer.txt").read_text()

# Making a list of location names by splitting the text file by '\n'.
gazetteer = gazetteer.split('\n')

# Should print list of location names.
print(gazetteer)
```


    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    Input In [58], in <cell line: 5>()
          2 from pathlib import Path
          4 # Using pathlib to open a text file to-be read.
    ----> 5 gazetteer = Path("gazetteer.txt").read_text()
          7 # Making a list of location names by splitting the text file by '\n'.
          8 gazetteer = gazetteer.split('\n')


    File ~/opt/anaconda3/lib/python3.9/pathlib.py:1266, in Path.read_text(self, encoding, errors)
       1262 def read_text(self, encoding=None, errors=None):
       1263     """
       1264     Open the file in text mode, read it, and close the file.
       1265     """
    -> 1266     with self.open(mode='r', encoding=encoding, errors=errors) as f:
       1267         return f.read()


    File ~/opt/anaconda3/lib/python3.9/pathlib.py:1252, in Path.open(self, mode, buffering, encoding, errors, newline)
       1246 def open(self, mode='r', buffering=-1, encoding=None,
       1247          errors=None, newline=None):
       1248     """
       1249     Open the file pointed by this path and return a file object, as
       1250     the built-in open() function does.
       1251     """
    -> 1252     return io.open(self, mode, buffering, encoding, errors, newline,
       1253                    opener=self._opener)


    File ~/opt/anaconda3/lib/python3.9/pathlib.py:1120, in Path._opener(self, name, flags, mode)
       1118 def _opener(self, name, flags, mode=0o666):
       1119     # A stub for the opener argument to built-in open()
    -> 1120     return self._accessor.open(self, flags, mode)


    FileNotFoundError: [Errno 2] No such file or directory: 'gazetteer.txt'



```python
# Loading German language support from spaCy NLP library into a "language object" (nlp).
from spacy.lang.de import German
nlp = German()

# Initialization of variable with text in German.
doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")

# Loading Matcher support from spaCy NLP library.
from spacy.matcher import Matcher
matcher = Matcher(nlp.vocab)
```

##### Not working appropriately, gazetter variable not defined due to errors in file access.


```python
# Iterating through gazetteer list, using Matcher to establish, then match for, patterns.
for place in gazetteer:
    pattern = [{'LOWER': place.lower()}]
    matcher.add(place, [pattern])

# Use of Matcher tool to compose a list of patterns found in provided text, and their locations.
matcher = matcher(doc)

# Printing starting and ending indices for patterns matched along tokens of the text.
for match_id, start, end in matches:
    print(start, end, doc[start:end].text)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Input In [60], in <cell line: 2>()
          1 # Iterating through gazetteer list, using Matcher to establish, then match for, patterns.
    ----> 2 for place in gazetteer:
          3     pattern = [{'LOWER': place.lower()}]
          4     matcher.add(place, [pattern])


    NameError: name 'gazetteer' is not defined



```python
# Definition of pattern to be sought using Matcher.
pattern = [{'LOWER': 'larger'},
           {'LIKE_NUM': True}]

# Matching instances of tokens with "lager" (German for "camp"), when followed by a number, using Matcher.
matcher.add("LARGER_PATTERN", [pattern])
```

##### Not working appropriately, errors in file access.


```python
# Application of Matcher functions to various .txt files in a directory, through iteration.
for file in Path('folder_with_texts_in_it').iterdir():
    # read_text() method used to load text from files.
    doc = nlp(file.read_text())
    matches = matcher(doc)
    for matcher_id, start, end in matches:
        print(file.name, start, end, doc[start:end].text)
```


    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    Input In [62], in <cell line: 2>()
          1 # Application of Matcher functions to various .txt files in a directory, through iteration.
    ----> 2 for file in Path('folder_with_texts_in_it').iterdir():
          3     # read_text() method used to load text from files.
          4     doc = nlp(file.read_text())
          5     matches = matcher(doc)


    File ~/opt/anaconda3/lib/python3.9/pathlib.py:1160, in Path.iterdir(self)
       1156 def iterdir(self):
       1157     """Iterate over the files in this directory.  Does not yield any
       1158     result for the special paths '.' and '..'.
       1159     """
    -> 1160     for name in self._accessor.listdir(self):
       1161         if name in {'.', '..'}:
       1162             # Yielding a path object for these makes little sense
       1163             continue


    FileNotFoundError: [Errno 2] No such file or directory: 'folder_with_texts_in_it'


##### Not working appropriately, matches variable not defined due to errors in file access.


```python
# Loading Python's Counter tool.
from collections import Counter

# Addition of all matched terms to a list.
count_list = []
for match_id, start, end in matches:
    count_list.append(doc[start:end].text)
    
# Use of Counter tool to derive frequency of each term.
counter = Counter(count_list)

# Printing the ten most frequent matched terms.
for term, count in counter.most_common(10):
    print(term, count)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Input In [63], in <cell line: 6>()
          4 # Addition of all matched terms to a list.
          5 count_list = []
    ----> 6 for match_id, start, end in matches:
          7     count_list.append(doc[start:end].text)
          9 # Use of Counter tool to derive frequency of each term.


    NameError: name 'matches' is not defined


#### Application of Named Entity Recognition Tools.
* Important to assess results for validty, since they are machine-evaluated and prone to error.

##### Unknown Error.


```python
# Downloads "de_core_news_sm" pre-trained model.
python -m spacy download de_core_news_sm

# Loads "de_core_news_sm" model for use in program into "language object" (nlp).
import spacy
nlp = spacy.load("de_core_news_sm")

# Initialization of variable with model-processed text in German.
doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")

# Printing of those locations identified by model in provided text.
for ent in doc.ents:
    print(ent.text, ent.label_, ent.start, ent.end)
```


      Input In [75]
        python -m spacy download de_core_news_sm
                  ^
    SyntaxError: invalid syntax



#### Visualization of Index-Locations Pertaining to Matched Terms Along Text


```python
# Use of displaCy tool to visualize locations of matched terms along text.
displacy.render(doc, jupyter = True, style = "ent")
```


<span class="tex2jax_ignore"><div class="entities" style="line-height: 2.5; direction: ltr">Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.</div></span>


#### Visualization of Relations Between Tokens Along Text


```python
# Use of displaCy tool to visualize relations between tokens along text.
displacy.render(doc, jupyter = True, style = "dep")
```


<span class="tex2jax_ignore"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:lang="de" id="54f6e1c138964acab58df474a15fdb98-0" class="displacy" width="2675" height="137.0" direction="ltr" style="max-width: none; height: 137.0px; color: #000000; background: #ffffff; font-family: Arial; direction: ltr">
<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="50">Karl-Heinz</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="50"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="225">Quade</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="225"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="400">ist</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="400"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="575">von</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="575"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="750">März</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="750"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="925">1944</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="925"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1100">bis</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1100"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1275">August</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1275"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1450">1948</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1450"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1625">im</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1625"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1800">Lager</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1800"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1975">150</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1975"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="2150">in</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2150"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="2325">Grjasowez</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2325"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="2500">interniert.</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2500"></tspan>
</text>
</svg></span>



```python
from pathlib import Path

# Attempt at saving displacy visualizations to a file for use elsewhere.
svg = displacy.render(doc, style = "dep")
output_path = Path("sentence.svg")
output_path.write_text(svg)
```


<span class="tex2jax_ignore"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:lang="de" id="3dcac9b423a7472cb5db33e405335c10-0" class="displacy" width="2675" height="137.0" direction="ltr" style="max-width: none; height: 137.0px; color: #000000; background: #ffffff; font-family: Arial; direction: ltr">
<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="50">Karl-Heinz</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="50"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="225">Quade</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="225"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="400">ist</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="400"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="575">von</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="575"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="750">März</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="750"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="925">1944</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="925"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1100">bis</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1100"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1275">August</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1275"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1450">1948</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1450"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1625">im</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1625"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1800">Lager</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1800"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="1975">150</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="1975"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="2150">in</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2150"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="2325">Grjasowez</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2325"></tspan>
</text>

<text class="displacy-token" fill="currentColor" text-anchor="middle" y="47.0">
    <tspan class="displacy-word" fill="currentColor" x="2500">interniert.</tspan>
    <tspan class="displacy-tag" dy="2em" fill="currentColor" x="2500"></tspan>
</text>
</svg></span>



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Input In [73], in <cell line: 5>()
          3 svg = displacy.render(doc, style = "dep")
          4 output_path = Path("sentence.svg")
    ----> 5 output_path.write_text(svg)


    File ~/opt/anaconda3/lib/python3.9/pathlib.py:1283, in Path.write_text(self, data, encoding, errors)
       1279 """
       1280 Open the file in text mode, write to it, and close the file.
       1281 """
       1282 if not isinstance(data, str):
    -> 1283     raise TypeError('data must be str, not %s' %
       1284                     data.__class__.__name__)
       1285 with self.open(mode='w', encoding=encoding, errors=errors) as f:
       1286     return f.write(data)


    TypeError: data must be str, not NoneType



```python
# Installing DBpedia Spotlight library.
pip install spacy-dbpedia-spotlight
```

    Requirement already satisfied: spacy-dbpedia-spotlight in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (0.2.5)
    Requirement already satisfied: spacy>=3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy-dbpedia-spotlight) (3.4.1)
    Requirement already satisfied: loguru in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy-dbpedia-spotlight) (0.6.0)
    Requirement already satisfied: wasabi<1.1.0,>=0.9.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (0.10.1)
    Requirement already satisfied: spacy-legacy<3.1.0,>=3.0.9 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (3.0.10)
    Requirement already satisfied: cymem<2.1.0,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.0.6)
    Requirement already satisfied: numpy>=1.15.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (1.21.5)
    Requirement already satisfied: packaging>=20.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (21.3)
    Requirement already satisfied: pathy>=0.3.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (0.6.2)
    Requirement already satisfied: spacy-loggers<2.0.0,>=1.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (1.0.3)
    Requirement already satisfied: thinc<8.2.0,>=8.1.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (8.1.3)
    Requirement already satisfied: langcodes<4.0.0,>=3.2.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (3.3.0)
    Requirement already satisfied: preshed<3.1.0,>=3.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (3.0.7)
    Requirement already satisfied: catalogue<2.1.0,>=2.0.6 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.0.8)
    Requirement already satisfied: srsly<3.0.0,>=2.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.4.4)
    Requirement already satisfied: requests<3.0.0,>=2.13.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.27.1)
    Requirement already satisfied: pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (1.9.2)
    Requirement already satisfied: tqdm<5.0.0,>=4.38.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (4.64.0)
    Requirement already satisfied: jinja2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (2.11.3)
    Requirement already satisfied: typer<0.5.0,>=0.3.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (0.4.2)
    Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (1.0.8)
    Requirement already satisfied: setuptools in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from spacy>=3->spacy-dbpedia-spotlight) (61.2.0)
    Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from packaging>=20.0->spacy>=3->spacy-dbpedia-spotlight) (3.0.4)
    Requirement already satisfied: smart-open<6.0.0,>=5.2.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pathy>=0.3.5->spacy>=3->spacy-dbpedia-spotlight) (5.2.1)
    Requirement already satisfied: typing-extensions>=3.7.4.3 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from pydantic!=1.8,!=1.8.1,<1.10.0,>=1.7.4->spacy>=3->spacy-dbpedia-spotlight) (4.1.1)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy>=3->spacy-dbpedia-spotlight) (1.26.9)
    Requirement already satisfied: idna<4,>=2.5 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy>=3->spacy-dbpedia-spotlight) (3.3)
    Requirement already satisfied: charset-normalizer~=2.0.0 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy>=3->spacy-dbpedia-spotlight) (2.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from requests<3.0.0,>=2.13.0->spacy>=3->spacy-dbpedia-spotlight) (2021.10.8)
    Requirement already satisfied: blis<0.8.0,>=0.7.8 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy>=3->spacy-dbpedia-spotlight) (0.7.8)
    Requirement already satisfied: confection<1.0.0,>=0.0.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from thinc<8.2.0,>=8.1.0->spacy>=3->spacy-dbpedia-spotlight) (0.0.3)
    Requirement already satisfied: click<9.0.0,>=7.1.1 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from typer<0.5.0,>=0.3.0->spacy>=3->spacy-dbpedia-spotlight) (8.0.4)
    Requirement already satisfied: MarkupSafe>=0.23 in /Users/kevin/opt/anaconda3/lib/python3.9/site-packages (from jinja2->spacy>=3->spacy-dbpedia-spotlight) (2.0.1)
    Note: you may need to restart the kernel to use updated packages.


##### Unknown Error. Missing Python packages.


```python
import spacy
nlp = spacy.load('de_core_news_sm')
nlp.add_pipe('dbpedia_spotlight', config = {'language_code': 'de'})

doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")
for ent in doc.ents:
    print(ent.text, ent.label_, ent.kb_id_)
```


    ---------------------------------------------------------------------------

    OSError                                   Traceback (most recent call last)

    Input In [1], in <cell line: 2>()
          1 import spacy
    ----> 2 nlp = spacy.load('de_core_news_sm')
          3 nlp.add_pipe('dbpedia_spotlight', config = {'language_code': 'de'})
          5 doc = nlp("Karl-Heinz Quade ist von März 1944 bis August 1948 im Lager 150 in Grjasowez interniert.")


    File ~/opt/anaconda3/lib/python3.9/site-packages/spacy/__init__.py:54, in load(name, vocab, disable, enable, exclude, config)
         30 def load(
         31     name: Union[str, Path],
         32     *,
       (...)
         37     config: Union[Dict[str, Any], Config] = util.SimpleFrozenDict(),
         38 ) -> Language:
         39     """Load a spaCy model from an installed package or a local path.
         40 
         41     name (str): Package name or model path.
       (...)
         52     RETURNS (Language): The loaded nlp object.
         53     """
    ---> 54     return util.load_model(
         55         name,
         56         vocab=vocab,
         57         disable=disable,
         58         enable=enable,
         59         exclude=exclude,
         60         config=config,
         61     )


    File ~/opt/anaconda3/lib/python3.9/site-packages/spacy/util.py:436, in load_model(name, vocab, disable, enable, exclude, config)
        434 if name in OLD_MODEL_SHORTCUTS:
        435     raise IOError(Errors.E941.format(name=name, full=OLD_MODEL_SHORTCUTS[name]))  # type: ignore[index]
    --> 436 raise IOError(Errors.E050.format(name=name))


    OSError: [E050] Can't find model 'de_core_news_sm'. It doesn't seem to be a Python package or a valid path to a data directory.



```python
# Requesting access to data associated with a match for "Grjasowez" from the DBpedia library.
import requests
data = requests.get("http://de.dbpedia.org/data/Grjasowez.json").json()
```


```python
# Accessing latitude and longitutde of Grjasowez using DBpedia.
print(f"Latitude and Longitude of Grjasowez: {data['http://de.dbpedia.org/resource/Grjasowez']['http://www.georss.org/georss/point'][0]['value']}")
```

    Latitude and Longitude of Grjasowez: 58.88333333333333 40.25


### Reflection
This Programming Historian lesson introduced me to some of the methods used by Digital Historians to scan series of text for terms of interest, with a particular focus on those terms associated with geographical locations. Gazeteers serve as directories containing a series of location names. Using such gazeteers as references, we are able to associate words from text as terms pertaining to the names of locations. This lesson introduced various gazeteers, however, the World Historical Gazeteer was of special interest to me. This gazeteer serves as a database that has accumulated the names of geographical locations as they have changed throughout history. Using this database, digital historians have been offered the flexibility to analyze texts of historical periods at even greater degrees. Further, this lesson introduced me to Natural Language Processing tools that allow digital historians to analyze texts that have experienced transliteration, that is, the process of having been translated across languages and alphabets.

Using these tools, I should have been able to identify mentions of cities or towns in text, and then map them using the World Historical Gazeteer. However, I was unable to, as I struggled to deal with a series of errors related to an unusual inability to access certain Python packages mentioned in this lesson, which hindered my ability to complete this lesson's outlined objectives. Regardless, I now know more about the available resources that digital historians may use to associate texts with geographic locations, and to visualize geographic data for reconciliation and geocoding purposes. 
