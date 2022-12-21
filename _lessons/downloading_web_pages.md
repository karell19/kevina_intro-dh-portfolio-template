---
layout: page
title: Downloading Web Pages with Python & Downloading Multiple Records Using Query Strings
description: These Programming Historian lessons have taught me, and allowed me to develop skills pertinent to the engagement with data and information stored online, concurrently with such data that is stored on my local machine.
---
Kevin Arellano Flores

CLS-0161 -- Introduction to Digital Humanities

Dr. Saxton


```python
import urllib.request, urllib.error, urllib.parse
```


```python
url = 'http://www.oldbaileyonline.org/browse.jsp?id=t17800628-33&div=t17800628-33'

# deriving contents of a web-page into string format
response = urllib.request.urlopen(url)
webContent = response.read().decode('UTF-8')

print(webContent[0:300])
```

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
    <head>
    	<title>Browse - Central Criminal Court</title>
    	<meta http-equiv="content-type" content=



```python
# opening file to which html data is written
f = open('obo-t17800628-33.html', 'w')
f.write(webContent)
f.close
```




    <function TextIOWrapper.close()>




```python
def getSearchResults(query, kwparse, fromYear, fromMonth, toYear, toMonth, entries):
    import urllib.request, math, os, re, time
    
    # creates new directory for search result file storage
    cleanQuery = re.sub(r'\W+', '', query)
    if not os.path.exists(cleanQuery):
        os.makedirs(cleanQuery)
    
    startValue = 0
    
    # calculates number of files to be downloaded
    pageCount = entries / 10
    pageCount = math.ceil(pageCount)
    
    for pages in range(1, pageCount + 1):
        # the seperated parts of a url
        url = 'https://www.oldbaileyonline.org/search.jsp?gen=1&form=searchHomePage&_divs_fulltext='
        url += query
        url += '&kwparse=' + kwparse
        url += '&_divs_div0Type_div1Type=sessionsPaper_trialAccount'
        url += '&fromYear=' + fromYear
        url += '&fromMonth=' + fromMonth
        url += '&toYear=' + toYear
        url += '&toMonth=' + toMonth
        url += '&start=' + str(startValue)
        url += '&count=0'

        # downloads the web-page, saving the result
        response = urllib.request.urlopen(url)
        webContent = response.read().decode('UTF-8')
        filename = cleanQuery + '/' + 'search-result' + str(startValue)
        f = open(filename + '.html', 'w')
        f.write(webContent)
        f.close

        startValue = startValue + 10
        
        time.sleep(3)
```


```python
query = 'washington'

getSearchResults(query, 'advanced', '1700', '00', '1750', '99', 13)
```


```python
def getIndivTrials(query):
    import urllib.request, urllib.error, time, os, re, socket
    from os.path import join as pjoin
    
    failedAttempts = []
    
    cleanQuery = re.sub(r'\W+', '', query)
    searchResults = os.listdir(cleanQuery)
    
    urls = []
    
    # loop that finds the page for search results
    for files in searchResults:
        if files.find('search-result') != -1:
            f = open(cleanQuery + '/' + files, 'r')
            text = f.read().split(' ')
            f.close()
            
            # loop searching for trial IDs
            for words in text:
                if words.find('browse.jsp?id=') != -1:
                    urls.append(words[words.find('id=') + 3: words.find('&')])
    
    for items in urls:
        url = "http://www.oldbaileyonline.org/print.jsp?div=" + items
        
        # downloads page linked by url 
        socket.setdefaulttimeout(10)
        try:
            response = urllib.request.urlopen(url)
            webContent = response.read().decode('UTF-8')

            # places newly named file in newly made directory
            filename = items + '.html'
            filePath = pjoin(cleanQuery, filename)

            # saves newly made file
            f = open(filePath, 'w')
            f.write(webContent)
            f.close
        except urllib.error.URLError:
            failedAttempts.append(url)
        
        # produces a 3 second pause
        time.sleep(3)
    
    print("failed to download: " + str(failedAttempts))
```


```python
getIndivTrials('washington')
```

    failed to download: []



```python
getSearchResults(query, 'advanced', '1700', '00', '1750', '99', 13)
getIndivTrials(query)
```

    failed to download: []


### Reflection

For this Programming Historian assignment, I actually completed two Porgramming Historian lessons, given that they were intricately related to one-another, and thus, I wanted to learn as broadly as possible about their topics.

The first of these lessons, "Downloading Web Pages with Python," was a welcomed review to the processes involved in opening and accessing data that it derived from a web-page. While we have worked to write code that performs this action in the past, this lesson was an opportunity for me to further engage with this topic and the html code that goes into making a web-page.

This first lesson was, however, limited. I chose to venture further into this topic by completing the closely related lesson on "Downloading Multiple Records Using Query Strings." This second lesson expanded on the first, teaching me how to download and access the content of multiple web-pages through a single Python script.I was able to implement code that is able to download and save data and information from an html web-page onto a user's local hard drive.

As I seek to expand further into data science, I feel that this skill, of engaging with content on the web simultaneously with that stored on my local computer, would be of great importance; I expect that in such a position I would be working primarily with such content, and so, these lessons have been an invaluable opportunity to establish a foundational familiarity in these skills and areas.
