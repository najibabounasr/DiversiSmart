# DiversiSmart 

## Executive Summary
DiversiSmart is a Python program that conducts technical analysis on investors' portfolios using Google Sheets and Google APIs. It assesses the diversity of the portfolio and provides a score to help users understand and improve their investment strategies. The program generates graphs and tables to visualize portfolio performance and volatility.

## Features
1. Assessment of portfolio diversity.
2. Calculation of portfolio scores.
3. Graphical representation of cumulative returns.
4. Analysis of portfolio volatility.
5. Visualization of daily returns and rolling standard deviation.

## Purpose
The purpose of DiversiSmart is to help investors understand the diversity of their portfolios and identify areas for improvement. By providing comprehensive analysis and visualizations, the program enables users to make informed decisions about their investments.

## Technologies and Modules Used
DiversiSmart utilizes the following technologies and Python modules:
- Google Sheets API: To interact with Google Sheets and access portfolio data.
- Pandas: For data manipulation and analysis.
- Matplotlib: To create visualizations, such as line and bar plots.
- NumPy: For numerical computations.
- hvPlot: For interactive and customized plotting.

## Usage
1. Set up the necessary credentials for the Google Sheets API.
2. Define the portfolio spreadsheet and worksheet to be analyzed.
3. Run the program to calculate portfolio scores and generate visualizations.
4. Review the output to gain insights into portfolio diversity and performance.

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License
MIT


### DiversiSmart - Portfolio Assesment

this notebook is used to asses the portfolio of a user and to give him a score based on the diversity of his portfolio. It will provide a number of graphs and tables to help the user understand his portfolio and how to improve it.


```python
# Import the required libraries and dependencies
import gspread

import pandas as pd

import matplotlib.pyplot as plt

from pathlib import Path

%matplotlib inline

import numpy as np

import hvplot.pandas as hv

```


```python
# Use creds to create a client to interact with the Google Drive API
scope = ['https://spreadsheets.google.com/feeds','https://www.googleapis.com/auth/drive']


gc = gspread.oauth()


```


```python

```


```python
# Open the spreadsheet
spreadsheet = gc.open("Stock Portfolio - Fawaz")

# Specify the worksheet to be used
worksheet = spreadsheet.worksheet("AAPL")

# Get all values from the worksheet
data = worksheet.get_all_values()[1:]

```


