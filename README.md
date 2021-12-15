# Federal Funds Rate

## What is the Fed Funds rate?
One of the primary tools in monetary policy is the ability of a central bank to set interest rates.

The U.S. Federal Reserve does this by changing the **Federal Funds rate**, which is the rate charged on loans between private banks on their reserve balances held at the Fed.

The Fed controls all other interest rates in the economy via the Fed Funds rate. Rates on other types of lending such as government (public-proivate), wholesale (private bank to private bank), or retail (private bank to customer) are determined in part by this underlying rate.

Fed Funds is called the "risk-free" interest rate as it represents the closest thing to riskless borrowing in U.S. currency. For the banks invovled this represents the cheapest form of borrowing and short-term cash flow available. All other lending/borrowing activities in the financial system are priced relative to this risk-free rate. The cost of credit emanates from the center of the financial system to all its participants.

## Risk-free rate and credit spread

This pricing of risk is made explicit by the following mathematical formalization. The interest rate for a particular lending activity would be the risk-free rate plus a spread for based on creditworthiness, with a higher spread on riskier borrowers.

When the risk-free rate increases or decreases the rate will increase or decrease in a parallel shift. In practice there are also curve effect depending on the term of the loan and other factors such as liquidity. 

## Data on Fed Funds and other interest rates
Historically market rates follow close


## Increasing and decreasing interest rates and their effect on the economy
Interest rates represent the **cost of money** - how expensive borrowing for debtors. A low interest rate . Because borrowing activity increases, the amount of money circulating through the system also increases. On 

For this reason low interest rates are referred to as "expansionary" monetary policy 

Cheapening credit has the effect of taking pressure off of profits in real economic terms. The "hurdle rate" of productivity declines with the nominal cost of capital. It additionally can . 

# -*- coding: utf-8 -*-
"""
Created on Sun Nov 14 09:12:11 2021

@author: Michael
"""

import os
import numpy as np
import pandas as pd
import requests
import time

MY_API = '499659ea0d975475c7302585ae1c6d7a'

i = 'CSUSHPISA'

i = 'NASDAQCOM'

args_url = {'series_id':i,'api_key':MY_API,'file_type':'json'}
req = requests.get('https://api.stlouisfed.org/fred/series/observations',params=args_url)
data = req.json()
df = pd.DataFrame.from_dict(data['observations'])
df['value'] = df['value'].replace({'.':'0'}).astype(float)
df['value'] = df['value'].replace(0,np.nan)
df.index = pd.to_datetime(df['date'])
df['1Y_return'] = df['value']/df['value'].shift(12) - 1
df['1M_return'] = df['value']/df['value'].shift(1) - 1
df['avg'] = df['1M_return'].mean()
df['above_avg'] = df['1M_return'] > df['1M_return'].mean()


import plotly.express as px
fig = px.line(df['value'].dropna(),log_y=True)
fig.show()



import plotly.express as px
fig = px.line(df['1Y_return'].dropna())
fig.show()

import plotly.express as px
fig = px.line(df['1M_return'].dropna())
fig.show()

import plotly.express as px
fig = px.line((np.log(df['value'])-np.log(df['value']).shift(1)).dropna())
fig.show()


fig.write_html("FedFunds.html",auto_open=True)

