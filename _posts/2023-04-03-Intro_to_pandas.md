---
layout: post
title: You're up and running!
---
<div id="container" style="position:relative;">
<div style="float:left"><h1> Intro to Pandas </h1></div>
<div style="position:relative; float:right"><img style="height:65px" src ="https://drive.google.com/uc?export=view&id=1EnB0x-fdqMp6I5iMoEBBEuxB_s7AmE2k" />
</div>
</div>

We will now introduce Pandas, one of the most important Python tools when working with data.

Python for data analysis (Pandas) is a great tool for dealing with data in table format. It was originally created by Wes McKinney as an attempt to port R's dataframes into Python, and it quickly took on a life of its own. It builds on top of NumPy providing powerful tools for interacting with and visualizing data. The primary objects in Pandas are series and dataframes. We will examine each of these individually.


```python
import numpy as np
import pandas as pd
```

### Pandas Series

A Pandas `Series` is similar to an array but with an additional index. This index is constant and unchanging through the operations we apply to the `Series`. Creating a `Series` is similar to creating a NumPy array:


```python
# create a series with default indexing
a = pd.Series([1, 2, 3, 4, 5])
```


```python
print(a)
```

    0    1
    1    2
    2    3
    3    4
    4    5
    dtype: int64



```python
b = pd.Series([1, 2, 3, '4', 5])
```


```python
b
```




    0    1
    1    2
    2    3
    3    4
    4    5
    dtype: object



Notice the index in the first column and the datatype specified by the series. Unlike NumPy, we don't really have a notion of a higher dimensional series. If we tried to create a matrix version of a series we get the following:


```python
# a series with lists as elements
c = pd.Series([[1, 2, 3], [4, 5, 6]])
```


```python
c
```




    0    [1, 2, 3]
    1    [4, 5, 6]
    dtype: object



This isn't quite the same as NumPy: there are only 2 `Series` elements each of which is a list. Furthermore, the datatype is no longer `int64` (a 64 bit integer) it is simply `object` (a catch all term for non-numeric values).

When we build a `Series` we can specify the index value. We could specify a `Series` which pairs a person (the index) with their income (the Series value).


```python
income = pd.Series(
    [100000.00, 2300.00, 45000.00, 305000.00],
    index=['Grace', 'John', 'Erin', 'Hank']
)

print(income)
```

    Grace    100000.0
    John       2300.0
    Erin      45000.0
    Hank     305000.0
    dtype: float64


We can extract the index value and `Series` values, and we can even modify the `Series` by using the index:


```python
# get the index and values
print("The index: ", income.index)
print(f"The values: {income.values}") # Use an f string to embed variables inside print statements, using {}
```

    The index:  Index(['Grace', 'John', 'Erin', 'Hank'], dtype='object')
    The values: [100000.   2300.  45000. 305000.]



```python
# check John's income
income['John']
```




    2300.0




```python
# Increase John's income
income['John'] = 25000 # pd.series are mutable
```


```python
print(income)
```

    Grace    100000.0
    John      25000.0
    Erin      45000.0
    Hank     305000.0
    dtype: float64


The underlying index is still $0,1,\cdots$ and we can always index on that:


```python
# slice using the default index
income[2:4]
```




    Erin     45000.0
    Hank    305000.0
    dtype: float64



We can also slice on the index as before (notice the bounds on slice):


```python
# slice using the custom index
print(income['Erin':])
```

    Erin     45000.0
    Hank    305000.0
    dtype: float64


Unlike the value the index is immutable, once it is set we can not change it.


```python
# try to change one index
income.index[1] = 'New Index' # Changing a single index is not allowed. The whole .index would need to be overwritten.

```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[15], line 2
          1 # try to change one index
    ----> 2 income.index[1] = 'New Index'


    File /opt/homebrew/anaconda3/lib/python3.9/site-packages/pandas/core/indexes/base.py:5035, in Index.__setitem__(self, key, value)
       5033 @final
       5034 def __setitem__(self, key, value):
    -> 5035     raise TypeError("Index does not support mutable operations")


    TypeError: Index does not support mutable operations


There are several ways to build (or _initialize_ in object-oriented programming terms) a `Series`, one is by providing key value pairs (like with a python dict):


```python
income = pd.Series(
    {
        'Grace': 100000.0, 
        'John': 99000.0,
        'Erin': -99000.0,
        'Hank': 305000.0
    }
)

print(income)
```

    Grace    100000.0
    John      99000.0
    Erin     -99000.0
    Hank     305000.0
    dtype: float64


Note that the `dtype` is set to float now by default even though all values are integers.

### Pandas DataFrame

The `DataFrame` is a Pandas object which is like a collection of Pandas `Series`, all of which share a common index. We have several ways of creating `DataFrames`. We can initialize the `DataFrame` directly, supplying just its values:


```python
# a dataframe with default index and column names
df = pd.DataFrame(np.random.randint(0, 5, size=(10, 5)))

display(df)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>7</th>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>


We can also create a frame with custom column names:


```python
# a dataframe with default index and custom column names
df = pd.DataFrame(
    data=np.random.randint(0, 5, size=(10, 5)),
    columns=['First', 'Second', 'Third', 'Fourth', 'Fifth']
)

display(df)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>Fourth</th>
      <th>Fifth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>


As with a `Series`, we can specify the index value too (note how we are using a list here):


```python
# a dataframe with custom index and column names
df = pd.DataFrame(
    np.random.randint(0, 5, size=(10, 5)),
    columns=['First', 'Second', 'Third', 'Fourth', 'Fifth'],
    index=list('abcdefghij')
)

display(df)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>Fourth</th>
      <th>Fifth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>b</th>
      <td>0</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>c</th>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>d</th>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>e</th>
      <td>1</td>
      <td>3</td>
      <td>3</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>f</th>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>g</th>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>h</th>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <th>i</th>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>j</th>
      <td>2</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>


We can also create a frame with the default index and change it later by assigning a new value to the index.


```python
# default indexing and column names
df = pd.DataFrame(np.random.randint(0, 5, size=(10, 5)))

# update the index and column names
df.index = list('abcdefghij')
df.columns = ['First', 'Second', 'Third', 'Fourth', 'Fifth']

display(df)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>Fourth</th>
      <th>Fifth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>b</th>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>c</th>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>d</th>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>e</th>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <th>f</th>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>g</th>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>h</th>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>i</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>j</th>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>


