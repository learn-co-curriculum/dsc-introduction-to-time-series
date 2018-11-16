
# Introduction to Time Series

## Introduction
From stock prices to climate data, time series data is found in a wide variety of domains, and being able to effectively work with such data is an increasingly important skill for data scientists. 

In this lecture, you will be introduced to some common techniques used to import/load, clean and manipulate time series data. Additionally, you'll learn how you can effectively visualize time series data in Python.

## Objectives

You will be able to:

* Load time-series data using Pandas and perform time series indexing
* Perform index based slicing to create subsets of a time-series
* Change the granularity of a time series 
* Perform basic data cleasing operations on a time series
* Getting time series data ready for further analysis


## Loading Time Series Data

To get a sense of how to manipulate time series data, we'll walk through an example. The dataset we'll load contains daily minimum temperatures in Melbourne, Australia, from 1981-1986. The data is stored in a `.csv`-file, so our usual `pd.read_csv` can be used.


```python
import pandas as pd
temp_data = pd.read_csv("min_temp.csv")
temp_data.head(15)
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
      <th>Date</th>
      <th>Daily_min</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1/1/81</td>
      <td>20.7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2/1/81</td>
      <td>17.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3/1/81</td>
      <td>18.8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4/1/81</td>
      <td>14.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5/1/81</td>
      <td>15.8</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6/1/81</td>
      <td>15.8</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7/1/81</td>
      <td>15.8</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8/1/81</td>
      <td>17.4</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9/1/81</td>
      <td>21.8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10/1/81</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11/1/81</td>
      <td>16.2</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12/1/81</td>
      <td>13.3</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13/1/81</td>
      <td>16.7</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14/1/81</td>
      <td>21.5</td>
    </tr>
    <tr>
      <th>14</th>
      <td>15/1/81</td>
      <td>25.0</td>
    </tr>
  </tbody>
</table>
</div>



Now, let's look at the information of our data set. 


```python
temp_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3650 entries, 0 to 3649
    Data columns (total 2 columns):
    Date         3650 non-null object
    Daily_min    3650 non-null float64
    dtypes: float64(1), object(1)
    memory usage: 57.1+ KB


While working with time series data in Python, it's important to always ensure that dates are used as index values and are understood my Python as a true "date" object. This can be done either by using Pandas' `Timestamp` or base Python’s `Datetime` and is interchangeable in most cases. It’s the type used for the entries that make up a `DatetimeIndex`, and other timeseries oriented data structures in pandas. Further details on Timestamp can be found [here](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Timestamp.html).

