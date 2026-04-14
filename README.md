# EDA EXP 1 - Combined Code
import numpy as np
import pandas as pd

# -------- NUMPY OPERATIONS --------

# Array creation
list1 = [0,1,2,3,4]
arr = np.array(list1)
print(type(arr))
print("arr1:", arr)

# Array addition
arr1 = arr + 2
print("arr1 after adding 2:", arr1)

# 2D Array
list2 = [[0,1,2],[3,4,5],[6,7,8]]
arr3 = np.array(list2)
print("arr3:\n", arr3)

arr4 = np.array(list2, dtype='float')
print("float array:\n", arr4)

# Shape, datatype, size, dimensions
print("shape:", arr3.shape)
print("datatype:", arr3.dtype)
print("size:", arr3.size)
print("dimensions:", arr3.ndim)

# Slicing
print("whole array:\n", arr4)
print("partial array:\n", arr4[:2, :2])
# Boolean condition
boo = arr4 > 2
print("greater than 2:\n", boo)
# 1D slicing
arr = np.array([1,2,3,4,5,6,7])
print("slice from index 4:", arr[4:])
# Concatenation (1D)
a1 = np.array([1,2,3])
a2 = np.array([4,5,6])
arr = np.concatenate((a1, a2))
print("concatenated 1D:", arr)
# Concatenation (2D axis=1)
a1 = np.array([[1,2,3],[3,4,5]])
a2 = np.array([[4,5,6],[7,8,9]])
arr = np.concatenate((a1, a2), axis=1)
print("concat axis=1:\n", arr)
# Concatenation (2D axis=0)
arr = np.concatenate((a1, a2), axis=0)
print("concat axis=0:\n", arr)
# -------- PANDAS SERIES --------
# Series creation
new_series = pd.Series([5,6,7,8,9,10])
print(new_series)
# Access element
print("element at index 4:", new_series[4])
# Series with custom index
new_series = pd.Series([5,6,7,8,9,10], index=['a','b','c','d','e','f'])
print(new_series)
print("value at 'f':", new_series['f'])
# Modify values
new_series[['a','b','f']] = 0
print("after modification:\n", new_series)

# Filtering
new_series = new_series[new_series > 0]
print("filtered series:\n", new_series)

print("values > 7 multiplied by 2:\n", new_series[new_series > 7] * 2)


# -------- DATAFRAME OPERATIONS --------

# Creating DataFrame with NaN
data = {
    'First Score': [100,90,np.nan,95],
    'Second Score': [30,45,56,np.nan],
    'Third': [np.nan,40,80,98]
}

df = pd.DataFrame(data)
print("original dataframe:\n", df)

# Fill NaN with 0
print("fill NaN with 0:\n", df.fillna(0))

# Mean
print("mean values:\n", df.mean())


# -------- ADVANCED DATAFRAME --------

df = pd.DataFrame(
{
    "mark1": [78, np.nan, 80, 88, 89, np.nan, 98, 76, 78, np.nan],
    "mark2": [78, 88, np.nan, np.nan, 87, 87, 85, np.nan, 76, 84]
},
index=['keerthana K', 'Anu', 'Keerthana M R', 'vaish', 'Divya',
       'Sowj', 'chand', 'sush', 'sree', 'zuvi']
)
print("original:\n", df)
# Fill NaN with 0
print("fill NaN with 0:\n", df.fillna(0))
# Fill NaN with mean
print("fill NaN with mean:\n", df.fillna(df.mean()))
# =========================
# EXP 2 - NUMPY + PANDAS BASICS
# =========================
import numpy as np
# Create 2D array
list2 = [[0,1,2], [3,4,5], [6,7,8]]
arr2 = np.array(list2)
print(list2)
print("Shape:", arr2.shape) # rows x columns
print("Data Type:", arr2.dtype) # datatype of elements
print("Size:", arr2.size) # total elements
print("Num Dimensions:", arr2.ndim) # dimensions

# Convert to float type
arr3 = np.array(list2, dtype="float")
print("Whole:", arr3)

# Slicing (first 2 rows & cols)
print("Part:", arr3[0:2, 0:2])

# Boolean condition
boo = arr3 > 2
print(boo)

# 1D array slicing
arr = np.array([1,2,3,4,5,6,7])
print(arr[4:7]) # elements from index 4
print(arr[3:5]) # subset

# Concatenation (1D)
arr1 = np.array([1,2,3])
arr2 = np.array([4,5,6])
arr = np.concatenate((arr1, arr2))
print(arr)

# Concatenation (2D)
arr1 = np.array([[1,2],[3,4]])
arr2 = np.array([[5,6],[7,8]])

arr = np.concatenate((arr1, arr2), axis=1) # column-wise
print(arr)

arr = np.concatenate((arr1, arr2), axis=0) # row-wise
print(arr)

# Stack arrays
arr1 = np.array([1,2,3])
arr2 = np.array([4,5,6])
arr = np.stack((arr1, arr2), axis=1)
print(arr)

# -------- PANDAS --------

import pandas as pd

# Create series
new_series = pd.Series([5,6,7,8,9,10])
print(new_series)

print(new_series[4]) # access element

# Custom index
new_series = pd.Series([5,6,7,8,9,10],
index=['a','b','c','d','e','f'])

print(new_series['f']) # access by label

# Modify values
new_series[['a','b','f']] = 0
print(new_series)

