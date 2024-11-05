# Exploratory-Data-Analysis-on-Spotify-2023-Dataset

Author: Arabelle Y. Villarin

Section: 2ECE-D

Date Submitted: November 9, 2024
___

### What is an Exploratory Data Analysis? 

Exploratory Data Analysis (EDA) is a method of analyzing data, focusing on its main characteristics. This involves using statistical visualizations to help us understand the data better, detect errors, and test assumptions. It is one of the first steps before diving into more complex data analysis. 

### What is the goal of this deliverable? 

The goal is to explore and analyze a dataset of popular Spotify tracks from 2023. By examining and visualizing the data, we aim to uncover trends, popular artists, and other insights about the most-streamed songs. This will give a clear overview of the dataset and highlight key patterns. 

_____

Before starting the data analysis, I first imported the Python Libraries (Pandas, Matplotlib, and Seaborn):

````
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
````

After importing the necessary libraries, I began by reading the CSV file to be used for the dataset analysis:

````
df = pd.read_csv('spotify-2023.csv', encoding='latin1')
````
Initially, I encountered a problem because the file was not encoded in UTF-8, the standard character encoding for many datasets. UTF-8 is often the default encoding when working with CSV files in Python, but some files may use different encodings, such as 'latin1' (also known as ISO-8859-1). Using the correct encoding (in this case, 'latin1') solved the problem, allowing the file to be read correctly.

## Overview of Dataet

In the first part of this analysis, we will focus on gathering basic information from the 2023 Spotify dataset. This includes finding the number of rows and columns, as well as identifying the data type of each column. This information is important because it helps us understand the structure of the dataset and ensures we can apply the right methods in the next steps of the analysis.

### How many rows and columns does the dataset contain?¶

To get the rows and columns of the dataset, I used the variable named 'shape'. By using the attribute of pandas '.shape', I was able to get the dimensions of the DataFrame easily.

````
shape = df.shape
print("Rows and Columns: ", shape)
````

### What are the data types of each column? Are there any missing values?

>>desc
 
````
dtype = df.dtypes
print("Data types of each column:\n", dtype)
````
>>>>result

>>desc

to get missing values:
````
missing_values = df.isnull().sum().sum()
print("Missing values: ", missing_values)
````
## Basic Descriptive Statistics

>>desc

### What are the mean, median, and standard deviation of the streams column?

>>desc

````
mean = df['streams'].mean
median = df['streams'].median
sd = df['streams'].std

print("Mean:\n",mean,"\n\n", "Median:\n",median,"\n\n","Standard Deviation:\n",sd)
````

>>> result

### What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

>>desc

````
#Create a variable for released_year and artist_count
released_year = df['released_year']
artist_count = df['artist_count']

#Create separate histograms for each variable
plt.figure(figsize=(8, 6))  #Input a figure size for a better visual

#Histogram for released_year
plt.subplot(1, 2, 1)  #Create subplot at position 1, 1 (top left)
plt.hist(released_year, bins=10)
plt.xlabel('released_year')
plt.title('Distribution of released_year')

#Histogram for artist_count
plt.subplot(1, 2, 2)  #Create subplot at position 1, 2 (top right)
plt.hist(artist_count, bins=5)
plt.xlabel('artist_count')
plt.title('Distribution of artist_count')
````

>>>result

>>for outliers desc

````
#Create a box plot to identify the outliers
sns.boxplot(x='released_year', y='artist_count', data=df)
plt.title('Box plot of artist_count by released_year')
````

>>>result

## Top Performers

>>desc

### Which track has the highest number of streams? Display the top 5 most streamed tracks.

>> desc

````
streams = df['streams']
# Ensure 'streams' is numeric and retrieve top 5 tracks
df['streams'] = pd.to_numeric(df['streams'], errors='coerce')

top_5_streams = df.nlargest(5, 'streams')['track_name']
print("Top 5 streams:\n", top_5_streams)
````

>>>result

### Who are the top 5 most frequent artists based on the number of tracks in the dataset?

>> desc

````
# Get the 5 most frequent names in the 'track_name' column
top_5_frequent_names = df['artist(s)_name'].value_counts().head(5)

# Display the result
print("Top 5 most frequent artists:\n", top_5_frequent_names)
````

>>>result

## Temporal Trends

>>desc

### Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.

>> desc

````
# Group data by 'released_year' and calculate mean of 'artist_count'
group = df.groupby('released_year')['track_name'].count()

# Create a line plot with the grouped data
plt.plot(group.index, group.values, color='orange', linestyle='-')
plt.title('Line Plot of Average tracks released per Year')

# Add labels
plt.xlabel('Released Year')
plt.ylabel('Average Track Count')

# Add a grid
plt.grid(True)

# Display the result
plt.show()
````

>>>result

### Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

>> desc

````
# Group data by 'released_year' and calculate mean of 'artist_count'
group = df.groupby('released_month')['track_name'].count()

