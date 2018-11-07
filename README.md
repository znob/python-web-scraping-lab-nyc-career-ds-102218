
# Web Scraping Lab Solutions

## Questions

Resident Advisor is an events listing website for electronic music.

Go to www.residentadvisor.net/events.  This is the url we'll be starting with for this lab.  For question 1, just use this url.  In the next two, you'll use country and region in the format: http://www.residentadvisor.net/country/region/ i.e. us/losangeles/.  Be sure to explore the web pages in both the browswer and the HTML file.  You'll need both to really understand what's going on.

1. Which venues are hosting events this week?
2. Make a function which returns the events this week given region and country (this will take two arguments)
    - return the event name, link, and list of artists
    - function returns list of ['event name', 'www.linkaddress.com', ['artist1','artist2','artist3']]
3. Create a function which returns the users attending 
4. Bonus


### Question 1 - Which venues are hosting events this week?


```python
import requests
from bs4 import BeautifulSoup

r = requests.get('https://www.residentadvisor.net/events')
c = r.content
soup = BeautifulSoup(c, 'html.parser')
```


```python
set([i.find('span').text.lstrip('at ') for i in soup.find_all('h1', class_ = 'event-title')])
```




    {'101bklyn',
     '291 Hooper St',
     '99 Scott Ave',
     'Alphaville',
     'Analog Bkny',
     'Avant Gardner',
     'Bar 13',
     'Better Days',
     'Black Flamingo',
     'Bossa Nova Civic Club',
     'Chelsea Music Hall',
     'Cielo',
     'El Cortez / The Safari Room',
     'Elsewhere',
     'Eris',
     'Forest City Lodge 180',
     'Future Space',
     'Good Room',
     'H0L0',
     'House Of Yes',
     'Japan Society',
     'Jing Fong Restaurant',
     "John's House of Music",
     'Jupiter Disco',
     'Knockdown Center',
     'Le Bain',
     'Mood Ring',
     'Nowadays',
     'Nublu',
     'Output',
     'Pioneer Works',
     'Ponyboy',
     'Public Arts',
     'Rose Gold',
     'Schimanski',
     'Studio 299',
     'Sugar Hill Disco',
     'Sunnyvale',
     'Sutherland - Three Dollar Bill',
     'TBA - Brooklyn',
     'TBA - New York',
     'TBA Brooklyn',
     'The Hart',
     'The Kings Hall',
     'Trans Pecos',
     'Tropicana',
     'Tsunami Bass Brown Note',
     'Unnamed Gallery'}



### Question 2 - Write a function to which returns the events this week given region and country.


```python
def find_events(country, region):
    events = []
    soup = BeautifulSoup(requests.get('https://www.residentadvisor.net/events/'+country+'/'+region).content)
    for i in soup.find_all('article', class_ = 'event-item'):
        title = i.find('h1', class_ = 'event-title').text
        link = 'http://residentadvisor.net' + i.find('h1', class_ = 'event-title').find('a')['href']
        try:
            artists = i.find('div', class_ = 'grey event-lineup').text.split(', ')
        except:
            artists = []
        events.append([title,link,artists])
    return events
```


```python
find_events('us','austin')[0]
```




    ['Heard presents: Infected Mushroom Live at Empire Control Room & Garage',
     'http://residentadvisor.net/events/1154329',
     ['Infected Mushroom']]



### Question 3 - Create a function which returns the numbers of users attending each event this week, given country and region.  Then plot a histogram


```python
def users_attending(country, region):
    users = []
    soup = BeautifulSoup(requests.get('https://www.residentadvisor.net/events/'+country+'/'+region).content)
    for i in soup.find_all('p', class_ = 'attending'):
        users.append(int(i.text.rstrip(' Attending')))
    return users
```


```python
import plotly.offline as offline
import plotly.graph_objs as go

offline.init_notebook_mode()

offline.iplot([go.Histogram(x = users_attending('us','losangeles'))])
```

## Bonus: Build object relations between artists, venues, and events with sqlalchemy!
Think about what each table should include - URLs, dates, etc.


```python

```