# Filter values > 0
new_series2 = new_series[new_series > 0]
print(new_series2)

# -------- DATAFRAME --------

dict = {
'First Score': [100, 90, np.nan, 95],
'Second Score': [30, 45, 56, np.nan],
'Third Score': [np.nan, 40, 80, 98]
}

df = pd.DataFrame(dict)
print(df)

# Fill missing values with 0
df = df.fillna(0)
print(df)


# =========================
# EXP 3 - DATA VISUALIZATION
# =========================

import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("ds-1.csv", index_col=0, encoding="ISO-8859-1")

print(df.describe()) # summary stats
print(df.describe().T) # transpose

# Histogram
plt.hist(df['Speed'], bins=10, edgecolor='black')
plt.xlabel('Pokemon Speed')
plt.ylabel('Frequency')
plt.title("Speed Distribution")
plt.show()

# Custom bins
bin_edges = [0,10,20,30,40,50,60,70,80,90,100,110,120,130,140,150]
plt.hist(df['Speed'], bins=bin_edges, edgecolor='red')
plt.show()

# Seaborn plots
sns.displot(df['Speed']) # distribution plot
sns.scatterplot(x='Attack', y='Defense', data=df) # scatter plot
sns.lmplot(x='Attack', y='Defense', data=df) # regression

# Boxplot
sns.boxplot(data=df.drop(['Total','Stage','Legendary'], axis=1))

# Violin + swarm
sns.violinplot(x="Type 1", y="Attack", data=df)
sns.swarmplot(x="Type 1", y="Attack", data=df)

plt.show()


# =========================
# EXP 4 - FILE HANDLING
# =========================

# Write file
file = open('keer.txt','w')
file.write('sample text')
file.close()

# Read file
file = open('keer.txt','r')
print(file.read())
file.close()

# Append file
file = open('keer.txt','a')
file.write("\nnew line added")
file.close()

# CSV operations
df = pd.read_csv('store_data.csv')
df.to_csv('output.csv', index=False)

# Excel operations
df = pd.read_excel('store_data.csv')
df.to_excel('output.xlsx', index=False)


# =========================
# EXP 5 - EDA ANALYSIS
# =========================

df = pd.read_csv("EDA activiy 1 (1).csv")

print("rows:", df.shape[0])
print("columns:", df.shape[1])

# Check duplicates
print(df.duplicated(subset=['customerID']).sum())

# Missing values
print(df.isnull().sum())

# Fix TotalCharges
df['TotalCharges'] = pd.to_numeric(df['TotalCharges'], errors='coerce')
df['TotalCharges'].fillna(df['TotalCharges'].median(), inplace=True)

# Clean categorical columns
for col in df.select_dtypes(include='object').columns:
df[col] = df[col].str.strip().str.lower()

# Mean values
print(df[['tenure','MonthlyCharges','TotalCharges']].mean())

# Histogram
sns.histplot(df['tenure'])
sns.histplot(df['MonthlyCharges'])
sns.histplot(df['TotalCharges'])

plt.show()

# Churn plot
sns.countplot(x='Churn', data=df)
plt.show()


# =========================
# EXP 6 - OUTLIER DETECTION
# =========================

from sklearn.datasets import load_diabetes
from scipy import stats

diabetes = load_diabetes()
df = pd.DataFrame(diabetes.data, columns=diabetes.feature_names)

# Boxplot
sns.boxplot(df['bmi'])
plt.show()

# Z-score method
z = np.abs(stats.zscore(df['age']))
df_no_outliers = df[z < 2]

print(df.shape, df_no_outliers.shape)

# IQR method
Q1 = df['bmi'].quantile(0.25)
Q3 = df['bmi'].quantile(0.75)
IQR = Q3 - Q1

upper = Q3 + 1.5 * IQR
lower = Q1 - 1.5 * IQR

print("Outliers:", ((df['bmi'] > upper) | (df['bmi'] < lower)).sum())


# =========================
# EXP 7 - MISSING VALUES + BAR PLOT
# =========================

# Series with NaN
data = pd.Series([1,2,np.nan,4])
print(data.dropna()) # remove missing

# Fill with mean
data.fillna(data.mean(), inplace=True)

# DataFrame cleaning
df = pd.DataFrame([[1,np.nan],[3,4]])
df.fillna(0)

# Stacked bar plot
quarters = ['Q1','Q2','Q3','Q4']
p1 = [20,30,40,50]
p2 = [10,20,30,40]

plt.bar(quarters, p1)
plt.bar(quarters, p2, bottom=p1)
plt.show()
# =========================
#   exp 8 WEB SCRAPING
# =========================

import requests
from bs4 import BeautifulSoup

url = "http://books.toscrape.com/catalogue/category/books_1/index.html"
response = requests.get(url)

# Parse HTML
soup = BeautifulSoup(response.text, "html.parser")

titles, prices, ratings = [], [], []

# Extract data
books = soup.find_all("article", class_="product_pod")

for book in books:
titles.append(book.h3.a["title"])
prices.append(book.find("p", class_="price_color").text)
ratings.append(book.p["class"][1])

# Store in DataFrame
df = pd.DataFrame({
"Title": titles,
"Price": prices,
"Rating": ratings
})

# Save to CSV
df.to_csv("books_data.csv", index=False)

print("Scraping completed!")
