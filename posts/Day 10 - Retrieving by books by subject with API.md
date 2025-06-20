---
title: Day 10 - Retrieving by books by subject with API
date: 2025-06-20
---
I used this [API](https://openlibrary.org/developers/api), to fetch books by subject. Here I created a simple function where it's possible to insert the desired subject and print them in a dataframe.

See code below - can be viewed on github [here
](https://github.com/JonSkogland/1000daysofcoding/blob/main/daily-projects/day10_20250620/day10.ipynb)


```
import requests
import pandas as pd

def retrieve_books_subject(subject):
    # API URL
    url = f"https://openlibrary.org/subjects/{subject}"
    
    params = {
        "details" : "true" # In order to receive author information
    }

    headers = {
        "accept": "application/json" # To specify json output format
    }

    # Get reponse to API
    response = requests.get(url, headers=headers, params=params)
    if response.status_code != 200:
        return print("API call failed")

    # Store response as json format
    data = response.json()
    if int(data["work_count"]) == 0:
        return print("No books in this category. Please search for another category")

    # Initialize books variable for later use
    books = []

    # Iterate through json object to retrieve specific information (title and author)
    for keys in data["works"]:
        # Store title of each book
        title = keys["title"]
        authors = []
        for key in keys["authors"]:
            # Append each author to a list
            authors.append(key["name"])

        # Insert title and authors into books variable
        books.append({"title" : title, "authors" : authors})

    # Make list into dataframe
    df_books = pd.DataFrame(books)

    # Remove square brackets from authors columns
    df_books["authors"] = df_books["authors"].apply(lambda x: ", ".join(x))

    # Return dataframe of fetched books for display
    return df_books
```