We need to do two things now:
1. Make sure that we change the dates in our data sets fron "non-null object" to "non-null datetime". To make sure Python understands the date correctly, a `format` argument can be passed [as specified in the documentation](https://pandas.pydata.org/pandas-docs/stable/timeseries.html#providing-a-format-argument).
2. Make sure that the date becomes the index.



```python
# 1. convert Date to a datetime column
temp_data.Date = pd.to_datetime(temp_data.Date, format='%d/%m/%y')
```


```python
temp_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3650 entries, 0 to 3649
    Data columns (total 2 columns):
    Date         3650 non-null datetime64[ns]
    Daily_min    3650 non-null float64
    dtypes: datetime64[ns](1), float64(1)
    memory usage: 57.1 KB



```python
# 2. Make Date the index 
temp_data.set_index('Date', inplace = True)
```


```python
temp_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    DatetimeIndex: 3650 entries, 1981-01-01 to 1990-12-31
    Data columns (total 1 columns):
    Daily_min    3650 non-null float64
    dtypes: float64(1)
    memory usage: 57.0 KB



```python
temp_data.head(15)
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
      <th>Daily_min</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1981-01-01</th>
      <td>20.7</td>
    </tr>
    <tr>
      <th>1981-01-02</th>
      <td>17.9</td>
    </tr>
    <tr>
      <th>1981-01-03</th>
      <td>18.8</td>
    </tr>
    <tr>
      <th>1981-01-04</th>
      <td>14.6</td>
    </tr>
    <tr>
      <th>1981-01-05</th>
      <td>15.8</td>
    </tr>
    <tr>
      <th>1981-01-06</th>
      <td>15.8</td>
    </tr>
    <tr>
      <th>1981-01-07</th>
      <td>15.8</td>
    </tr>
    <tr>
      <th>1981-01-08</th>
      <td>17.4</td>
    </tr>
    <tr>
      <th>1981-01-09</th>
      <td>21.8</td>
    </tr>
    <tr>
      <th>1981-01-10</th>
      <td>20.0</td>
    </tr>
    <tr>
      <th>1981-01-11</th>
      <td>16.2</td>
    </tr>
    <tr>
      <th>1981-01-12</th>
      <td>13.3</td>
    </tr>
    <tr>
      <th>1981-01-13</th>
      <td>16.7</td>
    </tr>
    <tr>
      <th>1981-01-14</th>
      <td>21.5</td>
    </tr>
    <tr>
      <th>1981-01-15</th>
      <td>25.0</td>
    </tr>
  </tbody>
</table>
</div>



## Resampling

### Downsampling

Note how the Date is now the index! Having the date as the index has several advantages, among others, easy visualization with dates on the x-axis, and the functionality to *resample* the data. 

Pandas has a simple, powerful, and efficient functionality for performing resampling operations when converting the frequency conversion (e.g., converting monthly data into yearly data). This is very common in financial applications.


```python
temp_monthly= temp_data.resample('MS')
month_mean = temp_monthly.mean()
```


```python
month_mean.head(15)
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
      <th>Daily_min</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1981-01-01</th>
      <td>17.712903</td>
    </tr>
    <tr>
      <th>1981-02-01</th>
      <td>17.678571</td>
    </tr>
    <tr>
      <th>1981-03-01</th>
      <td>13.500000</td>
    </tr>
    <tr>
      <th>1981-04-01</th>
      <td>12.356667</td>
    </tr>
    <tr>
      <th>1981-05-01</th>
      <td>9.490323</td>
    </tr>
    <tr>
      <th>1981-06-01</th>
      <td>7.306667</td>
    </tr>
    <tr>
      <th>1981-07-01</th>
      <td>7.577419</td>
    </tr>
    <tr>
      <th>1981-08-01</th>
      <td>7.238710</td>
    </tr>
    <tr>
      <th>1981-09-01</th>
      <td>10.143333</td>
    </tr>
    <tr>
      <th>1981-10-01</th>
      <td>10.087097</td>
    </tr>
    <tr>
      <th>1981-11-01</th>
      <td>11.890000</td>
    </tr>
    <tr>
      <th>1981-12-01</th>
      <td>13.680645</td>
    </tr>
    <tr>
      <th>1982-01-01</th>
      <td>16.567742</td>
    </tr>
    <tr>
      <th>1982-02-01</th>
      <td>15.921429</td>
    </tr>
    <tr>
      <th>1982-03-01</th>
      <td>14.935484</td>
    </tr>
  </tbody>
</table>
</div>



### Upsampling

In some cases, it is useful to create upsampled time series as well, especially if you're trying to merge several time series together with different frequencies. You can do this by using "resample" to a time which is more frequent than the timestamp from the original time series. 


```python
temp_bidaily= temp_data.resample('12H').asfreq()
temp_bidaily.head()
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
      <th>Daily_min</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1981-01-01 00:00:00</th>
      <td>20.7</td>
    </tr>
    <tr>
      <th>1981-01-01 12:00:00</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1981-01-02 00:00:00</th>
      <td>17.9</td>
    </tr>
    <tr>
      <th>1981-01-02 12:00:00</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1981-01-03 00:00:00</th>
      <td>18.8</td>
    </tr>
  </tbody>
</table>
</div>




```python
temp_bidaily_fill= temp_data.resample('12H').ffill()
temp_bidaily_fill.head()
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
      <th>Daily_min</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1981-01-01 00:00:00</th>
      <td>20.7</td>
    </tr>
    <tr>
      <th>1981-01-01 12:00:00</th>
      <td>20.7</td>
    </tr>
    <tr>
      <th>1981-01-02 00:00:00</th>
      <td>17.9</td>
    </tr>
    <tr>
      <th>1981-01-02 12:00:00</th>
      <td>17.9</td>
    </tr>
    <tr>
      <th>1981-01-03 00:00:00</th>
      <td>18.8</td>
    </tr>
  </tbody>
</table>
</div>



## Selecting and slicing time series data

Pandas carries the ability to handle date stamp indices allowing quick and handy way of slicing data. For example, we can slice our dataset to only retrieve data points that come after the year 1985.


```python
temp_1985_onwards = temp_data['1985':]
print(temp_1985_onwards.head())
print(temp_1985_onwards.tail())
```

                Daily_min
    Date                 
    1985-01-01       13.3
    1985-01-02       15.2
    1985-01-03       13.1
    1985-01-04       12.7
    1985-01-05       14.6
                Daily_min
    Date                 
    1990-12-27       14.0
    1990-12-28       13.6
    1990-12-29       13.5
    1990-12-30       15.7
    1990-12-31       13.0


## Missing Data

It's pretty common for a time series dataset to have missing values as real world data tends to be messy and imperfect, just like any othe type of data. Simplest way to detect missing values is either plotting the data and identifying disjoint areas of timeseries, or by using `DataFrame.isnull()` function to get list of all missing values. This function can be used with `sum()` to get a total count of all missing values. 


```python
temp_data.isnull().sum()
```




    Daily_min    0
    dtype: int64



In this case, there is no missing data. When missingness is present, however, they can be handled in a multitude of ways. 
* Drop the data elements with missing values (this may result as low accuracy and loss of valuable information)
* Fill in the missing values under a defined criteria 
* Use advanced machine learning methods to predict the missing values 

in general, the`DataFrame.fillna()` function can be used along with, methods like `bfill()` of `ffill()` as an argument/criterion for filling in Null values . `bfill()` (backward filling) looks for the next valid entry in the time series and fills the gaps with this value. Similarly, `ffill()` can be used to copy forward the previous valid entry of the time series.

## Visualizing time series data

## Additional reading

An overview of Pandas time series functionality can be found [here](https://pandas.pydata.org/pandas-docs/stable/timeseries.html).

Some more manipulation tricks can be found [here](https://towardsdatascience.com/basic-time-series-manipulation-with-pandas-4432afee64ea).

## Summary

In this introductory lesson, we learnt how to create a time-series object in Python using Pandas. We learnt to fulfil all the requirements for a dataset to be classified as a time-series by ensuring timestamp values as data index. Basic data handling techniques for getting time-series data ready for further analysis were introduced.
