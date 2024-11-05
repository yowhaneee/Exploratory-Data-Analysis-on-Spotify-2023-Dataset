# Exploratory-Data-Analysis-on-Spotify-2023-Dataset

# Spotify 2023 Tracks - Exploratory Data Analysis

This repository contains an exploratory data analysis (EDA) of Spotify's top tracks for 2023. The purpose of this analysis is to uncover trends, correlations, and insights related to music characteristics and track popularity.

## Dataset

The dataset includes various features of popular Spotify tracks, such as:
- **Streams**: Number of streams a track received.
- **Release Date**: Year and month the track was released.
- **Musical Attributes**: Information like beats per minute (BPM), danceability, and energy level.
- **Platform Popularity**: Indicators of track presence across platforms (e.g., Spotify, Deezer, Apple Music).

## Repository Structure

- **`data/`**: Contains the dataset (`spotify-2023.csv`).
- **`notebooks/`**: Contains the Jupyter Notebook with the EDA (`EDA_Spotify_Tracks.ipynb`).
- **`README.md`**: This file, containing project documentation.

## Exploratory Data Analysis

The EDA is structured as follows:

### 1. Overview of Dataset
   - **Rows and Columns**: Provides the dataset's dimensions.
   - **Data Types and Missing Values**: Summary of data types for each column and any missing values.

### 2. Basic Descriptive Statistics
   - **Streams**: Mean, median, and standard deviation of streams.
   - **Distributions**: Visualization of `released_year` and `artist_count`, highlighting any notable trends or outliers.

### 3. Top Performers
   - **Most Streamed Tracks**: Displays the top 5 tracks by stream count.
   - **Frequent Artists**: Lists the top 5 artists with the most tracks in the dataset.

### 4. Temporal Trends
   - **Annual Trends**: Tracks released over the years, visualized per year.
   - **Monthly Trends**: Examines if track releases follow a monthly pattern.

### 5. Genre and Music Characteristics
   - **Correlation Analysis**: Correlation between streams and attributes like BPM, danceability, and energy.
   - **Attribute Relationships**: Examines relationships between danceability & energy, valence & acousticness.

### 6. Platform Popularity
   - **Platform Comparison**: Analysis of track counts on platforms (Spotify, Deezer, Apple Music) to determine where popular tracks are most represented.

### 7. Advanced Analysis
   - **Patterns by Key and Mode**: Looks for patterns in track popularity based on musical key and mode (Major vs. Minor).
   - **Artist and Genre Analysis**: Compares the most frequent artists in playlists and charts.

## Codes

Importing the libraries needed 
```python
# Start
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

Load the file
```python
#Read the file and make sure to use the "encoding = 'ISO-8859-1'" for it to load or else it will not load
df = pd.read_csv('spotify-2023.csv', encoding = 'ISO-8859-1')
df
```

## Key Insights

1. **Top Performers**: Certain tracks and artists dominate streams, with the top 5 most-streamed tracks and most frequent artists identified.
2. **Temporal Trends**: Analysis shows that certain years and months see more track releases, with potential seasonal peaks.
3. **Genre and Musical Attributes**: Attributes like BPM, energy, and danceability show a positive correlation with stream count.
4. **Platform Popularity**: Among platforms, Spotify playlists contain the most popular tracks, indicating its prominence in track discovery.

## Setup Instructions

### Prerequisites

- Python 3.x
- Jupyter Notebook (for running the `.ipynb` file)

### References

https://www.kaggle.com/code/maurosteban99/eda-l-spotify-songs-2023

