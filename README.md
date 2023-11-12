# Ex-08-Data-Visualization-
# AIM
To Perform Data Visualization on a complex dataset and save the data to a file.

# Explanation
Data visualization is the graphical representation of information and data. By using visual elements like charts, graphs, and maps, data visualization tools provide an accessible way to see and understand trends, outliers, and patterns in data.

# ALGORITHM
## STEP 1
Read the given Data

## STEP 2
Clean the Data Set using Data Cleaning Process

## STEP 3
Apply Feature generation and selection techniques to all the features of the data set

## STEP 4
Apply data visualization techniques to identify the patterns of the data.

# CODE

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from google.colab import files
uploaded = files.upload()
df=pd.read_csv("SuperStore.csv",encoding='unicode_escape')
df
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/4e5d3be8-43b8-481c-910f-15266a5b2f31)

```
df.isnull().sum()
```

![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/7e749834-d988-4c3d-9f38-ddd730ea7cdc)

```
df.drop('Row ID',axis=1,inplace=True)
df.drop('Order ID',axis=1,inplace=True)
df.drop('Customer ID',axis=1,inplace=True)
df.drop('Customer Name',axis=1,inplace=True)
df.drop('Country',axis=1,inplace=True)
df.drop('Postal Code',axis=1,inplace=True)
df.drop('Product ID',axis=1,inplace=True)
df.drop('Product Name',axis=1,inplace=True)
df.drop('Order Date',axis=1,inplace=True)
df.drop('Ship Date',axis=1,inplace=True)
print("Updated dataset")
df
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/1b0518c5-d05d-458d-907c-8697c0dbe9e6)

```
#detecting and removing outliers in current numeric data
plt.figure(figsize=(8,8))
plt.title("Data with outliers")
df.boxplot()
plt.show()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/03e58254-b175-470e-834b-b9a6f81cd072)

```
plt.figure(figsize=(8,8))
cols = ['Sales','Discount','Profit']
Q1 = df[cols].quantile(0.25)
Q3 = df[cols].quantile(0.75)
IQR = Q3 - Q1
df = df[~((df[cols] < (Q1 - 1.5 * IQR)) |(df[cols] > (Q3 + 1.5 * IQR))).any(axis=1)]
plt.title("Dataset after removing outliers")
df.boxplot()
plt.show()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/c7a8af7c-832c-4555-9cda-f6ed799ab674)

### Which Segment has the highest sales?

```
sns.lineplot(x="Segment",y="Sales",data=df,marker='o')
plt.title("Segment vs Sales")
plt.xticks(rotation = 90)
plt.show()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/349050ee-12e5-4d6a-a27e-51ade27397b5)


```
sns.barplot(x="Segment",y="Sales",data=df)
plt.xticks(rotation = 90)
plt.show()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/087179fe-8d85-4b52-8589-639366349027)

### Which City has the Highest Profit?
```
df.shape
df1 = df[(df.Profit >= 60)]
df1.shape
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/9e405a73-5cb2-4790-a38a-831f662a3cba)


```

plt.figure(figsize=(30,8))
states=df1.loc[:,["City","Profit"]]
states=states.groupby(by=["City"]).sum().sort_values(by="Profit")
sns.barplot(x=states.index,y="Profit",data=states)
plt.xticks(rotation = 90)
plt.xlabel=("City")
plt.ylabel=("Profit")
plt.show()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/a8cbb262-4fde-48c4-a10d-9a6e01eefc51)

### Which Ship mode is profitable?
```
sns.barplot(x="Ship Mode",y="Profit",data=df)
plt.show()
```

![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/4cfc53b8-a2e4-414e-a431-568ed1a9a8a7)

```

sns.lineplot(x="Ship Mode",y="Profit",data=df)
plt.show()
```

![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/c73c3087-09e5-47e5-a4f3-4418a23f04ca)

```
sns.violinplot(x="Profit",y="Ship Mode",data=df)
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/42ab4c76-dc0c-4a44-8de7-d23e417a965b)