```python
print(data)

data_2 = worksheet.col_values(2)[2:]

print(data_2)

```

    [['Date', 'Close'], ['2/1/2023 16:00:00', '145.43'], ['2/2/2023 16:00:00', '150.82'], ['2/3/2023 16:00:00', '154.5'], ['2/6/2023 16:00:00', '151.73'], ['2/7/2023 16:00:00', '154.65'], ['2/8/2023 16:00:00', '151.92'], ['2/9/2023 16:00:00', '150.87'], ['2/10/2023 16:00:00', '151.01'], ['2/13/2023 16:00:00', '153.85'], ['2/14/2023 16:00:00', '153.2'], ['2/15/2023 16:00:00', '155.33'], ['2/16/2023 16:00:00', '153.71'], ['2/17/2023 16:00:00', '152.55'], ['2/21/2023 16:00:00', '148.48'], ['2/22/2023 16:00:00', '148.91'], ['2/23/2023 16:00:00', '149.4'], ['2/24/2023 16:00:00', '146.71'], ['2/27/2023 16:00:00', '147.92'], ['2/28/2023 16:00:00', '147.41'], ['3/1/2023 16:00:00', '145.31'], ['3/2/2023 16:00:00', '145.91'], ['3/3/2023 16:00:00', '151.03'], ['3/6/2023 16:00:00', '153.83'], ['3/7/2023 16:00:00', '151.6'], ['3/8/2023 16:00:00', '152.87'], ['3/9/2023 16:00:00', '150.59'], ['3/10/2023 16:00:00', '148.5'], ['3/13/2023 16:00:00', '150.47'], ['3/14/2023 16:00:00', '152.59'], ['3/15/2023 16:00:00', '152.99'], ['3/16/2023 16:00:00', '155.85'], ['3/17/2023 16:00:00', '155'], ['3/20/2023 16:00:00', '157.4'], ['3/21/2023 16:00:00', '159.28'], ['3/22/2023 16:00:00', '157.83'], ['3/23/2023 16:00:00', '158.93'], ['3/24/2023 16:00:00', '160.25'], ['3/27/2023 16:00:00', '158.28'], ['3/28/2023 16:00:00', '157.65'], ['3/29/2023 16:00:00', '160.77'], ['3/30/2023 16:00:00', '162.36'], ['3/31/2023 16:00:00', '164.9'], ['4/3/2023 16:00:00', '166.17'], ['4/4/2023 16:00:00', '165.63'], ['4/5/2023 16:00:00', '163.76'], ['4/6/2023 16:00:00', '164.66'], ['4/10/2023 16:00:00', '162.03'], ['4/11/2023 16:00:00', '160.8'], ['4/12/2023 16:00:00', '160.1'], ['4/13/2023 16:00:00', '165.56'], ['4/14/2023 16:00:00', '165.21'], ['4/17/2023 16:00:00', '165.23'], ['4/18/2023 16:00:00', '166.47'], ['4/19/2023 16:00:00', '167.63'], ['4/20/2023 16:00:00', '166.65'], ['4/21/2023 16:00:00', '165.02'], ['4/24/2023 16:00:00', '165.33'], ['4/25/2023 16:00:00', '163.77'], ['4/26/2023 16:00:00', '163.76'], ['4/27/2023 16:00:00', '168.41'], ['4/28/2023 16:00:00', '169.68'], ['5/1/2023 16:00:00', '169.59'], ['5/2/2023 16:00:00', '168.54'], ['5/3/2023 16:00:00', '167.45'], ['5/4/2023 16:00:00', '165.79'], ['5/5/2023 16:00:00', '173.57'], ['5/8/2023 16:00:00', '173.5'], ['5/9/2023 16:00:00', '171.77'], ['5/10/2023 16:00:00', '173.56'], ['5/11/2023 16:00:00', '173.75'], ['5/12/2023 16:00:00', '172.57'], ['5/15/2023 16:00:00', '172.07'], ['5/16/2023 16:00:00', '172.07'], ['5/17/2023 16:00:00', '172.69'], ['5/18/2023 16:00:00', '175.05'], ['5/19/2023 16:00:00', '175.16'], ['5/22/2023 16:00:00', '174.2'], ['5/23/2023 16:00:00', '171.56'], ['5/24/2023 16:00:00', '171.84'], ['5/25/2023 16:00:00', '172.99'], ['5/26/2023 16:00:00', '175.43'], ['5/30/2023 16:00:00', '177.3'], ['5/31/2023 16:00:00', '177.25'], ['6/1/2023 16:00:00', '180.09'], ['6/2/2023 16:00:00', '180.95'], ['6/5/2023 16:00:00', '179.58'], ['6/6/2023 16:00:00', '179.21'], ['6/7/2023 16:00:00', '177.82'], ['6/8/2023 16:00:00', '180.57'], ['6/9/2023 16:00:00', '180.96'], ['6/12/2023 16:00:00', '183.79'], ['6/13/2023 16:00:00', '183.31'], ['6/14/2023 16:00:00', '183.95'], ['6/15/2023 16:00:00', '186.01'], ['6/16/2023 16:00:00', '184.92'], ['6/20/2023 16:00:00', '185.01'], ['6/21/2023 16:00:00', '183.96'], ['6/22/2023 16:00:00', '187'], ['6/23/2023 16:00:00', '186.68'], ['6/26/2023 16:00:00', '185.27'], ['6/27/2023 16:00:00', '188.06'], ['6/28/2023 16:00:00', '189.25'], ['6/29/2023 16:00:00', '189.59'], ['6/30/2023 16:00:00', '193.97'], ['7/3/2023 13:05:00', '192.46'], ['7/5/2023 16:00:00', '191.33'], ['7/6/2023 16:00:00', '191.81'], ['7/7/2023 16:00:00', '190.68'], ['7/10/2023 16:00:00', '188.61'], ['7/11/2023 16:00:00', '188.08'], ['7/12/2023 16:00:00', '189.77'], ['7/13/2023 16:00:00', '190.54'], ['7/14/2023 16:00:00', '190.69']]
    ['145.43', '150.82', '154.5', '151.73', '154.65', '151.92', '150.87', '151.01', '153.85', '153.2', '155.33', '153.71', '152.55', '148.48', '148.91', '149.4', '146.71', '147.92', '147.41', '145.31', '145.91', '151.03', '153.83', '151.6', '152.87', '150.59', '148.5', '150.47', '152.59', '152.99', '155.85', '155', '157.4', '159.28', '157.83', '158.93', '160.25', '158.28', '157.65', '160.77', '162.36', '164.9', '166.17', '165.63', '163.76', '164.66', '162.03', '160.8', '160.1', '165.56', '165.21', '165.23', '166.47', '167.63', '166.65', '165.02', '165.33', '163.77', '163.76', '168.41', '169.68', '169.59', '168.54', '167.45', '165.79', '173.57', '173.5', '171.77', '173.56', '173.75', '172.57', '172.07', '172.07', '172.69', '175.05', '175.16', '174.2', '171.56', '171.84', '172.99', '175.43', '177.3', '177.25', '180.09', '180.95', '179.58', '179.21', '177.82', '180.57', '180.96', '183.79', '183.31', '183.95', '186.01', '184.92', '185.01', '183.96', '187', '186.68', '185.27', '188.06', '189.25', '189.59', '193.97', '192.46', '191.33', '191.81', '190.68', '188.61', '188.08', '189.77', '190.54', '190.69']



```python

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
      <th>2/9/2023 16:00:00</th>
      <th>150.87</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2/9/2023 16:00:00</td>
      <td>150.87</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2/10/2023 16:00:00</td>
      <td>151.01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2/13/2023 16:00:00</td>
      <td>153.85</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2/14/2023 16:00:00</td>
      <td>153.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2/15/2023 16:00:00</td>
      <td>155.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# save the header row
header = data[0]

# save the 'Date' column
dates = [x[0] for x in data]

# remove the header row from the data
data = data[1:]
print(header)
# Iterate over the column values
for value in data_2:
    # Split the value using the comma as the delimiter
    split_values = value.split(',')

    # Print the split values
    print(split_values)

# Convert the data to a pandas DataFrame
df = pd.DataFrame(data[1:], columns=data[0])

df.head()


```

    ['2/8/2023 16:00:00', '151.92']
    ['145.43']
    ['150.82']
    ['154.5']
    ['151.73']
    ['154.65']
    ['151.92']
    ['150.87']
    ['151.01']
    ['153.85']
    ['153.2']
    ['155.33']
    ['153.71']
    ['152.55']
    ['148.48']
    ['148.91']
    ['149.4']
    ['146.71']
    ['147.92']
    ['147.41']
    ['145.31']
    ['145.91']
    ['151.03']
    ['153.83']
    ['151.6']
    ['152.87']
    ['150.59']
    ['148.5']
    ['150.47']
    ['152.59']
    ['152.99']
    ['155.85']
    ['155']
    ['157.4']
    ['159.28']
    ['157.83']
    ['158.93']
    ['160.25']
    ['158.28']
    ['157.65']
    ['160.77']
    ['162.36']
    ['164.9']
    ['166.17']
    ['165.63']
    ['163.76']
    ['164.66']
    ['162.03']
    ['160.8']
    ['160.1']
    ['165.56']
    ['165.21']
    ['165.23']
    ['166.47']
    ['167.63']
    ['166.65']
    ['165.02']
    ['165.33']
    ['163.77']
    ['163.76']
    ['168.41']
    ['169.68']
    ['169.59']
    ['168.54']
    ['167.45']
    ['165.79']
    ['173.57']
    ['173.5']
    ['171.77']
    ['173.56']
    ['173.75']
    ['172.57']
    ['172.07']
    ['172.07']
    ['172.69']
    ['175.05']
    ['175.16']
    ['174.2']
    ['171.56']
    ['171.84']
    ['172.99']
    ['175.43']
    ['177.3']
    ['177.25']
    ['180.09']
    ['180.95']
    ['179.58']
    ['179.21']
    ['177.82']
    ['180.57']
    ['180.96']
    ['183.79']
    ['183.31']
    ['183.95']
    ['186.01']
    ['184.92']
    ['185.01']
    ['183.96']
    ['187']
    ['186.68']
    ['185.27']
    ['188.06']
    ['189.25']
    ['189.59']
    ['193.97']
    ['192.46']
    ['191.33']
    ['191.81']
    ['190.68']
    ['188.61']
    ['188.08']
    ['189.77']
    ['190.54']
    ['190.69']





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
      <th>2/9/2023 16:00:00</th>
      <th>150.87</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2/10/2023 16:00:00</td>
      <td>151.01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2/13/2023 16:00:00</td>
      <td>153.85</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2/14/2023 16:00:00</td>
      <td>153.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2/15/2023 16:00:00</td>
      <td>155.33</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2/16/2023 16:00:00</td>
      <td>153.71</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create the larger dataframe, with all the stocks in the portfolio
