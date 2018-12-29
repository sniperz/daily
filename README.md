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

df22.to_csv("D:\\ll-*.csv",index=False)
------------------------------
dask.dataframe.to_csv 只能导出到多个文件中，一个partition一个文件。需要手工拼接成一个single file。
以下是拼接代码：
#os模块中包含很多操作文件和目录的函数
import os
#获取目标文件夹的路径
meragefiledir = os.getcwd()+'\\MerageFiles'
#获取当前文件夹中的文件名称列表
filenames=os.listdir(meragefiledir)
#打开当前目录下的result.txt文件，如果没有则创建
#文件也可以是其他类型的格式，如result.js
file=open('result.txt','w')
#向文件中写入字符
#file.write('python\n')
 
#先遍历文件名
for filename in filenames:
    filepath=meragefiledir+'\\'+filename
    #遍历单个文件，读取行数
    for line in open(filepath):
        file.writelines(line)
    file.write('\n')
 
#关闭文件
file.close()

版权声明：本文为博主原创文章，转载请附上博文链接！
