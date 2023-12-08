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


    ---------------------------------------------------------------------------

    RefreshError                              Traceback (most recent call last)

    Cell In[15], line 2
          1 # Open the spreadsheet
    ----> 2 spreadsheet = gc.open("Stock Portfolio - Fawaz")
          4 # Specify the worksheet to be used
          5 worksheet = spreadsheet.worksheet("AAPL")


    File /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/gspread/client.py:171, in Client.open(self, title, folder_id)
        153 """Opens a spreadsheet.
        154 
        155 :param str title: A title of a spreadsheet.
       (...)
        166 >>> gc.open('My fancy spreadsheet')
        167 """
        168 try:
        169     properties = finditem(
        170         lambda x: x["name"] == title,
    --> 171         self.list_spreadsheet_files(title, folder_id),
        172     )
        174     # Drive uses different terminology
        175     properties["title"] = properties["name"]


    File /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/gspread/client.py:146, in Client.list_spreadsheet_files(self, title, folder_id)
        143 if page_token:
        144     params["pageToken"] = page_token
    --> 146 res = self.request("get", url, params=params).json()
        147 files.extend(res["files"])
        148 page_token = res.get("nextPageToken", None)


    File /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/gspread/client.py:80, in Client.request(self, method, endpoint, params, data, json, files, headers)
         70 def request(
         71     self,
         72     method,
       (...)
         78     headers=None,
         79 ):
    ---> 80     response = getattr(self.session, method)(
         81         endpoint,
         82         json=json,
         83         params=params,
         84         data=data,
         85         files=files,
         86         headers=headers,
         87         timeout=self.timeout,
         88     )
         90     if response.ok:
         91         return response


    File /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/requests/sessions.py:600, in Session.get(self, url, **kwargs)
        592 r"""Sends a GET request. Returns :class:`Response` object.
        593 
        594 :param url: URL for the new :class:`Request` object.
        595 :param \*\*kwargs: Optional arguments that ``request`` takes.
        596 :rtype: requests.Response
        597 """
        599 kwargs.setdefault("allow_redirects", True)
    --> 600 return self.request("GET", url, **kwargs)


    File /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/google/auth/transport/requests.py:545, in AuthorizedSession.request(self, method, url, data, headers, max_allowed_time, timeout, **kwargs)
        542 remaining_time = max_allowed_time
        544 with TimeoutGuard(remaining_time) as guard:
    --> 545     self.credentials.before_request(auth_request, method, url, request_headers)
        546 remaining_time = guard.remaining_timeout
        548 with TimeoutGuard(remaining_time) as guard:


    File /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/google/auth/credentials.py:135, in Credentials.before_request(self, request, method, url, headers)
        131 # pylint: disable=unused-argument
        132 # (Subclasses may use these arguments to ascertain information about
        133 # the http request.)
        134 if not self.valid:
    --> 135     self.refresh(request)
        136 self.apply(headers)


    File /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/google/oauth2/credentials.py:335, in Credentials.refresh(self, request)
        317 if (
        318     self._refresh_token is None
        319     or self._token_uri is None
        320     or self._client_id is None
        321     or self._client_secret is None
        322 ):
        323     raise exceptions.RefreshError(
        324         "The credentials do not contain the necessary fields need to "
        325         "refresh the access token. You must specify refresh_token, "
        326         "token_uri, client_id, and client_secret."
        327     )
        329 (
        330     access_token,
        331     refresh_token,
        332     expiry,
        333     grant_response,
        334     rapt_token,
    --> 335 ) = reauth.refresh_grant(
        336     request,
        337     self._token_uri,
        338     self._refresh_token,
        339     self._client_id,
        340     self._client_secret,
        341     scopes=scopes,
        342     rapt_token=self._rapt_token,
        343     enable_reauth_refresh=self._enable_reauth_refresh,
        344 )
        346 self.token = access_token
        347 self.expiry = expiry


    File /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/google/oauth2/reauth.py:349, in refresh_grant(request, token_uri, refresh_token, client_id, client_secret, scopes, rapt_token, enable_reauth_refresh)
        342     (
        343         response_status_ok,
        344         response_data,
        345         retryable_error,
        346     ) = _client._token_endpoint_request_no_throw(request, token_uri, body)
        348 if not response_status_ok:
    --> 349     _client._handle_error_response(response_data, retryable_error)
        350 return _client._handle_refresh_grant_response(response_data, refresh_token) + (
        351     rapt_token,
        352 )


    File /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/google/oauth2/_client.py:69, in _handle_error_response(response_data, retryable_error)
         66 except (KeyError, ValueError):
         67     error_details = json.dumps(response_data)
    ---> 69 raise exceptions.RefreshError(
         70     error_details, response_data, retryable=retryable_error
         71 )


    RefreshError: ('invalid_grant: Token has been expired or revoked.', {'error': 'invalid_grant', 'error_description': 'Token has been expired or revoked.'})



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





