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

Before starting the data analysis, I first imported the Python Libraries (Pandas, Matplotlib, and Seaborn).

```ruby
# Import the necessary libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

After importing the necessary libraries, I began by reading the CSV file to be used for the dataset analysis.

```ruby
# Read the CSV file
df = pd.read_csv('spotify-2023.csv', encoding='latin1')
```
Initially, I encountered a problem because the file was not encoded in UTF-8, the standard character encoding for many datasets. UTF-8 is often the default encoding when working with CSV files in Python, but some files may use different encodings, such as ``latin1`` (also known as ISO-8859-1). Using the correct encoding (in this case, 'latin1') solved the problem, allowing the file to be read correctly.

## Overview of Dataet

In the first part of this analysis, we will focus on gathering basic information from the 2023 Spotify dataset. This includes finding the number of rows and columns, as well as identifying the data type of each column. This information is important because it helps us understand the structure of the dataset and ensures we can apply the right methods in the next steps of the analysis.

### How many rows and columns does the dataset contain?¶

To get the rows and columns of the dataset, I used the variable named ``shape``. By using the attribute of pandas ``.shape``, I was able to get the dimensions of the DataFrame easily.

```ruby
# Get the dimensions of the DataFrame
shape = df.shape

# Display the result
print("Rows and Columns: ", shape)
```
Result:

![Screen Shot 2024-11-06 at 10 23 08 AM](https://github.com/user-attachments/assets/4c72923f-6f90-41a9-8d31-c2191a314732)

>The dataset consists of 953 rows and 24 columns.

### What are the data types of each column? Are there any missing values?

To get the data types for each column, I used the variable named ``dtype`` and the Pandas attribute ``.dtypes``.
 
```ruby
# Get the data types of each column
dtype = df.dtypes

# Display the result
print("Data types of each column:\n", dtype)
```
Result:

![Screen Shot 2024-11-06 at 10 33 09 AM](https://github.com/user-attachments/assets/9a59bb49-5c9b-4765-9096-f9bc92bdff6c)

>The result shows the column's name on the left, while the data type is on the right.

To get missing values, I used ``.isnull()`` method, a pandas function that identifies all the NaN values in the DataFrame. Next, I used ``.sum()`` calculate the sum of the missing values for each column. To display the values, I used ``missing_values[missing_values > 0]`` to filter the variable ``missing_values`` to show only the columns that have more than 0 missing values, along with the count of missing values in each of those columns.
```ruby
# Get the missing values
missing_values = df.isnull().sum()

# Display the missing values
print("Missing values:\n", missing_values[missing_values > 0])
```
Result:

![Screen Shot 2024-11-06 at 12 34 54 PM](https://github.com/user-attachments/assets/03d56909-6a92-4930-8faf-c9578f18f829)

>The missing values in the dataset is in the `streams`, `in_shazam_charts`, and `key` column.
___
### Since there are missing values, I used ``coerce`` to convert the data to handle invalid or non-convertible values by replacing them with NaN, preventing errors and ensuring smooth data processing.

```ruby
# use coerce to convert the data to numeric
df['streams'] = pd.to_numeric(df['streams'], errors='coerce')
df['in_shazam_charts'] = pd.to_numeric(df['in_shazam_charts'], errors='coerce')
df['key'] = pd.to_numeric(df['key'], errors='coerce')
```
____

## Basic Descriptive Statistics
In this part of the analysis, it is focused on calculating the mean, median, and standard deviation of the streams column, and explore the distributions of the released_year and artist_count columns to identify any noticeable trends or outliers.

### What are the mean, median, and standard deviation of the streams column?
I accessed the streams column using ``df['streams']`` and calculated the mean, median, and standard deviation using the pandas functions ``.mean()``, ``.median()``, and ``.std()``, respectively.

```ruby
# Get the mean, median, and standard deviation of streams
mean = df['streams'].mean()
median = df['streams'].median()
sd = df['streams'].std()

# Display the mean, median, and standard deviation
print("Mean:\n",mean,"\n\n", "Median:\n",median,"\n\n","Standard Deviation:\n",sd)
```
Result:

![Screen Shot 2024-11-06 at 1 23 43 PM](https://github.com/user-attachments/assets/dd124a11-2bbf-4bde-9e6b-cfb98ef22d77)
>The mean, median, and standard deviation of the streams column is 514137424.93907565, 290530915.0, and 566856949.0388832, respectively.

### What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?
To analyze the distribution of the released_year and artist_count columns, I first extracted the data by assigning them to the variables ``released_year = df['released_year']`` and ``artist_count = df['artist_count']``. Then, I created separate histograms for each variable to visualize their distributions more clearly. To improve the clarity of the plots, I specified the figure size and used Matplotlib to generate the histograms for both released_year and artist_count.

```ruby
# Create a variable for released_year and artist_count
released_year = df['released_year']
artist_count = df['artist_count']