Finally we can load data from a comma separated value (CSV) file. Grab a sample CSV about customer info [here](https://drive.google.com/file/d/1AQU3UtFIHQcT1Hbm0UOBSana_2VKYfos/view?usp=sharing). Note since this file is large we are going to use the `head()` method to look at only the first few rows.


```python
# load from csv
df = pd.read_csv("data/customer_info.csv") # If the csv file and notebook are in the same folder, you can refer to it directly
# df = pd.read_csv("/Users/MarkWentink/Downloads/customer_info.csv") # If not in the same folder, you will need the full file path for your csv.
```


```python
df.head(10) # Shows the top 10 entries
df.tail() # Bottom 5
df.sample(5) # random rows

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>INDUSTRY</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>STATE</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8142</th>
      <td>123002</td>
      <td>Healthcare</td>
      <td>138.0</td>
      <td>6282375.0</td>
      <td>TX</td>
      <td>6419.013967</td>
      <td>3331.925892</td>
    </tr>
    <tr>
      <th>8784</th>
      <td>122216</td>
      <td>Finance and Insurance</td>
      <td>11.0</td>
      <td>714370.0</td>
      <td>NY</td>
      <td>1331.826335</td>
      <td>912.747509</td>
    </tr>
    <tr>
      <th>2617</th>
      <td>121712</td>
      <td>Food Services</td>
      <td>32.0</td>
      <td>844615.2</td>
      <td>TX</td>
      <td>1659.146223</td>
      <td>4590.821025</td>
    </tr>
    <tr>
      <th>3830</th>
      <td>122973</td>
      <td>Education</td>
      <td>50.0</td>
      <td>2252755.0</td>
      <td>NY</td>
      <td>1725.524692</td>
      <td>1121.010705</td>
    </tr>
    <tr>
      <th>3663</th>
      <td>121203</td>
      <td>Construction</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NY</td>
      <td>39489.391702</td>
      <td>5137.947296</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.info() # QWuick overview of columns data types and missing values
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10000 entries, 0 to 9999
    Data columns (total 7 columns):
     #   Column        Non-Null Count  Dtype  
    ---  ------        --------------  -----  
     0   CUSTOMER_ID   10000 non-null  int64  
     1   INDUSTRY      9891 non-null   object 
     2   EMP           8757 non-null   float64
     3   ANNUAL_SALES  9478 non-null   float64
     4   STATE         10000 non-null  object 
     5   MOBILITY      10000 non-null  float64
     6   INTERNET      10000 non-null  float64
    dtypes: float64(4), int64(1), object(2)
    memory usage: 547.0+ KB



```python
df.describe() # Show statistical features of numerical columns
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>10000.000000</td>
      <td>8757.000000</td>
      <td>9.478000e+03</td>
      <td>10000.000000</td>
      <td>10000.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>124999.830800</td>
      <td>83.010848</td>
      <td>3.947452e+06</td>
      <td>5159.959042</td>
      <td>3513.407322</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2887.279138</td>
      <td>150.782092</td>
      <td>6.935006e+06</td>
      <td>6324.461413</td>
      <td>3220.617458</td>
    </tr>
    <tr>
      <th>min</th>
      <td>120000.000000</td>
      <td>10.000000</td>
      <td>1.226724e+05</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>122499.750000</td>
      <td>17.000000</td>
      <td>7.727652e+05</td>
      <td>1434.381315</td>
      <td>1574.696984</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>124999.500000</td>
      <td>39.000000</td>
      <td>1.823444e+06</td>
      <td>3064.143332</td>
      <td>2096.721971</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>127500.250000</td>
      <td>91.000000</td>
      <td>4.303820e+06</td>
      <td>6461.376795</td>
      <td>4858.329282</td>
    </tr>
    <tr>
      <th>max</th>
      <td>130000.000000</td>
      <td>4552.000000</td>
      <td>1.856402e+08</td>
      <td>77668.300736</td>
      <td>17721.547768</td>
    </tr>
  </tbody>
</table>
</div>



### Peeking Into the Data

As with SQL tables, it is often a good idea to look at the `DataFrames` we are working with. Here are some basic methods for glancing at a `DataFrame`:


```python
# look at the list of columns
df.columns
```




    Index(['CUSTOMER_ID', 'INDUSTRY', 'EMP', 'ANNUAL_SALES', 'STATE', 'MOBILITY',
           'INTERNET'],
          dtype='object')




```python
# show the first 10 rows
df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>INDUSTRY</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>STATE</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>129078</td>
      <td>Finance and Insurance</td>
      <td>36.0</td>
      <td>NaN</td>
      <td>TX</td>
      <td>10192.825459</td>
      <td>699.539869</td>
    </tr>
    <tr>
      <th>1</th>
      <td>128424</td>
      <td>Construction</td>
      <td>261.0</td>
      <td>10675108.00</td>
      <td>NY</td>
      <td>17367.492873</td>
      <td>1907.819410</td>
    </tr>
    <tr>
      <th>2</th>
      <td>125960</td>
      <td>Finance and Insurance</td>
      <td>10.0</td>
      <td>756786.00</td>
      <td>TX</td>
      <td>6162.609229</td>
      <td>1789.017919</td>
    </tr>
    <tr>
      <th>3</th>
      <td>120981</td>
      <td>Construction</td>
      <td>31.0</td>
      <td>1223808.00</td>
      <td>NY</td>
      <td>19176.373541</td>
      <td>2123.016418</td>
    </tr>
    <tr>
      <th>4</th>
      <td>129251</td>
      <td>Education</td>
      <td>NaN</td>
      <td>1148650.00</td>
      <td>TX</td>
      <td>1538.194116</td>
      <td>1620.096543</td>
    </tr>
    <tr>
      <th>5</th>
      <td>128936</td>
      <td>Construction</td>
      <td>288.0</td>
      <td>9197420.44</td>
      <td>TX</td>
      <td>10177.116938</td>
      <td>8509.512884</td>
    </tr>
    <tr>
      <th>6</th>
      <td>120612</td>
      <td>Construction</td>
      <td>287.0</td>
      <td>12744917.00</td>
      <td>NY</td>
      <td>18418.015877</td>
      <td>16184.978195</td>
    </tr>
    <tr>
      <th>7</th>
      <td>123390</td>
      <td>Food Services</td>
      <td>14.0</td>
      <td>494593.20</td>
      <td>TX</td>
      <td>473.066999</td>
      <td>2366.373927</td>
    </tr>
    <tr>
      <th>8</th>
      <td>122024</td>
      <td>Education</td>
      <td>248.0</td>
      <td>NaN</td>
      <td>TX</td>
      <td>4468.821223</td>
      <td>9592.091142</td>
    </tr>
    <tr>
      <th>9</th>
      <td>120487</td>
      <td>Agriculture</td>
      <td>47.0</td>
      <td>1854011.00</td>
      <td>TX</td>
      <td>1675.872927</td>
      <td>9333.580904</td>
    </tr>
  </tbody>
</table>
</div>




```python
# show the last 10 rows
df.tail(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>INDUSTRY</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>STATE</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>9990</th>
      <td>129063</td>
      <td>Food Services</td>
      <td>40.0</td>
      <td>1388302.2</td>
      <td>TX</td>
      <td>1249.068844</td>
      <td>2368.751847</td>
    </tr>
    <tr>
      <th>9991</th>
      <td>125817</td>
      <td>Finance and Insurance</td>
      <td>10.0</td>
      <td>618211.5</td>
      <td>TX</td>
      <td>10646.644319</td>
      <td>2060.538164</td>
    </tr>
    <tr>
      <th>9992</th>
      <td>125010</td>
      <td>Finance and Insurance</td>
      <td>14.0</td>
      <td>1244916.0</td>
      <td>TX</td>
      <td>1565.632355</td>
      <td>2340.819552</td>
    </tr>
    <tr>
      <th>9993</th>
      <td>121190</td>
      <td>Agriculture</td>
      <td>10.0</td>
      <td>516848.0</td>
      <td>NY</td>
      <td>1568.250921</td>
      <td>2229.537051</td>
    </tr>
    <tr>
      <th>9994</th>
      <td>121448</td>
      <td>Food Services</td>
      <td>19.0</td>
      <td>857120.0</td>
      <td>NY</td>
      <td>1812.162406</td>
      <td>6981.409397</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>128551</td>
      <td>Finance and Insurance</td>
      <td>30.0</td>
      <td>1740370.0</td>
      <td>NY</td>
      <td>796.064772</td>
      <td>7985.736865</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>125090</td>
      <td>Construction</td>
      <td>51.0</td>
      <td>2758514.0</td>
      <td>NY</td>
      <td>6330.410019</td>
      <td>10344.420439</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>129035</td>
      <td>Food Services</td>
      <td>97.0</td>
      <td>2200024.8</td>
      <td>TX</td>
      <td>1033.965711</td>
      <td>2529.527266</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>126395</td>
      <td>Food Services</td>
      <td>40.0</td>
      <td>1745366.0</td>
      <td>NY</td>
      <td>3246.858268</td>
      <td>9542.715917</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>120275</td>
      <td>Finance and Insurance</td>
      <td>97.0</td>
      <td>NaN</td>
      <td>TX</td>
      <td>3633.894493</td>
      <td>12445.147141</td>
    </tr>
  </tbody>
</table>
</div>




```python
# get the summary stats for numeric columns
df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>10000.000000</td>
      <td>8757.000000</td>
      <td>9.478000e+03</td>
      <td>10000.000000</td>
      <td>10000.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>124999.830800</td>
      <td>83.010848</td>
      <td>3.947452e+06</td>
      <td>5159.959042</td>
      <td>3513.407322</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2887.279138</td>
      <td>150.782092</td>
      <td>6.935006e+06</td>
      <td>6324.461413</td>
      <td>3220.617458</td>
    </tr>
    <tr>
      <th>min</th>
      <td>120000.000000</td>
      <td>10.000000</td>
      <td>1.226724e+05</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>122499.750000</td>
      <td>17.000000</td>
      <td>7.727652e+05</td>
      <td>1434.381315</td>
      <td>1574.696984</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>124999.500000</td>
      <td>39.000000</td>
      <td>1.823444e+06</td>
      <td>3064.143332</td>
      <td>2096.721971</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>127500.250000</td>
      <td>91.000000</td>
      <td>4.303820e+06</td>
      <td>6461.376795</td>
      <td>4858.329282</td>
    </tr>
    <tr>
      <th>max</th>
      <td>130000.000000</td>
      <td>4552.000000</td>
      <td>1.856402e+08</td>
      <td>77668.300736</td>
      <td>17721.547768</td>
    </tr>
  </tbody>
</table>
</div>



### The `loc()` Method

To select certain columns and rows in a `DataFrame`, it is best to use the `loc()` (location) method. The method expects two parameters 
* a set of column names, and
* a set of row indices 

to search the `DataFrame` for.

We can find all of the values within a column:


```python
# get all annual sales
df.loc[:, "ANNUAL_SALES"] # Return all the rows, from the ANNUAL_SALES column
# df[['ANNUAL_SALES']] # Alternatively, to get a whole column, you can call its name with double [[]]
```




    0              NaN
    1       10675108.0
    2         756786.0
    3        1223808.0
    4        1148650.0
               ...    
    9995     1740370.0
    9996     2758514.0
    9997     2200024.8
    9998     1745366.0
    9999           NaN
    Name: ANNUAL_SALES, Length: 10000, dtype: float64



We can also use the slice notation on index values:


```python
# get rows 5-10 from annual sales
df.loc[5:10, 'ANNUAL_SALES'] # Inclusive of upper and lower bound
```




    5      9197420.44
    6     12744917.00
    7       494593.20
    8             NaN
    9      1854011.00
    10     3098014.50
    Name: ANNUAL_SALES, dtype: float64



Notice the return value of the last two operations is a Pandas `Series`. Like we said, within a `DataFrame`, each individual column is a Series.

**IMPORTANT NOTE**: When slicing rows using ``loc``, it performs an **inclusive slice**. Notice that ``5:10`` gives rows at indices 5,6,7,8,9 **and** 10. This is different from what we've seen so far where slices are not inclusive. 

We can use a list to select out particular columns (notice the return type is no longer a `Series`).


```python
# get the state and internet columns
df.loc[5:10, ['STATE', 'INTERNET']] # Return a slice of the DataFrame
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>STATE</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>TX</td>
      <td>8509.512884</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NY</td>
      <td>16184.978195</td>
    </tr>
    <tr>
      <th>7</th>
      <td>TX</td>
      <td>2366.373927</td>
    </tr>
    <tr>
      <th>8</th>
      <td>TX</td>
      <td>9592.091142</td>
    </tr>
    <tr>
      <th>9</th>
      <td>TX</td>
      <td>9333.580904</td>
    </tr>
    <tr>
      <th>10</th>
      <td>TX</td>
      <td>4140.604965</td>
    </tr>
  </tbody>
</table>
</div>



We can even use a list to pick out particular rows.


```python
df.loc[[1, 3, 5, 99], ['STATE', 'INTERNET']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>STATE</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>NY</td>
      <td>1907.819410</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NY</td>
      <td>2123.016418</td>
    </tr>
    <tr>
      <th>5</th>
      <td>TX</td>
      <td>8509.512884</td>
    </tr>
    <tr>
      <th>99</th>
      <td>NY</td>
      <td>7653.706991</td>
    </tr>
  </tbody>
</table>
</div>



Finally, we can take a continuous slice over a range of column values:

**Note**: You should almost never slice over a range of column values, they should not have a notion of order to them. It is more appropriate to pick out particular values using a list.

### `loc()` and `iloc()`

As we saw before, we can use a non-default index value for a `DataFrame`. The underlying index $0,1,2\cdots$ is still there and we can access it using the `iloc()` method. Note, `iloc()` can't take the column names like `loc()` would, so we need to use an additional `loc()` to pick out the column values we want.


```python
df = pd.DataFrame(
    np.random.randint(0, 5, size=(10, 5)),
    columns=['First', 'Second', 'Third', 'Fourth', 'Fifth'],
    index=list('abcdefghij')
)

df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>Fourth</th>
      <th>Fifth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>c</th>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>d</th>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>e</th>
      <td>0</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# using .loc and only numeric indices
df.iloc[1:5, 1:3]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Second</th>
      <th>Third</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>b</th>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>c</th>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>d</th>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>e</th>
      <td>1</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Using .loc with actual index names and column names
df.loc[['b', 'c', 'd', 'e'], ['Second','Third']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Second</th>
      <th>Third</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>b</th>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>c</th>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>d</th>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>e</th>
      <td>1</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
# using an iloc to slice rows by numeric indices
# and loc to slice columns by column names
df.iloc[1:5].loc[:, ['Second', 'Third']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Second</th>
      <th>Third</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>b</th>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>c</th>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>d</th>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>e</th>
      <td>1</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



**IMPORTANT NOTE**: the `iloc` method works like usual list slicing and the right end of **the right end of the slice is excluded**.

### Modifying the DataFrame

There are several methods for adding to, removing from and modifying the contents of a `DataFrame`. Adding a new column (all with the same value):


```python
# new column with a constant value
df['Sixth'] = 'filler value'
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>Fourth</th>
      <th>Fifth</th>
      <th>Sixth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>filler value</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>filler value</td>
    </tr>
    <tr>
      <th>c</th>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>3</td>
      <td>filler value</td>
    </tr>
    <tr>
      <th>d</th>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
      <td>1</td>
      <td>filler value</td>
    </tr>
    <tr>
      <th>e</th>
      <td>0</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>filler value</td>
    </tr>
  </tbody>
</table>
</div>



Or as a predefined array:


```python
# new column with random values
df['Seventh'] = np.random.randn(10)
# df.Seventh also refers to the column
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>Fourth</th>
      <th>Fifth</th>
      <th>Sixth</th>
      <th>Seventh</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>filler value</td>
      <td>0.154955</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>filler value</td>
      <td>0.806611</td>
    </tr>
    <tr>
      <th>c</th>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>3</td>
      <td>filler value</td>
      <td>0.062842</td>
    </tr>
    <tr>
      <th>d</th>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
      <td>1</td>
      <td>filler value</td>
      <td>-0.648417</td>
    </tr>
    <tr>
      <th>e</th>
      <td>0</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>filler value</td>
      <td>0.041664</td>
    </tr>
    <tr>
      <th>f</th>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>4</td>
      <td>4</td>
      <td>filler value</td>
      <td>-0.365568</td>
    </tr>
    <tr>
      <th>g</th>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>filler value</td>
      <td>-3.177354</td>
    </tr>
    <tr>
      <th>h</th>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>filler value</td>
      <td>-1.174056</td>
    </tr>
    <tr>
      <th>i</th>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>2</td>
      <td>filler value</td>
      <td>-1.421996</td>
    </tr>
    <tr>
      <th>j</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>filler value</td>
      <td>-0.208318</td>
    </tr>
  </tbody>
</table>
</div>



We can modify a DataFrame with the `.loc()` method.


```python
# add new row with constant values
df.loc['k', :] = 'Last Row'
```


```python
df.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>Fourth</th>
      <th>Fifth</th>
      <th>Sixth</th>
      <th>Seventh</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>g</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>filler value</td>
      <td>-3.177354</td>
    </tr>
    <tr>
      <th>h</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>filler value</td>
      <td>-1.174056</td>
    </tr>
    <tr>
      <th>i</th>
      <td>1.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>filler value</td>
      <td>-1.421996</td>
    </tr>
    <tr>
      <th>j</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>filler value</td>
      <td>-0.208318</td>
    </tr>
    <tr>
      <th>k</th>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
    </tr>
  </tbody>
</table>
</div>



If we use a `pd.Series` to fill in the values, the indices of the dataframe and series must be matching:


```python
df.loc['c':'e', ['Third', 'Fifth']] = 'The Middle'
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>Fourth</th>
      <th>Fifth</th>
      <th>Sixth</th>
      <th>Seventh</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>2.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>filler value</td>
      <td>0.154955</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>filler value</td>
      <td>0.806611</td>
    </tr>
    <tr>
      <th>c</th>
      <td>4.0</td>
      <td>1.0</td>
      <td>The Middle</td>
      <td>2.0</td>
      <td>The Middle</td>
      <td>filler value</td>
      <td>0.062842</td>
    </tr>
    <tr>
      <th>d</th>
      <td>1.0</td>
      <td>3.0</td>
      <td>The Middle</td>
      <td>4.0</td>
      <td>The Middle</td>
      <td>filler value</td>
      <td>-0.648417</td>
    </tr>
    <tr>
      <th>e</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>The Middle</td>
      <td>0.0</td>
      <td>The Middle</td>
      <td>filler value</td>
      <td>0.041664</td>
    </tr>
    <tr>
      <th>f</th>
      <td>2.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>filler value</td>
      <td>-0.365568</td>
    </tr>
    <tr>
      <th>g</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>filler value</td>
      <td>-3.177354</td>
    </tr>
    <tr>
      <th>h</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>filler value</td>
      <td>-1.174056</td>
    </tr>
    <tr>
      <th>i</th>
      <td>1.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>filler value</td>
      <td>-1.421996</td>
    </tr>
    <tr>
      <th>j</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>filler value</td>
      <td>-0.208318</td>
    </tr>
    <tr>
      <th>k</th>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc[:, 'First'] = pd.Series([0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20],
                              index = df.index) # Length of series has to match length of dataframe
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Second</th>
      <th>Third</th>
      <th>Fourth</th>
      <th>Fifth</th>
      <th>Sixth</th>
      <th>Seventh</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>filler value</td>
      <td>0.154955</td>
    </tr>
    <tr>
      <th>b</th>
      <td>2</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>filler value</td>
      <td>0.806611</td>
    </tr>
    <tr>
      <th>c</th>
      <td>4</td>
      <td>1.0</td>
      <td>The Middle</td>
      <td>2.0</td>
      <td>The Middle</td>
      <td>filler value</td>
      <td>0.062842</td>
    </tr>
    <tr>
      <th>d</th>
      <td>6</td>
      <td>3.0</td>
      <td>The Middle</td>
      <td>4.0</td>
      <td>The Middle</td>
      <td>filler value</td>
      <td>-0.648417</td>
    </tr>
    <tr>
      <th>e</th>
      <td>8</td>
      <td>1.0</td>
      <td>The Middle</td>
      <td>0.0</td>
      <td>The Middle</td>
      <td>filler value</td>
      <td>0.041664</td>
    </tr>
    <tr>
      <th>f</th>
      <td>10</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>filler value</td>
      <td>-0.365568</td>
    </tr>
    <tr>
      <th>g</th>
      <td>12</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>filler value</td>
      <td>-3.177354</td>
    </tr>
    <tr>
      <th>h</th>
      <td>14</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>filler value</td>
      <td>-1.174056</td>
    </tr>
    <tr>
      <th>i</th>
      <td>16</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>filler value</td>
      <td>-1.421996</td>
    </tr>
    <tr>
      <th>j</th>
      <td>18</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>filler value</td>
      <td>-0.208318</td>
    </tr>
    <tr>
      <th>k</th>
      <td>20</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
      <td>Last Row</td>
    </tr>
  </tbody>
</table>
</div>



When adding rows, it's generally best to combine `DataFrames` with the `concat` method. But if we just need to add one row, we can use `.loc` and add to the end (or length) of the `DataFrame`.

---

#### Exercise 1

1. Use the following code to create a DataFrame with 10 rows and 5 columns (call the columns A-E). The contents of the frame are just random numbers from the Normal distribution.

    1. Double the value of all the numbers in columns A and E.
    2. Halve the value of all the numbers in rows 2-7.
    3. Set the value in row 8 column B to 0
    
---


```python
mydf = pd.DataFrame(
    np.random.randn(10, 5), 
    columns=list('ABCDE')
)

display(mydf)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.374859</td>
      <td>-0.902609</td>
      <td>-0.984289</td>
      <td>0.471039</td>
      <td>0.547656</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.124745</td>
      <td>0.964995</td>
      <td>-1.394960</td>
      <td>0.959817</td>
      <td>0.391754</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.181011</td>
      <td>0.840361</td>
      <td>0.891238</td>
      <td>-0.912300</td>
      <td>-1.317295</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.587314</td>
      <td>0.777105</td>
      <td>-2.274251</td>
      <td>1.226753</td>
      <td>-0.268346</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.585237</td>
      <td>-0.376161</td>
      <td>0.317072</td>
      <td>-0.066616</td>
      <td>-1.204114</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.268263</td>
      <td>1.050314</td>
      <td>0.711447</td>
      <td>-0.290362</td>
      <td>-1.198061</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.224012</td>
      <td>-0.766828</td>
      <td>-0.920542</td>
      <td>0.965686</td>
      <td>0.733263</td>
    </tr>
    <tr>
      <th>7</th>
      <td>-0.072731</td>
      <td>2.227437</td>
      <td>-0.796649</td>
      <td>0.356827</td>
      <td>-1.454472</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.496643</td>
      <td>-0.586505</td>
      <td>-0.764505</td>
      <td>0.618213</td>
      <td>0.378631</td>
    </tr>
    <tr>
      <th>9</th>
      <td>-0.389325</td>
      <td>-1.762236</td>
      <td>0.121732</td>
      <td>0.282148</td>
      <td>0.437051</td>
    </tr>
  </tbody>
</table>
</div>



```python
# Double columns A and E
mydf.loc[:, ['A', 'E']] = mydf.loc[:, ['A', 'E']] * 2
# mydf.loc[:, ['A', 'E']] *= 2
# mydf[['A', 'E']] *= 2 # Call the entire column directly
```


```python
# Half rows 2:7
mydf.loc[2:7, :] = mydf.loc[2:7, :] / 2

```


```python
# Set the value in row 8 column B to 0
mydf.loc[8, 'B'] = 0
```


```python
mydf
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.749717</td>
      <td>-0.902609</td>
      <td>-0.984289</td>
      <td>0.471039</td>
      <td>1.095311</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.249490</td>
      <td>0.964995</td>
      <td>-1.394960</td>
      <td>0.959817</td>
      <td>0.783508</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.181011</td>
      <td>0.420181</td>
      <td>0.445619</td>
      <td>-0.456150</td>
      <td>-1.317295</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.587314</td>
      <td>0.388552</td>
      <td>-1.137125</td>
      <td>0.613376</td>
      <td>-0.268346</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.585237</td>
      <td>-0.188081</td>
      <td>0.158536</td>
      <td>-0.033308</td>
      <td>-1.204114</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.268263</td>
      <td>0.525157</td>
      <td>0.355724</td>
      <td>-0.145181</td>
      <td>-1.198061</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.224012</td>
      <td>-0.383414</td>
      <td>-0.460271</td>
      <td>0.482843</td>
      <td>0.733263</td>
    </tr>
    <tr>
      <th>7</th>
      <td>-0.072731</td>
      <td>1.113719</td>
      <td>-0.398324</td>
      <td>0.178413</td>
      <td>-1.454472</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.993286</td>
      <td>0.000000</td>
      <td>-0.764505</td>
      <td>0.618213</td>
      <td>0.757261</td>
    </tr>
    <tr>
      <th>9</th>
      <td>-0.778649</td>
      <td>-1.762236</td>
      <td>0.121732</td>
      <td>0.282148</td>
      <td>0.874102</td>
    </tr>
  </tbody>
</table>
</div>



### Querying Data with Conditions

All of the selection we've done up until this point has been by row or column label/index. What if we wanted to find out which parts of our data satisfy a specific condition?  There are a number of ways to do this but most commonly we use the syntax ```df[conditions]``` where ```conditions``` is a True/False array (or Series) specifying which rows to select.

Let's use the `customer_info.csv` again:


```python
df = pd.read_csv("data/customer_info.csv")
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>INDUSTRY</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>STATE</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>129078</td>
      <td>Finance and Insurance</td>
      <td>36.0</td>
      <td>NaN</td>
      <td>TX</td>
      <td>10192.825459</td>
      <td>699.539869</td>
    </tr>
    <tr>
      <th>1</th>
      <td>128424</td>
      <td>Construction</td>
      <td>261.0</td>
      <td>10675108.0</td>
      <td>NY</td>
      <td>17367.492873</td>
      <td>1907.819410</td>
    </tr>
    <tr>
      <th>2</th>
      <td>125960</td>
      <td>Finance and Insurance</td>
      <td>10.0</td>
      <td>756786.0</td>
      <td>TX</td>
      <td>6162.609229</td>
      <td>1789.017919</td>
    </tr>
    <tr>
      <th>3</th>
      <td>120981</td>
      <td>Construction</td>
      <td>31.0</td>
      <td>1223808.0</td>
      <td>NY</td>
      <td>19176.373541</td>
      <td>2123.016418</td>
    </tr>
    <tr>
      <th>4</th>
      <td>129251</td>
      <td>Education</td>
      <td>NaN</td>
      <td>1148650.0</td>
      <td>TX</td>
      <td>1538.194116</td>
      <td>1620.096543</td>
    </tr>
  </tbody>
</table>
</div>



To begin with, let's find all rows where the values of *EMP* is larger than 3000.


```python
# Define a filter
EMP_filter = df['EMP'] > 3000 # Returns a list of True/False values
df[EMP_filter] # Use the defined variable name as a filter
# df[df['EMP'] > 3000] alternatively, use the filter directly
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>INDUSTRY</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>STATE</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>351</th>
      <td>129495</td>
      <td>Construction</td>
      <td>4552.0</td>
      <td>1.293381e+08</td>
      <td>TX</td>
      <td>64092.287431</td>
      <td>10718.696925</td>
    </tr>
    <tr>
      <th>9486</th>
      <td>129052</td>
      <td>Construction</td>
      <td>3233.0</td>
      <td>1.856402e+08</td>
      <td>TX</td>
      <td>74252.814304</td>
      <td>2516.890826</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Define a filter for state is texas
state_filter = df['STATE'] == 'TX'
# Slice the dataframe based on that filter
df[state_filter].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>INDUSTRY</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>STATE</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>129078</td>
      <td>Finance and Insurance</td>
      <td>36.0</td>
      <td>NaN</td>
      <td>TX</td>
      <td>10192.825459</td>
      <td>699.539869</td>
    </tr>
    <tr>
      <th>2</th>
      <td>125960</td>
      <td>Finance and Insurance</td>
      <td>10.0</td>
      <td>756786.00</td>
      <td>TX</td>
      <td>6162.609229</td>
      <td>1789.017919</td>
    </tr>
    <tr>
      <th>4</th>
      <td>129251</td>
      <td>Education</td>
      <td>NaN</td>
      <td>1148650.00</td>
      <td>TX</td>
      <td>1538.194116</td>
      <td>1620.096543</td>
    </tr>
    <tr>
      <th>5</th>
      <td>128936</td>
      <td>Construction</td>
      <td>288.0</td>
      <td>9197420.44</td>
      <td>TX</td>
      <td>10177.116938</td>
      <td>8509.512884</td>
    </tr>
    <tr>
      <th>7</th>
      <td>123390</td>
      <td>Food Services</td>
      <td>14.0</td>
      <td>494593.20</td>
      <td>TX</td>
      <td>473.066999</td>
      <td>2366.373927</td>
    </tr>
  </tbody>
</table>
</div>



Let's now use these Boolean values to pull out all the rows of our DataFrame where EMP>3000:


```python

```

We can combine multiple conditions, but we have to be careful with the syntax.  
* use ``&`` instead of ``and``,
* use ``|`` instead of ``or``.

Also, we must wrap each condition in parentheses, so Pandas knows where to parse their beginnings and endings.

If we wanted all rows with the *EMP* larger than 2000 and the state was NY, we would use:


```python
filter1 =  df['EMP'] > 2000 # filter for EMP
filter2 =  df['STATE'] == 'NY' # filter for state
filter3 = (df['EMP'] > 2000) & (df['STATE'] == 'NY')  # combined filter
df[filter1 & filter2]   # Use individual filters

df[filter3]    # Use combined filter
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>INDUSTRY</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>STATE</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7641</th>
      <td>121664</td>
      <td>Construction</td>
      <td>2278.0</td>
      <td>72690900.0</td>
      <td>NY</td>
      <td>64098.358888</td>
      <td>1277.335967</td>
    </tr>
  </tbody>
</table>
</div>



---
#### Exercise 2

2. Using `customer_info.csv` how would you find the following rows:
    1. All rows in the Construction industry in either NY or TX
    2. All rows in the Construction or Education industry in TX or NY
    3. All rows in the Construction Industry in TX or all rows outside TX.
---


```python
df['STATE'].unique()
```




    array(['TX', 'NY'], dtype=object)




```python
construction_filter = df['INDUSTRY'] == 'Construction'
education_filter = df['INDUSTRY'] == 'Education'
TX_filter = df['STATE'] == 'TX'
NY_filter = df['STATE'] == 'NY'
# All rows in the Construction industry in either NY or TX
df[construction_filter & (NY_filter | TX_filter)]
# All rows in the Construction or Education industry in TX or NY
df[(construction_filter | education_filter) & (TX_filter | NY_filter)]
# All rows in the Construction Industry in TX or all rows outside TX
df[ (construction_filter & TX_filter) | ~TX_filter]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUSTOMER_ID</th>
      <th>INDUSTRY</th>
      <th>EMP</th>
      <th>ANNUAL_SALES</th>
      <th>STATE</th>
      <th>MOBILITY</th>
      <th>INTERNET</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>128424</td>
      <td>Construction</td>
      <td>261.0</td>
      <td>10675108.00</td>
      <td>NY</td>
      <td>17367.492873</td>
      <td>1907.819410</td>
    </tr>
    <tr>
      <th>3</th>
      <td>120981</td>
      <td>Construction</td>
      <td>31.0</td>
      <td>1223808.00</td>
      <td>NY</td>
      <td>19176.373541</td>
      <td>2123.016418</td>
    </tr>
    <tr>
      <th>5</th>
      <td>128936</td>
      <td>Construction</td>
      <td>288.0</td>
      <td>9197420.44</td>
      <td>TX</td>
      <td>10177.116938</td>
      <td>8509.512884</td>
    </tr>
    <tr>
      <th>6</th>
      <td>120612</td>
      <td>Construction</td>
      <td>287.0</td>
      <td>12744917.00</td>
      <td>NY</td>
      <td>18418.015877</td>
      <td>16184.978195</td>
    </tr>
    <tr>
      <th>11</th>
      <td>126049</td>
      <td>Construction</td>
      <td>72.0</td>
      <td>3590242.00</td>
      <td>NY</td>
      <td>8010.260516</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9993</th>
      <td>121190</td>
      <td>Agriculture</td>
      <td>10.0</td>
      <td>516848.00</td>
      <td>NY</td>
      <td>1568.250921</td>
      <td>2229.537051</td>
    </tr>
    <tr>
      <th>9994</th>
      <td>121448</td>
      <td>Food Services</td>
      <td>19.0</td>
      <td>857120.00</td>
      <td>NY</td>
      <td>1812.162406</td>
      <td>6981.409397</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>128551</td>
      <td>Finance and Insurance</td>
      <td>30.0</td>
      <td>1740370.00</td>
      <td>NY</td>
      <td>796.064772</td>
      <td>7985.736865</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>125090</td>
      <td>Construction</td>
      <td>51.0</td>
      <td>2758514.00</td>
      <td>NY</td>
      <td>6330.410019</td>
      <td>10344.420439</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>126395</td>
      <td>Food Services</td>
      <td>40.0</td>
      <td>1745366.00</td>
      <td>NY</td>
      <td>3246.858268</td>
      <td>9542.715917</td>
    </tr>
  </tbody>
</table>
<p>5962 rows × 7 columns</p>
</div>



### Merging DataFrames using ``concat``

Just like with SQL tables, we can merge together (join) Pandas `DataFrames`. 

The `concat` method will merge dataframes by either 

* extending the index (axis = 0), which is the default, or
* extending the columns (axis = 1).

#### Scenario 1: Extending the index (axis = 0)

Let's say we have two data frames that have the **same column names**.


```python
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3']})

df2 = pd.DataFrame({'A': ['A0*', 'A1*', 'A2*', 'A3*'],
                    'B': ['B0*', 'B1*', 'B2*', 'B3*'],
                    'C': ['C0*', 'C1*', 'C2*', 'C3*']})
```


```python
display(df1)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
    </tr>
  </tbody>
</table>
</div>



```python
display(df2)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0*</td>
      <td>B0*</td>
      <td>C0*</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1*</td>
      <td>B1*</td>
      <td>C1*</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2*</td>
      <td>B2*</td>
      <td>C2*</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3*</td>
      <td>B3*</td>
      <td>C3*</td>
    </tr>
  </tbody>
</table>
</div>


The simplest join is done using the `concat` method. Concat just appends one dataframe onto another. 

By default (`axis = 0`), it works to extend the index which just means that it will try to stack the data frames vertically. Hence, the number of rows in the resulting dataframe should be the sum of number of rows in the individual frames. 

If the dataframes have exact column names, the concatenation of two $(4 \times 3)$ data frames will result in one $(8 \times 3)$  dataframe


Let's put our two frames together and see if this holds up. 


```python
pd.concat([df1, df2], axis = 0) # Stack rows on top of each other, match columns

pd.concat([df1, df2], axis = 1) # Stack columns side by side, match rows
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>A0*</td>
      <td>B0*</td>
      <td>C0*</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>A1*</td>
      <td>B1*</td>
      <td>C1*</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>A2*</td>
      <td>B2*</td>
      <td>C2*</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>A3*</td>
      <td>B3*</td>
      <td>C3*</td>
    </tr>
  </tbody>
</table>
</div>



We got exactly what we expected, with maybe the exception of the index values. The index values stay the same as in the original dataframes, which we might not have wanted. 

In order to reset the index after merging, we can use the `.reset_index` method. The `drop = True` signifies that we should drop the old index values. Reseting the index will be a very common task in data cleaning and wrangling.


```python
#pd.concat([df1, df2], axis = 0).reset_index() # Stack rows on top of each other, match columns
# Reindex to start from 0. But we have kept our old index as a new column
pd.concat([df1, df2], axis = 0).reset_index(drop = True)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A0*</td>
      <td>B0*</td>
      <td>C0*</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A1*</td>
      <td>B1*</td>
      <td>C1*</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A2*</td>
      <td>B2*</td>
      <td>C2*</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A3*</td>
      <td>B3*</td>
      <td>C3*</td>
    </tr>
  </tbody>
</table>
</div>



The same result can be achieved by setting `ignore_index=True` in `concat`.


```python
pd.concat([df1, df2], axis = 0, ignore_index = True)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A0*</td>
      <td>B0*</td>
      <td>C0*</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A1*</td>
      <td>B1*</td>
      <td>C1*</td>
    </tr>
    <tr>
      <th>6</th>
      <td>A2*</td>
      <td>B2*</td>
      <td>C2*</td>
    </tr>
    <tr>
      <th>7</th>
      <td>A3*</td>
      <td>B3*</td>
      <td>C3*</td>
    </tr>
  </tbody>
</table>
</div>



What if we have two data frames that have **different column names**?


```python
df3 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3']})
display(df3)

df4 = pd.DataFrame({'D': ['D1', 'D2', 'D3', 'D4'],
                    'E': ['E1', 'E2', 'E3', 'E4'],
                    'F': ['F0', 'F1', 'F2', 'F3']})
display(df4)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>D</th>
      <th>E</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>D1</td>
      <td>E1</td>
      <td>F0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>D2</td>
      <td>E2</td>
      <td>F1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>D3</td>
      <td>E3</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D4</td>
      <td>E4</td>
      <td>F3</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.concat([df3, df4], axis=0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>D1</td>
      <td>E1</td>
      <td>F0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>D2</td>
      <td>E2</td>
      <td>F1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>D3</td>
      <td>E3</td>
      <td>F2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>D4</td>
      <td>E4</td>
      <td>F3</td>
    </tr>
  </tbody>
</table>
</div>



This looks really weird, it's full of `NaN` values! The problem is that the column names did not line up, so when we concatenated the new dataframe, it kept all six columns but filled in `NaN` values where there were no values before.

#### Scenario 2: Extending the columns (axis = 1)

Let's say we have two data frames that have the **same index names**.


We can do the concatenate the dataframes by extending the columns, which means we are going to try to stack the dataframes horizontally. In this case, the number of columns in the resulting dataframe should be the sum of number ofcolumns in the individual frames. 

If the dataframes have exact index names, the concatenation of two $(4 \times 3)$ data frames will result in one $(4 \times 6)$  dataframe




```python
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3']})

display(df1)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
    </tr>
  </tbody>
</table>
</div>



```python
df2 = pd.DataFrame({'A': ['A0*', 'A1*', 'A2*', 'A3*'],
                    'B': ['B0*', 'B1*', 'B2*', 'B3*'],
                    'C': ['C0*', 'C1*', 'C2*', 'C3*']})

display(df2)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0*</td>
      <td>B0*</td>
      <td>C0*</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1*</td>
      <td>B1*</td>
      <td>C1*</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2*</td>
      <td>B2*</td>
      <td>C2*</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3*</td>
      <td>B3*</td>
      <td>C3*</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.concat([df1, df2], axis = 1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>A0*</td>
      <td>B0*</td>
      <td>C0*</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>A1*</td>
      <td>B1*</td>
      <td>C1*</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>A2*</td>
      <td>B2*</td>
      <td>C2*</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>A3*</td>
      <td>B3*</td>
      <td>C3*</td>
    </tr>
  </tbody>
</table>
</div>



What if we have two data frames that have **different index names**?


```python
# with different indexes
df3 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3']},
                    index = list('abcd')) # changing the index here

display(df3)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
    </tr>
    <tr>
      <th>c</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
    </tr>
    <tr>
      <th>d</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
    </tr>
  </tbody>
</table>
</div>



```python
df4 = pd.DataFrame({'A': ['A0*', 'A1*', 'A2*', 'A3*'],
                    'B': ['B0*', 'B1*', 'B2*', 'B3*'],
                    'C': ['C0*', 'C1*', 'C2*', 'C3*']},
                    index = list('efgh')) # changing the index here

display(df4)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>e</th>
      <td>A0*</td>
      <td>B0*</td>
      <td>C0*</td>
    </tr>
    <tr>
      <th>f</th>
      <td>A1*</td>
      <td>B1*</td>
      <td>C1*</td>
    </tr>
    <tr>
      <th>g</th>
      <td>A2*</td>
      <td>B2*</td>
      <td>C2*</td>
    </tr>
    <tr>
      <th>h</th>
      <td>A3*</td>
      <td>B3*</td>
      <td>C3*</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.concat([df3, df4], axis = 1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>b</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>c</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>d</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>e</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A0*</td>
      <td>B0*</td>
      <td>C0*</td>
    </tr>
    <tr>
      <th>f</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A1*</td>
      <td>B1*</td>
      <td>C1*</td>
    </tr>
    <tr>
      <th>g</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A2*</td>
      <td>B2*</td>
      <td>C2*</td>
    </tr>
    <tr>
      <th>h</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A3*</td>
      <td>B3*</td>
      <td>C3*</td>
    </tr>
  </tbody>
</table>
</div>



If we try to concatenate the two data frames with different index values by extending the columns:


```python
pd.concat([df3, df4], axis = 1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>A0</td>
      <td>B0</td>
      <td>C0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>b</th>
      <td>A1</td>
      <td>B1</td>
      <td>C1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>c</th>
      <td>A2</td>
      <td>B2</td>
      <td>C2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>d</th>
      <td>A3</td>
      <td>B3</td>
      <td>C3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>e</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A0*</td>
      <td>B0*</td>
      <td>C0*</td>
    </tr>
    <tr>
      <th>f</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A1*</td>
      <td>B1*</td>
      <td>C1*</td>
    </tr>
    <tr>
      <th>g</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A2*</td>
      <td>B2*</td>
      <td>C2*</td>
    </tr>
    <tr>
      <th>h</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A3*</td>
      <td>B3*</td>
      <td>C3*</td>
    </tr>
  </tbody>
</table>
</div>



This example also came out a bit weird. This happened because the frames had a different index. It's just like the example when they didn't match on column values and we were **lengthening** the frame. Now that it does not match on index values, we are **widening** the frame. 

### Merging DataFrames using ``merge`` 

If we want to perform joins in a way that is more similar to the SQL we've seen, we should instead use the `merge` function. First, let's get some data.
- [Publishers data](https://drive.google.com/open?id=1Fj1-9M4LFpsWrFqYeKi5H_HFC2CEN7Yk)
- [Superheroes data](https://drive.google.com/open?id=1xVEtZhRTewbZI2ba_7tzOx5ahE7M6G4B)


```python
superheroes = pd.read_csv('data/superheroes.csv')

display(superheroes)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>alignment</th>
      <th>gender</th>
      <th>publisher</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Magneto</td>
      <td>bad</td>
      <td>male</td>
      <td>Marvel</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Storm</td>
      <td>good</td>
      <td>female</td>
      <td>Marvel</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mystique</td>
      <td>bad</td>
      <td>female</td>
      <td>Marvel</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Batman</td>
      <td>good</td>
      <td>male</td>
      <td>DC</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joker</td>
      <td>bad</td>
      <td>male</td>
      <td>DC</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Catwoman</td>
      <td>bad</td>
      <td>female</td>
      <td>DC</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hellboy</td>
      <td>good</td>
      <td>male</td>
      <td>Dark Horse Comics</td>
    </tr>
  </tbody>
</table>
</div>



```python
publishers = pd.read_csv('data/publishers.csv')

display(publishers)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>publisher</th>
      <th>yr_founded</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>DC</td>
      <td>1934</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Marvel</td>
      <td>1939</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Image</td>
      <td>1992</td>
    </tr>
  </tbody>
</table>
</div>


Let's start with an example where we join on the *publisher* column:


```python
# SQL:
# SELECT *
# FROM superheroes INNER JOIN publishers
# ON superheroes.publisher = publishers.publisher;

pd.merge(superheroes, publishers, on = 'publisher')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>alignment</th>
      <th>gender</th>
      <th>publisher</th>
      <th>yr_founded</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Magneto</td>
      <td>bad</td>
      <td>male</td>
      <td>Marvel</td>
      <td>1939</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Storm</td>
      <td>good</td>
      <td>female</td>
      <td>Marvel</td>
      <td>1939</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mystique</td>
      <td>bad</td>
      <td>female</td>
      <td>Marvel</td>
      <td>1939</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Batman</td>
      <td>good</td>
      <td>male</td>
      <td>DC</td>
      <td>1934</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joker</td>
      <td>bad</td>
      <td>male</td>
      <td>DC</td>
      <td>1934</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Catwoman</td>
      <td>bad</td>
      <td>female</td>
      <td>DC</td>
      <td>1934</td>
    </tr>
  </tbody>
</table>
</div>



Just like SQL, we can specify all sorts of joins (left/right/inner/outer):


```python
# SQL:
# SELECT *
# FROM superheroes LEFT JOIN publishers
# ON superheroes.publisher = publishers.publisher;

pd.merge(superheroes, publishers, on = 'publisher', how = 'left')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>alignment</th>
      <th>gender</th>
      <th>publisher</th>
      <th>yr_founded</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Magneto</td>
      <td>bad</td>
      <td>male</td>
      <td>Marvel</td>
      <td>1939.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Storm</td>
      <td>good</td>
      <td>female</td>
      <td>Marvel</td>
      <td>1939.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mystique</td>
      <td>bad</td>
      <td>female</td>
      <td>Marvel</td>
      <td>1939.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Batman</td>
      <td>good</td>
      <td>male</td>
      <td>DC</td>
      <td>1934.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joker</td>
      <td>bad</td>
      <td>male</td>
      <td>DC</td>
      <td>1934.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Catwoman</td>
      <td>bad</td>
      <td>female</td>
      <td>DC</td>
      <td>1934.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hellboy</td>
      <td>good</td>
      <td>male</td>
      <td>Dark Horse Comics</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# SQL:
# SELECT *
# FROM superheroes FULL OUTER JOIN publishers
# ON superheroes.publisher = publishers.publisher;
pd.merge(superheroes, publishers, on = 'publisher', how = 'outer')

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>alignment</th>
      <th>gender</th>
      <th>publisher</th>
      <th>yr_founded</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Magneto</td>
      <td>bad</td>
      <td>male</td>
      <td>Marvel</td>
      <td>1939.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Storm</td>
      <td>good</td>
      <td>female</td>
      <td>Marvel</td>
      <td>1939.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mystique</td>
      <td>bad</td>
      <td>female</td>
      <td>Marvel</td>
      <td>1939.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Batman</td>
      <td>good</td>
      <td>male</td>
      <td>DC</td>
      <td>1934.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joker</td>
      <td>bad</td>
      <td>male</td>
      <td>DC</td>
      <td>1934.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Catwoman</td>
      <td>bad</td>
      <td>female</td>
      <td>DC</td>
      <td>1934.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hellboy</td>
      <td>good</td>
      <td>male</td>
      <td>Dark Horse Comics</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Image</td>
      <td>1992.0</td>
    </tr>
  </tbody>
</table>
</div>



<div id="container" style="position:relative;">
<div style="position:relative; float:right"><img style="height:25px""width: 50px" src ="https://drive.google.com/uc?export=view&id=14VoXUJftgptWtdNhtNYVm6cjVmEWpki1" />
</div>
</div>
