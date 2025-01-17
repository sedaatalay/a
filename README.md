# Sample Data Analysis Techniques with PySpark  
<p> <br/ >

## What is Apache Spark?
Apache Spark is a fast, in-memory data processing engine with elegant and impressive development APIs that enable data workers to efficiently execute streaming, machine learning or SQL workloads that require fast iterative access to datasets. Spark was originally developed at UC Berkeley at the AMP Lab. Later, its code base was open sourced and eventually donated to the Apache Software Foundation; hence its current name, Apache Spark.

## Setup
For this tutorial, we will be loading Apache Spark on Jupyter Notebook. There are many tutorials on how to install Apache Spark, and they are easy to follow along. 
To setup jupyter notebook on EC2 Server, check the link below :

https://github.com/sedaatalay/Running-Jupyter-Notebook-on-AWS-EC2-Server
 
<p> <br/ >
 
## Creating an sample in Jupyter Notebook
  
### Simply import Apache Spark via 
  
```console
import findspark
findspark.init()
import pyspark
from pyspark import SparkConf, SparkContext
from pyspark.sql import SparkSession
from pyspark.sql.functions import *
from random import randint, choice
import string  
```
<img width="739" alt="Ekran Resmi 2022-05-24 21 34 06" src="https://user-images.githubusercontent.com/91700155/170107919-03638278-6085-43ed-959b-4ec1ba95901d.png">
  
### Spark Context 
  
#### To run Spark, we need to initialize a Spark context. A Spark context is the entry point to Spark that is needed to deal. We can initialize one simply as follows:
  
```console
conf = SparkConf().setMaster("local").setAppName("app")
sc = SparkContext.getOrCreate()
spark = SparkSession.builder.getOrCreate()
```
<img width="735" alt="Ekran Resmi 2022-05-24 21 34 13" src="https://user-images.githubusercontent.com/91700155/170107977-be7296d6-952b-4cd8-a6a7-167443187e58.png">

  
### Create random generated samples (has 50 dimensions) 
```console
def randCol():
  return rand(seed=randint(0,1000)).alias(choice(string.ascii_letters))
```
<img width="732" alt="Ekran Resmi 2022-05-24 21 34 58" src="https://user-images.githubusercontent.com/91700155/170108080-238809e2-0841-4269-9c8d-c97c67e73374.png">  
  
#### or
```console
import pandas as pd
import
numpy as np
df= pd.DataFrame np.random.randint (0,100,size=(1000000, 51)), np.arange (
df
``` 
```console
df.to_csv('data.csv',index =None)
```
<img width="575" alt="Ekran Resmi 2022-05-24 21 32 50" src="https://user-images.githubusercontent.com/91700155/170107420-08c696d1-4219-4646-a063-c4e37ac51a5c.png">  
  
### SQLContext
  
#### SqlContext is the entry point to SparkSQL which is a Spark module for structured data processing. Once SQLContext is initialised, the user can then use it in order to perform various “sql-like” operations over Datasets and Dataframes.
```console
df = sqlContext.range(0, 50).select("id", randCol(),randCol(),randCol())
df.show(5)
``` 
<img width="729" alt="Ekran Resmi 2022-05-24 21 37 30 2" src="https://user-images.githubusercontent.com/91700155/170108441-6f6763ea-5f42-4d53-adf4-c75becb88b55.png">
<img width="727" alt="Ekran Resmi 2022-05-24 21 37 30" src="https://user-images.githubusercontent.com/91700155/170108427-83997cd8-f505-414d-af91-967c29e2d410.png">
  
  
### Mean, Min, Max
  
#### Mean
```console
df.select([mean(df.schema.names[1]), mean(df.schema.names[2]), mean(df.schema.names[3])]).show()
``` 
<img width="734" alt="Ekran Resmi 2022-05-24 21 39 21" src="https://user-images.githubusercontent.com/91700155/170108693-5a24189a-e3d7-40e0-a0f9-6507bcb23c5d.png">

