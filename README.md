# Exploratory-Data-Analysis-on-Spotify-2023-Dataset
___

### What is an Exploratory Data Analysis? 

Exploratory Data Analysis (EDA) is a method of analyzing data, focusing on its main characteristics. This involves using statistical visualizations to help us understand the data better, detect errors, and test assumptions. It is one of the first steps before diving into more complex data analysis. 

### What is the goal of this deliverable? 

The goal is to explore and analyze a dataset of popular Spotify tracks from 2023. By examining and visualizing the data, we aim to uncover trends, popular artists, and other insights about the most-streamed songs. This will give a clear overview of the dataset and highlight key patterns. 

_____

## Overview of the Dataset?

In the first part of this analysis, we will focus on gathering basic information from the 2023 Spotify dataset. This includes finding the number of rows and columns, as well as identifying the data type of each column. This information is important because it helps us understand the structure of the dataset and ensures we can apply the right methods in the next steps of the analysis.

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

### How many rows and columns does the dataset contain?¶

To get the rows and columns of the dataset, I used the variable named 'shape'. By using the attribute of pandas '.shape', I was able to get the dimensions of the DataFrame easily.

````
shape = df.shape
print("Rows and Columns: ", shape)
````







