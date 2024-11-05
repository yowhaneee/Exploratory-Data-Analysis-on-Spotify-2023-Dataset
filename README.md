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
Filling in every song that has a missing key with "C"
```python
df.iloc[:, 15] = df.iloc[:, 15].fillna("C")
df
```
Check for rows, columns, data types of each columns and missing values
```python
# Check the number of rows and columns, data types, and missing values
print(f"Number of rows: {df.shape[0]}, Number of columns: {df.shape[1]}")

# For data types of each column 
print(df.dtypes)

# Check for missing values in each column
print(df.isnull().sum())
```
Mean, Median, and Standard deviation of the streams column
```python
# Converting 'streams' into numeric then converting any non-numeric values to NaN
df['streams'] = pd.to_numeric(df['streams'], errors='coerce')

print(f"The mean, median, and standard deviation of the streams column")

# Drop rows with NaN in 'streams' (if any) to avoid calculation errors
df = df.dropna(subset=['streams'])

# Calculate mean, median, and standard deviation for 'streams' column
# Mean
print("Mean streams:", df['streams'].mean())

# Median
print("Median streams:", df['streams'].median())

# Standard deviation
print("Standard deviation of streams:", df['streams'].std())
```

Graph distribution of released_year and artist_count 
```python
# Plot the distribution and box plot for 'released_year'
plt.figure(figsize=(14, 6))

# Histogram for released_year
plt.subplot(1, 2, 1)
sns.histplot(df['released_year'], kde=True, bins=15)
plt.title('Distribution of Released Year')
plt.xlabel('Released Year')
plt.ylabel('Frequency')

# Box plot for released_year to identify outliers
plt.subplot(1, 2, 2)
sns.boxplot(x=df['released_year'])
plt.title('Box Plot of Released Year')
plt.xlabel('Released Year')

plt.tight_layout()
plt.show()

# Plot the distribution and box plot for 'artist_count'
plt.figure(figsize=(14, 6))

# Histogram for artist_count
plt.subplot(1, 2, 1)
sns.histplot(df['artist_count'], kde=True, bins=15)
plt.title('Distribution of Artist Count')
plt.xlabel('Artist Count')
plt.ylabel('Frequency')

# Box plot for artist_count to identify outliers
plt.subplot(1, 2, 2)
sns.boxplot(x=df['artist_count'])
plt.title('Box Plot of Artist Count')
plt.xlabel('Artist Count')

plt.tight_layout()
plt.show()

# Scatter plot to compare released_year and artist_count for the noticeable trends or outliers
plt.figure(figsize=(10, 6))
sns.scatterplot(x='released_year', y='artist_count', data=df)
plt.title('Comparison of Released Year and Artist Count')
plt.xlabel('Released Year')
plt.ylabel('Artist Count')
plt.show()
```
Top 5 tracks by streams
```python
top_tracks = df.nlargest(5, 'streams')
print("Top 5 Most Streamed Tracks:")
print(top_tracks[['track_name', 'streams']])
```
Top 5 most frequent artists based on the number of tracks in the dataset
```python
# Get the top 5 most frequent artists based on the number of tracks
top_artists = df['artist(s)_name'].value_counts().head(5).reset_index()
top_artists.columns = ['Artist Name', 'Number of Tracks']

# Display the result
print("Top 5 Most Frequent Artists:")
print(top_artists)

# Set up the plotting style
sns.set(style="whitegrid")

# Plot the top 5 most frequent artists
plt.figure(figsize=(10, 6))
sns.barplot(x="Number of Tracks", y="Artist Name", data=top_artists, palette="viridis", hue="Artist Name", dodge=False, legend=False)
plt.title('Top 5 Most Frequent Artists by Number of Tracks')
plt.xlabel('Number of Tracks')
plt.ylabel('Artist Name')
plt.show()
```
Plotting the number of tracks released per year
```python
plt.figure(figsize=(10, 6))
sns.countplot(x='released_year', data=df, palette="viridis", hue='released_year', dodge=False, legend=False)
plt.title('Number of Tracks Released Per Year')
plt.xlabel('Year')
plt.ylabel('Number of Tracks')
plt.xticks(rotation=45)
plt.show()
```
Number of tracks released per month
```python
# Calculate the count of tracks released per month and reset the index
track_month = df['released_month'].value_counts().reset_index()
track_month.columns = ['released_month', 'num_tracks']

# Sort by 'released_month' to get months in chronological order
track_month = track_month.sort_values(by='released_month')

# Create a dictionary for mapping month numbers to month names
months = {1: "January", 2: "February", 3: "March", 4: "April", 5: "May", 6: "June", 
          7: "July", 8: "August", 9: "September", 10: "October", 11: "November", 12: "December"}

# Map month numbers to names using the dictionary
track_month['name_month'] = track_month['released_month'].map(months)

# Display the DataFrame
print(track_month)

# Find the month with the highest track release count
max_month = track_month.loc[track_month['num_tracks'].idxmax()]
print(f"Month with highest release is {max_month['name_month']} with {max_month['num_tracks']} songs")

## Tracks released per month (if the dataset includes a 'released_month' column)
if 'released_month' in df.columns:
    plt.figure(figsize=(10, 6))
    sns.countplot(x='released_month', data=df, palette="magma", hue='released_month', legend=False)
    plt.title('Number of Tracks Released Per Month')
    plt.xlabel('Month')
    plt.ylabel('Number of Tracks')
    plt.show()
```
Correlation between streams and musical attributes
```python
musical_attributes = ['bpm', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']
correlations = df[['streams'] + musical_attributes].corr()
print("Correlations with Streams:")
print(correlations['streams'].sort_values(ascending=False))

# Plot heatmap
plt.figure(figsize=(9, 6))
sns.heatmap(correlations, annot=True, cmap="coolwarm", fmt=".1g", linewidths=0.5)
plt.show()
```
Correlation between danceability_% and energy_% and valence_% and acousticness_%
```python
# Calculate the correlation matrix for the relevant attributes
correl = df[['danceability_%', 'energy_%', 'valence_%', 'acousticness_%']].corr()

# Extract the specific correlations
danceability_energy_corr = correl.loc['danceability_%', 'energy_%']
valence_acousticness_corr = correl.loc['valence_%', 'acousticness_%']

# Print the correlation results
print(f"Correlation between danceability_% and energy_%: {danceability_energy_corr:.2f}")
print(f"Correlation between valence_% and acousticness_%: {valence_acousticness_corr:.2f}")
```
Comparing numbers of tracks in spotify_playlists, deezer_playlist, and apple_playlists 
```python
# Compare the number of tracks across Spotify, Deezer, and Apple playlists to analyze platform popularity and track inclusion.
platform_columns = ['in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']

# Ensure columns are numeric for aggregation
for col in platform_columns:
    if col in df.columns:
        df.loc[:,col] = pd.to_numeric(df[col], errors='coerce')  # Use .loc to modify columns directly in the original DataFrame
    else:
        print(f"Column '{col}' not found in dataset.")

# Calculate the sum of each platform column
platform_counts = df[platform_columns].sum()

# Display platform counts
print("Track Counts Across Platforms:")
print(platform_counts)

# Plot platform popularity
plt.figure(figsize=(8, 6))
sns.barplot(x=platform_counts.index, y=platform_counts.values, hue=None) 
plt.title('Platform Popularity Based on Track Counts')
plt.xlabel('Platform')
plt.ylabel('Number of Tracks')
plt.show()

# Based on the results in number 5, which is the 'sorted' data frame here are the Top 5 tracks with their respective count in the playlists
top_tracks = df.loc[[55, 179, 86, 620, 41], ['track_name', 'artist(s)_name', 'in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']]
print("Top 5 Tracks in Playlists:")
print(top_tracks)
```
Analyzing patterns among tracks with the same key or mode
```python
# Analyze patterns based on track key and mode
if 'key' in df.columns and 'mode' in df.columns:
    key_mode_patterns = df.groupby(['key', 'mode'])['streams'].mean().unstack()
    print("Average Streams by Key and Mode:")
    print(key_mode_patterns)

    # Heatmap of average streams by key and mode
    plt.figure(figsize=(10, 8))
    sns.heatmap(key_mode_patterns, annot=True, fmt=".0f", cmap="YlGnBu")
    plt.title('Average Streams by Key and Mode')
    plt.xlabel('Mode (0=Minor, 1=Major)')
    plt.ylabel('Key')
    plt.show()
```
Analysis to compare the most frequently appearing artists in playlists 
```python
# Ensure columns used for playlist counts are numeric for aggregation
for col in platform_columns:
    if col in df.columns:
        df.loc[:, col] = pd.to_numeric(df[col], errors='coerce')  # Use .loc to modify columns directly
    else:
        print(f"Column '{col}' not found in dataset.")

# Sum across specified columns to get 'total_playlists' and assign it to a new column in the DataFrame
df.loc[:, 'total_playlists'] = df[platform_columns].sum(axis=1)

# Top 5 artists by playlist appearances
top_playlist_artists = df.groupby('artist(s)_name')['total_playlists'].sum().nlargest(5)
print("\nTop 5 Artists by Playlist Appearances:")
print(top_playlist_artists)

# Visualization of the Top 5 Artists by Playlist Appearances
plt.figure(figsize=(10, 6))
sns.barplot(x=top_playlist_artists.index, y=top_playlist_artists.values, hue=top_playlist_artists.index, palette="viridis", legend=False)
plt.title('Top 5 Artists by Playlist Appearances')
plt.xlabel('Artist Name')
plt.ylabel('Total Playlist Appearances')
plt.xticks(rotation=45)  # Rotate x labels for better readability
plt.show()
```

## Key Insights

1. **Top Performers**: Certain tracks and artists dominate streams, with the top 5 most-streamed tracks and most frequent artists identified.
2. **Temporal Trends**: Analysis shows that certain years and months see more track releases, with potential seasonal peaks.
3. **Genre and Musical Attributes**: Attributes like BPM, energy, and danceability show a positive correlation with stream count.
4. **Platform Popularity**: Among platforms, Spotify playlists contain the most popular tracks, indicating its prominence in track discovery.

### Reflection:
This assignment was challenging, especially since I’m not very confident with coding yet. Kaggle and ChatGPT were very helpful resources, guiding me through the process. Working with a dataset that had missing values was new for me, but it was a great way to learn more about coding.

### References:

https://www.kaggle.com/code/maurosteban99/eda-l-spotify-songs-2023
https://www.kaggle.com/code/esraaahmedd111/most-streamed-spotify-songs-2023-visualization

Chatgpt.com


