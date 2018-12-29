#join
import dask.dataframe as dd
import pandas as pd

df = dd.read_csv('D:\\tmp\\311_Service_Requests.csv',dtype={'Incident Zip': 'object',
       'Landmark': 'object',
       'Taxi Company Borough': 'object',
       'Vehicle Type': 'object','Agency': 'object'})

df2=dd.read_csv('D:\\tmp\\Agency.csv',dtype={'Agency':'object'})

df3=df[df['Agency'].isin(df2['Agency'].tolist())].compute()
-------------------
#从未排序的列设置新索引非常昂贵
#许多操作，例如groupby-apply和join on unsorted columns，需要设置索引，如上所述，索引很昂贵
#df3=df.join(df2,on="Agency") 会报错，原因是没有设置索引。所以我们使用isin的方式。

df22.to_csv("D:\\ll-*.csv")
------------------------------
dask.dataframe.to_csv 只能导出到多个文件中，一个partition一个文件。需要手工拼接成一个single file。