#### Min
```console
df.select([min(df.schema.names[1]), min(df.schema.names[2]), min(df.schema.names[3])]).show()
``` 
<img width="733" alt="Ekran Resmi 2022-05-24 21 39 30" src="https://user-images.githubusercontent.com/91700155/170108728-85a594dc-d03b-49d7-9410-17e8c4d10ab0.png">

#### Max
```console
df.select([max(df.schema.names[1]), max(df.schema.names[2]), max(df.schema.names[3])]).show()
``` 
<img width="731" alt="Ekran Resmi 2022-05-24 21 39 56" src="https://user-images.githubusercontent.com/91700155/170108746-67059f6e-b422-4b8a-b307-a13676e65080.png">

  
### Variance, Covariance
  
#### Variance
```console
df.agg({df.schema.names[1]: 'variance'}).collect()
``` 
<img width="734" alt="Ekran Resmi 2022-05-24 21 41 18" src="https://user-images.githubusercontent.com/91700155/170109152-ba789676-3874-4f13-882d-58d23d91a6b9.png">
  
#### Covariance
```console
df.stat.cov(df.schema.names[1], df.schema.names[2])
``` 
<img width="731" alt="Ekran Resmi 2022-05-24 21 41 26" src="https://user-images.githubusercontent.com/91700155/170109186-1861e197-617a-4ddc-ad57-00484cc7709d.png">
  
### FreqItems  
```console
df.stat.freqItems(df.schema.names, 0.3).collect()
```
<img width="734" alt="Ekran Resmi 2022-05-24 21 41 33" src="https://user-images.githubusercontent.com/91700155/170109238-4c1eeeab-ffaf-4442-a305-2b077ac70552.png">
  
### Math Functions  
```console
df.select(df.schema.names[1],
  (pow(sin(df[df.schema.names[1]]), 4) * sin(df[df.schema.names[1]])).alias("Sinus Equeation"),
  toDegrees(df.schema.names[1]),).show()
```   
<img width="729" alt="Ekran Resmi 2022-05-24 21 41 40" src="https://user-images.githubusercontent.com/91700155/170109297-3fa28937-8c38-4987-8b95-ca087954bff0.png">
<img width="729" alt="Ekran Resmi 2022-05-24 21 54 13" src="https://user-images.githubusercontent.com/91700155/170111316-cdcd08aa-ee7b-4ca5-9c39-4baeb4f12ff0.png">

### ColStats  
```console
df.summary().show()
```   
<img width="733" alt="Ekran Resmi 2022-05-24 21 42 05" src="https://user-images.githubusercontent.com/91700155/170109437-8c43356f-1115-4edc-b9ed-5ee2c0ff10c6.png">
  
### Describe 
```console
df.describe().show()
```    
<img width="734" alt="Ekran Resmi 2022-05-24 21 41 59" src="https://user-images.githubusercontent.com/91700155/170109489-4bc22f06-3b1d-4694-93fa-1601103d9408.png">

### Crosstab 
```console
companies = ["IBM", "Google", "Huawei","Netflix","Meta","Tesla"]
positions = ["Software Engineer", "Big Data Analyst", "MLOps Specialist", "Data Scientist", "Machine Learning Engineer"
compy_df = sqlContext.createDataFrame([(companies[i % len(companies)], positions[i % len(positions)]) for i in range
compy_df.show()
```   
<img width="736" alt="Ekran Resmi 2022-05-24 21 42 15" src="https://user-images.githubusercontent.com/91700155/170109565-67c9bd5b-35d1-4465-96f4-3f528f237423.png">
<img width="736" alt="Ekran Resmi 2022-05-24 21 56 57" src="https://user-images.githubusercontent.com/91700155/170111671-898e9a83-5218-44c4-938c-c9bfd00640a8.png">
 
```console
compy_df.stat.crosstab("Companies", "Positions").show()
```      
<img width="734" alt="Ekran Resmi 2022-05-24 21 42 33" src="https://user-images.githubusercontent.com/91700155/170109666-3bd1ccc9-0e33-4c18-b8bd-6d463df9fe68.png">
  
  
#### Thank you :) 
<p>  <br /><br />
</p>

### Seda Atalay
