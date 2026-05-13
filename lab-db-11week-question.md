### Pandas + Query

#### Question: Complete the following python code: *yourname-db-lab-11.py*

For Python installation, please do anything you like. (e.g., Visual Studio Code, python.org)

We can discuss more on python installation in class.

Download ***trade-data.csv*** from our course LMS.

```python
#! /usr/bin/env python3

import pandas as pd
import scipy
import matplotlib.pyplot as plt
import numpy as np
import matplotlib

def sample():
    
    data_fn = "./trade-data.csv"
    df = pd.read_csv(data_fn)
    df['timestamp'] = pd.to_datetime(df['timestamp'])
    df = df.set_index('timestamp')
    
    start_date = ''; end_date = ''
    df.query("@start_date <= index <= @end_date", inplace=True)
    
    print (df.to_string())

    del df

#####################
# Dataset description
#####################
# 'trade-data.csv' has some cryptocurrency datasets in 2025.
# Each row indicates an action involving a price of cryptocurrency.
# Schema: timestamp,quantity,price,fee,amount,side

# References:
# https://pandas.pydata.org/docs/reference/api/pandas.read_sql_query.html
# https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.query.html
# https://sparkbyexamples.com/pandas/pandas-dataframe-query-examples/

# Do the following questions. Write your code and results. 
# NOTE: Your answer should be a combination of 'df.query' and 'pandas'.
# Do not change the function names in this template.

def q_1():
    
    #How many rows (trades) are available from Jan 10 to 20, 2025?
    #print its count. 
    
    pass

def q_2():
    
    #How many rows exist per day during the entire month?
    #print its counts in sequence (list). If there's a missing day, fill it with -1.  
    
    #output: [1 2 3 4 5 -1 6 7 ...  ]
    
    pass

def q_3():
    
    #What is the hourly sum of 'amount' during the entire month?
    #print its sum in sequence (list). If there's a missing hour, fill it with 0.  
    #no floating digits, round it to integer.

    #output: [ 0 0 0 0 ... ]
   
    pass

def q_4():
    
    #Repeat q_2, but this time let's count them weekly and separately where the side is equal to 0 or 1.
    #Don't print them in sequence. Instead, save a figure ('yourname.png') for weekly row counts, using a stacked bar of side 0 and side 1.
    #No need to submit a figure, your code is just fine. 
    #Reference: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.bar.html
   
    pass

#Do not change anything below.
def main():
    
    #Run q_1 to 4

    q_1()
    q_2()
    q_3()
    q_4()
    
    return

if __name__=="__main__":
    main()
```



For example, the figure from q_4 may look like the following:

- 'speed' should be where side is 0. 'lifespan' should be where side is 1. 
- x-axis labels like 'snail' would be week1, 'pig' would be week2, and others. 
- Make sure it's saved to 'yourname.png'. Again no need to submit the figure itself.

![../../_images/pandas-DataFrame-plot-bar-3.png](https://pandas.pydata.org/pandas-docs/stable/_images/pandas-DataFrame-plot-bar-3.png)



**Be very careful!**

- Submit it to LMS (check the deadline), .py file only (db-lab-11week-question-submission).
- DO NOT put any space in your submit file name. e.g.) 'young  db-lab-11 .py' (X). 0 will be given.
- If your program shows any errors, 0 will be given.
