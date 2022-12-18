---
layout: page
title: Parse TEI - Gibbon's Decline and Fall of the Roman Empire
description: This notebook contains my parsing of Gibbon's Decline and Fall of the Roman Empire in the form of TEI text.
---
# Parsing a TEI Document - Assignment
Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Dr. Saxton

## Instructions

1. Parse the tei text of Gibbon's _Decline and Fall_ as found in `gibbon.xml`.
2. Extract all the footnotes and remove extraneous white space.
3. Place footnotes in a dataframe.
4. Save the dataframe as a csv file.

## Parsing TEI


```python
# imports
from bs4 import BeautifulSoup
import re
import pandas as pd
```


```python
# load xml file
with open('gibbon.xml', encoding='utf8', mode='r') as f:
    tei_string = f.read()
```


```python
# use BeatifulSoup to instntiate tei object
tei_object = BeautifulSoup(tei_string)
```


```python
# find all footnotes
footnotes = tei_object.find_all('note', attrs={'place': 'bottom'})
```


```python
# clean margin notes and add to a list
clean_footnotes = []
for footnote in footnotes:
    footnote = re.sub(r'[\ \n]{2,}', '', footnote.text)
    clean_footnotes.append(footnote)
```


```python
# convert list to dataframe
footnotes_df = pd.DataFrame(clean_footnotes)
```


```python
# check dataframe
footnotes_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>\nPons Aureoli,thirteen miles from Bergamo, an...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>On the death of Gallienus, ſee Trebellius Poll...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Some ſuppoſed him, oddly enough, to be a baſta...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>\nNotoria,a periodical and official diſpatch w...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Hiſt. Auguſt. p. 208. Gallienus deſcribes the ...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# save datafram as csv
footnotes_df.to_csv('footnotes.csv')
```