worksheet = spreadsheet.worksheet("Portfolio")

# Get the ticker values from the worksheet
symbol_column = worksheet.col_values(2)[2:]

dfs = []

print(symbol_column)

```

    ['AAPL', 'MSFT', 'TSLA', 'WM', 'RSG', 'CNR', 'ECL', 'WCN', 'NVDA']



```python
for symbol in symbol_column:
    worksheet = spreadsheet.worksheet(symbol)
    
    # Fetch the closing data for the ticker
    data = worksheet.get_all_values()  # Exclude the header row
    
    # Convert the data to a DataFrame
    df = pd.DataFrame(data)
    # df.set_index("Date", inplace=True)
    # df.index = pd.to_datetime(df.index)  # Convert index to datetime
    
    dfs.append(df)
```


```python
print(dfs)
```

    [                      0       1
    0            Apple Inc.        
    1                  Date   Close
    2     2/1/2023 16:00:00  145.43
    3     2/2/2023 16:00:00  150.82
    4     2/3/2023 16:00:00   154.5
    ..                  ...     ...
    110  7/10/2023 16:00:00  188.61
    111  7/11/2023 16:00:00  188.08
    112  7/12/2023 16:00:00  189.77
    113  7/13/2023 16:00:00  190.54
    114  7/14/2023 16:00:00  190.69
    
    [115 rows x 2 columns],                          0       1
    0    Microsoft Corporation        
    1                     Date   Close
    2        2/1/2023 16:00:00  252.75
    3        2/2/2023 16:00:00   264.6
    4        2/3/2023 16:00:00  258.35
    ..                     ...     ...
    110     7/10/2023 16:00:00  331.83
    111     7/11/2023 16:00:00  332.47
    112     7/12/2023 16:00:00   337.2
    113     7/13/2023 16:00:00  342.66
    114     7/14/2023 16:00:00  345.24
    
    [115 rows x 2 columns],                       0       1
    0           Tesla, Inc.        
    1                  Date   Close
    2     2/1/2023 16:00:00  181.41
    3     2/2/2023 16:00:00  188.27
    4     2/3/2023 16:00:00  189.98
    ..                  ...     ...
    110  7/10/2023 16:00:00  269.61
    111  7/11/2023 16:00:00  269.79
    112  7/12/2023 16:00:00  271.99
    113  7/13/2023 16:00:00   277.9
    114  7/14/2023 16:00:00  281.38
    
    [115 rows x 2 columns],                           0       1
    0    Waste Management, Inc.        
    1                      Date   Close
    2         2/1/2023 16:00:00  154.41
    3         2/2/2023 16:00:00  150.34
    4         2/3/2023 16:00:00  151.06
    ..                      ...     ...
    110      7/10/2023 16:00:00  171.13
    111      7/11/2023 16:00:00  170.22
    112      7/12/2023 16:00:00  169.18
    113      7/13/2023 16:00:00  168.34
    114      7/14/2023 16:00:00  168.56
    
    [115 rows x 2 columns],                            0       1
    0    Republic Services, Inc.        
    1                       Date   Close
    2          2/1/2023 16:00:00  124.79
    3          2/2/2023 16:00:00  122.54
    4          2/3/2023 16:00:00  123.08
    ..                       ...     ...
    110       7/10/2023 16:00:00  150.89
    111       7/11/2023 16:00:00  149.85
    112       7/12/2023 16:00:00  149.18
    113       7/13/2023 16:00:00   149.6
    114       7/14/2023 16:00:00  149.98
    
    [115 rows x 2 columns],                              0      1
    0    Canadian National Railway       
    1                         Date  Close
    2            2/1/2023 16:30:00   17.5
    3            2/2/2023 16:30:00  17.25
    4            2/3/2023 16:30:00  17.25
    ..                         ...    ...
    109         7/10/2023 16:30:00  23.25
    110         7/11/2023 16:30:00  23.25
    111         7/12/2023 16:30:00   21.5
    112         7/13/2023 16:30:00  22.25
    113         7/14/2023 16:30:00  22.75
    
    [114 rows x 2 columns],                       0       1
    0            Ecolab Inc        
    1                  Date   Close
    2     2/1/2023 16:00:00  155.73
    3     2/2/2023 16:00:00  159.32
    4     2/3/2023 16:00:00  153.29
    ..                  ...     ...
    110  7/10/2023 16:00:00  183.69
    111  7/11/2023 16:00:00  184.66
    112  7/12/2023 16:00:00  186.78
    113  7/13/2023 16:00:00  186.33
    114  7/14/2023 16:00:00  188.46
    
    [115 rows x 2 columns],                            0       1
    0    Waste Connections, Inc.        
    1                       Date   Close
    2          2/1/2023 16:00:00  133.02
    3          2/2/2023 16:00:00  131.97
    4          2/3/2023 16:00:00  132.23
    ..                       ...     ...
    110       7/10/2023 16:00:00  138.69
    111       7/11/2023 16:00:00  138.63
    112       7/12/2023 16:00:00  139.18
    113       7/13/2023 16:00:00  141.12
    114       7/14/2023 16:00:00  141.42
    
    [115 rows x 2 columns],                       0       1
    0           NVIDIA Corp        
    1                  Date   Close
    2     2/1/2023 16:00:00  145.43
    3     2/2/2023 16:00:00  150.82
    4     2/3/2023 16:00:00   154.5
    ..                  ...     ...
    110  7/10/2023 16:00:00  188.61
    111  7/11/2023 16:00:00  188.08
    112  7/12/2023 16:00:00  189.77
    113  7/13/2023 16:00:00  190.54
    114  7/14/2023 16:00:00  190.69
    
    [115 rows x 2 columns]]



```python
# Concatenate all the dataframes, make sure the 
df = pd.concat(dfs, axis=1, join="inner")
df.head()

