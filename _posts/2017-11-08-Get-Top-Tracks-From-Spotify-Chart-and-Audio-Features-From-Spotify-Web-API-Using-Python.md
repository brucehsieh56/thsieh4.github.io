---
layout: default
title: Get Top Tracks From Spotify Chart and Audio Features From Spotify Web API Using Python
date: 2017-11-08
category: DataScience
tags: [Python, DataCollecion]
---

---

### Motivation
Frequently, the data we analyzed came from other websites like Kaggle or UC Irvine Machine Learning Repository. In practice, full data set can be hard to get, and the process of manually collecting data could be time-consuming and expensive. Fortunately, many websites have provided Web API for us to get the data from their database quickly and efficiently. Even more fortunately, there are many `Python` packages that help us retreive those data without understanding the detail of Web API and reading Web API documentation (Thank you!! open source developers).

---

In this post, we will collect data from [Spotify Chart](https://spotifycharts.com/regional) and [Spotify Web API](https://developer.spotify.com/web-api/). Basically, Spotify Chart provides weekly and daily Top 200 tracks. More importantly, they have `csv` file that we can download directly. Alternatively, Spotify Web API offers audio features for each song, such as *loudness* and *tempo*, which can be obtained via their Web API.

But, why choosing musical data? 

> Because all the cool kids are listening to music!

### Data Collection From Spotify Chart
Let's import the usual Python library for data science.


```python
# import library
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

Next, we use `pandas` to get the [Top 200 Tracks on 2017-11-05](https://spotifycharts.com/regional/global/daily/2017-11-05) data and save it as `DataFrame` object.


```python
# link to Spotify Chart on 2017-11-05
url = 'https://spotifycharts.com/regional/global/daily/2017-11-05/download'

# get data as 'DataFrame' object
df = pd.read_csv(url)

# have quick look
df.head()
```




<div class="scroll">
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe table-striped">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Position</th>
      <th>Track Name</th>
      <th>Artist</th>
      <th>Streams</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>rockstar</td>
      <td>Post Malone</td>
      <td>5086329</td>
      <td>https://open.spotify.com/track/7wGoVu4Dady5GV0...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Havana</td>
      <td>Camila Cabello</td>
      <td>4219170</td>
      <td>https://open.spotify.com/track/0ofbQMrRDsUaVKq...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Too Good At Goodbyes</td>
      <td>Sam Smith</td>
      <td>3652071</td>
      <td>https://open.spotify.com/track/1mXVgsBdtIVeCLJ...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>New Rules</td>
      <td>Dua Lipa</td>
      <td>3295470</td>
      <td>https://open.spotify.com/track/2ekn2ttSfGqwhha...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>What Lovers Do (feat. SZA)</td>
      <td>Maroon 5</td>
      <td>2994195</td>
      <td>https://open.spotify.com/track/4nYsmWkuTaowTMy...</td>
    </tr>
  </tbody>
</table>
</div>



Spotify Chart gives us five columns **Position**, **Track Name**, **Artist**, **Streams**, and **URL**. Let's do a simple EDA to find out *who has the most songs being listed in Top on **Nov. 05 2017***.


```python
df.Artist.value_counts().head(10)
```




    Sam Smith         16
    Ed Sheeran         7
    Post Malone        6
    XXXTENTACION       4
    Taylor Swift       4
    21 Savage          4
    Kendrick Lamar     3
    Clean Bandit       3
    Kygo               3
    Lil Uzi Vert       3
    Name: Artist, dtype: int64



Looks like **Sam Smith** is the superstar on **Nov. 05**. *How about the **total number of streams**?*


```python
df.groupby(['Artist'])['Streams'].sum().sort_values(ascending=False).head(10)
```




    Artist
    Sam Smith         14160873
    Post Malone       10948100
    Ed Sheeran         7242014
    Camila Cabello     4650867
    J Balvin           4108988
    21 Savage          3899862
    Maroon 5           3722522
    Charlie Puth       3609415
    Taylor Swift       3488365
    Dua Lipa           3295470
    Name: Streams, dtype: int64



It seems like **Sam Smith** and **Post Malone** made lots of money from Spotify. Cool!!!

### Extracting Audio Features From Spotify Web API
We already have Top 200 tracks from **Spotify Chart**, but we can get **[audio features](https://developer.spotify.com/web-api/get-audio-features/)** using **Spotify Web API**. To do this, we need

- **[Spotify ID](https://developer.spotify.com/web-api/user-guide/#spotify-uris-and-ids)** of a track.
- Python package **[spotipy](https://github.com/plamere/spotipy)** (It's spoty-**py**, not spoti-**fy**).
- **Client ID** and **Client Secret** from [Spotify Developer](https://developer.spotify.com/my-applications/#!/).

Basically, **Spotify ID** is a specific *code* or *name* representing a song, and we need this ID to retrieve audio features. Fortunately, Spotify ID is the last few characters of the column `URL` in our `DataFrame` `df`, so we don't have to look up this ID one by one for every song. Instead, all we need to do is some string operation. On the other hand, **Spotipy** is a `Python` library to help us connect to Spotify Web API (very) easily. Finally, **Client ID** and **Client Secret** are somewhat like account and password for us to get data from Spotify Web API.

---

Once having the **spotipy** package installed and a set of **Client ID** and **Client Secret**, we can continue on.


```python
# import libaray 
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials
```


```python
# get Spotify ID
df['Spotify_ID'] = df.apply(lambda row: row['URL'].split('/')[-1], axis=1)

# have a quick look
df.head()
```




<div class="scroll">

<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe table-striped">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Position</th>
      <th>Track Name</th>
      <th>Artist</th>
      <th>Streams</th>
      <th>URL</th>
      <th>Spotify_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>rockstar</td>
      <td>Post Malone</td>
      <td>5086329</td>
      <td>https://open.spotify.com/track/7wGoVu4Dady5GV0...</td>
      <td>7wGoVu4Dady5GV0Sv4UIsx</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Havana</td>
      <td>Camila Cabello</td>
      <td>4219170</td>
      <td>https://open.spotify.com/track/0ofbQMrRDsUaVKq...</td>
      <td>0ofbQMrRDsUaVKq2mGLEAb</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Too Good At Goodbyes</td>
      <td>Sam Smith</td>
      <td>3652071</td>
      <td>https://open.spotify.com/track/1mXVgsBdtIVeCLJ...</td>
      <td>1mXVgsBdtIVeCLJnSnmtdV</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>New Rules</td>
      <td>Dua Lipa</td>
      <td>3295470</td>
      <td>https://open.spotify.com/track/2ekn2ttSfGqwhha...</td>
      <td>2ekn2ttSfGqwhhate0LSR0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>What Lovers Do (feat. SZA)</td>
      <td>Maroon 5</td>
      <td>2994195</td>
      <td>https://open.spotify.com/track/4nYsmWkuTaowTMy...</td>
      <td>4nYsmWkuTaowTMy4gskmBw</td>
    </tr>
  </tbody>
</table>
</div>



The last column **Spotify_ID** is the specific ID that allows us to extract audio features from Spotfy Web API. Let's connect to Spotify Web API.


```python
# setup Spotify client ID and password
client_id = 'YOUR CLIENT ID HERE'
client_secret = 'YOUR CLIENT SECRET HERE'

# connect to Spotify Web API via spotipy package 
client_credentials_manager = SpotifyClientCredentials(client_id, client_secret)
sp = spotipy.Spotify(client_credentials_manager=client_credentials_manager)
```

Now we use the first Spotify ID to have a quick look at what kinds of audio features does Spotfy Web API have. To do this, we can use the function `audio_features` from `spotipy`.


```python
# use Spotify ID of the first song
this_id = df.Spotify_ID[0]

# take a look for audio features of the first song
sp.audio_features(this_id)[0]
```




    {'acousticness': 0.13,
     'analysis_url': 'https://api.spotify.com/v1/audio-analysis/7wGoVu4Dady5GV0Sv4UIsx',
     'danceability': 0.577,
     'duration_ms': 218320,
     'energy': 0.522,
     'id': '7wGoVu4Dady5GV0Sv4UIsx',
     'instrumentalness': 9.03e-05,
     'key': 5,
     'liveness': 0.142,
     'loudness': -6.594,
     'mode': 0,
     'speechiness': 0.0984,
     'tempo': 159.772,
     'time_signature': 4,
     'track_href': 'https://api.spotify.com/v1/tracks/7wGoVu4Dady5GV0Sv4UIsx',
     'type': 'audio_features',
     'uri': 'spotify:track:7wGoVu4Dady5GV0Sv4UIsx',
     'valence': 0.119}



Wow, Spotify provides many interesting audio features of a song!! [Here](https://developer.spotify.com/web-api/get-audio-features/) is the description of each feature.

---

Personally, I think most of the top songs are quite like **"Party Songs"** in recent years. So let's extract the audio feature **danceability**.


```python
# get the audio feature 'danceability' for every song 
# (this will take a few minutes)
df['danceability'] = df.apply(lambda row: sp.audio_features(row['Spotify_ID'])[0]['danceability'], axis=1)

# have a quick look
df.head()
```




<div class="scroll">
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe table-striped">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Position</th>
      <th>Track Name</th>
      <th>Artist</th>
      <th>Streams</th>
      <th>URL</th>
      <th>Spotify_ID</th>
      <th>danceability</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>rockstar</td>
      <td>Post Malone</td>
      <td>5086329</td>
      <td>https://open.spotify.com/track/7wGoVu4Dady5GV0...</td>
      <td>7wGoVu4Dady5GV0Sv4UIsx</td>
      <td>0.577</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Havana</td>
      <td>Camila Cabello</td>
      <td>4219170</td>
      <td>https://open.spotify.com/track/0ofbQMrRDsUaVKq...</td>
      <td>0ofbQMrRDsUaVKq2mGLEAb</td>
      <td>0.768</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Too Good At Goodbyes</td>
      <td>Sam Smith</td>
      <td>3652071</td>
      <td>https://open.spotify.com/track/1mXVgsBdtIVeCLJ...</td>
      <td>1mXVgsBdtIVeCLJnSnmtdV</td>
      <td>0.698</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>New Rules</td>
      <td>Dua Lipa</td>
      <td>3295470</td>
      <td>https://open.spotify.com/track/2ekn2ttSfGqwhha...</td>
      <td>2ekn2ttSfGqwhhate0LSR0</td>
      <td>0.756</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>What Lovers Do (feat. SZA)</td>
      <td>Maroon 5</td>
      <td>2994195</td>
      <td>https://open.spotify.com/track/4nYsmWkuTaowTMy...</td>
      <td>4nYsmWkuTaowTMy4gskmBw</td>
      <td>0.792</td>
    </tr>
  </tbody>
</table>
</div>

Now we have a new column, **danceability**. 

---

For simple graphical analysis, we can use scatterplot to view the distribution of this feature.

```python
# scatterplot for danceability across 200 songs
df.plot('Position', 'danceability', kind='scatter', figsize=(12,6), marker='o', s=45, color='y', edgecolors='k')

# add averaged danceability
plt.hlines(df.danceability.mean(), 0, 200, alpha=.6, label='Mean of danceability')

# add 0.5 line
plt.hlines(.5, 0, 200, alpha=.8, color='navy', linestyles='dashed', label='Danceability = 0.5')

# set properties
plt.legend()
plt.margins(.01)
plt.show()
```

<div class="scroll">
  <img src="/figure/2017Nov08_scatterplot.png" alt="scatterplot">
</div>

According to Spotify Web API [documentation](https://developer.spotify.com/web-api/get-audio-features/),

> *Danceability describes how suitable a track is for dancing... A value of 0.0 is least danceable and 1.0 is most danceable.*

From the above scatterplot, we can see that most of the top songs having **danceability** higher than 0.5, which shows that people who using Spotfy like high-danceability songs.

---

In this post, we learned that Spotify Web API have many interesting audio features (e.g. acousticness, danceability, energy, and so on) for music anlysis. Here, we use only one of those features for demonstration. However, I believe it would be very interesting if we use all of them for musical data analysis.

That’s it for this time, Happy Data Collecting!!!