# Input a figure size for a better visual
plt.figure(figsize=(8, 6)) 

# Histogram for released_year
plt.subplot(1, 2, 1)  #Create subplot at position 1, 1 (top left)
plt.hist(released_year, color='pink', bins=5)
plt.xlabel('released_year')
plt.title('Distribution of released_year')

# Histogram for artist_count
plt.subplot(1, 2, 2)  #Create subplot at position 1, 2 (top right)
plt.hist(artist_count, color='skyblue', bins=5)
plt.xlabel('artist_count')
plt.title('Distribution of artist_count')
```
Result:

![Screen Shot 2024-11-06 at 1 29 00 PM](https://github.com/user-attachments/assets/f27fefc4-92a5-4f48-91cd-a9b8915cc577)


>A box plot is the most effective visualization method for identifying outliers in the data. Therefore, I used a box plot to highlight any outliers.

Box plot of released_year:
```ruby
# Create a box plot to identify the outliers
df['released_year'].plot(kind='box')

# Add a title
plt.title('Box plot of released_year')
```
Result:

![Screen Shot 2024-11-07 at 12 38 30 AM](https://github.com/user-attachments/assets/4c2325cb-6063-4310-8ea3-a5f98b241df9)

>Based on the plot, there are outliers around the 1940s and below.

Box plot of artist_count:
```ruby
# Create a box plot to identify the outliers
df['artist_count'].plot(kind='box')

# Add a title
plt.title('Box plot of artist_count')
```
![Screen Shot 2024-11-07 at 12 39 09 AM](https://github.com/user-attachments/assets/9b350c29-b660-4110-8001-05af634b3a34)

>Based on the plot, there are only a few outliers, which means that most values are clustered closely together with only a few high values.

## Top Performers
In this part of the analysis, we aim to identify the track with the highest number of streams and determine the most frequently appearing artists in the dataset.

### Which track has the highest number of streams? Display the top 5 most streamed tracks.
To identify the track with the highest number of streams, I first used ``.idxmax()`` to find the index of the row with the maximum stream count. Then, I used ``df.loc`` to retrieve the track name corresponding to that index.

```ruby
# Create a variable for streams
streams = df['streams']

# Find the index of the row with the highest number of streams
max_streams = streams.idxmax()

# Identify the track name with the highest streams
highest_streams = df.loc[max_streams, 'track_name']

# Display the result
print("Track with highest number of streams: ", highest_streams)
```
Result:

![Screen Shot 2024-11-06 at 2 33 21 PM](https://github.com/user-attachments/assets/3ba43e65-cbb2-4b89-a6d9-f3c02968dbda)

>The track with the highest stream is Blinding Lights by The Weeknd

To identify the top 5 most streamed tracks, I used the `.nlargest(5)` function on the streams column to retrieve the 5 highest stream counts. Then, I created a variable `top5` by applying `.loc[]` to access the track names corresponding to these top 5 streams. I also used `.reset_index(drop=True)` to reset the index for a cleaner result.
```ruby
# Get the top 5 most streamed stracks
top_5_streams = streams.nlargest(5)

# Get the top 5 most streamed tracks
top5 = df.loc[top_5_streams.index, 'track_name'].reset_index(drop=True)

# Display the result
print("Top 5 streams:\n", top5)
```
Result:

![Screen Shot 2024-11-06 at 2 38 38 PM](https://github.com/user-attachments/assets/00e760f7-a9c2-45ba-84a1-ea546b63c4aa)

>The top 5 highest streams from the data are Blinding Lights, Shape of You, Someone You Loved, and Sunflower - Spider-Man: Into the Spider-Verse.

### Who are the top 5 most frequent artists based on the number of tracks in the dataset?

To identify the top 5 most frequent artists, I first created a variable named `frequent_names` and used the `.value_counts()` function on the `artist(s)_name` column to count the occurrences of each artist. Then, I applied `.head(5)` to extract only the top 5 most frequent artist names.

```ruby
# Get the 5 most frequent names in the 'track_name' column
frequent_names = df['artist(s)_name'].value_counts().head(5)