# Set the first two rows as the header
header = df.iloc[0]
df = df[1:]
df.columns = header
df.head()

# Drop the header column row from the dataframe
df_2 = df.drop(df.columns[0], axis=1)

display(df_2.head())
# save the values of the second column
dates = df.iloc[:, 2]
display(dates.head())
# move the column headers to the right one column
df = df.shift(-1, axis=1)

# Set the index_col parameter to the second column
date_col = dates
df.set_index(date_col, inplace=True)
# delete all columns that contain a value of str 'Date'
df = df.loc[:, ~df.columns.str.contains('Date')]

display(df.head())

# delete duplicate columns
df = df.loc[:,~df.columns.duplicated()]

df.index.name = None


# drop the third column
df = df.drop(df.columns[1], axis=1)

# name the index column as 'Date'
df.index.name = str('Date')

df.head()

df.head()

# save the df dataframe as portfolio
portfolio_df = df.to_csv('portfolio.csv')


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
      <th></th>
      <th>Microsoft Corporation</th>
      <th></th>
      <th>Tesla, Inc.</th>
      <th></th>
      <th>Waste Management, Inc.</th>
      <th></th>
      <th>Republic Services, Inc.</th>
      <th></th>
      <th>Canadian National Railway</th>
      <th></th>
      <th>Ecolab Inc</th>
      <th></th>
      <th>Waste Connections, Inc.</th>
      <th></th>
      <th>NVIDIA Corp</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
    </tr>
    <tr>
      <th>2</th>
      <td>145.43</td>
      <td>2/1/2023 16:00:00</td>
      <td>252.75</td>
      <td>2/1/2023 16:00:00</td>
      <td>181.41</td>
      <td>2/1/2023 16:00:00</td>
      <td>154.41</td>
      <td>2/1/2023 16:00:00</td>
      <td>124.79</td>
      <td>2/1/2023 16:30:00</td>
      <td>17.5</td>
      <td>2/1/2023 16:00:00</td>
      <td>155.73</td>
      <td>2/1/2023 16:00:00</td>
      <td>133.02</td>
      <td>2/1/2023 16:00:00</td>
      <td>145.43</td>
    </tr>
    <tr>
      <th>3</th>
      <td>150.82</td>
      <td>2/2/2023 16:00:00</td>
      <td>264.6</td>
      <td>2/2/2023 16:00:00</td>
      <td>188.27</td>
      <td>2/2/2023 16:00:00</td>
      <td>150.34</td>
      <td>2/2/2023 16:00:00</td>
      <td>122.54</td>
      <td>2/2/2023 16:30:00</td>
      <td>17.25</td>
      <td>2/2/2023 16:00:00</td>
      <td>159.32</td>
      <td>2/2/2023 16:00:00</td>
      <td>131.97</td>
      <td>2/2/2023 16:00:00</td>
      <td>150.82</td>
    </tr>
    <tr>
      <th>4</th>
      <td>154.5</td>
      <td>2/3/2023 16:00:00</td>
      <td>258.35</td>
      <td>2/3/2023 16:00:00</td>
      <td>189.98</td>
      <td>2/3/2023 16:00:00</td>
      <td>151.06</td>
      <td>2/3/2023 16:00:00</td>
      <td>123.08</td>
      <td>2/3/2023 16:30:00</td>
      <td>17.25</td>
      <td>2/3/2023 16:00:00</td>
      <td>153.29</td>
      <td>2/3/2023 16:00:00</td>
      <td>132.23</td>
      <td>2/3/2023 16:00:00</td>
      <td>154.5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>151.73</td>
      <td>2/6/2023 16:00:00</td>
      <td>256.77</td>
      <td>2/6/2023 16:00:00</td>
      <td>194.76</td>
      <td>2/6/2023 16:00:00</td>
      <td>151.82</td>
      <td>2/6/2023 16:00:00</td>
      <td>123.89</td>
      <td>2/6/2023 16:30:00</td>
      <td>17.25</td>
      <td>2/6/2023 16:00:00</td>
      <td>151.69</td>
      <td>2/6/2023 16:00:00</td>
      <td>132.93</td>
      <td>2/6/2023 16:00:00</td>
      <td>151.73</td>
    </tr>
  </tbody>
