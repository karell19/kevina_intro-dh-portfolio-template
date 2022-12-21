---
layout: page
title: Analyzing Documents with TF-IDF
description: This Programming Historian lesson has taught me about the Term Frequency-Inverse Document Frequency analysis methodologies implemented by scholars of Data Science when seeking to derive the significance of a text corpus.
---
Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Dr. Saxton

- TF-IDF: Weighing significance of a term by division of term frequency by number of documents in corpus utilizing the word.
    - Finds those terms distinctively frequent in a document, relative to others. Emphasizing the term's importance.


```python
from pathlib import Path
```


```python
all_txt_files = []
```


```python
# congregating all txt files into a list
for file in Path('lesson-files').rglob('*.txt'):
    all_txt_files.append(file.parent / file.name)
```


```python
# counting the number of .txt files
len(all_txt_files)
```




    366




```python
# sorting txt files, then ensuring A-Z assortment has occured
all_txt_files.sort()
all_txt_files[0]
```




    PosixPath('lesson-files/txt/0101.txt')




```python
all_docs = []
```


```python
# converting contents of txt files into string format
for txt_file in all_txt_files:
    with open(txt_file) as f:
        txt_file_as_string = f.read()
    all_docs.append(txt_file_as_string)
```


```python
# counting the number of txt files now in the form of strings
len(all_docs)
```




    366




```python
from sklearn.feature_extraction.text import TfidfVectorizer as TFIDF
```


```python
# converting list of strings into sparse matrix, representing tf-idf values for all texts
vectorizer = TFIDF(max_df = .65, min_df = 1, stop_words = None, use_idf = True, norm = None)
transformed_documents = vectorizer.fit_transform(all_docs)
```


```python
# converting sparse martices into a numpy array
transformed_documents_as_array = transformed_documents.toarray()
```


```python
# counting the number of items newly formed numpy array holds
len(transformed_documents_as_array)
```




    366




```python
import pandas as pd
```


```python
# creating folder for output storage
Path('./tf_idf_output').mkdir(parents = True, exist_ok = True)
```


```python
# list comprehension of output file-paths
output_filenames = [str(txt_file).replace('.txt', '.csv').replace('txt/', 'tf_idf_output/') for txt_file in all_txt_files]
```


```python
# counting the number of output file-paths formed
len(output_filenames)
```




    366




```python
# loop responsible for formation of dataframe for all files
for counter, doc in enumerate(transformed_documents_as_array):
    # construction of dataframe (csv format)
    tf_idf_tuples = list(zip(vectorizer.get_feature_names_out(), doc))
    one_doc_as_df = pd.DataFrame.from_records(tf_idf_tuples, columns = ['term', 'score']).sort_values(by = 'score', ascending = False).reset_index(drop = True)
    
    # sending output to a csv file
    one_doc_as_df.to_csv(output_filenames[counter])
```


```python
# creation of dataframe using data pertaining to tf-idf of obituary 0101's text
with open('lesson-files/0101.csv', 'r') as f:
    obituary_0101_df = pd.read_csv(f, header = None)
    obituary_0101_df.columns = ['TF-IDF Rank', 'Term', 'Score']
```


```python
# tf-idf rankings of obituary 0101
obituary_0101_df
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
      <th>TF-IDF Rank</th>
      <th>Term</th>
      <th>Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>term</td>
      <td>score</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>hoover</td>
      <td>387.08103243456884</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.0</td>
      <td>bureau</td>
      <td>66.57912486145588</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.0</td>
      <td>director</td>
      <td>56.62732476289798</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3.0</td>
      <td>tolson</td>
      <td>55.90993200745163</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>36265</th>
      <td>36264.0</td>
      <td>falcons</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>36266</th>
      <td>36265.0</td>
      <td>falcon</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>36267</th>
      <td>36266.0</td>
      <td>falangist</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>36268</th>
      <td>36267.0</td>
      <td>falange</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>36269</th>
      <td>36268.0</td>
      <td>zwilich</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>36270 rows × 3 columns</p>
</div>



### Reflection

This Programming Historian lesson provided me with a well-needed review of the idea of Term Frequency–Inverse Document Frequency (TF-IDF) and its potential provisioned to data science projects. TD-IDF is a method of deriving the most significant terms utilized by an author across a text; Significance is measured by this method in terms of the relation between a term's frequency in a text, and the same term's frequency in other texts. 

This lesson provided me with a set of texts upon which I was able to apply TF-IDF methodologies. Once applied, this method provides those words that are, for some reason, niche to a particular text. As a result, the term's emplacement in the text is made much more significant. Its application in the context of research would provide a small, yet incredibly insightful, glimpse into the essence of that text in focus.

TF-IDF's most notable potential would apply when studying a vast and extensive text corpus. Given that a researcher is not single-handedly able to analyze and gain an understanding of hundreds of texts at a time--an understanding at any level of depth--methods such as TD-IDF provide the researcher the opportunity to connect with all those texts in a significantly faster, more efficient manner that permits and promotes the development of their research. For those researchers seeking an in-depth and complete understanding of a text, this method would not be suitable, as it demands that the user interpret the significance of those terms said to be most important to a text based on TF-IDF analyses.