<div id='1002'>
  <div class="bk-root" id="bf012fc5-70b8-422b-a312-13f2782c0ec4" data-root-id="1002"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"9cfd452b-671a-4a4a-9aa0-267e0b39091c":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"GridStack1","overrides":[],"properties":[{"default":"warn","kind":null,"name":"mode"},{"default":null,"kind":null,"name":"ncols"},{"default":null,"kind":null,"name":"nrows"},{"default":true,"kind":null,"name":"allow_resize"},{"default":true,"kind":null,"name":"allow_drag"},{"default":[],"kind":null,"name":"state"}]},{"extends":null,"module":null,"name":"click1","overrides":[],"properties":[{"default":"","kind":null,"name":"terminal_output"},{"default":"","kind":null,"name":"debug_name"},{"default":0,"kind":null,"name":"clears"}]},{"extends":null,"module":null,"name":"NotificationAreaBase1","overrides":[],"properties":[{"default":"bottom-right","kind":null,"name":"position"},{"default":0,"kind":null,"name":"_clear"}]},{"extends":null,"module":null,"name":"NotificationArea1","overrides":[],"properties":[{"default":[],"kind":null,"name":"notifications"},{"default":"bottom-right","kind":null,"name":"position"},{"default":0,"kind":null,"name":"_clear"},{"default":[{"background":"#ffc107","icon":{"className":"fas fa-exclamation-triangle","color":"white","tagName":"i"},"type":"warning"},{"background":"#007bff","icon":{"className":"fas fa-info-circle","color":"white","tagName":"i"},"type":"info"}],"kind":null,"name":"types"}]},{"extends":null,"module":null,"name":"Notification","overrides":[],"properties":[{"default":null,"kind":null,"name":"background"},{"default":3000,"kind":null,"name":"duration"},{"default":null,"kind":null,"name":"icon"},{"default":"","kind":null,"name":"message"},{"default":null,"kind":null,"name":"notification_type"},{"default":false,"kind":null,"name":"_destroyed"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"line_alpha":0.2,"line_color":"#9467bd","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1205","type":"Line"},{"attributes":{"label":{"value":"Ecolab Inc"},"renderers":[{"id":"1206"}]},"id":"1230","type":"LegendItem"},{"attributes":{},"id":"1201","type":"Selection"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1074","type":"Line"},{"attributes":{"line_color":"#1f77b4","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1301","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"1232"},"glyph":{"id":"1235"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1237"},"nonselection_glyph":{"id":"1236"},"selection_glyph":{"id":"1265"},"view":{"id":"1239"}},"id":"1238","type":"GlyphRenderer"},{"attributes":{},"id":"1021","type":"LinearScale"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc.","Waste Connections, Inc."],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f0Ub4kllWXw/5Bp6QWVOfD8ldYGoERCAP9aNy6iEzH8/vlZAk+gRgD/ihQuhTKSAPxycROcIlYE/BAgoQ2GTgj+qq2fNc3CCP0jifkJevoI/SUc1UWlrgz85UBCZ4ZyDP1wL+ZBp1oQ/Syt3G1n8hD8MUrS4HPyEP76EdXAWmoY/NSMCwNCBhj9tFeCl+CKGP1WCLshUaoU/VwD4Ugshhj/vX9y7zEmGP097UFo6SIY/SK3oviZehj903R/ulYqFPwT3iEqKmIU/2WIbQzhIhT+k0G5X4PSEP9xpsj3AhIU/uY9xjx69hD/GnYyY7xmFP67dorSkBoU/LXjJ7kVEhD9HgaE/5yOEP7Ej95MKK4I/x7vQUf1kgT+046WfH/2DP5UkNJVFJIM/rE99DgSggz/6JmsS7lyGPysZSArKHIc/yZGzWXlEhj/LbyKdyAOGPwQiHnxa+oU/VtGYxU+6hT/fqv06x6yFP2a5LBapyYU/eFqKGQzfhT8Ra4cbuh+GP7JPKfklXIU/pDb+EwfUhD/tufS1i6qEP+YlT59x/4Q/bs4WfBZVhT9+tlat0paFP5p5Zy/voYU/ifnEovA3hT+IZ5ZXBQWEP4mniHNa7YM/D6jF3gLMgz/Jg6PZDvGAP9F9pyGiB4A/GZfz1/IIgD/Rnwi74WaAP9WlZhF+mYE/pkpPq3Qggj+02OXb4N+BPyWTVWXA7YE/XUWdvosJgj8TXGxW8S+APzTH3xK69n8/7OLAcDGnfz/Fae2XCwSBP7lDrn1g1YA/g5iES/JIgD9BT1Dm0RyAPy2s0BDlkH8/B42Qez8xfz+J/ov8e9N9P6kZCPMW3n8/UbstW+0xgD/B95J3C/F/P4jPrzZhSn8/Ca8ZaDd0gT/6KPglBj6CP8wRx65yS4E/PqHqRIUYgT/Uh6XUwuaAP3KfGLvlt38/rV/mw9FFfz9mSN6EW1B/Pw1sI75Xn4A/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"1233"},"selection_policy":{"id":"1261"}},"id":"1232","type":"ColumnDataSource"},{"attributes":{"label":{"value":"Waste Connections, Inc."},"renderers":[{"id":"1238"}]},"id":"1264","type":"LegendItem"},{"attributes":{},"id":"1031","type":"PanTool"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1075","type":"Line"},{"attributes":{"line_color":"#9467bd","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1231","type":"Line"},{"attributes":{},"id":"1064","type":"UnionRenderers"},{"attributes":{"line_color":"#e5ae38","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1115","type":"Line"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1052","type":"Line"},{"attributes":{},"id":"1024","type":"CategoricalTicker"},{"attributes":{},"id":"1071","type":"Selection"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1053","type":"Line"},{"attributes":{"axis":{"id":"1023"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1025","type":"Grid"},{"attributes":{"label":{"value":"Microsoft Corporation"},"renderers":[{"id":"1076"}]},"id":"1090","type":"LegendItem"},{"attributes":{"line_color":"#9467bd","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1203","type":"Line"},{"attributes":{"coordinates":null,"group":null,"text":"Rolling Standard Deviation of Portfolios and S&P 500","text_color":"black","text_font_size":"12pt"},"id":"1015","type":"Title"},{"attributes":{},"id":"1087","type":"UnionRenderers"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"1044"},"group":null,"major_label_orientation":1.5707963267948966,"major_label_policy":{"id":"1045"},"ticker":{"id":"1024"}},"id":"1023","type":"CategoricalAxis"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc.","Waste Management, Inc."],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f2/aJ8lTHYQ/vxVtRzJ0gD+8QbJQIs2APx5+bS+js4A/6vIYy8HRgD/8n8K4xjCBP1VPlw0vqIE/YWQMA8pLgz8+N3hNXvaCP5zUaMdgYYI/qv+gNqPMgz+LXTZ/WGOEPzvdjvtb4YI/NfXp53rugj/j2HHuwFWDP2KfS81/EYY/eJjDRzZFhj/nOZ7tVXmGP5IHbatXs4Y/ipjsPj0niT/v7mbihT+JP3PxYwRoMYk/fFKJ8r9ZiT9d44rai+yIP1h3L/au7Ig/qg0iB9eOiT/K8NzL3emIP7ws29NZC4k/uqAiLzvxhz8Zj8acOKOIP0mcCLFzqYg/beaKNVwahz+9I1mHo/GGP0AYRB9RzYY/74pElt1AhT+M9QhHV1qEPzhiadyLD4I/tJcy5CDlgj+1rbAuhOWDP3u2dZgBSIY/0aUTxjS7gj+8XrNHnE6CPxncQ/cvSYI//VZKg48+gj948KBlMRSCP4DYYJglB4I/WBtZACVogT9Z/DvhMXWBP/Uyh7/bjYA/q8ei55qCgD/dkzYZkvx/P1YK/FiuN4A/an9sMe59gD9Mcler7SiBP2RvXZ+OG4E/+9qfdipJgT/mCK2mFaqBP74GNUMQqoE/AkVgEbL/gD8fHrHDbjCAP/iYLj1fmHQ/he7BRJTrcz+3LEBScdR0P0gKSI/rLXg/paOPiKPydz9oNQ7DNq15P5/laoBLnHk/e9zypQJqej/Bbgj4Elt5PwzmJ5R7kHk/pnQ/A+l/eT/W4UGiuK15PyvF07ii2Xs/mGNzfWHuez8mtWSub196P2i7wZNQhXo/xKoMDG/aej+LDnQg6nR6P6AFUsqy8Xo/UV3Dhc0VfD/rm+lZM/98P8oVZBH1d34/SHEE2gGbgT9zIRHLysmCP0fJwEDrLoI/uoj1oAYggj/Sxw9ie3WBP4nBE7ldBoE/mjYJX39OgT8pP+iEu7GBP9mViRJ79YE/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"1117"},"selection_policy":{"id":"1137"}},"id":"1116","type":"ColumnDataSource"},{"attributes":{"line_alpha":0.2,"line_color":"#17becf","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1175","type":"Line"},{"attributes":{},"id":"1045","type":"AllLabels"},{"attributes":{"axis_label":"Standard Deviation","coordinates":null,"formatter":{"id":"1047"},"group":null,"major_label_policy":{"id":"1048"},"ticker":{"id":"1027"}},"id":"1026","type":"LinearAxis"},{"attributes":{"source":{"id":"1142"}},"id":"1149","type":"CDSView"},{"attributes":{"line_color":"#d62728","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1235","type":"Line"},{"attributes":{"axis":{"id":"1026"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1029","type":"Grid"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1073","type":"Line"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp","NVIDIA Corp"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f+5T/i8rHZI/1Fm6ywqlkD+gSkqdXwuQP4yTbTL7SI8/p2CaHme0jj/CXR+W0VKOP4jb6YEQ1Y4/JoUep7F4jz9lCaU3nVGOP6s9NGLfYo8/4nxsF37ljj/TJWNxvjqPP6C8S9FGS48/NHDcgO/HjD+KfJXd09WMP4cCx9nv7Iw/77lHtbEjjD8yWHIwoUGMPyx8Em45BY0/ARYrDGHGiz8/LV6B4COMP7b1Cz7SoYg/MRZsSqL3hz/be3OohH+HP3exjrUBaoc/1xJr7PeXhz+2BooFtMWGP+HZRqgnm4Y/wCP5BT8nij9D8W68LUqKP4qXp+J2Xok/GTqF/W8jiT+Lc5+RJpGIPxQ3NJWFgYg/5TFcMsqSiD8GPRh0jniIP3aNoyd0yYg/6I81uIr9hz94aGlOZpuKPxUYr4PWjIk/BhJnd6paiT9sqVNM18mIP8i4RCeWzog/hRq+BrkwiT/gVopG0xGQPzEXXurSEpA//TIZmIeGjz8r2dvrx0uPP4usYjAcGI8/aZ7TqkMDjD9EqaL0FwqMPwXB4HOMCow/rob1x2nviz/0Z46LwFmMP3AAceA1HYw/1y+h+/3Iiz+lceKtHeiMP5+6zvdJa4w/aAPriCB1jD9XXg5f0mSKP31AxQEXkYo/Gpx8zcqPij98cUwJ9fGKP6QHawCuj4o/11LOFOFTij9iZYcpsfmAP2E/Mq/jdoE/aJlQuGe/gT8mlLbzi1iBPxtGwkq6XoI/36/H1voBgj906M4d5cyBP6omKD+zE4I/zyDrbsSUgj+Q1L85fPaBP0q29qyjVII/jZbjfMf6gj8DJo6dShOBP5gXkqaU04E/X6ZyieqBgj8zrJHzye+BP7CBCMBLpYE/2sWxiWumgz/PXk8sOZCDPyHkD6Wi9YM/PTBCy/Jegz+YbGYJcqmDPyKPt9/yD4Q/pXPfqSFNgz9eteJiwI2DPyDW5ZvFjoI/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"1267"},"selection_policy":{"id":"1297"}},"id":"1266","type":"ColumnDataSource"},{"attributes":{},"id":"1027","type":"BasicTicker"},{"attributes":{},"id":"1032","type":"WheelZoomTool"},{"attributes":{"children":[{"id":"1014"}],"height":500,"margin":[0,0,0,0],"name":"Row01116","sizing_mode":"fixed","tags":["embedded"],"width":700},"id":"1002","type":"Row"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway","Canadian National Railway"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f0BJ04H7y44/5i06+gVmjj/mLTr6BWaOP+YtOvoFZo4/39phW4NtkD85L9YxiGeQP5PrbVvNq5A/Ynkv9KdLoj+b3xM0oMSpP0hAAr5ukK0/mMs7/L1Erj/1TlhTTECuP5jrc047yK4/LBP8bGdZsD/Wq4VnhUWwP0mz+Xl/mbA/X/d/V+6KsD9f939X7oqwP4L3ZImWc7A/BETktyshsD90cZDFpzWwPzoVz+2UwbA/OzMajtAWsT+MyAYWhAyxP4tpCCi5GrE/6bsjiPvCsD/Qcc82KgyxP0gurni2DbE/guoI4bCbrz8psxbz9ESrP5tbS4BZuqc/Cgpa0ewvpz9czh1KFCqnP8mmXaUVPKY/Z7NIDFOYoz8vmYCLyXujP1BIW1A/yKI/EPCW8BdOoz/XUjcZBC+jP5bWEIhaa6M/kGriclrDoz81b7zsZGylP9kKz9nzXqM/u6CTJ20moT8tf0wyKFuhP4mJhmIJBKI/DCGAJq+loj9MuPEcwPSgPwssz3pO9KA/IoBsW3McoT9vgWsFpDygP8xlUeR5SaA/oJlAbVy5nT+zzjAycQSeP4Xow8zVCZ4/1csb8+kKnj9JMEmwZlmdPxrJW+AxbaA/UviEf/x8oD+NJ0AZ1aCgP223H6pCqKA/VnlZdh6qoD838HI4hmejPwXjHCAJ96M/p/deHbqQpD+6sVg195KkP6Id6y/fdaM/xw/fw7wboz9Ig7Djd3iiP9Qxk0a2OKM/Cr+5s0ceoz+W5rbttAejP5xX8wW94aI/3sOkQ8TUoj93fGFADISjP7fUEKzxkKM/GvfwYayYoz9S+Z/M15+jP+or9GuqFqI/Cc42oreooj/GcpD5Ej+iP8ZykPkSP6I/bYustWlKoj8+VGaJ23ubP9kMv9z4EZY/Y0Vu6KqDlT8LqSyK06yVP1FnWbFL05U/C+YA6wILmj8AekvtQkmcP9VivP9ryps/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"1171"},"selection_policy":{"id":"1195"}},"id":"1170","type":"ColumnDataSource"},{"attributes":{"line_color":"#8b8b8b","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1169","type":"Line"},{"attributes":{"source":{"id":"1266"}},"id":"1273","type":"CDSView"},{"attributes":{"coordinates":null,"data_source":{"id":"1116"},"glyph":{"id":"1119"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1121"},"nonselection_glyph":{"id":"1120"},"selection_glyph":{"id":"1141"},"view":{"id":"1123"}},"id":"1122","type":"GlyphRenderer"},{"attributes":{},"id":"1030","type":"SaveTool"},{"attributes":{"overlay":{"id":"1035"}},"id":"1033","type":"BoxZoomTool"},{"attributes":{"line_alpha":0.1,"line_color":"#6d904f","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1120","type":"Line"},{"attributes":{},"id":"1034","type":"ResetTool"},{"attributes":{},"id":"1137","type":"UnionRenderers"},{"attributes":{"source":{"id":"1170"}},"id":"1177","type":"CDSView"},{"attributes":{"line_alpha":0.2,"line_color":"#6d904f","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1121","type":"Line"},{"attributes":{"line_alpha":0.1,"line_color":"#17becf","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1174","type":"Line"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc.","Apple Inc."],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f+5T/i8rHZI/1Fm6ywqlkD+gSkqdXwuQP4yTbTL7SI8/p2CaHme0jj/CXR+W0VKOP4jb6YEQ1Y4/JoUep7F4jz9lCaU3nVGOP6s9NGLfYo8/4nxsF37ljj/TJWNxvjqPP6C8S9FGS48/NHDcgO/HjD+KfJXd09WMP4cCx9nv7Iw/77lHtbEjjD8yWHIwoUGMPyx8Em45BY0/ARYrDGHGiz8/LV6B4COMP7b1Cz7SoYg/MRZsSqL3hz/be3OohH+HP3exjrUBaoc/1xJr7PeXhz+2BooFtMWGP+HZRqgnm4Y/wCP5BT8nij9D8W68LUqKP4qXp+J2Xok/GTqF/W8jiT+Lc5+RJpGIPxQ3NJWFgYg/5TFcMsqSiD8GPRh0jniIP3aNoyd0yYg/6I81uIr9hz94aGlOZpuKPxUYr4PWjIk/BhJnd6paiT9sqVNM18mIP8i4RCeWzog/hRq+BrkwiT/gVopG0xGQPzEXXurSEpA//TIZmIeGjz8r2dvrx0uPP4usYjAcGI8/aZ7TqkMDjD9EqaL0FwqMPwXB4HOMCow/rob1x2nviz/0Z46LwFmMP3AAceA1HYw/1y+h+/3Iiz+lceKtHeiMP5+6zvdJa4w/aAPriCB1jD9XXg5f0mSKP31AxQEXkYo/Gpx8zcqPij98cUwJ9fGKP6QHawCuj4o/11LOFOFTij9iZYcpsfmAP2E/Mq/jdoE/aJlQuGe/gT8mlLbzi1iBPxtGwkq6XoI/36/H1voBgj906M4d5cyBP6omKD+zE4I/zyDrbsSUgj+Q1L85fPaBP0q29qyjVII/jZbjfMf6gj8DJo6dShOBP5gXkqaU04E/X6ZyieqBgj8zrJHzye+BP7CBCMBLpYE/2sWxiWumgz/PXk8sOZCDPyHkD6Wi9YM/PTBCy/Jegz+YbGYJcqmDPyKPt9/yD4Q/pXPfqSFNgz9eteJiwI2DPyDW5ZvFjoI/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"1050"},"selection_policy":{"id":"1064"}},"id":"1049","type":"ColumnDataSource"},{"attributes":{},"id":"1117","type":"Selection"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1035","type":"BoxAnnotation"},{"attributes":{"label":{"value":"Waste Management, Inc."},"renderers":[{"id":"1122"}]},"id":"1140","type":"LegendItem"},{"attributes":{"line_alpha":0.1,"line_color":"#9467bd","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1204","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"1170"},"glyph":{"id":"1173"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1175"},"nonselection_glyph":{"id":"1174"},"selection_glyph":{"id":"1199"},"view":{"id":"1177"}},"id":"1176","type":"GlyphRenderer"},{"attributes":{"active_drag":{"id":"1031"},"active_scroll":{"id":"1032"},"tools":[{"id":"1005"},{"id":"1030"},{"id":"1031"},{"id":"1032"},{"id":"1033"},{"id":"1034"}]},"id":"1036","type":"Toolbar"},{"attributes":{"line_alpha":0.1,"line_color":"#d62728","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1236","type":"Line"},{"attributes":{},"id":"1195","type":"UnionRenderers"},{"attributes":{"line_color":"#6d904f","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1119","type":"Line"},{"attributes":{},"id":"1048","type":"AllLabels"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"1068"},{"id":"1090"},{"id":"1114"},{"id":"1140"},{"id":"1168"},{"id":"1198"},{"id":"1230"},{"id":"1264"},{"id":"1300"}],"location":"top_left","title":"Variable"},"id":"1067","type":"Legend"},{"attributes":{},"id":"1261","type":"UnionRenderers"},{"attributes":{},"id":"1047","type":"BasicTickFormatter"},{"attributes":{},"id":"1171","type":"Selection"},{"attributes":{"line_color":"#d62728","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1265","type":"Line"},{"attributes":{"line_color":"#17becf","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1199","type":"Line"},{"attributes":{"source":{"id":"1049"}},"id":"1056","type":"CDSView"},{"attributes":{"label":{"value":"Canadian National Railway"},"renderers":[{"id":"1176"}]},"id":"1198","type":"LegendItem"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1091","type":"Line"},{"attributes":{"line_alpha":0.2,"line_color":"#1f77b4","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1271","type":"Line"},{"attributes":{"line_color":"#17becf","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1173","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"1049"},"glyph":{"id":"1052"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1054"},"nonselection_glyph":{"id":"1053"},"selection_glyph":{"id":"1069"},"view":{"id":"1056"}},"id":"1055","type":"GlyphRenderer"},{"attributes":{"source":{"id":"1070"}},"id":"1077","type":"CDSView"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc.","Tesla, Inc."],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f8QRy1+eKqM/ZgaYhjfzoj9ssGjDg0qjP9x/hU8hWKM/ifeeZgf6oz89qcDXV7ujP5io0ljRVKM/vxkVvU2toz96xAkgWLSjP/7Dtk5Gt6E/V0LvF810oT95bdzmk6mgPwmXMfivraI/thBCKcoXoj/c5okg3/qhPwqRpwJZ+qE//3tMLpTKoT8efaO4W5ugP0kfabmP5aA/i9B56RjeoD/eEkrqq+WgP2RXs0vh+KE/9maxs7XeoT/GKdwk6P+hPxbPctbBpaE/E28uTBucoD8+s9gyaaOgPz+eb0ZDJ6E/jLY5yrGJoD+alGzfHnGgPzOSTMTFVaA/UL4RSQo5oD9T0QrhVEmgP+n6YJ5mAqE/jDYQsEPtoD8zo/bst+SgP6k0PNgB5qA/+fweW4VAoT+1P36VdC+iP3DdYUIHNaI/rr6wgUAloj9b2wEepiSgP0PhCVI+O54/1C8RfHdZnj8OtHlmEVGgP6HYOoG4Y6A/i4AkZYJyoD9EYYjKHlegPxFYePnhM6A/U0VBEbXhnz8Xxk1ed+efP28+Pyu6uZ8/XxDtOxa7oD+KvbwZ9bqgP+JiYRlwupg/3dN6IjuImj+kDX3jpZSaPyIjYrH4u5o/JqqZGh4SmD/tASZqeXmYP46boiqkQ5k/20Jt3ruTmD91l3hbfR6YP7sVKrKLUZg/1X70IBU0mD/bjGr550yWPxJ7BkGHS5Y/2SlZf4Zllj/g4rpzjoyWPweYvZUVj5Y/GG9r7TCVlD/HDSAf+2iUP2Xmh8UQqpQ/q7q/kb7vkz/wg+4+I02VP+4kI4mTP5s/udGB6RhMmj+WNSU//V2bP8AbIJjymp8/q/e5r7EEoD8Hz4btCDufP42Zqr+cnJ4/zB0ytCCgnj831/JyvJCgP5s4hiWQdqA/V22RNKLooD/3GaBfYgmhP/jFS6zBUaE/PFz3Mj/MoD88+tJ3u1WgP2beueEVVKA/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"1093"},"selection_policy":{"id":"1111"}},"id":"1092","type":"ColumnDataSource"},{"attributes":{"source":{"id":"1116"}},"id":"1123","type":"CDSView"},{"attributes":{"coordinates":null,"data_source":{"id":"1266"},"glyph":{"id":"1269"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1271"},"nonselection_glyph":{"id":"1270"},"selection_glyph":{"id":"1301"},"view":{"id":"1273"}},"id":"1272","type":"GlyphRenderer"},{"attributes":{"line_color":"#6d904f","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1141","type":"Line"},{"attributes":{},"id":"1044","type":"CategoricalTickFormatter"},{"attributes":{"line_alpha":0.1,"line_color":"#1f77b4","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1270","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"1092"},"glyph":{"id":"1095"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1097"},"nonselection_glyph":{"id":"1096"},"selection_glyph":{"id":"1115"},"view":{"id":"1099"}},"id":"1098","type":"GlyphRenderer"},{"attributes":{},"id":"1297","type":"UnionRenderers"},{"attributes":{"below":[{"id":"1023"}],"center":[{"id":"1025"},{"id":"1029"},{"id":"1067"}],"height":500,"left":[{"id":"1026"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"1055"},{"id":"1076"},{"id":"1098"},{"id":"1122"},{"id":"1148"},{"id":"1176"},{"id":"1206"},{"id":"1238"},{"id":"1272"}],"sizing_mode":"fixed","title":{"id":"1015"},"toolbar":{"id":"1036"},"width":700,"x_range":{"id":"1003"},"x_scale":{"id":"1019"},"y_range":{"id":"1004"},"y_scale":{"id":"1021"}},"id":"1014","subtype":"Figure","type":"Plot"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation","Microsoft Corporation"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f3DTMzMYWZU/QA7pGTp5kj8/vdhlr92RP96FMSqK0pE/B8yELkOWjT8fbPOm2xOOP8SdCUFsuo8/FtGVLrAckT820pSDrAuQPw//F7nShpI/U6maZmt6kj9eCiIA52iSP4hXgeNg9JE/iEcNQ8oxkT/LfAILtW+RP/oDT713YpE/JyXGJ6/lkD8OKMhuhwWRP2oM2F/QN5E/E/bv2AW5kD/OW5vEZ5GQP+LQfI2djZA/MUBRQfqakD9VXcfTFZKQPxBWvhZZD5E/KgCGN5QlkT8UPkoalL6RP2t/yWJ8ZpE/QvIbRxkgkT+yeKAwv02RPz8t8gk0K44/sMnw2hLgjT9pFnUZMACLPx81gvCjZYs/g/qrngk3iz+v/dVtZuCKP5BHBKzOYow/p506HsBIlT8bcO5u0yGWPyM9UsgA5pU/kkRQxX3slT/QWRcJZMeVP3mRKrTGxZU/s8ucqJHClT9PbGOx/b+VP8hGxgrUSpU/DOl1cIs9lT9DOMZo1JKUPyYLio/LvZQ/YJtqNwBglD9+ip0qpQiUPy/Q7R7gApQ/ecunxCD8kz8muY2JPRaUPwNXpzV35pM//8ry7BLbkz+PJOMZpRyUP6fY8FVDLJM/Gw2gHhmRiz+VEmW6b7WJPxQeWDwo+Yk/PxEX4jc6ij94Qb7zbXiKP31WuNtNTIo/OWeDNfpTij/bBfobUhuKPwY5Y3aHCo4/6VKPAGTbjT/FLAcKvRmNPwFu2pjRVI0/vfJHBFcxjT+pOym8oUONP3gxN5vYw48/v12RV3SckD9+2Lop4dWQPwDE4DwANpE/07w8d5CHkT/v/jZymUORP6owzFk75pE/SOgu4jJAkD/YcCkmJiePPwte42xVE48/9xv9wfiFjz/zGqe58lmPP18aZwfeJY8/9FtowtNjjz8SwaNAlbuPP/yLPVgKPI0/fUJ1fDIpjT+VZP+72LWNP55emDUIyI0/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"1071"},"selection_policy":{"id":"1087"}},"id":"1070","type":"ColumnDataSource"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc.","Republic Services, Inc."],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f4ZHLmYNKIc/uBypHYk/hT+jBzv06IGGPx2ftgRMZ4Y/5Xf/iHFlhj/MbrDqJPqGP5YWSRAHKYc/pBGOmNFuiT+XtXe9LdWIP0AN9/EcOYg/9VPTLzl5ij8VuhpCBkiGP+6c3I2clYU/l121JKk2hT8olRXiWYiFPycJr1ly34c//2aeay/Ehz8ZXbcvtduHP9dUIldLXYg/bI2PGzntiD8XTQpCcOqIP4DHWMdi7Yg/xqEReD3ziD8px6j9FhuIPx9R5ruCG4g/Q6UOrMRliD8o0faQGLiHP20iY8OnA4g/JwhE8OkahT+Xf9GnLkiFP2ibpZxHfoU/umpg3Icigz/yxJ9dLAKDP8ClFJKqXII/5iI9kHiPgT8bTDfXeRmAPzz8tD4EDns/3jbTDh6YfT+leN+3pWJ9P+MA88dnwok/oDu9gldSiT8yPkNIfl+JP1k64WOGZIk/UBPdUCpxiT8pCyPEahCJPwLfmjlKH4k/4aMezvPSiD/fRsdotdCIPxEMdkGocog/mlu3K11niD8OvWggKjyIP8aIyH6Hh4g/08VrXNKYiD8dfqrvxNeIP5buIXXYy4g/d8iFlgI1iT9hUFBLyaGJP+ILCkKMoYk/GJVPSlsbiT/u93lFgjyJP0WFj0IfuXU/xLVRoZeKdT9NilNOMfN2P8PBIqgGzXg/OSa6E68neT+zW045+DZ5P7QddvFAkno/9pdjX5qtez8YG1OuKnl6P1xn1g7kyHo/F0JGuXPgej+ccRFfcDR7Py79YgWrk3s/lUIqeHq5ez/hSjheRDF7P0dXIqwnQHs/9KaWWikKez8GzTy4IaR6P76O2Q7TPns/n1RsrZJRfT9T5vy8oJR+P4Y5vv2WpYA/JB2Vcm/RgD/TnfhqPweCPwGGeoPyjYE/SvfaSus0gT+5/r9ifZmBP9dKcabtlYA/YgKhV5ysgD/NhRNyzOGAP8tPIMwo4IA/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"1143"},"selection_policy":{"id":"1165"}},"id":"1142","type":"ColumnDataSource"},{"attributes":{"line_alpha":0.1,"line_color":"#e5ae38","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1096","type":"Line"},{"attributes":{},"id":"1111","type":"UnionRenderers"},{"attributes":{},"id":"1267","type":"Selection"},{"attributes":{"end":0.07300908239707626,"reset_end":0.07300908239707626,"reset_start":-0.001331730018417531,"start":-0.001331730018417531,"tags":[[["value","value",null]]]},"id":"1004","type":"Range1d"},{"attributes":{"label":{"value":"NVIDIA Corp"},"renderers":[{"id":"1272"}]},"id":"1300","type":"LegendItem"},{"attributes":{"line_alpha":0.2,"line_color":"#e5ae38","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1097","type":"Line"},{"attributes":{},"id":"1093","type":"Selection"},{"attributes":{},"id":"1050","type":"Selection"},{"attributes":{"label":{"value":"Tesla, Inc."},"renderers":[{"id":"1098"}]},"id":"1114","type":"LegendItem"},{"attributes":{"coordinates":null,"data_source":{"id":"1142"},"glyph":{"id":"1145"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1147"},"nonselection_glyph":{"id":"1146"},"selection_glyph":{"id":"1169"},"view":{"id":"1149"}},"id":"1148","type":"GlyphRenderer"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1054","type":"Line"},{"attributes":{"line_color":"#1f77b4","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1269","type":"Line"},{"attributes":{"line_alpha":0.1,"line_color":"#8b8b8b","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1146","type":"Line"},{"attributes":{},"id":"1019","type":"CategoricalScale"},{"attributes":{},"id":"1165","type":"UnionRenderers"},{"attributes":{"line_alpha":0.2,"line_color":"#8b8b8b","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1147","type":"Line"},{"attributes":{"line_color":"#e5ae38","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1095","type":"Line"},{"attributes":{},"id":"1233","type":"Selection"},{"attributes":{"source":{"id":"1200"}},"id":"1207","type":"CDSView"},{"attributes":{},"id":"1143","type":"Selection"},{"attributes":{"label":{"value":"Republic Services, Inc."},"renderers":[{"id":"1148"}]},"id":"1168","type":"LegendItem"},{"attributes":{"source":{"id":"1232"}},"id":"1239","type":"CDSView"},{"attributes":{"callback":null,"renderers":[{"id":"1055"},{"id":"1076"},{"id":"1098"},{"id":"1122"},{"id":"1148"},{"id":"1176"},{"id":"1206"},{"id":"1238"},{"id":"1272"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"1005","type":"HoverTool"},{"attributes":{"label":{"value":"Apple Inc."},"renderers":[{"id":"1055"}]},"id":"1068","type":"LegendItem"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1069","type":"Line"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc","Ecolab Inc"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f2oz6yN6S5g/ei76Y5wWmD+mxkn/aOCWPwcTgSs2z5Y/4oIfiIwTlz/y+ulDR/uWP9EctDVpL5U/EpdHRh1llT8x10jFoYCVP7PE+wEjNY0/OAjIlS+iij+4VtBMrfGKPwt9mL9CWos/N3xccEWHiT965TJ5D7WJPx9Mv0CqcYs/SQToECZwiz9bfofxrj+LP/L2PDGkyIs/r8Cq7d65iz+ay35C/8CKP8wugSHlgoo/yux6nVnGiT9Dk5vIGfCHP3rDJgaNj4c/Uc23I9TVhj9bqMYJeROHP7x7Be1NQ4c/6oQaqzDwhj84nvoXWp+HP5Q415IVnIc/cuIa4A2ggz+w7DU0uECDP5TiRt0uDYM/5EXC2ayugT+zavcJTnGBP882GZWJJIA/jbzMBpjygD/fRkwFqGuDP/Gg5c/YIoM/0nedyBUYgz9VB7aF7guHP9ty3z9MUIc/FkdfIIE3hz9B1wEsPDmHPzGYJX8ZXoc/juNfD8ERhz8n7Zc4wD6HP5EgRK12J4c/EDHaA31xhj9sNC+ZDDmFP5MvCMK7h4U/d0Lz+3DbhT9/K1fg4dqFP6tNJWXqvoU/TI7Wi/THhj8+T+IcXUyLPw83X0NayYo/sHi7Qn5tij+WIbhIw5KIP5/ynj7JTog/WOHSm8QxiD894C1zFg2GPxZgu2LLlog/j/3FHjR0iD/EmEZQEjmIP+B//sn7r4g/9DkZOOgEiT98GaxwHYSIP85z0RO7p4g/6gIENDY9iT91/sx90KiJP+vDq/us1ok/d9aqQwOWiT8rIdmFGPiJP45U+3Zv+Ik/srAd581DiT+shTeuQ6OEPzVUcEjrC4U/Kzj9oTCfhD+raye7QaCEP+mu4CYJnYQ/R/jTHdmYhD8h9VevcMuCP1TgygwIn4I/cDu5koLUgj/OzRMvFsCCP1s6epx5QII/dvM4oeb2gT+ZofQclHOCP6S5viSOVII/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"1201"},"selection_policy":{"id":"1227"}},"id":"1200","type":"ColumnDataSource"},{"attributes":{"line_color":"#8b8b8b","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1145","type":"Line"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"1003","type":"FactorRange"},{"attributes":{},"id":"1227","type":"UnionRenderers"},{"attributes":{"coordinates":null,"data_source":{"id":"1200"},"glyph":{"id":"1203"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1205"},"nonselection_glyph":{"id":"1204"},"selection_glyph":{"id":"1231"},"view":{"id":"1207"}},"id":"1206","type":"GlyphRenderer"},{"attributes":{"source":{"id":"1092"}},"id":"1099","type":"CDSView"},{"attributes":{"line_alpha":0.2,"line_color":"#d62728","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"1237","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"1070"},"glyph":{"id":"1073"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1075"},"nonselection_glyph":{"id":"1074"},"selection_glyph":{"id":"1091"},"view":{"id":"1077"}},"id":"1076","type":"GlyphRenderer"}],"root_ids":["1002"]},"title":"Bokeh Application","version":"2.4.3"}};
    var render_items = [{"docid":"9cfd452b-671a-4a4a-9aa0-267e0b39091c","root_ids":["1002"],"roots":{"1002":"bf012fc5-70b8-422b-a312-13f2782c0ec4"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
    for (const render_item of render_items) {
      for (const root_id of render_item.root_ids) {
	const id_el = document.getElementById(root_id)
	if (id_el.children.length && (id_el.children[0].className === 'bk-root')) {
	  const root_el = id_el.children[0]
	  root_el.id = root_el.id + '-rendered'
	}
      }
    }
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>


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






<div id='2017'>
  <div class="bk-root" id="5a78dc29-8faf-48bb-82db-6df5855468e0" data-root-id="2017"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"c49a12fd-411b-4198-abd8-7ca86337d08c":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"GridStack1","overrides":[],"properties":[{"default":"warn","kind":null,"name":"mode"},{"default":null,"kind":null,"name":"ncols"},{"default":null,"kind":null,"name":"nrows"},{"default":true,"kind":null,"name":"allow_resize"},{"default":true,"kind":null,"name":"allow_drag"},{"default":[],"kind":null,"name":"state"}]},{"extends":null,"module":null,"name":"click1","overrides":[],"properties":[{"default":"","kind":null,"name":"terminal_output"},{"default":"","kind":null,"name":"debug_name"},{"default":0,"kind":null,"name":"clears"}]},{"extends":null,"module":null,"name":"NotificationAreaBase1","overrides":[],"properties":[{"default":"bottom-right","kind":null,"name":"position"},{"default":0,"kind":null,"name":"_clear"}]},{"extends":null,"module":null,"name":"NotificationArea1","overrides":[],"properties":[{"default":[],"kind":null,"name":"notifications"},{"default":"bottom-right","kind":null,"name":"position"},{"default":0,"kind":null,"name":"_clear"},{"default":[{"background":"#ffc107","icon":{"className":"fas fa-exclamation-triangle","color":"white","tagName":"i"},"type":"warning"},{"background":"#007bff","icon":{"className":"fas fa-info-circle","color":"white","tagName":"i"},"type":"info"}],"kind":null,"name":"types"}]},{"extends":null,"module":null,"name":"Notification","overrides":[],"properties":[{"default":null,"kind":null,"name":"background"},{"default":3000,"kind":null,"name":"duration"},{"default":null,"kind":null,"name":"icon"},{"default":"","kind":null,"name":"message"},{"default":null,"kind":null,"name":"notification_type"},{"default":false,"kind":null,"name":"_destroyed"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{},"id":"2053","type":"AllLabels"},{"attributes":{"label":{"value":"Rolling Std"},"renderers":[{"id":"2170"}]},"id":"2184","type":"LegendItem"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2320","type":"Line"},{"attributes":{},"id":"2040","type":"WheelZoomTool"},{"attributes":{"coordinates":null,"data_source":{"id":"2164"},"glyph":{"id":"2167"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2169"},"nonselection_glyph":{"id":"2168"},"selection_glyph":{"id":"2185"},"view":{"id":"2171"}},"id":"2170","type":"GlyphRenderer"},{"attributes":{},"id":"2572","type":"AllLabels"},{"attributes":{},"id":"2039","type":"PanTool"},{"attributes":{},"id":"2571","type":"BasicTickFormatter"},{"attributes":{"source":{"id":"2680"}},"id":"2687","type":"CDSView"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2592"},{"id":"2614"}],"location":[0,0],"title":"Variable"},"id":"2591","type":"Legend"},{"attributes":{"coordinates":null,"data_source":{"id":"2680"},"glyph":{"id":"2683"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2685"},"nonselection_glyph":{"id":"2684"},"selection_glyph":{"id":"2701"},"view":{"id":"2687"}},"id":"2686","type":"GlyphRenderer"},{"attributes":{"source":{"id":"2164"}},"id":"2171","type":"CDSView"},{"attributes":{},"id":"2038","type":"SaveTool"},{"attributes":{"label":{"value":"Rolling Std"},"renderers":[{"id":"2686"}]},"id":"2700","type":"LegendItem"},{"attributes":{"children":[[{"id":"2022"},0,0],[{"id":"2108"},0,1],[{"id":"2194"},1,0],[{"id":"2280"},1,1],[{"id":"2366"},2,0],[{"id":"2452"},2,1],[{"id":"2538"},3,0],[{"id":"2624"},3,1],[{"id":"2710"},4,0]]},"id":"2918","type":"GridBox"},{"attributes":{"coordinates":null,"data_source":{"id":"2422"},"glyph":{"id":"2425"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2427"},"nonselection_glyph":{"id":"2426"},"selection_glyph":{"id":"2443"},"view":{"id":"2429"}},"id":"2428","type":"GlyphRenderer"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2425","type":"Line"},{"attributes":{"overlay":{"id":"2043"}},"id":"2041","type":"BoxZoomTool"},{"attributes":{},"id":"2042","type":"ResetTool"},{"attributes":{},"id":"2211","type":"PanTool"},{"attributes":{"label":{"value":"Rolling Std"},"renderers":[{"id":"2428"}]},"id":"2442","type":"LegendItem"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2169","type":"Line"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2577","type":"Line"},{"attributes":{},"id":"2316","type":"Selection"},{"attributes":{"below":[{"id":"2031"}],"center":[{"id":"2033"},{"id":"2037"}],"height":300,"left":[{"id":"2034"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"2063"},{"id":"2084"}],"right":[{"id":"2075"}],"sizing_mode":"fixed","title":{"id":"2023"},"toolbar":{"id":"2044"},"toolbar_location":null,"width":700,"x_range":{"id":"2018"},"x_scale":{"id":"2027"},"y_range":{"id":"2019"},"y_scale":{"id":"2029"}},"id":"2022","subtype":"Figure","type":"Plot"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2685","type":"Line"},{"attributes":{},"id":"2457","type":"CategoricalScale"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2043","type":"BoxAnnotation"},{"attributes":{"source":{"id":"2422"}},"id":"2429","type":"CDSView"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2426","type":"Line"},{"attributes":{"active_drag":{"id":"2039"},"active_scroll":{"id":"2040"},"tools":[{"id":"2020"},{"id":"2038"},{"id":"2039"},{"id":"2040"},{"id":"2041"},{"id":"2042"}]},"id":"2044","type":"Toolbar"},{"attributes":{},"id":"2314","type":"AllLabels"},{"attributes":{},"id":"2181","type":"UnionRenderers"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2168","type":"Line"},{"attributes":{},"id":"2313","type":"BasicTickFormatter"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2578","type":"Line"},{"attributes":{},"id":"2697","type":"UnionRenderers"},{"attributes":{},"id":"2204","type":"CategoricalTicker"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2684","type":"Line"},{"attributes":{},"id":"2311","type":"AllLabels"},{"attributes":{},"id":"2439","type":"UnionRenderers"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2427","type":"Line"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2318","type":"Line"},{"attributes":{"label":{"value":"Rolling Std"},"renderers":[{"id":"2342"}]},"id":"2356","type":"LegendItem"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"2190","type":"FactorRange"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2062","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"2057"},"glyph":{"id":"2060"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2062"},"nonselection_glyph":{"id":"2061"},"selection_glyph":{"id":"2077"},"view":{"id":"2064"}},"id":"2063","type":"GlyphRenderer"},{"attributes":{},"id":"2052","type":"CategoricalTickFormatter"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns"],"value":{"__ndarray__":"4DTRVoh2kr8ANmoOyQxyPwDQDK7E9Ho/AJ6qUwNzaj8ADKGJ/iJRP4B2wCciyoA/AIjG+tBYjz/AZtCy4zmFv8AeScRwLIO/AEY/8xwqfz8A2X8R1AKhP4AfrESXCY8/QCQQENtzib/Alf6c7fiAv4C7Xf/3enC/AP16IxlSfD8AQBcH+cBmvwAkk9csSGS/ACh0XmeyY78A6jLfxz1wPwCUFBJwG1i/AD7JDKIhbz8ASa1J3fSMvwCend5vZWg/ANKbN27DZT+ArfCX8kSEv4CAKGbywoU/ACNhkxTznD8ApaCxTeN3PwBNPoXYv3c/QJKVFMbFlb8AU/kE4kqAP8D/7OFrB4K/QPH+PSHggb+ALplvvOKHv8B8Fpn5r5c/ADgS3/znar8A2SwiQr15P4COipcJvIk/gAWSWqYcjT8ADhW3JHpmPwBtXN+tkHs/AABSVcNBE7+A+byQ9Md6vwDqrCbHeGw/AJfbRF+jdr8AwELOsbVKP4DpkVc6HY0/AObDR4CYgD8A96sERFt1vwCzK3+YYYM/AHYeqoNsfr8AjEO0mc5HvwCxTS3Lhnc/AKB7qPCUYD8AlDOLo/VsPwA6/76x6Vm/wCXgEnAJiL8Agno4HENqP0B+qr7pxKk/AKBKPGdkZD8A8JwC1hQivwDveCLnO34/AGjJveKuWL8AwPTcG3k2PwCAJ8zo+CG/AN7uRTImgD8AJsVYBad+PwDA/sJwjSo/APCJs734Pj8AWcz5rAFlvwAIdnWqpn2/wCzIP3y/gL8A7rZw7Ft3vwBE5aD/gmU/AEFqopfmf79AdC1vy+yBvwDAo1RXQlm/ANmh8sgidr8Aw3A6pzVyvwB8S5DktXo/AI39/mhYZb8A9775/c9/PwBTKQC3ZYk/gGDrdQCLdb8AvwLhwe9nv4CsbxuAAYW/ANTSrsH0hj8AkPJdbvtPvwBAAK1O3mo/ABtrmZgWc78ASih+XlVqPwBoRA0OP3w/gJ0l7Oa3gD8ACBwexnpPPwB2bNSohGs/gB9dR4yhgT+AfI/7fz5+v4DLuz/AVIE/QGxYkBTFkD9AMQr+WNGCv8CiZWwDGZI/AA64g0plhz8At46Rj6qHvwCUhYHdDVu/ADzDi8xZXj8AonpG8cB9vwAYVkAuhG4/gEWKyj47fL8AelbqU1ByvwA4lm1NEGc/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2402"},"selection_policy":{"id":"2416"}},"id":"2401","type":"ColumnDataSource"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2076"},{"id":"2098"}],"location":[0,0],"title":"Variable"},"id":"2075","type":"Legend"},{"attributes":{},"id":"2330","type":"UnionRenderers"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2185","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"2078"},"glyph":{"id":"2081"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2083"},"nonselection_glyph":{"id":"2082"},"selection_glyph":{"id":"2099"},"view":{"id":"2085"}},"id":"2084","type":"GlyphRenderer"},{"attributes":{"axis":{"id":"2203"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2205","type":"Grid"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2701","type":"Line"},{"attributes":{"below":[{"id":"2719"}],"center":[{"id":"2721"},{"id":"2725"}],"height":300,"left":[{"id":"2722"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"2751"},{"id":"2772"}],"right":[{"id":"2763"}],"sizing_mode":"fixed","title":{"id":"2711"},"toolbar":{"id":"2732"},"toolbar_location":null,"width":700,"x_range":{"id":"2706"},"x_scale":{"id":"2715"},"y_range":{"id":"2707"},"y_scale":{"id":"2717"}},"id":"2710","subtype":"Figure","type":"Plot"},{"attributes":{"source":{"id":"2057"}},"id":"2064","type":"CDSView"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"2224"},"group":null,"major_label_policy":{"id":"2225"},"ticker":{"id":"2204"}},"id":"2203","type":"CategoricalAxis"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2443","type":"Line"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2082","type":"Line"},{"attributes":{"coordinates":null,"group":null,"text":"NVIDIA Corp","text_color":"black","text_font_size":"12pt"},"id":"2711","type":"Title"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2077","type":"Line"},{"attributes":{"below":[{"id":"2203"}],"center":[{"id":"2205"},{"id":"2209"}],"height":300,"left":[{"id":"2206"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"2235"},{"id":"2256"}],"right":[{"id":"2247"}],"sizing_mode":"fixed","title":{"id":"2195"},"toolbar":{"id":"2216"},"toolbar_location":null,"width":700,"x_range":{"id":"2190"},"x_scale":{"id":"2199"},"y_range":{"id":"2191"},"y_scale":{"id":"2201"}},"id":"2194","subtype":"Figure","type":"Plot"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"2482"},"group":null,"major_label_policy":{"id":"2483"},"ticker":{"id":"2462"}},"id":"2461","type":"CategoricalAxis"},{"attributes":{"callback":null,"renderers":[{"id":"2235"},{"id":"2256"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"2192","type":"HoverTool"},{"attributes":{"axis":{"id":"2719"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2721","type":"Grid"},{"attributes":{"active_drag":{"id":"2297"},"active_scroll":{"id":"2298"},"tools":[{"id":"2278"},{"id":"2296"},{"id":"2297"},{"id":"2298"},{"id":"2299"},{"id":"2300"}]},"id":"2302","type":"Toolbar"},{"attributes":{"end":0.05428749638042012,"reset_end":0.05428749638042012,"reset_start":-0.03404043834977104,"start":-0.03404043834977104,"tags":[[["value","value",null]]]},"id":"2707","type":"Range1d"},{"attributes":{},"id":"2469","type":"PanTool"},{"attributes":{},"id":"2199","type":"CategoricalScale"},{"attributes":{"coordinates":null,"group":null,"text":"Canadian National Railway","text_color":"black","text_font_size":"12pt"},"id":"2453","type":"Title"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2061","type":"Line"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2319","type":"Line"},{"attributes":{},"id":"2058","type":"Selection"},{"attributes":{},"id":"2574","type":"Selection"},{"attributes":{"callback":null,"renderers":[{"id":"2751"},{"id":"2772"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"2708","type":"HoverTool"},{"attributes":{},"id":"2483","type":"AllLabels"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"2740"},"group":null,"major_label_policy":{"id":"2741"},"ticker":{"id":"2720"}},"id":"2719","type":"CategoricalAxis"},{"attributes":{"below":[{"id":"2461"}],"center":[{"id":"2463"},{"id":"2467"}],"height":300,"left":[{"id":"2464"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"2493"},{"id":"2514"}],"right":[{"id":"2505"}],"sizing_mode":"fixed","title":{"id":"2453"},"toolbar":{"id":"2474"},"toolbar_location":null,"width":700,"x_range":{"id":"2448"},"x_scale":{"id":"2457"},"y_range":{"id":"2449"},"y_scale":{"id":"2459"}},"id":"2452","subtype":"Figure","type":"Plot"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2334"},{"id":"2356"}],"location":[0,0],"title":"Variable"},"id":"2333","type":"Legend"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"2706","type":"FactorRange"},{"attributes":{"source":{"id":"2315"}},"id":"2322","type":"CDSView"},{"attributes":{"coordinates":null,"group":null,"text":"Tesla, Inc.","text_color":"black","text_font_size":"12pt"},"id":"2195","type":"Title"},{"attributes":{"coordinates":null,"data_source":{"id":"2315"},"glyph":{"id":"2318"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2320"},"nonselection_glyph":{"id":"2319"},"selection_glyph":{"id":"2335"},"view":{"id":"2322"}},"id":"2321","type":"GlyphRenderer"},{"attributes":{},"id":"2201","type":"LinearScale"},{"attributes":{},"id":"2225","type":"AllLabels"},{"attributes":{"axis_label":"Value","coordinates":null,"formatter":{"id":"2227"},"group":null,"major_label_policy":{"id":"2228"},"ticker":{"id":"2207"}},"id":"2206","type":"LinearAxis"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2060","type":"Line"},{"attributes":{},"id":"2588","type":"UnionRenderers"},{"attributes":{"axis":{"id":"2206"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2209","type":"Grid"},{"attributes":{},"id":"2715","type":"CategoricalScale"},{"attributes":{},"id":"2072","type":"UnionRenderers"},{"attributes":{},"id":"2717","type":"LinearScale"},{"attributes":{},"id":"2207","type":"BasicTicker"},{"attributes":{},"id":"2462","type":"CategoricalTicker"},{"attributes":{},"id":"2212","type":"WheelZoomTool"},{"attributes":{"callback":null,"renderers":[{"id":"2493"},{"id":"2514"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"2450","type":"HoverTool"},{"attributes":{"axis":{"id":"2461"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2463","type":"Grid"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2678"},{"id":"2700"}],"location":[0,0],"title":"Variable"},"id":"2677","type":"Legend"},{"attributes":{},"id":"2720","type":"CategoricalTicker"},{"attributes":{},"id":"2459","type":"LinearScale"},{"attributes":{},"id":"2741","type":"AllLabels"},{"attributes":{},"id":"2210","type":"SaveTool"},{"attributes":{"axis_label":"Value","coordinates":null,"formatter":{"id":"2743"},"group":null,"major_label_policy":{"id":"2744"},"ticker":{"id":"2723"}},"id":"2722","type":"LinearAxis"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2576","type":"Line"},{"attributes":{"axis":{"id":"2722"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2725","type":"Grid"},{"attributes":{"source":{"id":"2573"}},"id":"2580","type":"CDSView"},{"attributes":{"overlay":{"id":"2215"}},"id":"2213","type":"BoxZoomTool"},{"attributes":{},"id":"2723","type":"BasicTicker"},{"attributes":{"axis_label":"Value","coordinates":null,"formatter":{"id":"2485"},"group":null,"major_label_policy":{"id":"2486"},"ticker":{"id":"2465"}},"id":"2464","type":"LinearAxis"},{"attributes":{},"id":"2214","type":"ResetTool"},{"attributes":{},"id":"2728","type":"WheelZoomTool"},{"attributes":{"coordinates":null,"data_source":{"id":"2573"},"glyph":{"id":"2576"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2578"},"nonselection_glyph":{"id":"2577"},"selection_glyph":{"id":"2593"},"view":{"id":"2580"}},"id":"2579","type":"GlyphRenderer"},{"attributes":{},"id":"2727","type":"PanTool"},{"attributes":{"axis":{"id":"2464"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2467","type":"Grid"},{"attributes":{},"id":"2465","type":"BasicTicker"},{"attributes":{},"id":"2726","type":"SaveTool"},{"attributes":{},"id":"2470","type":"WheelZoomTool"},{"attributes":{"overlay":{"id":"2731"}},"id":"2729","type":"BoxZoomTool"},{"attributes":{},"id":"2730","type":"ResetTool"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2215","type":"BoxAnnotation"},{"attributes":{},"id":"2468","type":"SaveTool"},{"attributes":{"overlay":{"id":"2473"}},"id":"2471","type":"BoxZoomTool"},{"attributes":{"active_drag":{"id":"2211"},"active_scroll":{"id":"2212"},"tools":[{"id":"2192"},{"id":"2210"},{"id":"2211"},{"id":"2212"},{"id":"2213"},{"id":"2214"}]},"id":"2216","type":"Toolbar"},{"attributes":{},"id":"2472","type":"ResetTool"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2731","type":"BoxAnnotation"},{"attributes":{"active_drag":{"id":"2727"},"active_scroll":{"id":"2728"},"tools":[{"id":"2708"},{"id":"2726"},{"id":"2727"},{"id":"2728"},{"id":"2729"},{"id":"2730"}]},"id":"2732","type":"Toolbar"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2473","type":"BoxAnnotation"},{"attributes":{"active_drag":{"id":"2469"},"active_scroll":{"id":"2470"},"tools":[{"id":"2450"},{"id":"2468"},{"id":"2469"},{"id":"2470"},{"id":"2471"},{"id":"2472"}]},"id":"2474","type":"Toolbar"},{"attributes":{"coordinates":null,"data_source":{"id":"2250"},"glyph":{"id":"2253"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2255"},"nonselection_glyph":{"id":"2254"},"selection_glyph":{"id":"2271"},"view":{"id":"2257"}},"id":"2256","type":"GlyphRenderer"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2234","type":"Line"},{"attributes":{},"id":"2055","type":"BasicTickFormatter"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2771","type":"Line"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns"],"value":{"__ndarray__":"IGu7R9v5oj/Aws+4TPyYP2DPGpPtW5K/wOJ1seK0kz+giYX1kBOSv4Af+VhET3y/AFgnEDhoTj8AUkpgEEKTPwB25XIfTnG/gBsP/WF5jD+AgnV+A1yFvwAf+CpE6X6/AJHRiPFRm78AlnsaYblnPwBwWLjX9Go/ALtB5/5vkr8ASmkyGeSAPwCt4bKUPmy/wH0nJv8sjb8A5eq5relwP8AcaC5V96E/gCrKl/v7kj9AacxDWrCNvwCeZ1cfKIE/QGK2M5CLjr/AgUNvdWyMv4Cpz3kzK4s/gFp4ssrajD8AOqNte3llP0AlQkWHJJM/AGCc0ORWdr8AIGD7AbaPP4BohtIkdog/gKgXeNakgr8AcCtmE4x8PwAigyd/AoE/QOeUrzktib8Aj0lzok1wv0BO9sABRJQ/gMaLjihBhD9AZfuqCwWQPwDoHInAi38/ADUd2BSfar+AdgNgVB+HvwBCC1rQgnY/oGF1WQtbkL8AFuOx7xd/vwAW906y1HG/oK4vyQl2oT8Ap1YVdVFhvwCAt2oRvB8/AAuF/zy9fj8AzCf8tYp8PwD5WsEx8ne/gINsqAwIhL8AdNegQMdePwCLnr0CU4O/AIAFMcABEL/AiU50ohOdPwCgkfRv434/ALxUMmdhQb+AI1UIKFx5vwDz0qB2fXq/wBFq9npNhL8AsZJMywaoPwBoF/wsbjq/wJ99VcVrhL8AsQYXj1eFPwAUuhqZ71E/gJgirEPRe78AUNRYPLxnvwAAAAAAAAAAAJxCFW2EbT+AQ3vR+fyLPwB42E1Vl0Q/AGLVGu9ydr+AV7nllAmPvwDUecdyvVo/ADDTDVtpez+A1ap4AuOMPwAP7d2o1IU/ABAFsU57Mr/Aq/pEOGiQPwAosgNcj3M/ANcLte0Cf78ATQcA5eBgv4CvHCgIxX+/AGoUgSesjz8AvPJnfLFhP0Bhdh+fA5A/AJcRExVlZb8A8K0b5plsPwBdfXBX74Y/AEAY7owAeL8AkMN7a+U/PwCFBT0OP3e/wA4BOwTskD8AHAAqawlcv4C5vpHt736/ANTClE7Xjj8AI5E3Jet5PwD4AWZXb10/QLU9ei2olz+Apljn2+J/vwBWPHyODHi/ACAWUj2NZD8A1fdzayF4v8DW/xycO4a/ANyMDhAFZ78AsiSBAmeCPwCxSn+knnA/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2746"},"selection_policy":{"id":"2760"}},"id":"2745","type":"ColumnDataSource"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2233","type":"Line"},{"attributes":{},"id":"2353","type":"UnionRenderers"},{"attributes":{"label":{"value":"Daily Returns"},"renderers":[{"id":"2321"}]},"id":"2334","type":"LegendItem"},{"attributes":{},"id":"2056","type":"AllLabels"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2750","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"2594"},"glyph":{"id":"2597"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2599"},"nonselection_glyph":{"id":"2598"},"selection_glyph":{"id":"2615"},"view":{"id":"2601"}},"id":"2600","type":"GlyphRenderer"},{"attributes":{},"id":"2224","type":"CategoricalTickFormatter"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2335","type":"Line"},{"attributes":{"label":{"value":"Daily Returns"},"renderers":[{"id":"2579"}]},"id":"2592","type":"LegendItem"},{"attributes":{"coordinates":null,"data_source":{"id":"2508"},"glyph":{"id":"2511"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2513"},"nonselection_glyph":{"id":"2512"},"selection_glyph":{"id":"2529"},"view":{"id":"2515"}},"id":"2514","type":"GlyphRenderer"},{"attributes":{"source":{"id":"2229"}},"id":"2236","type":"CDSView"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2598","type":"Line"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2749","type":"Line"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2764"},{"id":"2786"}],"location":[0,0],"title":"Variable"},"id":"2763","type":"Legend"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2593","type":"Line"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f2/aJ8lTHYQ/vxVtRzJ0gD+8QbJQIs2APx5+bS+js4A/6vIYy8HRgD/8n8K4xjCBP1VPlw0vqIE/YWQMA8pLgz8+N3hNXvaCP5zUaMdgYYI/qv+gNqPMgz+LXTZ/WGOEPzvdjvtb4YI/NfXp53rugj/j2HHuwFWDP2KfS81/EYY/eJjDRzZFhj/nOZ7tVXmGP5IHbatXs4Y/ipjsPj0niT/v7mbihT+JP3PxYwRoMYk/fFKJ8r9ZiT9d44rai+yIP1h3L/au7Ig/qg0iB9eOiT/K8NzL3emIP7ws29NZC4k/uqAiLzvxhz8Zj8acOKOIP0mcCLFzqYg/beaKNVwahz+9I1mHo/GGP0AYRB9RzYY/74pElt1AhT+M9QhHV1qEPzhiadyLD4I/tJcy5CDlgj+1rbAuhOWDP3u2dZgBSIY/0aUTxjS7gj+8XrNHnE6CPxncQ/cvSYI//VZKg48+gj948KBlMRSCP4DYYJglB4I/WBtZACVogT9Z/DvhMXWBP/Uyh7/bjYA/q8ei55qCgD/dkzYZkvx/P1YK/FiuN4A/an9sMe59gD9Mcler7SiBP2RvXZ+OG4E/+9qfdipJgT/mCK2mFaqBP74GNUMQqoE/AkVgEbL/gD8fHrHDbjCAP/iYLj1fmHQ/he7BRJTrcz+3LEBScdR0P0gKSI/rLXg/paOPiKPydz9oNQ7DNq15P5/laoBLnHk/e9zypQJqej/Bbgj4Elt5PwzmJ5R7kHk/pnQ/A+l/eT/W4UGiuK15PyvF07ii2Xs/mGNzfWHuez8mtWSub196P2i7wZNQhXo/xKoMDG/aej+LDnQg6nR6P6AFUsqy8Xo/UV3Dhc0VfD/rm+lZM/98P8oVZBH1d34/SHEE2gGbgT9zIRHLysmCP0fJwEDrLoI/uoj1oAYggj/Sxw9ie3WBP4nBE7ldBoE/mjYJX39OgT8pP+iEu7GBP9mViRJ79YE/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2337"},"selection_policy":{"id":"2353"}},"id":"2336","type":"ColumnDataSource"},{"attributes":{"coordinates":null,"data_source":{"id":"2229"},"glyph":{"id":"2232"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2234"},"nonselection_glyph":{"id":"2233"},"selection_glyph":{"id":"2249"},"view":{"id":"2236"}},"id":"2235","type":"GlyphRenderer"},{"attributes":{},"id":"2740","type":"CategoricalTickFormatter"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2492","type":"Line"},{"attributes":{"source":{"id":"2745"}},"id":"2752","type":"CDSView"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2491","type":"Line"},{"attributes":{"below":[{"id":"2633"}],"center":[{"id":"2635"},{"id":"2639"}],"height":300,"left":[{"id":"2636"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"2665"},{"id":"2686"}],"right":[{"id":"2677"}],"sizing_mode":"fixed","title":{"id":"2625"},"toolbar":{"id":"2646"},"toolbar_location":null,"width":700,"x_range":{"id":"2620"},"x_scale":{"id":"2629"},"y_range":{"id":"2621"},"y_scale":{"id":"2631"}},"id":"2624","subtype":"Figure","type":"Plot"},{"attributes":{"coordinates":null,"data_source":{"id":"2745"},"glyph":{"id":"2748"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2750"},"nonselection_glyph":{"id":"2749"},"selection_glyph":{"id":"2765"},"view":{"id":"2752"}},"id":"2751","type":"GlyphRenderer"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2597","type":"Line"},{"attributes":{"source":{"id":"2336"}},"id":"2343","type":"CDSView"},{"attributes":{},"id":"2482","type":"CategoricalTickFormatter"},{"attributes":{},"id":"2230","type":"Selection"},{"attributes":{"source":{"id":"2487"}},"id":"2494","type":"CDSView"},{"attributes":{"coordinates":null,"data_source":{"id":"2336"},"glyph":{"id":"2339"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2341"},"nonselection_glyph":{"id":"2340"},"selection_glyph":{"id":"2357"},"view":{"id":"2343"}},"id":"2342","type":"GlyphRenderer"},{"attributes":{"coordinates":null,"data_source":{"id":"2487"},"glyph":{"id":"2490"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2492"},"nonselection_glyph":{"id":"2491"},"selection_glyph":{"id":"2507"},"view":{"id":"2494"}},"id":"2493","type":"GlyphRenderer"},{"attributes":{"label":{"value":"Rolling Std"},"renderers":[{"id":"2600"}]},"id":"2614","type":"LegendItem"},{"attributes":{},"id":"2746","type":"Selection"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2340","type":"Line"},{"attributes":{"coordinates":null,"group":null,"text":"Waste Connections, Inc.","text_color":"black","text_font_size":"12pt"},"id":"2625","type":"Title"},{"attributes":{"coordinates":null,"data_source":{"id":"2766"},"glyph":{"id":"2769"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2771"},"nonselection_glyph":{"id":"2770"},"selection_glyph":{"id":"2787"},"view":{"id":"2773"}},"id":"2772","type":"GlyphRenderer"},{"attributes":{"source":{"id":"2594"}},"id":"2601","type":"CDSView"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2341","type":"Line"},{"attributes":{},"id":"2488","type":"Selection"},{"attributes":{},"id":"2337","type":"Selection"},{"attributes":{"toolbar":{"id":"2919"},"toolbar_location":"above"},"id":"2920","type":"ToolbarBox"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2162"},{"id":"2184"}],"location":[0,0],"title":"Variable"},"id":"2161","type":"Legend"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2232","type":"Line"},{"attributes":{},"id":"2611","type":"UnionRenderers"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2339","type":"Line"},{"attributes":{},"id":"2595","type":"Selection"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2599","type":"Line"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2748","type":"Line"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2490","type":"Line"},{"attributes":{},"id":"2760","type":"UnionRenderers"},{"attributes":{"toolbars":[{"id":"2044"},{"id":"2130"},{"id":"2216"},{"id":"2302"},{"id":"2388"},{"id":"2474"},{"id":"2560"},{"id":"2646"},{"id":"2732"}],"tools":[{"id":"2020"},{"id":"2038"},{"id":"2039"},{"id":"2040"},{"id":"2041"},{"id":"2042"},{"id":"2106"},{"id":"2124"},{"id":"2125"},{"id":"2126"},{"id":"2127"},{"id":"2128"},{"id":"2192"},{"id":"2210"},{"id":"2211"},{"id":"2212"},{"id":"2213"},{"id":"2214"},{"id":"2278"},{"id":"2296"},{"id":"2297"},{"id":"2298"},{"id":"2299"},{"id":"2300"},{"id":"2364"},{"id":"2382"},{"id":"2383"},{"id":"2384"},{"id":"2385"},{"id":"2386"},{"id":"2450"},{"id":"2468"},{"id":"2469"},{"id":"2470"},{"id":"2471"},{"id":"2472"},{"id":"2536"},{"id":"2554"},{"id":"2555"},{"id":"2556"},{"id":"2557"},{"id":"2558"},{"id":"2622"},{"id":"2640"},{"id":"2641"},{"id":"2642"},{"id":"2643"},{"id":"2644"},{"id":"2708"},{"id":"2726"},{"id":"2727"},{"id":"2728"},{"id":"2729"},{"id":"2730"}]},"id":"2919","type":"ProxyToolbar"},{"attributes":{"label":{"value":"Daily Returns"},"renderers":[{"id":"2063"}]},"id":"2076","type":"LegendItem"},{"attributes":{},"id":"2502","type":"UnionRenderers"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2081","type":"Line"},{"attributes":{},"id":"2244","type":"UnionRenderers"},{"attributes":{"label":{"value":"Rolling Std"},"renderers":[{"id":"2084"}]},"id":"2098","type":"LegendItem"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f3DTMzMYWZU/QA7pGTp5kj8/vdhlr92RP96FMSqK0pE/B8yELkOWjT8fbPOm2xOOP8SdCUFsuo8/FtGVLrAckT820pSDrAuQPw//F7nShpI/U6maZmt6kj9eCiIA52iSP4hXgeNg9JE/iEcNQ8oxkT/LfAILtW+RP/oDT713YpE/JyXGJ6/lkD8OKMhuhwWRP2oM2F/QN5E/E/bv2AW5kD/OW5vEZ5GQP+LQfI2djZA/MUBRQfqakD9VXcfTFZKQPxBWvhZZD5E/KgCGN5QlkT8UPkoalL6RP2t/yWJ8ZpE/QvIbRxkgkT+yeKAwv02RPz8t8gk0K44/sMnw2hLgjT9pFnUZMACLPx81gvCjZYs/g/qrngk3iz+v/dVtZuCKP5BHBKzOYow/p506HsBIlT8bcO5u0yGWPyM9UsgA5pU/kkRQxX3slT/QWRcJZMeVP3mRKrTGxZU/s8ucqJHClT9PbGOx/b+VP8hGxgrUSpU/DOl1cIs9lT9DOMZo1JKUPyYLio/LvZQ/YJtqNwBglD9+ip0qpQiUPy/Q7R7gApQ/ecunxCD8kz8muY2JPRaUPwNXpzV35pM//8ry7BLbkz+PJOMZpRyUP6fY8FVDLJM/Gw2gHhmRiz+VEmW6b7WJPxQeWDwo+Yk/PxEX4jc6ij94Qb7zbXiKP31WuNtNTIo/OWeDNfpTij/bBfobUhuKPwY5Y3aHCo4/6VKPAGTbjT/FLAcKvRmNPwFu2pjRVI0/vfJHBFcxjT+pOym8oUONP3gxN5vYw48/v12RV3SckD9+2Lop4dWQPwDE4DwANpE/07w8d5CHkT/v/jZymUORP6owzFk75pE/SOgu4jJAkD/YcCkmJiePPwte42xVE48/9xv9wfiFjz/zGqe58lmPP18aZwfeJY8/9FtowtNjjz8SwaNAlbuPP/yLPVgKPI0/fUJ1fDIpjT+VZP+72LWNP55emDUIyI0/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2165"},"selection_policy":{"id":"2181"}},"id":"2164","type":"ColumnDataSource"},{"attributes":{"end":0.1911764705882354,"reset_end":0.1911764705882354,"reset_start":-0.1029411764705883,"start":-0.1029411764705883,"tags":[[["value","value",null]]]},"id":"2449","type":"Range1d"},{"attributes":{"source":{"id":"2078"}},"id":"2085","type":"CDSView"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2357","type":"Line"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2615","type":"Line"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f0BJ04H7y44/5i06+gVmjj/mLTr6BWaOP+YtOvoFZo4/39phW4NtkD85L9YxiGeQP5PrbVvNq5A/Ynkv9KdLoj+b3xM0oMSpP0hAAr5ukK0/mMs7/L1Erj/1TlhTTECuP5jrc047yK4/LBP8bGdZsD/Wq4VnhUWwP0mz+Xl/mbA/X/d/V+6KsD9f939X7oqwP4L3ZImWc7A/BETktyshsD90cZDFpzWwPzoVz+2UwbA/OzMajtAWsT+MyAYWhAyxP4tpCCi5GrE/6bsjiPvCsD/Qcc82KgyxP0gurni2DbE/guoI4bCbrz8psxbz9ESrP5tbS4BZuqc/Cgpa0ewvpz9czh1KFCqnP8mmXaUVPKY/Z7NIDFOYoz8vmYCLyXujP1BIW1A/yKI/EPCW8BdOoz/XUjcZBC+jP5bWEIhaa6M/kGriclrDoz81b7zsZGylP9kKz9nzXqM/u6CTJ20moT8tf0wyKFuhP4mJhmIJBKI/DCGAJq+loj9MuPEcwPSgPwssz3pO9KA/IoBsW3McoT9vgWsFpDygP8xlUeR5SaA/oJlAbVy5nT+zzjAycQSeP4Xow8zVCZ4/1csb8+kKnj9JMEmwZlmdPxrJW+AxbaA/UviEf/x8oD+NJ0AZ1aCgP223H6pCqKA/VnlZdh6qoD838HI4hmejPwXjHCAJ96M/p/deHbqQpD+6sVg195KkP6Id6y/fdaM/xw/fw7wboz9Ig7Djd3iiP9Qxk0a2OKM/Cr+5s0ceoz+W5rbttAejP5xX8wW94aI/3sOkQ8TUoj93fGFADISjP7fUEKzxkKM/GvfwYayYoz9S+Z/M15+jP+or9GuqFqI/Cc42oreooj/GcpD5Ej+iP8ZykPkSP6I/bYustWlKoj8+VGaJ23ubP9kMv9z4EZY/Y0Vu6KqDlT8LqSyK06yVP1FnWbFL05U/C+YA6wILmj8AekvtQkmcP9VivP9ryps/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2509"},"selection_policy":{"id":"2525"}},"id":"2508","type":"ColumnDataSource"},{"attributes":{},"id":"2423","type":"Selection"},{"attributes":{"end":0.0574897236611928,"reset_end":0.0574897236611928,"reset_start":-0.02842153068491531,"start":-0.02842153068491531,"tags":[[["value","value",null]]]},"id":"2363","type":"Range1d"},{"attributes":{"axis":{"id":"2633"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2635","type":"Grid"},{"attributes":{"below":[{"id":"2375"}],"center":[{"id":"2377"},{"id":"2381"}],"height":300,"left":[{"id":"2378"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"2407"},{"id":"2428"}],"right":[{"id":"2419"}],"sizing_mode":"fixed","title":{"id":"2367"},"toolbar":{"id":"2388"},"toolbar_location":null,"width":700,"x_range":{"id":"2362"},"x_scale":{"id":"2371"},"y_range":{"id":"2363"},"y_scale":{"id":"2373"}},"id":"2366","subtype":"Figure","type":"Plot"},{"attributes":{},"id":"2095","type":"UnionRenderers"},{"attributes":{},"id":"2079","type":"Selection"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2083","type":"Line"},{"attributes":{"axis":{"id":"2375"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2377","type":"Grid"},{"attributes":{"end":0.023198951729267246,"reset_end":0.023198951729267246,"reset_start":-0.027627336919566557,"start":-0.027627336919566557,"tags":[[["value","value",null]]]},"id":"2621","type":"Range1d"},{"attributes":{"coordinates":null,"group":null,"text":"Republic Services, Inc.","text_color":"black","text_font_size":"12pt"},"id":"2367","type":"Title"},{"attributes":{"callback":null,"renderers":[{"id":"2665"},{"id":"2686"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"2622","type":"HoverTool"},{"attributes":{"callback":null,"renderers":[{"id":"2407"},{"id":"2428"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"2364","type":"HoverTool"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"2654"},"group":null,"major_label_policy":{"id":"2655"},"ticker":{"id":"2634"}},"id":"2633","type":"CategoricalAxis"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"2396"},"group":null,"major_label_policy":{"id":"2397"},"ticker":{"id":"2376"}},"id":"2375","type":"CategoricalAxis"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"2620","type":"FactorRange"},{"attributes":{},"id":"2227","type":"BasicTickFormatter"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"2362","type":"FactorRange"},{"attributes":{},"id":"2228","type":"AllLabels"},{"attributes":{},"id":"2743","type":"BasicTickFormatter"},{"attributes":{},"id":"2629","type":"CategoricalScale"},{"attributes":{},"id":"2744","type":"AllLabels"},{"attributes":{},"id":"2371","type":"CategoricalScale"},{"attributes":{},"id":"2631","type":"LinearScale"},{"attributes":{},"id":"2373","type":"LinearScale"},{"attributes":{},"id":"2641","type":"PanTool"},{"attributes":{},"id":"2383","type":"PanTool"},{"attributes":{},"id":"2485","type":"BasicTickFormatter"},{"attributes":{},"id":"2634","type":"CategoricalTicker"},{"attributes":{},"id":"2376","type":"CategoricalTicker"},{"attributes":{},"id":"2655","type":"AllLabels"},{"attributes":{},"id":"2397","type":"AllLabels"},{"attributes":{},"id":"2486","type":"AllLabels"},{"attributes":{"axis_label":"Value","coordinates":null,"formatter":{"id":"2657"},"group":null,"major_label_policy":{"id":"2658"},"ticker":{"id":"2637"}},"id":"2636","type":"LinearAxis"},{"attributes":{"axis_label":"Value","coordinates":null,"formatter":{"id":"2399"},"group":null,"major_label_policy":{"id":"2400"},"ticker":{"id":"2379"}},"id":"2378","type":"LinearAxis"},{"attributes":{"below":[{"id":"2117"}],"center":[{"id":"2119"},{"id":"2123"}],"height":300,"left":[{"id":"2120"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"2149"},{"id":"2170"}],"right":[{"id":"2161"}],"sizing_mode":"fixed","title":{"id":"2109"},"toolbar":{"id":"2130"},"toolbar_location":null,"width":700,"x_range":{"id":"2104"},"x_scale":{"id":"2113"},"y_range":{"id":"2105"},"y_scale":{"id":"2115"}},"id":"2108","subtype":"Figure","type":"Plot"},{"attributes":{"axis":{"id":"2636"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2639","type":"Grid"},{"attributes":{"axis":{"id":"2378"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2381","type":"Grid"},{"attributes":{},"id":"2637","type":"BasicTicker"},{"attributes":{},"id":"2379","type":"BasicTicker"},{"attributes":{},"id":"2642","type":"WheelZoomTool"},{"attributes":{},"id":"2384","type":"WheelZoomTool"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2099","type":"Line"},{"attributes":{},"id":"2640","type":"SaveTool"},{"attributes":{},"id":"2142","type":"AllLabels"},{"attributes":{},"id":"2382","type":"SaveTool"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2506"},{"id":"2528"}],"location":[0,0],"title":"Variable"},"id":"2505","type":"Legend"},{"attributes":{"overlay":{"id":"2645"}},"id":"2643","type":"BoxZoomTool"},{"attributes":{"overlay":{"id":"2387"}},"id":"2385","type":"BoxZoomTool"},{"attributes":{},"id":"2644","type":"ResetTool"},{"attributes":{},"id":"2386","type":"ResetTool"},{"attributes":{"end":0.08276509922963089,"reset_end":0.08276509922963089,"reset_start":-0.04119816980638587,"start":-0.04119816980638587,"tags":[[["value","value",null]]]},"id":"2105","type":"Range1d"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2645","type":"BoxAnnotation"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2387","type":"BoxAnnotation"},{"attributes":{"active_drag":{"id":"2641"},"active_scroll":{"id":"2642"},"tools":[{"id":"2622"},{"id":"2640"},{"id":"2641"},{"id":"2642"},{"id":"2643"},{"id":"2644"}]},"id":"2646","type":"Toolbar"},{"attributes":{"axis":{"id":"2117"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2119","type":"Grid"},{"attributes":{"active_drag":{"id":"2383"},"active_scroll":{"id":"2384"},"tools":[{"id":"2364"},{"id":"2382"},{"id":"2383"},{"id":"2384"},{"id":"2385"},{"id":"2386"}]},"id":"2388","type":"Toolbar"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2421","type":"Line"},{"attributes":{"coordinates":null,"group":null,"text":"Microsoft Corporation","text_color":"black","text_font_size":"12pt"},"id":"2109","type":"Title"},{"attributes":{"end":0.05428749638042012,"reset_end":0.05428749638042012,"reset_start":-0.03404043834977104,"start":-0.03404043834977104,"tags":[[["value","value",null]]]},"id":"2019","type":"Range1d"},{"attributes":{"label":{"value":"Daily Returns"},"renderers":[{"id":"2235"}]},"id":"2248","type":"LegendItem"},{"attributes":{"source":{"id":"2401"}},"id":"2408","type":"CDSView"},{"attributes":{"callback":null,"renderers":[{"id":"2149"},{"id":"2170"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"2106","type":"HoverTool"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2254","type":"Line"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"2138"},"group":null,"major_label_policy":{"id":"2139"},"ticker":{"id":"2118"}},"id":"2117","type":"CategoricalAxis"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2249","type":"Line"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"2104","type":"FactorRange"},{"attributes":{},"id":"2285","type":"CategoricalScale"},{"attributes":{"label":{"value":"Daily Returns"},"renderers":[{"id":"2493"}]},"id":"2506","type":"LegendItem"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2253","type":"Line"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2683","type":"Line"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2404","type":"Line"},{"attributes":{},"id":"2113","type":"CategoricalScale"},{"attributes":{"source":{"id":"2143"}},"id":"2150","type":"CDSView"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2512","type":"Line"},{"attributes":{},"id":"2115","type":"LinearScale"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2664","type":"Line"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2507","type":"Line"},{"attributes":{},"id":"2125","type":"PanTool"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2405","type":"Line"},{"attributes":{},"id":"2118","type":"CategoricalTicker"},{"attributes":{"label":{"value":"Rolling Std"},"renderers":[{"id":"2256"}]},"id":"2270","type":"LegendItem"},{"attributes":{},"id":"2139","type":"AllLabels"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2663","type":"Line"},{"attributes":{"label":{"value":"Daily Returns"},"renderers":[{"id":"2751"}]},"id":"2764","type":"LegendItem"},{"attributes":{"axis_label":"Value","coordinates":null,"formatter":{"id":"2141"},"group":null,"major_label_policy":{"id":"2142"},"ticker":{"id":"2121"}},"id":"2120","type":"LinearAxis"},{"attributes":{},"id":"2783","type":"UnionRenderers"},{"attributes":{"axis":{"id":"2120"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2123","type":"Grid"},{"attributes":{},"id":"2654","type":"CategoricalTickFormatter"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2765","type":"Line"},{"attributes":{},"id":"2121","type":"BasicTicker"},{"attributes":{},"id":"2396","type":"CategoricalTickFormatter"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2511","type":"Line"},{"attributes":{},"id":"2126","type":"WheelZoomTool"},{"attributes":{"source":{"id":"2250"}},"id":"2257","type":"CDSView"},{"attributes":{"source":{"id":"2659"}},"id":"2666","type":"CDSView"},{"attributes":{"coordinates":null,"data_source":{"id":"2401"},"glyph":{"id":"2404"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2406"},"nonselection_glyph":{"id":"2405"},"selection_glyph":{"id":"2421"},"view":{"id":"2408"}},"id":"2407","type":"GlyphRenderer"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2420"},{"id":"2442"}],"location":[0,0],"title":"Variable"},"id":"2419","type":"Legend"},{"attributes":{"below":[{"id":"2289"}],"center":[{"id":"2291"},{"id":"2295"}],"height":300,"left":[{"id":"2292"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"2321"},{"id":"2342"}],"right":[{"id":"2333"}],"sizing_mode":"fixed","title":{"id":"2281"},"toolbar":{"id":"2302"},"toolbar_location":null,"width":700,"x_range":{"id":"2276"},"x_scale":{"id":"2285"},"y_range":{"id":"2277"},"y_scale":{"id":"2287"}},"id":"2280","subtype":"Figure","type":"Plot"},{"attributes":{"coordinates":null,"data_source":{"id":"2659"},"glyph":{"id":"2662"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2664"},"nonselection_glyph":{"id":"2663"},"selection_glyph":{"id":"2679"},"view":{"id":"2666"}},"id":"2665","type":"GlyphRenderer"},{"attributes":{},"id":"2124","type":"SaveTool"},{"attributes":{"label":{"value":"Rolling Std"},"renderers":[{"id":"2514"}]},"id":"2528","type":"LegendItem"},{"attributes":{"overlay":{"id":"2129"}},"id":"2127","type":"BoxZoomTool"},{"attributes":{"label":{"value":"Rolling Std"},"renderers":[{"id":"2772"}]},"id":"2786","type":"LegendItem"},{"attributes":{},"id":"2128","type":"ResetTool"},{"attributes":{"line_alpha":0.1,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2770","type":"Line"},{"attributes":{},"id":"2660","type":"Selection"},{"attributes":{"source":{"id":"2508"}},"id":"2515","type":"CDSView"},{"attributes":{},"id":"2267","type":"UnionRenderers"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"2018","type":"FactorRange"},{"attributes":{},"id":"2402","type":"Selection"},{"attributes":{},"id":"2251","type":"Selection"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f4ZHLmYNKIc/uBypHYk/hT+jBzv06IGGPx2ftgRMZ4Y/5Xf/iHFlhj/MbrDqJPqGP5YWSRAHKYc/pBGOmNFuiT+XtXe9LdWIP0AN9/EcOYg/9VPTLzl5ij8VuhpCBkiGP+6c3I2clYU/l121JKk2hT8olRXiWYiFPycJr1ly34c//2aeay/Ehz8ZXbcvtduHP9dUIldLXYg/bI2PGzntiD8XTQpCcOqIP4DHWMdi7Yg/xqEReD3ziD8px6j9FhuIPx9R5ruCG4g/Q6UOrMRliD8o0faQGLiHP20iY8OnA4g/JwhE8OkahT+Xf9GnLkiFP2ibpZxHfoU/umpg3Icigz/yxJ9dLAKDP8ClFJKqXII/5iI9kHiPgT8bTDfXeRmAPzz8tD4EDns/3jbTDh6YfT+leN+3pWJ9P+MA88dnwok/oDu9gldSiT8yPkNIfl+JP1k64WOGZIk/UBPdUCpxiT8pCyPEahCJPwLfmjlKH4k/4aMezvPSiD/fRsdotdCIPxEMdkGocog/mlu3K11niD8OvWggKjyIP8aIyH6Hh4g/08VrXNKYiD8dfqrvxNeIP5buIXXYy4g/d8iFlgI1iT9hUFBLyaGJP+ILCkKMoYk/GJVPSlsbiT/u93lFgjyJP0WFj0IfuXU/xLVRoZeKdT9NilNOMfN2P8PBIqgGzXg/OSa6E68neT+zW045+DZ5P7QddvFAkno/9pdjX5qtez8YG1OuKnl6P1xn1g7kyHo/F0JGuXPgej+ccRFfcDR7Py79YgWrk3s/lUIqeHq5ez/hSjheRDF7P0dXIqwnQHs/9KaWWikKez8GzTy4IaR6P76O2Q7TPns/n1RsrZJRfT9T5vy8oJR+P4Y5vv2WpYA/JB2Vcm/RgD/TnfhqPweCPwGGeoPyjYE/SvfaSus0gT+5/r9ifZmBP9dKcabtlYA/YgKhV5ysgD/NhRNyzOGAP8tPIMwo4IA/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2423"},"selection_policy":{"id":"2439"}},"id":"2422","type":"ColumnDataSource"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2513","type":"Line"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2129","type":"BoxAnnotation"},{"attributes":{"line_alpha":0.2,"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2255","type":"Line"},{"attributes":{"source":{"id":"2766"}},"id":"2773","type":"CDSView"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns"],"value":{"__ndarray__":"gM1eU34qgL8AWNxlsSNgPwDjNkv1rnU/AOqnlZkdaz8AyHPwuZFIPwDMzLlDx1k/ADMH5wQgiT8Au1q3P/pzvwAAtwSUdyM/AJ1B99TZeT8AUBkElxV5PwAsOyzDCV6/gHxJt02Ueb8A17gHnDJ1P4AKP+2+n4G/gNnau1sOdL8AEe1fBsqLP0CGhsillI6/AHJ1gKxcbb8A2D+YCLJePwB2PlIBk2s/gIzHHLo8gz9goxkVMS+QvwDIew/rlji/ACtg1G2Feb8AjwMsCruEvwCJiZiIiYg/gAwcnwZNkj8AEB8xpSE9PwDNSKyGi38/gCwllc3iib8A3IreaGCBP2BTWVwsZpC/wMSfQ8aCgb8AgHYT0RyAv0AXld4PSZM/AC7cvkPYVL+ApsMJoy2FPwBe1xPB1YI/AAzWWX9ZjT8Afzi5Iyl/PwAk3UEi+WY/AMUNPZWLfr9AN8u6lq2DvwCfYHIfHHY/AJ7Y9yBXaz8Ap/WiucSJP0DNVrw2epE/gFQyCFwAhj8ADt02qS94vwDTdynjwXc/gKU5D+hRe78AAAAAAAAAAAD1jUgMonY/gN38ne8Wgz8AW1tVu/WUvwDlCgdl63e/gFos+JdOf79AfwH9BfSXv8C6QV6ZhpA/AAAAAAAAAAAAcJ0pnjFFvwAIhojk2UK/AKg5Z3YKZD8AW0T6yQyGPwCFzIfpaX8/ABgwjQHTeD8A/fzJAEWOP0BWzcRZGIK/AFBPuYodYT8AVjJq0+Ffv4A7kC2k9oS/gGO8Blcwhr9ADoH8j+WAvwDPgP0eQ3k/gBnjJ418fb9A0yyRXZWJvwBzVaOooWa/ABJV6iSuaT8A1pQ1ZU15P0AXMn/doYa/AJCY+HwnYL8AsidZCS18PwBEOND3R4w/QNGxc+BChL9AUCSDb4CFv4ANuroIR4i/AAvN7VQefT8AEq5H4XpUvwCExFrQH0i/AJZXLF0kaL+AI/SON7OGPwAcfVsC9nE/ACi0+U06Sj+A74yu+wN5vwDARFeC+Dc/AJJQkGkrYz8AOB7Kbvx8vwDBMDBDbok/AAvUCfW5fT8AGObqR+NCPwAGPLiKpYQ/AHq9FiZrkz+A/UN8TlqGvwAWrOlYYlG/wB9QZpLSgL9AwKoTkmyCvwB4SEMI+Em/ALDmpiZaPL8AotiLHUBwP4B0/vXvi4w/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2660"},"selection_policy":{"id":"2674"}},"id":"2659","type":"ColumnDataSource"},{"attributes":{"active_drag":{"id":"2125"},"active_scroll":{"id":"2126"},"tools":[{"id":"2106"},{"id":"2124"},{"id":"2125"},{"id":"2126"},{"id":"2127"},{"id":"2128"}]},"id":"2130","type":"Toolbar"},{"attributes":{"line_alpha":0.1,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2147","type":"Line"},{"attributes":{},"id":"2525","type":"UnionRenderers"},{"attributes":{},"id":"2509","type":"Selection"},{"attributes":{"callback":null,"renderers":[{"id":"2579"},{"id":"2600"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"2536","type":"HoverTool"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2662","type":"Line"},{"attributes":{},"id":"2165","type":"Selection"},{"attributes":{},"id":"2158","type":"UnionRenderers"},{"attributes":{},"id":"2144","type":"Selection"},{"attributes":{"label":{"value":"Daily Returns"},"renderers":[{"id":"2149"}]},"id":"2162","type":"LegendItem"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2787","type":"Line"},{"attributes":{"coordinates":null,"data_source":{"id":"2143"},"glyph":{"id":"2146"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"2148"},"nonselection_glyph":{"id":"2147"},"selection_glyph":{"id":"2163"},"view":{"id":"2150"}},"id":"2149","type":"GlyphRenderer"},{"attributes":{},"id":"2310","type":"CategoricalTickFormatter"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2146","type":"Line"},{"attributes":{},"id":"2138","type":"CategoricalTickFormatter"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2271","type":"Line"},{"attributes":{},"id":"2416","type":"UnionRenderers"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2406","type":"Line"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"2448","type":"FactorRange"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f0Ub4kllWXw/5Bp6QWVOfD8ldYGoERCAP9aNy6iEzH8/vlZAk+gRgD/ihQuhTKSAPxycROcIlYE/BAgoQ2GTgj+qq2fNc3CCP0jifkJevoI/SUc1UWlrgz85UBCZ4ZyDP1wL+ZBp1oQ/Syt3G1n8hD8MUrS4HPyEP76EdXAWmoY/NSMCwNCBhj9tFeCl+CKGP1WCLshUaoU/VwD4Ugshhj/vX9y7zEmGP097UFo6SIY/SK3oviZehj903R/ulYqFPwT3iEqKmIU/2WIbQzhIhT+k0G5X4PSEP9xpsj3AhIU/uY9xjx69hD/GnYyY7xmFP67dorSkBoU/LXjJ7kVEhD9HgaE/5yOEP7Ej95MKK4I/x7vQUf1kgT+046WfH/2DP5UkNJVFJIM/rE99DgSggz/6JmsS7lyGPysZSArKHIc/yZGzWXlEhj/LbyKdyAOGPwQiHnxa+oU/VtGYxU+6hT/fqv06x6yFP2a5LBapyYU/eFqKGQzfhT8Ra4cbuh+GP7JPKfklXIU/pDb+EwfUhD/tufS1i6qEP+YlT59x/4Q/bs4WfBZVhT9+tlat0paFP5p5Zy/voYU/ifnEovA3hT+IZ5ZXBQWEP4mniHNa7YM/D6jF3gLMgz/Jg6PZDvGAP9F9pyGiB4A/GZfz1/IIgD/Rnwi74WaAP9WlZhF+mYE/pkpPq3Qggj+02OXb4N+BPyWTVWXA7YE/XUWdvosJgj8TXGxW8S+APzTH3xK69n8/7OLAcDGnfz/Fae2XCwSBP7lDrn1g1YA/g5iES/JIgD9BT1Dm0RyAPy2s0BDlkH8/B42Qez8xfz+J/ov8e9N9P6kZCPMW3n8/UbstW+0xgD/B95J3C/F/P4jPrzZhSn8/Ca8ZaDd0gT/6KPglBj6CP8wRx65yS4E/PqHqRIUYgT/Uh6XUwuaAP3KfGLvlt38/rV/mw9FFfz9mSN6EW1B/Pw1sI75Xn4A/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2681"},"selection_policy":{"id":"2697"}},"id":"2680","type":"ColumnDataSource"},{"attributes":{},"id":"2674","type":"UnionRenderers"},{"attributes":{"end":0.03408642548569427,"reset_end":0.03408642548569427,"reset_start":-0.03185338029169447,"start":-0.03185338029169447,"tags":[[["value","value",null]]]},"id":"2277","type":"Range1d"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns"],"value":{"__ndarray__":"gG/TTXlcoz8A+aFo85mCPwCLCHmvw5k/ANLhpYmOhT9AILPVM0+XP4Ccj2T9rJ4/4F6A5w/Cqb/AIs0HaGeHv7Avu409N7M/APx8+F1rmD/Q/CJL9Setv4CVJyk8x58/YHVQs5/jqr9AzeMtXhuSPwDzaw+5rHg/gM77ZPNMmr/AP5f6w/SrP4B+Eqw08IK/wJAY/hpFjb8gYd6q2vitv0AtLxmteqI/QI7aLvSalL+w1LhbYB2gv2DkCDw6Jp+/kGW+TDKLqb8AZL6bfqJoPwDVAjOWj3g/IP7PBKvDqT9AXojtHmePv0Chn/wF4pQ/gOrWVsI+lr9AfOTPjLyRP8CQ4o7cBLQ/EF5MC5SpoL8A90ynnO12PwABZZDZSIO/ACkodbYdfj/AD/v5bvmLv0A+15uFYpk/AD9GErqTfT/gDmJROO+vP6COyYV3Ua+/ALPZWRwHh78AEld5HMWivwDZKJLsT2S/AI5oOcFYaL8AgTGwp06JP6Cc9nqsIaG/gAFEtrdmnj+AnAbwe9Rzv4AhoINWlYY/wD5RPWjkjb+gUfIm9KqUv5jRengH87i/gLGX0uBCij9AiFbTL2OPv8DNbzu+r4e/cBDSgDkNpr+gSLvwGnKlP4BOY54zVpo/QOmxq0zpjr/AqhK8aTyDvwCEocIcqV4/AEQLgeAXbj+AiPLVFCSsPwCjvtyE1YQ/wEWLWgx5j78A2DCn44ptv4Bu9vAMgpU/YDiYV+BlmL+ADdqSct+DvwB8jIhVvlA/AGVHvX6Rpj+AWSzmmNiRP8Bobitf0JI/AHgeMA3QqD/gFb4Irc6Qv0D4XQ3Wo4+/gII+/3KUgT8ASmnToiWoPwAK0DZ4LaU/AF77/oMzjD9AT5vMzQaSP8AhBYbJ058/gHuV6oVrkT9Ablr5NGmRP4AqOQYCK44/oBmTWNZ1pz/ASVckI8ykP8CBPf46wJY/wKG7b9gyoj+AiTrc8GV+vwA1WTRzZGy/AOROezeRkj9gzwtt0VWrP0ATTfXu9qu/QFcvRUZTlD9gZZ0PWv+ev2BpUM/7Bq+/wH0LVsp0oz/A7PZCm62YPwCjK0ogJHQ/QBW7XQL7kD/gZY958qaxPwCBT6Hwd4M/4HJws1+Ilb+A08tVokB/v4Bn0XA2/JG/ACgztH7gRT+Asx8xTbOAP8BXMw8PQJY/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2230"},"selection_policy":{"id":"2244"}},"id":"2229","type":"ColumnDataSource"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2529","type":"Line"},{"attributes":{"line_alpha":0.2,"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2148","type":"Line"},{"attributes":{"axis":{"id":"2289"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2291","type":"Grid"},{"attributes":{"coordinates":null,"group":null,"text":"Waste Management, Inc.","text_color":"black","text_font_size":"12pt"},"id":"2281","type":"Title"},{"attributes":{},"id":"2543","type":"CategoricalScale"},{"attributes":{"callback":null,"renderers":[{"id":"2321"},{"id":"2342"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"2278","type":"HoverTool"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"2310"},"group":null,"major_label_policy":{"id":"2311"},"ticker":{"id":"2290"}},"id":"2289","type":"CategoricalAxis"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"2276","type":"FactorRange"},{"attributes":{"coordinates":null,"group":null,"text":"Ecolab Inc","text_color":"black","text_font_size":"12pt"},"id":"2539","type":"Title"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"2568"},"group":null,"major_label_policy":{"id":"2569"},"ticker":{"id":"2548"}},"id":"2547","type":"CategoricalAxis"},{"attributes":{"children":[{"id":"2920"},{"id":"2918"}]},"id":"2921","type":"Column"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2167","type":"Line"},{"attributes":{},"id":"2287","type":"LinearScale"},{"attributes":{"end":0.08195780194503385,"reset_end":0.08195780194503385,"reset_start":-0.048739824370553776,"start":-0.048739824370553776,"tags":[[["value","value",null]]]},"id":"2535","type":"Range1d"},{"attributes":{},"id":"2297","type":"PanTool"},{"attributes":{"below":[{"id":"2547"}],"center":[{"id":"2549"},{"id":"2553"}],"height":300,"left":[{"id":"2550"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"output_backend":"webgl","renderers":[{"id":"2579"},{"id":"2600"}],"right":[{"id":"2591"}],"sizing_mode":"fixed","title":{"id":"2539"},"toolbar":{"id":"2560"},"toolbar_location":null,"width":700,"x_range":{"id":"2534"},"x_scale":{"id":"2543"},"y_range":{"id":"2535"},"y_scale":{"id":"2545"}},"id":"2538","subtype":"Figure","type":"Plot"},{"attributes":{},"id":"2290","type":"CategoricalTicker"},{"attributes":{"factors":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"tags":[[["Date","Date",null]]]},"id":"2534","type":"FactorRange"},{"attributes":{"click_policy":"mute","coordinates":null,"group":null,"items":[{"id":"2248"},{"id":"2270"}],"location":[0,0],"title":"Variable"},"id":"2247","type":"Legend"},{"attributes":{"axis_label":"Value","coordinates":null,"formatter":{"id":"2313"},"group":null,"major_label_policy":{"id":"2314"},"ticker":{"id":"2293"}},"id":"2292","type":"LinearAxis"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2163","type":"Line"},{"attributes":{},"id":"2657","type":"BasicTickFormatter"},{"attributes":{"axis":{"id":"2292"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2295","type":"Grid"},{"attributes":{},"id":"2399","type":"BasicTickFormatter"},{"attributes":{},"id":"2293","type":"BasicTicker"},{"attributes":{},"id":"2658","type":"AllLabels"},{"attributes":{},"id":"2400","type":"AllLabels"},{"attributes":{},"id":"2298","type":"WheelZoomTool"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns"],"value":{"__ndarray__":"oB9wJjcBqD/A8Gzn/C+YvwBljPjPDHm/YJVsyemDpT8Am5yEl2lpv8A/NZQO4Ye/AOGjUrUoYL9AJ3++If6fPwCcgWoGqmk/QBzuDQdyhb9gN5rI9EKbv0DkRxLQ84+/4OhBCk1jlb+AfBG2+s1yvwDY1G+ri4o/gAjXSKNOlr8AqBl7+OVuPwBFc4CZO2i/AMbx8mPdib+ANwau+R+UP4Ct5EesC5E/AN17wK1ZeT/A+Bxjsa+FvwBY98N3Al2/AME0crlHdr+Agfnnc0aOv0CrEz6c9JU/AGCasYG0mz+A+jR1JEKSPyDUkyYywaQ/gJJZED/zhz8Aj8WcmGKavwCiyfJKUnc/gEndP7FKdr9APGOJ5zGUP4C+ylfFdoU/wNtmD6iVjr+Akwo/Dwtxv0AwaXH1pJM/AK0OaHLYiT+AssRjeqSOPwCBPP9kZ26/ABCc9AnRJr8AQwQX00CEv8BhUBBGJZo/gN4H0AYLf79gmUynYDaXvwCewQPTHWM/AIDj49zvlj/AvMJk4iSKvwASe/bbCYM/ALzVUftkWL8AEFS0Wy4yP8CVIsMxnYC/AKJMhOoKVL+ABTG5gpiMvyCAHM+0E5e/sH49vBaLsj8gAqHP7GWgPwB69plxU4A/gIMJl4ipdr8AgFjp+xVAvwDbwydYF2u/AIjTGFsuaz+Aoix/rJGRPwCmAfjZXnq/gIclo4rldb8AQlnkJraRPwDThEx32ny/AIVnLF8dbr8AFKC3zvtZPwCFGJaQLX4/AJXqB8qxfT+AoQdfE3uNPwCQ8VKEhEK/ABQB0VFFgj9AGIm42N+Sv4DlaEfBUXK/APxvt7+woz/Asg1nHOaVPwARnXjcq3S/wKBpXulvgb+AFTdqgSGKP4C9GvuFXYE/AKx+i+dgWj+Angd8Lo57v0DFFofVm5+/ALW+u/7Pdz8AMcdebURzPwBtv5QMto8/ANqmo+Idfj8A7Vusga+CP4DuDGTAVKA/wNHjKzj5kL9AgYup8JqJvwDaqMOfM4u/QM9FKkXhkj+AWGqgQ0aMv8CjM4N5n5O/wJlh3KCakj8AsmQYTFdvPwAPE/xyg2O/wJqF8mPHkD8AkU1z2at+vwDgSsQcBj8/AE6jS2/lgj8AK8RX9E2Iv6C8VE8DXpC/ANiKK4yZXz+Aphsn9yKNP0CHdzCulJA/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2144"},"selection_policy":{"id":"2158"}},"id":"2143","type":"ColumnDataSource"},{"attributes":{},"id":"2569","type":"AllLabels"},{"attributes":{"axis":{"id":"2547"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2549","type":"Grid"},{"attributes":{"callback":null,"renderers":[{"id":"2063"},{"id":"2084"}],"tags":["hv_created"],"tooltips":[["Variable","@{Variable}"],["Date","@{Date}"],["value","@{value}"]]},"id":"2020","type":"HoverTool"},{"attributes":{},"id":"2296","type":"SaveTool"},{"attributes":{},"id":"2545","type":"LinearScale"},{"attributes":{},"id":"2555","type":"PanTool"},{"attributes":{"overlay":{"id":"2301"}},"id":"2299","type":"BoxZoomTool"},{"attributes":{},"id":"2548","type":"CategoricalTicker"},{"attributes":{},"id":"2300","type":"ResetTool"},{"attributes":{"axis_label":"Value","coordinates":null,"formatter":{"id":"2571"},"group":null,"major_label_policy":{"id":"2572"},"ticker":{"id":"2551"}},"id":"2550","type":"LinearAxis"},{"attributes":{"axis":{"id":"2550"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2553","type":"Grid"},{"attributes":{},"id":"2551","type":"BasicTicker"},{"attributes":{},"id":"2556","type":"WheelZoomTool"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2301","type":"BoxAnnotation"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f+5T/i8rHZI/1Fm6ywqlkD+gSkqdXwuQP4yTbTL7SI8/p2CaHme0jj/CXR+W0VKOP4jb6YEQ1Y4/JoUep7F4jz9lCaU3nVGOP6s9NGLfYo8/4nxsF37ljj/TJWNxvjqPP6C8S9FGS48/NHDcgO/HjD+KfJXd09WMP4cCx9nv7Iw/77lHtbEjjD8yWHIwoUGMPyx8Em45BY0/ARYrDGHGiz8/LV6B4COMP7b1Cz7SoYg/MRZsSqL3hz/be3OohH+HP3exjrUBaoc/1xJr7PeXhz+2BooFtMWGP+HZRqgnm4Y/wCP5BT8nij9D8W68LUqKP4qXp+J2Xok/GTqF/W8jiT+Lc5+RJpGIPxQ3NJWFgYg/5TFcMsqSiD8GPRh0jniIP3aNoyd0yYg/6I81uIr9hz94aGlOZpuKPxUYr4PWjIk/BhJnd6paiT9sqVNM18mIP8i4RCeWzog/hRq+BrkwiT/gVopG0xGQPzEXXurSEpA//TIZmIeGjz8r2dvrx0uPP4usYjAcGI8/aZ7TqkMDjD9EqaL0FwqMPwXB4HOMCow/rob1x2nviz/0Z46LwFmMP3AAceA1HYw/1y+h+/3Iiz+lceKtHeiMP5+6zvdJa4w/aAPriCB1jD9XXg5f0mSKP31AxQEXkYo/Gpx8zcqPij98cUwJ9fGKP6QHawCuj4o/11LOFOFTij9iZYcpsfmAP2E/Mq/jdoE/aJlQuGe/gT8mlLbzi1iBPxtGwkq6XoI/36/H1voBgj906M4d5cyBP6omKD+zE4I/zyDrbsSUgj+Q1L85fPaBP0q29qyjVII/jZbjfMf6gj8DJo6dShOBP5gXkqaU04E/X6ZyieqBgj8zrJHzye+BP7CBCMBLpYE/2sWxiWumgz/PXk8sOZCDPyHkD6Wi9YM/PTBCy/Jegz+YbGYJcqmDPyKPt9/yD4Q/pXPfqSFNgz9eteJiwI2DPyDW5ZvFjoI/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2079"},"selection_policy":{"id":"2095"}},"id":"2078","type":"ColumnDataSource"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns"],"value":{"__ndarray__":"QJRfC7L9mr8Ao043yJ1zPwAutDyBm3Q/AL9XOpJwar8AzTslU3FpvwAIGkf3RoM/APfcFtQSiD+A22+MxCWBv0BonbGUTYO/ADBMjOOmdT8AEAOK3f9LPwAK+Xkr0pE/gMv4zIN2j78AYyBRzfFkv4DIno1Rf3+/APxIeMnnYT8ATnYQyfxZv4B4EupZwoG/gJZv+ZZveb+A+GpHmBmAPwCIvZzseUE/AAMS4+DUcj8A18KpCpyCvwBIzbuUams/AJNbwYWscz8APl0bUYqBvwATJgfjMYo/gON61eMflT8AGbZ9I5NyPwDGOqygDm0/IAOBbiswkL+AnDwSLwCHPwA2Ca3kCmO/wAKI/c4JkL/Ap5V9o7eCv8AoKL6NjJk/AJb8tARWd7+AwtqiFYmAPwCDgDrjXY0/ADCwKxNHnT+Ai4HiDMuEPwAaAUJlknE/AI2PCTT/YL+AYCEsnchxvwBky6ytIXU/AFqyXdtEgb8AKNz36LR1P4C/I3FnoI0/AAZ2Y6u0gD8Al+xUhQaDvwD4qCnH0kM/AADxVLyyH78AWoecQL1bvwB+UmRjxW0/AL9oqvkwa78AVmwRNMtpPwAo2bZxx0O/wHntJXc+iL+AfXIz6MiIvwBVD2iyAJw/ABvNqPdAcj8AyJfbG28/vwCOy8iU+W0/ABCfT439Zz8A0CSfQERuPwDougW1Lm0/AFAxeADXZD8ANhWSpVh6PwBx6cC0SmG/gInyzCXddr8AKR9bcvJvv4B+stcKp3S/gHgdMRaqeL/AC6ja6yCFvwBGk7ea32E/AKRBGqRBer+ABLfJYfKCvwDOKi2sGlC/gPa7uoejcr8Avh7G/FxuvwBlWZZlWXY/AFibFqc/WL8AIgtZyEJ2P4D4mMAnLIs/APDk6sLZVb/AXLik/JuJvwDiqHeRc3y/AIeUWXEdej8AoMweridEPwAs7Rj8KVc/gEfhAJhddL8AbvyI9ytgPwDutn3McYY/AKcQjVq0dD8AdJFZnuBQvwBaCEHmzm4/AGkVNqaygT/AmJKuTjKAvwB8u683cnw/AN0zd3YOiT8AIw8bnMp/vwCdJuaQK4o/wK68a54Alz/AlVQR69qHvwAOOSlmumC/AJRegXyxXT/AayKQayKAvwCH0uvb+3o/gEXn/ObHdb+AnOLuhgZ5v4B/39VPVnS/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2316"},"selection_policy":{"id":"2330"}},"id":"2315","type":"ColumnDataSource"},{"attributes":{},"id":"2554","type":"SaveTool"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns"],"value":{"__ndarray__":"ANRBHdRBjb8AAAAAAAAAAAAAAAAAAAAAAM/QFXF+fL8AsEZXOLeWvwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACAt/CrMZGOv8AYMI0B04i/4MWW4Nv8pT8AAAAAAAAAAAAeHh4eHo6/MHdcSWDeo78Aigm6qxSDPwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIPiBH/iBn78ABEEQBEGQPwAAAAAAAJC/kCRJkiRJwj9YVVVVVVXFP5AkSZIkScI/UFVVVVVVpb8AWchCFrKgP9Atcer33LI/GBQUFBQUtL8AAAAAAAAAANBeu/xI96e/ACVJkiRJcj8AAAAAAAAAAABswRZswZY/gCELWchChj8AFlhggQWGv5CFLGQhC7k/QDUPctfPtj+AEuQpQZ6CP0AngbhQc4K/ABzWvmHtmz9Qemphpae2P6AQaAqBppC/wOrZIXBjmT/AGGOMMcaoPyB6oRd6oZe/AIfD4XA4rD8At/CrMZF+PwDlAck6V34/AB4eHh4efr8AbMEWbMGWP+CBuXZgrp2/gIn0QOXslr/Asbc2THOXP4C38KsxkX6/wOwBswfMjr9wQkqeZUSvv+AUAk0h0KQ/AAAAAAAAkD8A+IEf+IGPvwAAAAAAAKw/cC+hvYT2or8QeqEXeqGnv4AQQgghhJC/QNq8T3HJoL+gui+PrQiavwCBoq0Gz5E/ABiBERiBgT8AJ3VfHluRPwAREREREYE/gJzma/XsgL8AERERERGBvxCBXBOBXLO/4BvWvmHtm78AmUDv1LWcPwAAAAAAAAAAAAAAAAAAAACgmZmZmZm5PwCS03ytnq0/AAAAAAAAqL9A2rxPccmQvwAREREREYG/AMg1Ecg1gb8AJ3VfHluBvyCkQRqkQaq/wLrBFPmsm79gL6G9hPaSvwAAAAAAAAAAAAAAAAAAAAAgNcF4K/usv8AehetRuJ6/AAAAAAAAAAAAAAAAAAAAAAD44uoHHYU/gMb60Fgfqr8AAAAAAAAAAAAAAAAAAAAAABZYYIEFlr8AaIEWaIGWPwAWWGCBBYa/gCELWchChj8AAAAAAAAAAAAAAAAAAAAASBNNNNFEs7+AxB1xR9yhP4CBC1zgApc/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2488"},"selection_policy":{"id":"2502"}},"id":"2487","type":"ColumnDataSource"},{"attributes":{"overlay":{"id":"2559"}},"id":"2557","type":"BoxZoomTool"},{"attributes":{},"id":"2558","type":"ResetTool"},{"attributes":{"coordinates":null,"group":null,"text":"Apple Inc.","text_color":"black","text_font_size":"12pt"},"id":"2023","type":"Title"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns"],"value":{"__ndarray__":"IGu7R9v5oj/Aws+4TPyYP2DPGpPtW5K/wOJ1seK0kz+giYX1kBOSv4Af+VhET3y/AFgnEDhoTj8AUkpgEEKTPwB25XIfTnG/gBsP/WF5jD+AgnV+A1yFvwAf+CpE6X6/AJHRiPFRm78AlnsaYblnPwBwWLjX9Go/ALtB5/5vkr8ASmkyGeSAPwCt4bKUPmy/wH0nJv8sjb8A5eq5relwP8AcaC5V96E/gCrKl/v7kj9AacxDWrCNvwCeZ1cfKIE/QGK2M5CLjr/AgUNvdWyMv4Cpz3kzK4s/gFp4ssrajD8AOqNte3llP0AlQkWHJJM/AGCc0ORWdr8AIGD7AbaPP4BohtIkdog/gKgXeNakgr8AcCtmE4x8PwAigyd/AoE/QOeUrzktib8Aj0lzok1wv0BO9sABRJQ/gMaLjihBhD9AZfuqCwWQPwDoHInAi38/ADUd2BSfar+AdgNgVB+HvwBCC1rQgnY/oGF1WQtbkL8AFuOx7xd/vwAW906y1HG/oK4vyQl2oT8Ap1YVdVFhvwCAt2oRvB8/AAuF/zy9fj8AzCf8tYp8PwD5WsEx8ne/gINsqAwIhL8AdNegQMdePwCLnr0CU4O/AIAFMcABEL/AiU50ohOdPwCgkfRv434/ALxUMmdhQb+AI1UIKFx5vwDz0qB2fXq/wBFq9npNhL8AsZJMywaoPwBoF/wsbjq/wJ99VcVrhL8AsQYXj1eFPwAUuhqZ71E/gJgirEPRe78AUNRYPLxnvwAAAAAAAAAAAJxCFW2EbT+AQ3vR+fyLPwB42E1Vl0Q/AGLVGu9ydr+AV7nllAmPvwDUecdyvVo/ADDTDVtpez+A1ap4AuOMPwAP7d2o1IU/ABAFsU57Mr/Aq/pEOGiQPwAosgNcj3M/ANcLte0Cf78ATQcA5eBgv4CvHCgIxX+/AGoUgSesjz8AvPJnfLFhP0Bhdh+fA5A/AJcRExVlZb8A8K0b5plsPwBdfXBX74Y/AEAY7owAeL8AkMN7a+U/PwCFBT0OP3e/wA4BOwTskD8AHAAqawlcv4C5vpHt736/ANTClE7Xjj8AI5E3Jet5PwD4AWZXb10/QLU9ei2olz+Apljn2+J/vwBWPHyODHi/ACAWUj2NZD8A1fdzayF4v8DW/xycO4a/ANyMDhAFZ78AsiSBAmeCPwCxSn+knnA/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2058"},"selection_policy":{"id":"2072"}},"id":"2057","type":"ColumnDataSource"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns","Daily Returns"],"value":{"__ndarray__":"gPx90CGblz+A6kAS3GCjvwB/grpgYIW/AIzXHJMzcD+A5obzXcd8v5DI/Kbcc6K/AJTBAVwZXD+ACUeeRjqKPyDNcjhnMbI/4MEHH3zwoT/ALCi5ynuIvwCyUnllPWU/AH2zVXObmr8As0niAzB7PwAovFqxoma/AAI1pb+yVr8AqcjBfnp+PwAmKbt8/2O/AGdyrpTYdb8AOf0QGweWP4CeUhZ8l4I/gFjGN64Ujr+gEjNFwDmUvwAds3Vc6og/wMyrsKjLir8AerqUFdNlPwBcnbVO/Fu/wPfNit0akj+AFspBE1iBvwDlsOd3qme/gDiqJxQGm78AJbrMy6eGP4CXc2vgp4M/QOtEeHlLjL8ALYG0BNKCPwCskekDppU/AGyvPaqmTr8A0OKTNYsovwAK+DDkNYo/AMiuxLCCej8A+2ebsTiMPwDihRf3P2g/ANJfMVtffL8ALcsl0o5+PwCFKQ0lLnc/ALmnEZZ7er8AAOgQ9jx0v4A+jhQpE3a/APMBywcsjz+AT/dRFzuNvwC17Fqq7mS/AOx5gBT9YD8ANv4eB3NiPwBS36J62F6/AGBQyCwnfz8AAi6Ss75nP4Db5eIs84W/wAtqE5dwhb8AI6hlZIyWPwARoxnwtIQ/AJNtF6Dwdj9AA0WLN6+gPwC0NLOeF3a/gFlgZkIod78AFdMjkSF/P4CYFrW1z3O/AMCIouFVHj8AkxBtDKWIPwDTtMzsn3C/APAVA/GRJr8Azr3HW7FkvwDS7jGdDXy/AEkuImBuhj8A/uH4wHpnPwBctcHG910/wIO/eDOkir+w5btd9iigvwBg5T6seEc/AJq+ljB6dr8AQHWsILBRPwAvgshVPXu/gOePxkGHcr/Au0GCQkCZP8BkYDIwGZg/AGjVXyhlXD8AnOcyfslUv4CLepXAeog/gJEAVZE3hD8AGszh0yRnPwBBMjXO4oE/gNMqg4Xpiz9AIuED77uCvwBNzRwNc4s/AFCtWDQvUL8Ay+EeTTOAvwBAW91A2E4/gOBDM4xkdL/AKdjke5WDv4A7lemn3Yk/ABaMcsEoZz8A4IoBucpOP4A11gzzqo8/AN9syQNMjz8AACBFq05DvwDZQjws9I6/ADnbGdgVbL8AeCKn7CdYPwBw3vA26Fk/AFi25CWhdT+A9OqGHoOHPwBrXRCRvGO/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2574"},"selection_policy":{"id":"2588"}},"id":"2573","type":"ColumnDataSource"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"2559","type":"BoxAnnotation"},{"attributes":{"active_drag":{"id":"2555"},"active_scroll":{"id":"2556"},"tools":[{"id":"2536"},{"id":"2554"},{"id":"2555"},{"id":"2556"},{"id":"2557"},{"id":"2558"}]},"id":"2560","type":"Toolbar"},{"attributes":{"line_color":"#fc4f30","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2769","type":"Line"},{"attributes":{},"id":"2568","type":"CategoricalTickFormatter"},{"attributes":{},"id":"2141","type":"BasicTickFormatter"},{"attributes":{"axis":{"id":"2031"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"2033","type":"Grid"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f8QRy1+eKqM/ZgaYhjfzoj9ssGjDg0qjP9x/hU8hWKM/ifeeZgf6oz89qcDXV7ujP5io0ljRVKM/vxkVvU2toz96xAkgWLSjP/7Dtk5Gt6E/V0LvF810oT95bdzmk6mgPwmXMfivraI/thBCKcoXoj/c5okg3/qhPwqRpwJZ+qE//3tMLpTKoT8efaO4W5ugP0kfabmP5aA/i9B56RjeoD/eEkrqq+WgP2RXs0vh+KE/9maxs7XeoT/GKdwk6P+hPxbPctbBpaE/E28uTBucoD8+s9gyaaOgPz+eb0ZDJ6E/jLY5yrGJoD+alGzfHnGgPzOSTMTFVaA/UL4RSQo5oD9T0QrhVEmgP+n6YJ5mAqE/jDYQsEPtoD8zo/bst+SgP6k0PNgB5qA/+fweW4VAoT+1P36VdC+iP3DdYUIHNaI/rr6wgUAloj9b2wEepiSgP0PhCVI+O54/1C8RfHdZnj8OtHlmEVGgP6HYOoG4Y6A/i4AkZYJyoD9EYYjKHlegPxFYePnhM6A/U0VBEbXhnz8Xxk1ed+efP28+Pyu6uZ8/XxDtOxa7oD+KvbwZ9bqgP+JiYRlwupg/3dN6IjuImj+kDX3jpZSaPyIjYrH4u5o/JqqZGh4SmD/tASZqeXmYP46boiqkQ5k/20Jt3ruTmD91l3hbfR6YP7sVKrKLUZg/1X70IBU0mD/bjGr550yWPxJ7BkGHS5Y/2SlZf4Zllj/g4rpzjoyWPweYvZUVj5Y/GG9r7TCVlD/HDSAf+2iUP2Xmh8UQqpQ/q7q/kb7vkz/wg+4+I02VP+4kI4mTP5s/udGB6RhMmj+WNSU//V2bP8AbIJjymp8/q/e5r7EEoD8Hz4btCDufP42Zqr+cnJ4/zB0ytCCgnj831/JyvJCgP5s4hiWQdqA/V22RNKLooD/3GaBfYgmhP/jFS6zBUaE/PFz3Mj/MoD88+tJ3u1WgP2beueEVVKA/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2251"},"selection_policy":{"id":"2267"}},"id":"2250","type":"ColumnDataSource"},{"attributes":{"end":0.09576493269333136,"reset_end":0.09576493269333136,"reset_start":-0.11502408227330047,"start":-0.11502408227330047,"tags":[[["value","value",null]]]},"id":"2191","type":"Range1d"},{"attributes":{"children":[{"id":"2921"}],"margin":[0,0,0,0],"name":"Row02556","tags":["embedded"]},"id":"2017","type":"Row"},{"attributes":{},"id":"2027","type":"CategoricalScale"},{"attributes":{"label":{"value":"Daily Returns"},"renderers":[{"id":"2665"}]},"id":"2678","type":"LegendItem"},{"attributes":{},"id":"2029","type":"LinearScale"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f2oz6yN6S5g/ei76Y5wWmD+mxkn/aOCWPwcTgSs2z5Y/4oIfiIwTlz/y+ulDR/uWP9EctDVpL5U/EpdHRh1llT8x10jFoYCVP7PE+wEjNY0/OAjIlS+iij+4VtBMrfGKPwt9mL9CWos/N3xccEWHiT965TJ5D7WJPx9Mv0CqcYs/SQToECZwiz9bfofxrj+LP/L2PDGkyIs/r8Cq7d65iz+ay35C/8CKP8wugSHlgoo/yux6nVnGiT9Dk5vIGfCHP3rDJgaNj4c/Uc23I9TVhj9bqMYJeROHP7x7Be1NQ4c/6oQaqzDwhj84nvoXWp+HP5Q415IVnIc/cuIa4A2ggz+w7DU0uECDP5TiRt0uDYM/5EXC2ayugT+zavcJTnGBP882GZWJJIA/jbzMBpjygD/fRkwFqGuDP/Gg5c/YIoM/0nedyBUYgz9VB7aF7guHP9ty3z9MUIc/FkdfIIE3hz9B1wEsPDmHPzGYJX8ZXoc/juNfD8ERhz8n7Zc4wD6HP5EgRK12J4c/EDHaA31xhj9sNC+ZDDmFP5MvCMK7h4U/d0Lz+3DbhT9/K1fg4dqFP6tNJWXqvoU/TI7Wi/THhj8+T+IcXUyLPw83X0NayYo/sHi7Qn5tij+WIbhIw5KIP5/ynj7JTog/WOHSm8QxiD894C1zFg2GPxZgu2LLlog/j/3FHjR0iD/EmEZQEjmIP+B//sn7r4g/9DkZOOgEiT98GaxwHYSIP85z0RO7p4g/6gIENDY9iT91/sx90KiJP+vDq/us1ok/d9aqQwOWiT8rIdmFGPiJP45U+3Zv+Ik/srAd581DiT+shTeuQ6OEPzVUcEjrC4U/Kzj9oTCfhD+raye7QaCEP+mu4CYJnYQ/R/jTHdmYhD8h9VevcMuCP1TgygwIn4I/cDu5koLUgj/OzRMvFsCCP1s6epx5QII/dvM4oeb2gT+ZofQclHOCP6S5viSOVII/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2595"},"selection_policy":{"id":"2611"}},"id":"2594","type":"ColumnDataSource"},{"attributes":{"data":{"Date":["2/2/2023 16:00:00","2/3/2023 16:00:00","2/6/2023 16:00:00","2/7/2023 16:00:00","2/8/2023 16:00:00","2/9/2023 16:00:00","2/10/2023 16:00:00","2/13/2023 16:00:00","2/14/2023 16:00:00","2/15/2023 16:00:00","2/16/2023 16:00:00","2/17/2023 16:00:00","2/21/2023 16:00:00","2/22/2023 16:00:00","2/23/2023 16:00:00","2/24/2023 16:00:00","2/27/2023 16:00:00","2/28/2023 16:00:00","3/1/2023 16:00:00","3/2/2023 16:00:00","3/3/2023 16:00:00","3/6/2023 16:00:00","3/7/2023 16:00:00","3/8/2023 16:00:00","3/9/2023 16:00:00","3/10/2023 16:00:00","3/13/2023 16:00:00","3/14/2023 16:00:00","3/15/2023 16:00:00","3/16/2023 16:00:00","3/17/2023 16:00:00","3/20/2023 16:00:00","3/21/2023 16:00:00","3/22/2023 16:00:00","3/23/2023 16:00:00","3/24/2023 16:00:00","3/27/2023 16:00:00","3/28/2023 16:00:00","3/29/2023 16:00:00","3/30/2023 16:00:00","3/31/2023 16:00:00","4/3/2023 16:00:00","4/4/2023 16:00:00","4/5/2023 16:00:00","4/6/2023 16:00:00","4/10/2023 16:00:00","4/11/2023 16:00:00","4/12/2023 16:00:00","4/13/2023 16:00:00","4/14/2023 16:00:00","4/17/2023 16:00:00","4/18/2023 16:00:00","4/19/2023 16:00:00","4/20/2023 16:00:00","4/21/2023 16:00:00","4/24/2023 16:00:00","4/25/2023 16:00:00","4/26/2023 16:00:00","4/27/2023 16:00:00","4/28/2023 16:00:00","5/1/2023 16:00:00","5/2/2023 16:00:00","5/3/2023 16:00:00","5/4/2023 16:00:00","5/5/2023 16:00:00","5/8/2023 16:00:00","5/9/2023 16:00:00","5/10/2023 16:00:00","5/11/2023 16:00:00","5/12/2023 16:00:00","5/15/2023 16:00:00","5/16/2023 16:00:00","5/17/2023 16:00:00","5/18/2023 16:00:00","5/19/2023 16:00:00","5/22/2023 16:00:00","5/23/2023 16:00:00","5/24/2023 16:00:00","5/25/2023 16:00:00","5/26/2023 16:00:00","5/30/2023 16:00:00","5/31/2023 16:00:00","6/1/2023 16:00:00","6/2/2023 16:00:00","6/5/2023 16:00:00","6/6/2023 16:00:00","6/7/2023 16:00:00","6/8/2023 16:00:00","6/9/2023 16:00:00","6/12/2023 16:00:00","6/13/2023 16:00:00","6/14/2023 16:00:00","6/15/2023 16:00:00","6/16/2023 16:00:00","6/20/2023 16:00:00","6/21/2023 16:00:00","6/22/2023 16:00:00","6/23/2023 16:00:00","6/26/2023 16:00:00","6/27/2023 16:00:00","6/28/2023 16:00:00","6/29/2023 16:00:00","6/30/2023 16:00:00","7/3/2023 13:05:00","7/5/2023 16:00:00","7/6/2023 16:00:00","7/7/2023 16:00:00","7/10/2023 16:00:00","7/11/2023 16:00:00","7/12/2023 16:00:00","7/13/2023 16:00:00"],"Variable":["Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std","Rolling Std"],"value":{"__ndarray__":"AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4fwAAAAAAAPh/AAAAAAAA+H8AAAAAAAD4f+5T/i8rHZI/1Fm6ywqlkD+gSkqdXwuQP4yTbTL7SI8/p2CaHme0jj/CXR+W0VKOP4jb6YEQ1Y4/JoUep7F4jz9lCaU3nVGOP6s9NGLfYo8/4nxsF37ljj/TJWNxvjqPP6C8S9FGS48/NHDcgO/HjD+KfJXd09WMP4cCx9nv7Iw/77lHtbEjjD8yWHIwoUGMPyx8Em45BY0/ARYrDGHGiz8/LV6B4COMP7b1Cz7SoYg/MRZsSqL3hz/be3OohH+HP3exjrUBaoc/1xJr7PeXhz+2BooFtMWGP+HZRqgnm4Y/wCP5BT8nij9D8W68LUqKP4qXp+J2Xok/GTqF/W8jiT+Lc5+RJpGIPxQ3NJWFgYg/5TFcMsqSiD8GPRh0jniIP3aNoyd0yYg/6I81uIr9hz94aGlOZpuKPxUYr4PWjIk/BhJnd6paiT9sqVNM18mIP8i4RCeWzog/hRq+BrkwiT/gVopG0xGQPzEXXurSEpA//TIZmIeGjz8r2dvrx0uPP4usYjAcGI8/aZ7TqkMDjD9EqaL0FwqMPwXB4HOMCow/rob1x2nviz/0Z46LwFmMP3AAceA1HYw/1y+h+/3Iiz+lceKtHeiMP5+6zvdJa4w/aAPriCB1jD9XXg5f0mSKP31AxQEXkYo/Gpx8zcqPij98cUwJ9fGKP6QHawCuj4o/11LOFOFTij9iZYcpsfmAP2E/Mq/jdoE/aJlQuGe/gT8mlLbzi1iBPxtGwkq6XoI/36/H1voBgj906M4d5cyBP6omKD+zE4I/zyDrbsSUgj+Q1L85fPaBP0q29qyjVII/jZbjfMf6gj8DJo6dShOBP5gXkqaU04E/X6ZyieqBgj8zrJHzye+BP7CBCMBLpYE/2sWxiWumgz/PXk8sOZCDPyHkD6Wi9YM/PTBCy/Jegz+YbGYJcqmDPyKPt9/yD4Q/pXPfqSFNgz9eteJiwI2DPyDW5ZvFjoI/","dtype":"float64","order":"little","shape":[111]}},"selected":{"id":"2767"},"selection_policy":{"id":"2783"}},"id":"2766","type":"ColumnDataSource"},{"attributes":{"axis_label":"Date","coordinates":null,"formatter":{"id":"2052"},"group":null,"major_label_policy":{"id":"2053"},"ticker":{"id":"2032"}},"id":"2031","type":"CategoricalAxis"},{"attributes":{"line_color":"#30a2da","line_width":2,"tags":["apply_ranges"],"x":{"field":"Date"},"y":{"field":"value"}},"id":"2679","type":"Line"},{"attributes":{},"id":"2032","type":"CategoricalTicker"},{"attributes":{"label":{"value":"Daily Returns"},"renderers":[{"id":"2407"}]},"id":"2420","type":"LegendItem"},{"attributes":{"axis":{"id":"2034"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"2037","type":"Grid"},{"attributes":{},"id":"2681","type":"Selection"},{"attributes":{"axis_label":"Value","coordinates":null,"formatter":{"id":"2055"},"group":null,"major_label_policy":{"id":"2056"},"ticker":{"id":"2035"}},"id":"2034","type":"LinearAxis"},{"attributes":{},"id":"2035","type":"BasicTicker"},{"attributes":{},"id":"2767","type":"Selection"}],"root_ids":["2017"]},"title":"Bokeh Application","version":"2.4.3"}};
    var render_items = [{"docid":"c49a12fd-411b-4198-abd8-7ca86337d08c","root_ids":["2017"],"roots":{"2017":"5a78dc29-8faf-48bb-82db-6df5855468e0"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
    for (const render_item of render_items) {
      for (const root_id of render_item.root_ids) {
	const id_el = document.getElementById(root_id)
	if (id_el.children.length && (id_el.children[0].className === 'bk-root')) {
	  const root_el = id_el.children[0]
	  root_el.id = root_el.id + '-rendered'
	}
      }
    }
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




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

