# TMDB Top Rated Movies Dataset (API-Based)

## Overview
This project builds a structured dataset of top-rated movies using the TMDB API. It fetches multiple pages of movie data, extracts key features, and combines everything into a single pandas DataFrame for analysis.

The dataset is suitable for:
- Data analysis (EDA)
- Machine learning projects
- Recommendation systems
- Portfolio showcase

---

## Features
- Fetches data from TMDB API
- Handles pagination automatically
- Extracts important movie attributes
- Cleans and merges data into one DataFrame
- Ready for CSV export or ML use

---

## Tech Stack
- Python
- Pandas
- Requests
- TMDB API

---

## Dataset Columns
- `id` → Unique movie ID  
- `title` → Movie title  
- `release_date` → Release date  
- `vote_average` → Average rating  
- `vote_count` → Number of votes  
- `popularity` → Popularity score  

---

## Full Python Code

```python
import requests
import pandas as pd

API_KEY = "YOUR_API_KEY"

movies = pd.DataFrame()

# TMDB usually has limited valid pages (check total_pages dynamically if needed)
for i in range(1, 10):

    url = "https://api.themoviedb.org/3/movie/top_rated"

    params = {
        "api_key": API_KEY,
        "language": "en-US",
        "page": i
    }

    response = requests.get(url, params=params)
    data = response.json()

    # Stop if API returns error instead of results
    if "results" not in data:
        print(f"Stopped at page {i}")
        break

    temp_df = pd.DataFrame(data["results"])[
        ["id", "title", "release_date", "vote_average", "vote_count", "popularity"]
    ]

    movies = pd.concat([movies, temp_df], ignore_index=True)

# Reset index
movies.reset_index(drop=True, inplace=True)

# Show dataset
print(movies.head())

# Optional: Save dataset
movies.to_csv("tmdb_top_rated_movies.csv", index=False)


#Output Example
   id                title  release_date  vote_average  vote_count  popularity
0  278  The Shawshank Redemption   1994-09-23          8.7       25000      120.5
1  238        The Godfather        1972-03-14          8.7       19000      110.2