# Create a line plot with the grouped data
plt.plot(group.index, group.values, color='pink', linestyle='-')
plt.title('Line Plot of Average tracks released per Month')

# Add label
plt.xlabel('Released Month')
plt.ylabel('Average Track Count')

# Add a grid
plt.grid(True)

# Display the result
plt.show()
````

>>>result

## Genre and Music Characteristics

>>desc

### Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?

>> desc

````
# Ensure numeric types
df['bpm'] = pd.to_numeric(df['bpm'], errors='coerce')
df['danceability_%'] = pd.to_numeric(df['danceability_%'], errors='coerce')
df['energy_%'] = pd.to_numeric(df['energy_%'], errors='coerce')
df['streams'] = pd.to_numeric(df['streams'], errors='coerce')

# Drop any rows with NaN values in critical columns
df = df.dropna(subset=['bpm', 'danceability_%', 'energy_%', 'streams'])

# Calculate mean streams for each musical attribute
mean_streams_bpm = df.groupby('bpm')['streams'].mean()
mean_streams_danceability = df.groupby('danceability_%')['streams'].mean()
mean_streams_energy = df.groupby('energy_%')['streams'].mean()

plt.style.use('default')
plt.figure(figsize=(10, 6))

# Create scatter plots for correlation
plt.scatter(mean_streams_bpm.index, mean_streams_bpm.values, color='skyblue', label='BPM')
plt.scatter(mean_streams_danceability.index, mean_streams_danceability.values, color='orange', label='Danceability')
plt.scatter(mean_streams_energy.index, mean_streams_energy.values, color='pink', label='Energy')

# Add a label
plt.ylabel('Average Streams')
plt.title('Correlation between Streams and Musical Attributes')

# Add a legend
plt.legend()

# Add a grid
plt.grid(True)

# Display the result
plt.show()
````

>>>result

### Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

>>desc

````
# Ensure numeric types
df['danceability_%'] = pd.to_numeric(df['danceability_%'], errors='coerce')
df['energy_%'] = pd.to_numeric(df['energy_%'], errors='coerce')

# Drop any rows with NaN values in critical columns
df = df.dropna(subset=['danceability_%', 'energy_%'])

plt.style.use('default')
plt.figure(figsize=(10, 6))

# Create scatter plots for correlation
plt.scatter(df['danceability_%'], df['energy_%'], color='skyblue')

# Add a label
plt.ylabel('energy_%')
plt.xlabel('danceability_%')
plt.title('Correlation between Danceability% and Energy%')

# Add a grid
plt.grid(True)

# Display the result
plt.show()
````

>>>result

>>valence and acousticness desc

````
# Ensure numeric types
df['valence_%'] = pd.to_numeric(df['valence_%'], errors='coerce')
df['acousticness_%'] = pd.to_numeric(df['acousticness_%'], errors='coerce')

# Drop any rows with NaN values in critical columns
df = df.dropna(subset=['valence_%', 'acousticness_%'])

plt.style.use('default')
plt.figure(figsize=(10, 6))

# Create scatter plots for correlation
plt.scatter(df['valence_%'], df['acousticness_%'], color='pink')

# Add a label
plt.ylabel('acousticness_%')
plt.xlabel('valence_%')
plt.title('Correlation between Valence% and Acousticness%')

# Add a grid
plt.grid(True)

# Display the result
plt.show()
````

>>>result

## Platform Popularity

>>desc

### How do the numbers of tracks in spotify_playlists, deezer_playlist, and apple_playlists compare? Which platform seems to favor the most popular tracks?

>>desc

````
# Calculate mean streams for each musical attribute
counts_spotplay = df['in_spotify_playlists'].value_counts()
counts_deezplay = df['in_deezer_playlists'].value_counts()
counts_appleplay = df['in_apple_playlists'].value_counts()

plt.style.use('default')

# Create bar plots for number of tracks
fig, ax=plt.subplots(1,3,figsize=(15,8))

ax[0].plot(counts_spotplay.index, counts_spotplay.values, '-', color='skyblue')
ax[0].set_xlabel('spotify_playlists')
ax[0].set_title('Distribution of spotify_playlists')


ax[1].plot(counts_deezplay.index, counts_deezplay.values, '-', color='orange')
ax[1].set_xlabel('deezer_playlists')
ax[1].set_title('Distribution of deezer_playlists')

ax[2].plot(counts_appleplay.index, counts_appleplay.values, '-', color='pink')
ax[2].set_xlabel('apple_playlists')
ax[2].set_title('Distribution of apple_playlists')

plt.suptitle('numbers of tracks in spotify_playlists, deezer_playlists, and apple_playlists')
plt.tight_layout()
plt.show()
````

>>>result

## Advanced Analysis

>>desc

### Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?

>>desc

````

````

>>>result

### Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

>>desc

````

````

>>>result

____





