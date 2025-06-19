---
title: Day 9 - Retrieving image with API
date: 2025-06-19
---

I initially wanted to fetch all the youtube videos from my favourite yoga youtuber, Tim Senesi, however the Google API proved too difficult at this time to be able to retrieve data from youtube.

Instead I created a simple program to fetch and download the astronomy picture of the day from NASA.

### Code from program:

```
# Libraries

import requests

from dotenv import load_dotenv

import os
  

# Load dot environment files

load_dotenv()


# Get api key from environment file

API_KEY = os.getenv("API_KEY")

# The url of Nasas image api

api_url = "https://api.nasa.gov/planetary/apod"

# Inserting api key when calling API

params = {

"api_key": API_KEY

}

# Get response

response = requests.get(api_url, params=params)

# Convert response to json format

data = response.json()

# Saving url of image to variable

image_url = data["hdurl"]

# Get request to fetch data from image url

img_data = requests.get(image_url).content


# Open new file and write image data to file, where title of file is the title of the image

with open(f"{data["title"]}.jpg", 'wb') as handler:

handler.write(img_data)
```