</table>
</div>



    1                 Date
    2    2/1/2023 16:00:00
    3    2/2/2023 16:00:00
    4    2/3/2023 16:00:00
    5    2/6/2023 16:00:00
    Name: Microsoft Corporation, dtype: object



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
      <th>Apple Inc.</th>
      <th></th>
      <th>Microsoft Corporation</th>
      <th></th>
      <th>Tesla, Inc.</th>
      <th></th>
      <th>Waste Management, Inc.</th>
      <th></th>
      <th>Republic Services, Inc.</th>
      <th></th>
      <th>Canadian National Railway</th>
      <th></th>
      <th>Ecolab Inc</th>
      <th></th>
      <th>Waste Connections, Inc.</th>
      <th></th>
      <th>NVIDIA Corp</th>
      <th></th>
    </tr>
    <tr>
      <th>Microsoft Corporation</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Date</th>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>Date</td>
      <td>Close</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2/1/2023 16:00:00</th>
      <td>145.43</td>
      <td>2/1/2023 16:00:00</td>
      <td>252.75</td>
      <td>2/1/2023 16:00:00</td>
      <td>181.41</td>
      <td>2/1/2023 16:00:00</td>
      <td>154.41</td>
      <td>2/1/2023 16:00:00</td>
      <td>124.79</td>
      <td>2/1/2023 16:30:00</td>
      <td>17.5</td>
      <td>2/1/2023 16:00:00</td>
      <td>155.73</td>
      <td>2/1/2023 16:00:00</td>
      <td>133.02</td>
      <td>2/1/2023 16:00:00</td>
      <td>145.43</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2/2/2023 16:00:00</th>
      <td>150.82</td>
      <td>2/2/2023 16:00:00</td>
      <td>264.6</td>
      <td>2/2/2023 16:00:00</td>
      <td>188.27</td>
      <td>2/2/2023 16:00:00</td>
      <td>150.34</td>
      <td>2/2/2023 16:00:00</td>
      <td>122.54</td>
      <td>2/2/2023 16:30:00</td>
      <td>17.25</td>
      <td>2/2/2023 16:00:00</td>
      <td>159.32</td>
      <td>2/2/2023 16:00:00</td>
      <td>131.97</td>
      <td>2/2/2023 16:00:00</td>
      <td>150.82</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2/3/2023 16:00:00</th>
      <td>154.5</td>
      <td>2/3/2023 16:00:00</td>
      <td>258.35</td>
      <td>2/3/2023 16:00:00</td>
      <td>189.98</td>
      <td>2/3/2023 16:00:00</td>
      <td>151.06</td>
      <td>2/3/2023 16:00:00</td>
      <td>123.08</td>
      <td>2/3/2023 16:30:00</td>
      <td>17.25</td>
      <td>2/3/2023 16:00:00</td>
      <td>153.29</td>
      <td>2/3/2023 16:00:00</td>
      <td>132.23</td>
      <td>2/3/2023 16:00:00</td>
      <td>154.5</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2/6/2023 16:00:00</th>
      <td>151.73</td>
      <td>2/6/2023 16:00:00</td>
      <td>256.77</td>
      <td>2/6/2023 16:00:00</td>
      <td>194.76</td>
      <td>2/6/2023 16:00:00</td>
      <td>151.82</td>
      <td>2/6/2023 16:00:00</td>
      <td>123.89</td>
      <td>2/6/2023 16:30:00</td>
      <td>17.25</td>
      <td>2/6/2023 16:00:00</td>
      <td>151.69</td>
      <td>2/6/2023 16:00:00</td>
      <td>132.93</td>
      <td>2/6/2023 16:00:00</td>
      <td>151.73</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>



```python
# Read the portfolio csv file
portfolio_df = pd.read_csv('portfolio.csv',
                           index_col='Date',
                           parse_dates=True,
                           infer_datetime_format=True)


# Drop the first row
portfolio_df = portfolio_df.drop(portfolio_df.index[0])

portfolio_df.head()

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
      <th>Apple Inc.</th>
      <th>Microsoft Corporation</th>
      <th>Tesla, Inc.</th>
      <th>Waste Management, Inc.</th>
      <th>Republic Services, Inc.</th>
      <th>Canadian National Railway</th>
      <th>Ecolab Inc</th>
      <th>Waste Connections, Inc.</th>
      <th>NVIDIA Corp</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2/1/2023 16:00:00</th>
      <td>145.43</td>
      <td>252.75</td>
      <td>181.41</td>
      <td>154.41</td>
      <td>124.79</td>
      <td>17.5</td>
      <td>155.73</td>
      <td>133.02</td>
      <td>145.43</td>
    </tr>
    <tr>
      <th>2/2/2023 16:00:00</th>
      <td>150.82</td>
      <td>264.6</td>
      <td>188.27</td>
      <td>150.34</td>
      <td>122.54</td>
      <td>17.25</td>
      <td>159.32</td>
      <td>131.97</td>
      <td>150.82</td>
    </tr>
    <tr>
      <th>2/3/2023 16:00:00</th>
      <td>154.5</td>
      <td>258.35</td>
      <td>189.98</td>
      <td>151.06</td>
      <td>123.08</td>
      <td>17.25</td>
      <td>153.29</td>
      <td>132.23</td>
      <td>154.5</td>
    </tr>
    <tr>
      <th>2/6/2023 16:00:00</th>
      <td>151.73</td>
      <td>256.77</td>
      <td>194.76</td>
      <td>151.82</td>
      <td>123.89</td>
      <td>17.25</td>
      <td>151.69</td>
      <td>132.93</td>
      <td>151.73</td>
    </tr>
    <tr>
      <th>2/7/2023 16:00:00</th>
      <td>154.65</td>
      <td>267.56</td>
      <td>196.81</td>
      <td>151.33</td>
      <td>124.29</td>
      <td>17.13</td>
      <td>152.29</td>
      <td>133.37</td>
      <td>154.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Check dtypes
portfolio_df.dtypes

# Convert the dtypes of the dataframe to float
portfolio_df = portfolio_df.astype(float)

headers = portfolio_df.columns
display(headers)
```


    Index(['Apple Inc.', 'Microsoft Corporation', 'Tesla, Inc.',
           'Waste Management, Inc.', 'Republic Services, Inc.',
           'Canadian National Railway', 'Ecolab Inc', 'Waste Connections, Inc.',
           'NVIDIA Corp'],
          dtype='object')