# Display the result
print("Top 5 most frequent artists:\n", frequent_names)
```
Result:

![Screen Shot 2024-11-07 at 12 51 59 AM](https://github.com/user-attachments/assets/bada655f-34b9-49b4-b5da-f0a6fd71aad6)

>The frequent names that appear in the `track_name` column are Taylor Swift, The Weeknd, Bad Bunny, SZA, and Harry Styles.

## Temporal Trends
In this part of the analysis, we examine the trends in track releases over time by plotting the number of tracks released per year and analyzing the monthly release patterns to identify any noticeable trends, including which month sees the most releases.

### Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
In this part of the analysis, I grouped the data by `released_year` and counted the number of tracks released each year using `groupby()` and `count()`. I then created a line plot to visualize the trend in track releases over time, with labels for clarity and a grid for better readability.

```ruby
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
```
Result:

![Screen Shot 2024-11-07 at 5 31 08 PM](https://github.com/user-attachments/assets/26eeeb3c-6223-40a0-8860-1836e42ccc78)

>The graph shows that the amount of released track per year is higher in 2020 than the past few years.

### Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?
In this code, I grouped the data by `released_month` and counted the number of tracks released in each month. Then, I created a line plot to visualize the monthly release patterns, with labels for the x and y axes, and added a grid for better readability.


```ruby
# Group data by 'released_month' and calculate mean of 'artist_count'
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
```
![Screen Shot 2024-11-07 at 5 34 44 PM](https://github.com/user-attachments/assets/5b2dcfc3-9671-4424-818d-4d25bbd3a443)

>In this graph, it shows that the month with the most released track is January and May, which indicates that these months might be popular for new music releases. This could suggest that artists or record labels tend to launch their tracks at the beginning of the year or in the spring.

## Genre and Music Characteristics
In this part, we are tasked to analyze how musical attributes like bpm, danceability, and energy correlate with the number of streams to identify which attributes have the strongest influence on streams. Additionally, we aim to explore whether there is a correlation between other attributes like danceability and energy, as well as valence and acousticness.

### Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
To get the correlation between streams and musical attributes, I first grouped the dataset by musical attributes (bpm, danceability, and energy) and calculated the average streams for each group. Then, I created scatter plots to visualize the relationship between these attributes and average streams. Finally, I added labels, a title, a legend, and a grid to improve the readability of the plot and displayed the result.

```ruby
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
```
Result:
![Screen Shot 2024-11-07 at 5 41 04 PM](https://github.com/user-attachments/assets/d3a19a70-ffd3-4f80-861c-74a71c643d21)

>Based on the scatter plot, we can conclude that the attribute that influence streams the most is BPM.

### Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?
In this code, I first set the plot style to default and defined the figure size to ensure the plot is clear. Then, I used `plt.scatter()` to create a scatter plot, plotting `danceability_%` on the x-axis and `energy_%` on the y-axis to visualize their correlation. Lastly, I added labels to the axes, a title to the plot, and a grid for better readability before displaying the result with plt.show().

```ruby
# Indicate the style to default and the figure size of the plot
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
```
Result:
![Screen Shot 2024-11-07 at 5 45 43 PM](https://github.com/user-attachments/assets/b276db59-ccba-4f2a-952b-b3f433f672b5)

>The scatter plot reveals a general trend where higher danceability percentages tend to be associated with higher energy percentages.

For `valence_%` and `acousticness_%`, I created a scatter plot using `plt.scatter()`, plotting `valence_%` on the x-axis and `acousticness_%` on the y-axis to visualize their correlation. After adding axis labels, a title, and a grid for clarity, I displayed the plot with `plt.show()` to analyze the relationship between these two attributes.

```ruby
# Indicate the style to default and the figure size of the plot
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
```
Result:
![Screen Shot 2024-11-07 at 5 49 02 PM](https://github.com/user-attachments/assets/72f85387-3cdc-4f19-a447-4a11486d2f8e)

>In this plot, there is no clear way to tell about the correlation between `valence_%` and `acousticness_%` since they are not well-aligned with each other.

## Platform Popularity
In this part, we will compare the number of unique tracks in playlists on Spotify, Deezer, and Apple to see which platform has the most variety. We will also check which platform has the most popular tracks based on the number of unique tracks in their playlists.


### How do the numbers of tracks in spotify_playlists, deezer_playlist, and apple_playlists compare? Which platform seems to favor the most popular tracks?

I counted the number of unique tracks in playlists across three platforms: Spotify, Deezer, and Apple, using the `.nunique()` method. Then, I created a bar chart to visually compare the unique track counts for each platform.

```ruby
```ruby
# Count the unique values in each playlist column
spotplay = df['in_spotify_playlists'].nunique()
deezplay = df['in_deezer_playlists'].nunique()
appleplay = df['in_apple_playlists'].nunique()

# Create a DataFrame for the counts
unique_counts = pd.DataFrame({
    'Platform': ['Spotify', 'Deezer', 'Apple'],
    'Unique Track Count': [spotplay, deezplay, appleplay]})

# Plot the unique counts as a bar chart
plt.figure(figsize=(8, 5))
sns.barplot(data=unique_counts, x='Platform', y='Unique Track Count', palette='Set2')

# Add a label
plt.title('Track Counts in Spotify, Deezer, and Apple Playlists')
plt.ylabel('Track Count')

# Display the result
plt.show()
```
Result:

