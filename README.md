#join
import dask.dataframe as dd
import pandas as pd

df = dd.read_csv('D:\\tmp\\311_Service_Requests.csv',dtype={'Incident Zip': 'object',
       'Landmark': 'object',
       'Taxi Company Borough': 'object',
       'Vehicle Type': 'object','Agency': 'object'})

df2=dd.read_csv('D:\\tmp\\Agency.csv',dtype={'Agency':'object'})
df3=df[df['Agency'].isin(df2['Agency'].tolist())].compute()