### Sales of the product based on region.
```
states=df.loc[:,["Region","Sales"]]
states=states.groupby(by=["Region"]).sum().sort_values(by="Sales")
sns.barplot(x=states.index,y="Sales",data=states)
plt.xticks(rotation = 90)
plt.xlabel=("Region")
plt.ylabel=("Sales")
plt.show()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/a8b17548-13aa-4aa2-aec7-e8dab84c4a7f)


```
df.groupby(['Region']).sum().plot(kind='pie', y='Sales',figsize=(6,9),pctdistance=1.7,labeldistance=1.2)
```

![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/32980513-372c-484d-97e2-c612f35f32fe)

### Find the relation between sales and profit
```
df["Sales"].corr(df["Profit"])
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/51961009-50c7-48c0-b6ab-568e989f379e)


```
df_corr = df.copy()
df_corr = df_corr[["Sales","Profit"]]
df_corr.corr()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/756d357f-863a-4fa5-be0f-196ac7784aa0)


```
sns.pairplot(df_corr, kind="scatter")
plt.show()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/5c3b7133-4d84-48f5-a472-12f5355c060a)

### Heatmap

```
df4=df.copy()

#encoding
from sklearn.preprocessing import LabelEncoder,OrdinalEncoder,OneHotEncoder
le=LabelEncoder()
ohe=OneHotEncoder
oe=OrdinalEncoder()

df4["Ship Mode"]=oe.fit_transform(df[["Ship Mode"]])
df4["Segment"]=oe.fit_transform(df[["Segment"]])
df4["City"]=le.fit_transform(df[["City"]])
df4["State"]=le.fit_transform(df[["State"]])
df4['Region'] = oe.fit_transform(df[['Region']])
df4["Category"]=oe.fit_transform(df[["Category"]])
df4["Sub-Category"]=le.fit_transform(df[["Sub-Category"]])

#scaling
from sklearn.preprocessing import RobustScaler
sc=RobustScaler()
df5=pd.DataFrame(sc.fit_transform(df4),columns=['Ship Mode', 'Segment', 'City', 'State','Region',
                                               'Category','Sub-Category','Sales','Quantity','Discount','Profit'])

#Heatmap
plt.subplots(figsize=(12,7))
sns.heatmap(df5.corr(),cmap="PuBu",annot=True)
plt.show()
```

![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/3b74df63-f157-4476-855d-8d51755adcae)


### Find the Realtion between sales and Profit based on the following category.

### Segment
```

grouped_data = df.groupby('Segment')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'], label='Profit')
ax.set_xlabel('Segment')
ax.set_ylabel('Value')
ax.legend()
plt.show()

```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/b9b2081e-0125-4f47-9a25-bc346b4bcd6d)

### City
```
grouped_data = df.groupby('City')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'], label='Profit')
ax.set_xlabel('City')
ax.set_ylabel('Value')
ax.legend()
plt.show()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/f101b240-1e80-4a3c-b9e9-de5f15fbc911)

### States
```
grouped_data = df.groupby('State')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'], label='Profit')
ax.set_xlabel('State')
ax.set_ylabel('Value')
ax.legend()
plt.show()
```
![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/435070f7-db66-4c7f-8c6e-c8ed118f03ee)


### Segment and Ship Mode
```
grouped_data = df.groupby(['Segment', 'Ship Mode'])[['Sales', 'Profit']].mean()
pivot_data = grouped_data.reset_index().pivot(index='Segment', columns='Ship Mode', values=['Sales', 'Profit'])
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
pivot_data.plot(kind='bar', ax=ax)
ax.set_xlabel('Segment')
ax.set_ylabel('Value')
plt.legend(title='Ship Mode')
plt.show()
```

![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/7773ae2a-7001-4976-9393-038baed9b863)


### Segment , Shipmode and Region

```
grouped_data = df.groupby(['Segment', 'Ship Mode','Region'])[['Sales', 'Profit']].mean()
pivot_data = grouped_data.reset_index().pivot(index=['Segment', 'Ship Mode'], columns='Region', values=['Sales', 'Profit'])
sns.set_style("whitegrid")
sns.set_palette("Set1")
pivot_data.plot(kind='bar', stacked=True, figsize=(10, 5))
plt.xlabel('Segment-Ship Mode')
plt.ylabel('Value')
plt.legend(title='Region')
plt.show()
```

![image](https://github.com/madhi43/ODD2023-Datascience-Ex-08/assets/103943383/cc671c54-2929-4128-91d7-88f0e1f6979b)


## Result 

Thus, Data Visualization is performed on the given dataset and save the data to a file.






