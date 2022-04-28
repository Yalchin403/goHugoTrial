---
title: "Accent and Case Insensitive Search and Minimum Distance Calculator in Pymongo"
date: 2022-04-28T14:22:27+04:00
draft: false
toc: false
cover: "blog-images/pymongo.jpeg"
useRelativeCover: true
images:
tags:
  - Python
  - Mongodb
  - Databases
---

# Ellaborate case

Before starting with set up and everything, I would love to confess that I find Mongodb and Python combination as **pain in the ass**. Recently I have got an job offer as Python/Flask developer in one of the local companies in my country. They are using Mongodb as their production database. They requested a little bit of task to accomplish before we move on to HR interview. So the task was about coding autocomplete search logic and also finding the closest location to our current location from the database. Task seems interesting and pretty much easy to solve, at least for me as a Django developer who have used Postgresql as default database in most of the cases. However, seemed like things don't go so well with Mongodb in django. It seemed like if Python is envolved, we normally have two pypi packages two use, namely [pymongo](https://pymongo.readthedocs.io/en/stable/) and [mongoengine](https://docs.mongoengine.org/). I was familiar with pymongo as in my [telegram bot](https://t.me/mp3converteryoutubebot) I use Mongodb as database and use pymongo to implement database actions. Hence, below I will explain how I implemented case and accent insensitive search and minimum distance finding via pymongo.
___

# Database setup

First we need to make sure we have all the dependencies to be able to run our Python modules successfully, I will name then, and also drop my requirements.txt content as well:

Dependencies:
  - pymongo
  - sqlparse
  - geographiclib
  - geopy

Here is the content of `requirements.txt` which you can use by running `pip install -r requirements.txt`:

```
backports.zoneinfo==0.2.1
geographiclib==1.52
geopy==2.2.0
pymongo==4.1.1
python-dotenv==0.20.0
sqlparse==0.2.4
```

Now, you can create local database or use [Mongodb atlas](https://cloud.mongodb.com) which I personally prefered. Create database, and also get your `URI` which you will use to connect to your database via utilizing pymongo. You can watch [this](https://www.youtube.com/watch?v=VQnmcBnguPY) video if you don't know how to set up remote mongodb database and configure connection via pymongo. If you want to keep your atlas account safe, do not include `URI` inside your project direcly but use `env` and some library like [python-dotenv](https://pypi.org/project/python-dotenv/) to load `.env` content into os enviroment variables. Here is my database set up that you can use:

```py
from dotenv import load_dotenv
import os
from pymongo import MongoClient


load_dotenv()

DATABASE_URI = os.getenv("DATABASE_URI")

client = MongoClient(DATABASE_URI)

client = MongoClient(DATABASE_URI)

db = client.get_database('autocompletedb') # here autocompletedb is the name of database

# This is added so that many files can reuse the function get_database()
addresses_table = db.addresses # here the addresses is the name of table inside autocomplete database
```
___

# Minimum distance calculation

MongoDB supports query operations on geospatial data so we can take advantage of this functionality without writing our own formula two calculate minimum distance between two coordinates containing latitude and longitude. I found [this](https://towardsdatascience.com/finding-distance-between-two-latitudes-and-longitudes-in-python-43e92d6829ff) very interesting if you are interested in the math side of the case. If not, you can use below snippet:

First ensure that you have a proper index in the database:

```py
from pymongo import MongoClient, GEOSPHERE

client = MongoClient()
collection = db['COLLECTION_NAME']

# create index
collection.create_index([('geo', GEOSPHERE)])

# store GPS coordinates in db
```

And make sure you keep your latitude and longitude that way:

```py
lat = ...
lon = ...
item = {
  'geo': {
      'type': "Point",
      'coordinates': [lon, lat]
  }  
}
collection.insert_one(item)  
```

As you have index and proper data in place, you can start retrieving it:

```py
def calculate_distance(coordinates):
    latitude, longitude = coordinates
    items = addresses_table.find({'geo': {"$nearSphere": [latitude, longitude], "$maxDistance": 5000}})
    for item in items:
        return item
```

This will take coordinates as tuple and from the database and find the coordinates with minimum distance relative to the given coordinates (`max_distance = 5 km`)


# Accent and Case Insensitive Search

This part was a bit tricky, as I couldn't make the accent insensitive search work using different function shown in the documentation, they somehow didn't work properly. I used regular expression with `i` option to ignore case sensitivity and searched for the given keywork from different fields in my collection. Additionally to consider accent, based on keyword, I created other versions of the keyword (considering accent). For example if someone searchs for Helen, I will be searching for both `Helen` and `Hélen` and vice versa. Below its implemnetation is given:

```py
def normalize_keyword(keyword):
  keyword = keyword.lower()
  variants = {'ə': ['e', 'a'],
        'ı': ['i'],
        'ç': ['c'],
        'ş': ['s'],
        'ö': ['o'],
        'ğ': ['g'],
        'e': ['ə', 'a'],
        'a': ['ə', 'e'],
        'i': ['ı'],
        'c': ['ç'],
        's': ['ş'],
        'o': ['ö'],
        'g': ['ğ']
        }

  possibilities = []

  for letter in keyword:

    if letter in variants:
      substitutes = variants[letter]

      for substitute in substitutes:
        possibilities.append(keyword.replace(letter, substitute))

  possibilities = set(possibilities)

  return possibilities
```

As mentioned with the help of above function and regex search we implement the search funcionality:

```py
def apply_multiple_filters(keywords):
    keywords = normalize_keyword(keywords)
    results = []

    for keyword in keywords:

        results.append(addresses_table.find({'$or': [
                            {'city': {'$regex': keyword, '$options': 'i'}},
                            {'street': {'$regex': keyword, '$options': 'i'}},
                            {'street_number': {'$regex': keyword, '$options': 'i'}}
                        ]
                    }
        ))

    return results
```