```python
# Prepare for the analysis by converting the dataframe of NAVs and prices to daily returns
# Drop any rows with all missing values
daily_returns_df = portfolio_df.dropna(axis=0, how='all')

daily_returns_df = portfolio_df.pct_change().dropna()

daily_returns_df.head()
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
      <th>Apple Inc.</th>
      <th>Microsoft Corporation</th>
      <th>Tesla, Inc.</th>
      <th>Waste Management, Inc.</th>
      <th>Republic Services, Inc.</th>
      <th>Canadian National Railway</th>
      <th>Ecolab Inc</th>
      <th>Waste Connections, Inc.</th>
      <th>NVIDIA Corp</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2/2/2023 16:00:00</th>
      <td>0.037063</td>
      <td>0.046884</td>
      <td>0.037815</td>
      <td>-0.026358</td>
      <td>-0.018030</td>
      <td>-0.014286</td>
      <td>0.023053</td>
      <td>-0.007894</td>
      <td>0.037063</td>
    </tr>
    <tr>
      <th>2/3/2023 16:00:00</th>
      <td>0.024400</td>
      <td>-0.023621</td>
      <td>0.009083</td>
      <td>0.004789</td>
      <td>0.004407</td>
      <td>0.000000</td>
      <td>-0.037848</td>
      <td>0.001970</td>
      <td>0.024400</td>
    </tr>
    <tr>
      <th>2/6/2023 16:00:00</th>
      <td>-0.017929</td>
      <td>-0.006116</td>
      <td>0.025161</td>
      <td>0.005031</td>
      <td>0.006581</td>
      <td>0.000000</td>
      <td>-0.010438</td>
      <td>0.005294</td>
      <td>-0.017929</td>
    </tr>
    <tr>
      <th>2/7/2023 16:00:00</th>
      <td>0.019245</td>
      <td>0.042022</td>
      <td>0.010526</td>
      <td>-0.003228</td>
      <td>0.003229</td>
      <td>-0.006957</td>
      <td>0.003955</td>
      <td>0.003310</td>
      <td>0.019245</td>
    </tr>
    <tr>
      <th>2/8/2023 16:00:00</th>
      <td>-0.017653</td>
      <td>-0.003102</td>
      <td>0.022763</td>
      <td>-0.003106</td>
      <td>0.001046</td>
      <td>-0.022183</td>
      <td>-0.007026</td>
      <td>0.000750</td>
      <td>-0.017653</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plot the daily returns of the portfolio
ax = daily_returns_df[headers[0]].plot(color='green', figsize=(20,10), label='Apple Inc.', title='Daily Returns of the Portfolio', legend=True, kind='line')
for header in headers[1:]:
    daily_returns_df[header].plot( figsize=(20,10), label=header, legend=True, kind='line', ax=ax)
ax.set_xlabel('Date')
ax.set_ylabel('Daily Returns')
plt.show()



```


    
![png](gsheet_portfolio_assesment_files/gsheet_portfolio_assesment_15_0.png)
    



```python
# Calculate and plot the cumulative returns of the portfolio
cumulative_returns = (1 + daily_returns_df).cumprod() 

cumulative_returns.columns = headers
display(cumulative_returns.head())

ax = cumulative_returns.plot(figsize=(20,10), title='Cumulative Returns of the Portfolio', legend=True, kind='line')
ax.set_xlabel('Date')
ax.set_ylabel('Cumulative Returns')
plt.show()




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
      <th>Apple Inc.</th>
      <th>Microsoft Corporation</th>
      <th>Tesla, Inc.</th>
      <th>Waste Management, Inc.</th>
      <th>Republic Services, Inc.</th>
      <th>Canadian National Railway</th>
      <th>Ecolab Inc</th>
      <th>Waste Connections, Inc.</th>
      <th>NVIDIA Corp</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2/2/2023 16:00:00</th>
      <td>1.037063</td>
      <td>1.046884</td>
      <td>1.037815</td>
      <td>0.973642</td>
      <td>0.981970</td>
      <td>0.985714</td>
      <td>1.023053</td>
      <td>0.992106</td>
      <td>1.037063</td>
    </tr>
    <tr>
      <th>2/3/2023 16:00:00</th>
      <td>1.062367</td>
      <td>1.022156</td>
      <td>1.047241</td>
      <td>0.978305</td>
      <td>0.986297</td>
      <td>0.985714</td>
      <td>0.984332</td>
      <td>0.994061</td>
      <td>1.062367</td>
    </tr>
    <tr>
      <th>2/6/2023 16:00:00</th>
      <td>1.043320</td>
      <td>1.015905</td>
      <td>1.073590</td>
      <td>0.983226</td>
      <td>0.992788</td>
      <td>0.985714</td>
      <td>0.974058</td>
      <td>0.999323</td>
      <td>1.043320</td>
    </tr>
    <tr>
      <th>2/7/2023 16:00:00</th>
      <td>1.063398</td>
      <td>1.058595</td>
      <td>1.084891</td>
      <td>0.980053</td>
      <td>0.995993</td>
      <td>0.978857</td>
      <td>0.977910</td>
      <td>1.002631</td>
      <td>1.063398</td>
    </tr>
    <tr>
      <th>2/8/2023 16:00:00</th>
      <td>1.044626</td>
      <td>1.055312</td>
      <td>1.109586</td>
      <td>0.977009</td>
      <td>0.997035</td>
      <td>0.957143</td>
      <td>0.971040</td>
      <td>1.003383</td>
      <td>1.044626</td>
    </tr>
  </tbody>
</table>
</div>



    
![png](gsheet_portfolio_assesment_files/gsheet_portfolio_assesment_16_1.png)
    



```python
# Volatility of the portfolio
daily_returns_df.plot(kind='box', figsize=(20,10), title='Volatility of the Portfolio')
plt.show()

