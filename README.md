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
#############################
df22.to_csv("D:\\ll-*.csv",index=False)
------------------------------
dask.dataframe.to_csv 只能导出到多个文件中，一个partition一个文件。需要手工拼接成一个single file。
以下是拼接代码：
from glob import glob
filenames = glob('/path/to/myfiles.*.csv')
with open('outfile.csv', 'w') as out:
    for fn in filenames:
        with open(fn) as f:
            out.write(f.read())  # maybe add endline here as well?

###########################
from dask.distributed import Client,progress
c=Client("127.0.0.1:9876")
c=Client()
----------------------------------------------
显示处理进度，这里有个bug，Client（）直接声明会显示找不到路径，先执行Client("127.0.0.1:9876")，再执行Client()声明就不会报错了。打印出c的地址和端口，就可以监控了。
############################
import dask
dask.visualize(df)
--------------
显示运算的路径。除了anaconda自带的graphviz以外，还需要从https://graphviz.gitlab.io下载安装文件,并添加环境变量PATH   C:\Program Files (x86)\Graphviz2.38\bin
##################################