![Screen Shot 2024-11-09 at 7 02 44 PM](https://github.com/user-attachments/assets/b0ae6b3f-5c77-40a3-b1a5-6c2d54ba9bcf)

>The bar graph shows that the number of tracks on Spotify is higher than that on Deezer and Apple. It also indicates that Spotify favors the most popular tracks.

## Advanced Analysis

In this section, we are tasked to identify patterns in tracks with the same `key` or `mode` (Major vs. Minor) based on `streams` data. We will also analyze which genres or artists appear most often in playlists or charts. 


### Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?

In this code, I cleaned the data by removing any rows with missing values in the streams, key, or mode columns. Then, I created two boxplots to visualize the stream distribution: one based on the musical key and the other on mode (Major vs. Minor), helping identify patterns between these attributes and their streams.

```ruby
# Handle missing values 
df.dropna(subset=['streams', 'key', 'mode'], inplace=True)

# Create the first boxplot for the stream distribution by key
plt.figure(figsize=(7, 5))  # Set figure size
sns.boxplot(data=df, x='key', y='streams', hue='key', palette='viridis')
plt.title('Stream Distribution by Key')
plt.xlabel('Key')
plt.ylabel('Streams')
plt.tight_layout()
plt.show()

# Create the second boxplot for the stream distribution by mode
plt.figure(figsize=(7, 5))  # Set figure size
sns.boxplot(data=df, x='mode', y='streams', hue='mode', palette='viridis')
plt.title('Stream Distribution by Mode')
plt.xlabel('Mode')
plt.ylabel('Streams')
plt.tight_layout()

# Display the result
plt.show()
```
Result:
![Screen Shot 2024-11-09 at 8 30 24 PM](https://github.com/user-attachments/assets/8d050bd9-f8ec-452d-b0ad-eedfdf905a34)

![Screen Shot 2024-11-09 at 8 30 38 PM](https://github.com/user-attachments/assets/463d1186-6b74-42d5-a21a-0b876d1c4e2f)





### Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

I ensured that the columns indicating whether tracks appeared in playlists or charts across different platforms (Spotify, Deezer, and Apple) were numeric, replacing any non-numeric values with 0. Then, I calculated the total number of appearances for each artist across all platforms and visualized the top 10 artists with the most appearances in playlists or charts using a bar plot.

```ruby
# Ensure the 'in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists' columns are numeric
df['in_spotify_playlists'] = pd.to_numeric(df['in_spotify_playlists'], errors='coerce').fillna(0)
df['in_deezer_playlists'] = pd.to_numeric(df['in_deezer_playlists'], errors='coerce').fillna(0)
df['in_apple_playlists'] = pd.to_numeric(df['in_apple_playlists'], errors='coerce').fillna(0)
df['in_spotify_charts'] = pd.to_numeric(df['in_spotify_playlists'], errors='coerce').fillna(0)
df['in_deezer_charts'] = pd.to_numeric(df['in_deezer_playlists'], errors='coerce').fillna(0)
df['in_apple_charts'] = pd.to_numeric(df['in_apple_playlists'], errors='coerce').fillna(0)

# Count how often each artist appears in any playlist (across Spotify, Deezer, Apple)
df['total_playcharts'] = df['in_spotify_playlists'] + df['in_deezer_playlists'] + df['in_apple_playlists'] + df['in_spotify_charts'] + df['in_deezer_charts'] + df['in_apple_charts']

# Group by artist and sum the total appearances across all platforms
artistplaychart = df.groupby('artist(s)_name')['total_playcharts'].sum().reset_index()

# Sort the artists by the number of playlist appearances (descending order)
artistplaychart = artistplaychart.sort_values(by='total_playcharts', ascending=False)

# Get the top N artists (for example, top 10)
toppy = artistplaychart.head(10)

# Plot the figure size
plt.figure(figsize=(10, 6))
sns.barplot(x='total_playcharts', y='artist(s)_name', data=toppy, palette='viridis')

# Add labels
plt.title('Most Frequently Appearing Artists on Playlists or Charts')
plt.xlabel('Number of Playlist or Charts Appearances')
plt.ylabel('Artist')
plt.tight_layout()

# Display the result
plt.show()
```
Result:
![Screen Shot 2024-11-09 at 8 35 47 PM](https://github.com/user-attachments/assets/09215257-5b08-480e-b55e-159d854f03dd)



_______________

## Applications used:

### Jupyter Notebook
- Used for coding and performing the overall data analysis, including data manipulation, visualization, and exploration.

### ChatGPT
- Assisted with clarifying concepts, providing explanations, and helping with challenging parts of the analysis.