plt.savefig('volatility.png')


```


    
![png](gsheet_portfolio_assesment_files/gsheet_portfolio_assesment_17_0.png)
    



    <Figure size 640x480 with 0 Axes>


# Using an Efficiency Frontier, find the optimal portfolio weight for a given risk level

We will use the Efficient Frontier to find the optimal portfolio weight for a given risk level. The Efficient Frontier is a set of optimal portfolios that offer the highest expected return for a defined level of risk or the lowest risk for a given level of expected return. Portfolios that lie below the efficient frontier are sub-optimal because they do not provide enough return for the level of risk. Portfolios that cluster to the right of the efficient frontier are also sub-optimal because they have a higher level of risk for the defined rate of return.



```python
# Lof of percentage change of the portfolio
log_pct_change = portfolio_df.pct_change().apply(lambda x: np.log(1+x))
log_pct_change = log_pct_change.dropna(axis=0, how='all')
log_pct_change.head()

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
      <th>Apple Inc.</th>
      <th>Microsoft Corporation</th>
      <th>Tesla, Inc.</th>
      <th>Waste Management, Inc.</th>
      <th>Republic Services, Inc.</th>
      <th>Canadian National Railway</th>
      <th>Ecolab Inc</th>
      <th>Waste Connections, Inc.</th>
      <th>NVIDIA Corp</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2/2/2023 16:00:00</th>
      <td>0.036392</td>
      <td>0.045818</td>
      <td>0.037117</td>
      <td>-0.026712</td>
      <td>-0.018195</td>
      <td>-0.014389</td>
      <td>0.022791</td>
      <td>-0.007925</td>
      <td>0.036392</td>
    </tr>
    <tr>
      <th>2/3/2023 16:00:00</th>
      <td>0.024107</td>
      <td>-0.023904</td>
      <td>0.009042</td>
      <td>0.004778</td>
      <td>0.004397</td>
      <td>0.000000</td>
      <td>-0.038583</td>
      <td>0.001968</td>
      <td>0.024107</td>
    </tr>
    <tr>
      <th>2/6/2023 16:00:00</th>
      <td>-0.018091</td>
      <td>-0.006135</td>
      <td>0.024849</td>
      <td>0.005018</td>
      <td>0.006560</td>
      <td>0.000000</td>
      <td>-0.010493</td>
      <td>0.005280</td>
      <td>-0.018091</td>
    </tr>
    <tr>
      <th>2/7/2023 16:00:00</th>
      <td>0.019062</td>
      <td>0.041163</td>
      <td>0.010471</td>
      <td>-0.003233</td>
      <td>0.003223</td>
      <td>-0.006981</td>
      <td>0.003948</td>
      <td>0.003305</td>
      <td>0.019062</td>
    </tr>
    <tr>
      <th>2/8/2023 16:00:00</th>
      <td>-0.017810</td>
      <td>-0.003107</td>
      <td>0.022508</td>
      <td>-0.003111</td>
      <td>0.001045</td>
      <td>-0.022433</td>
      <td>-0.007051</td>
      <td>0.000750</td>
      <td>-0.017810</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate and sort the stocks based on the standard deviation
portfolio_std = daily_returns_df.std()
portfolio_std.sort_values(inplace=True)

# Plot the standard deviation of the portfolio
portfolio_std.plot(kind='bar', figsize=(20,10), title='Standard Deviation of the Portfolio')

plt.show()

portfolio_std.head()

```


    
![png](gsheet_portfolio_assesment_files/gsheet_portfolio_assesment_20_0.png)
    





    Waste Connections, Inc.    0.009017
    Waste Management, Inc.     0.009275
    Republic Services, Inc.    0.010082
    Apple Inc.                 0.012755
    NVIDIA Corp                0.012755
    dtype: float64




```python
# Calculate and sort the annualized standard deviation (252 trading days) of the 4 portfolios and the S&P 500
# Review the annual standard deviations smallest to largest
trading_days = 252
annualized_std = portfolio_std * np.sqrt(trading_days)
annualized_std.sort_values()

```




    Waste Connections, Inc.      0.143147
    Waste Management, Inc.       0.147236
    Republic Services, Inc.      0.160047
    Apple Inc.                   0.202487
    NVIDIA Corp                  0.202487
    Ecolab Inc                   0.224572
    Microsoft Corporation        0.271893
    Tesla, Inc.                  0.505436
    Canadian National Railway    0.649994
    dtype: float64




```python
# Using the daily returns DataFrame and a 21-day rolling window, 
# plot the rolling standard deviation of the 4 portfolios and the S&P 500
# Include a title parameter and adjust the figure size
ax = daily_returns_df.plot(kind='bar',figsize=(14,9), title="Daily Returns , vs. 21-day Rolling Standard Deviation")
for header in headers:
    daily_returns_df[header].rolling(window=21).std().plot(ax=ax, label=f"{header} std", legend=True, kind='line')
ax.set_xlabel('Date')
ax.set_ylabel('Daily Returns')

plt.show()

```


    
![png](gsheet_portfolio_assesment_files/gsheet_portfolio_assesment_22_0.png)
    



```python
import hvplot.pandas

# Calculate rolling standard deviation for each column
rolling_std = daily_returns_df.rolling(window=21).std()

# Create a plot using hvplot
plot = rolling_std.hvplot(width=700, height=500, title="Rolling Standard Deviation of Portfolios and S&P 500", xlabel='Date', ylabel='Standard Deviation', legend='top_left', rot=90, shared_axes=False)

# Show the plot
hvplot.show(plot)
display(plot)
```

    Launching server at http://localhost:58916


    WARNING:tornado.access:404 GET /favicon.ico (::1) 0.83ms



```python
import pandas as pd
import hvplot.pandas
import holoviews as hv

