Let's walk through these labs in Azure Databricks.

# Lab 1: UDFs and Custom Transformations

First, let's import the necessary libraries.

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType
from pyspark.sql.functions import mean, stddev
```

## Exercise 1

Create a UDF that will convert a column of strings to uppercase. Here's an example of how to define a UDF:

```python
uppercase = udf(lambda x: x.upper(), StringType())
```

## Exercise 2

Apply this UDF to a DataFrame of your choice. Here's an example of applying the UDF to a DataFrame:

```python
df = spark.createDataFrame([('John Doe',), ('Jane Doe',)], ['Name'])
df = df.withColumn('Name_Upper', uppercase(df['Name']))
df.show()
```

## Exercise 3

Create a custom transformation that will standardize a numeric column (subtract mean and divide by standard deviation). 

```python
mean_val = df.select(mean(df['numeric_column'])).collect()[0][0]
stddev_val = df.select(stddev(df['numeric_column'])).collect()[0][0]

df = df.withColumn('standardized_column', (df['numeric_column'] - mean_val) / stddev_val)
df.show()
```

# Lab 2: Window Functions

Let's start by importing the necessary libraries.

```python
from pyspark.sql.window import Window
from pyspark.sql.functions import sum as _sum, rank
```

## Exercise 1

Use a window function to calculate the running total of a numeric column in a DataFrame.

```python
windowSpec = Window.orderBy('numeric_column')
df = df.withColumn('running_total', _sum('numeric_column').over(windowSpec))
df.show()
```

## Exercise 2

Use a window function to rank rows based on a specific condition.

```python
windowSpec = Window.orderBy(df['numeric_column'].desc())
df = df.withColumn('rank', rank().over(windowSpec))
df.show()
```

# Lab 3: Pivot and Rollup

## Exercise 1

Perform a pivot operation on a DataFrame to create a summary report.

```python
pivotDF = df.groupBy('column1').pivot('column2').sum('numeric_column')
pivotDF.show()
```

## Exercise 2

Use rollup to create a hierarchical summary report.

```python
rollupDF = df.rollup('column1', 'column2').sum('numeric_column')
rollupDF.show()
```

Remember to replace `'numeric_column'`, `'column1'`, and `'column2'` with your actual column names. Happy coding!