# Define the headers (assuming you have a list of headers)
headings = ['Portfolio 1', 'Portfolio 2', 'Portfolio 3', 'S&P 500']

# Create a list to store the plots
plots = []

# Iterate over each header
for header in headers:
    # Create a DataFrame with the daily returns and rolling standard deviation
    df = pd.DataFrame({
        'Daily Returns': daily_returns_df[header],
        'Rolling Std': daily_returns_df[header].rolling(window=21).std()
    })
    
    # Create an interactive plot for the DataFrame using hvplot
    plot = df.hvplot(x='Date', ylabel='Value', shared_axes=False)

    # set the title
    plot = plot.opts(title=header)
    
    # Add the plot to the list of plots
    plots.append(plot)

# Create a layout combining all the plots
layout = hv.Layout(plots).cols(2)

# Show the layout
layout
```




```python

```

## Key Metrics:

1. Average Annual Return

2. Annualized Sharpe Ratio

3. Annualized Volatility

4. Covariance

5. Beta 




```python
# Calculate the annual average return data for the for fund portfolios and the S&P 500
# Use 252 as the number of trading days in the year
# Review the annual average returns sorted from lowest to highest
trading_days = 252

annual_average_returns = daily_returns_df.mean() * trading_days

annual_average_returns.sort_values(inplace=True)

display(annual_average_returns)
```


    Waste Connections, Inc.      0.144388
    Waste Management, Inc.       0.206875
    Republic Services, Inc.      0.424576
    Ecolab Inc                   0.432391
    Apple Inc.                   0.634245
    NVIDIA Corp                  0.634245
    Microsoft Corporation        0.728065
    Canadian National Railway    0.797959
    Tesla, Inc.                  1.097086
    dtype: float64



```python
# Plot the annual average returns using hvplot
annual_average_returns.plot(kind='bar', figsize=(20,10), title='Annual Average Returns of the Portfolio')


```




    <Axes: title={'center': 'Annual Average Returns of the Portfolio'}>




    
![png](gsheet_portfolio_assesment_files/gsheet_portfolio_assesment_28_1.png)
    



```python
# Calculate and plot the sharpe ratios
# first, load the live risk-free rate using yfinance
import yfinance as yf

risk_free_rate = yf.Ticker('^IRX').history(period='max')['Close'][-1] / 100

sharpe_ratios = (annual_average_returns - risk_free_rate) / annualized_std

# Display the sharpe ratios in descending order
display(sharpe_ratios.sort_values(ascending=False))

# Plot the sharpe ratios
sharpe_ratios.plot(kind='bar', figsize=(20,10), title='Sharpe Ratios of the Portfolio')



```


    Apple Inc.                   2.874728
    NVIDIA Corp                  2.874728
    Microsoft Corporation        2.485961
    Republic Services, Inc.      2.326977
    Tesla, Inc.                  2.067394
    Ecolab Inc                   1.693183
    Canadian National Railway    1.147409
    Waste Management, Inc.       1.050860
    Waste Connections, Inc.      0.644358
    dtype: float64





    <Axes: title={'center': 'Sharpe Ratios of the Portfolio'}>




    
![png](gsheet_portfolio_assesment_files/gsheet_portfolio_assesment_29_2.png)
    



```python
# Calculate the Sortino Ratio aswell as the Calmar Ratio and plot them
# Sortino Ratio
sortino_ratios = (annual_average_returns - risk_free_rate) / daily_returns_df[daily_returns_df < 0].std()

# Calmar Ratio
calmar_ratios = annual_average_returns / daily_returns_df.rolling(window=252).min()

# Plot the sharpe ratios
sortino_ratios.plot(kind='bar', figsize=(20,10), title='Sortino Ratios of the Portfolio')

# Display the results

display(sortino_ratios.sort_values(ascending=False))
display(calmar_ratios.sort_values(ascending=False))

```




    <Axes: title={'center': 'Sortino Ratios of the Portfolio'}>




    
![png](gsheet_portfolio_assesment_files/gsheet_portfolio_assesment_30_1.png)
    



```python
# Plot the calmar ratios, sortino ratios, and sharpe ratios to compare risk/reward ratios

ax = calmar_ratios.plot(kind='bar', figsize=(20,10), title='Calmar Ratios of the Portfolio')

for header in headers:
    sortino_ratios[header].plot(ax=ax, label=f"{header} sortino", legend=True, kind='line')
    sharpe_ratios[header].plot(ax=ax, label=f"{header} sharpe", legend=True, kind='line')

ax.set_xlabel('Date')
ax.set_ylabel('Ratio')

plt.show()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Cell In[39], line 6
          3 ax = calmar_ratios.plot(kind='bar', figsize=(20,10), title='Calmar Ratios of the Portfolio')
          5 for header in headers:
    ----> 6     sortino_ratios[header].plot(ax=ax, label=f"{header} sortino", legend=True, kind='line')
          7     sharpe_ratios[header].plot(ax=ax, label=f"{header} sharpe", legend=True, kind='line')
          9 ax.set_xlabel('Date')


    AttributeError: 'numpy.float64' object has no attribute 'plot'



    
![png](gsheet_portfolio_assesment_files/gsheet_portfolio_assesment_31_1.png)
    



```python

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
      <th>Apple Inc.</th>
      <th>Canadian National Railway</th>
      <th>Ecolab Inc</th>
      <th>Microsoft Corporation</th>
      <th>NVIDIA Corp</th>
      <th>Republic Services, Inc.</th>
      <th>Tesla, Inc.</th>
      <th>Waste Connections, Inc.</th>
      <th>Waste Management, Inc.</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2/2/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2/3/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2/6/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2/7/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2/8/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7/7/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7/10/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7/11/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7/12/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7/13/2023 16:00:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>111 rows × 9 columns</p>
</div>

