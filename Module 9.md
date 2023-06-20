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
from pyspark.sql import SparkSession
from pyspark.sql.functions import mean, stddev

# Create a SparkSession
spark = SparkSession.builder.appName("Standardization").getOrCreate()

# Create DataFrame
data = [("James", "Sales", 3000), 
        ("Michael", "Sales", 4600), 
        ("Robert", "Sales", 4100), 
        ("Maria", "Finance", 3000), 
        ("James", "Sales", 3000), 
        ("Scott", "Finance", 3300), 
        ("Jen", "Finance", 3900), 
        ("Jeff", "Marketing", 3000), 
        ("Kumar", "Marketing", 2000), 
        ("Saif", "Sales", 4100)]

df = spark.createDataFrame(data, ["Employee Name", "Department", "Salary"])

# Calculate mean and standard deviation
mean_val = df.select(mean(df['Salary'])).collect()[0][0]
stddev_val = df.select(stddev(df['Salary'])).collect()[0][0]

# Perform standardization
df = df.withColumn('standardized_salary', (df['Salary'] - mean_val) / stddev_val)
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
windowSpec = Window.orderBy('Salary')

# Add running total column
df = df.withColumn('running_total', _sum('Salary').over(windowSpec))
df.show()
```

## Exercise 2

Use a window function to rank rows based on a specific condition.

```python
windowSpec = Window.orderBy(df['Salary'].desc())
df = df.withColumn('rank', rank().over(windowSpec))
df.show()
```

# Lab 3: Pivot and Rollup

## Exercise 1

Perform a pivot operation on a DataFrame to create a summary report.

```python
from pyspark.sql import SparkSession

# Create a SparkSession
spark = SparkSession.builder.appName("PivotExample").getOrCreate()

# Create DataFrame
data = [("James", "Sales", 3000), 
        ("Michael", "Sales", 4600), 
        ("Robert", "Sales", 4100), 
        ("Maria", "Finance", 3000), 
        ("James", "Sales", 3000), 
        ("Scott", "Finance", 3300), 
        ("Jen", "Finance", 3900), 
        ("Jeff", "Marketing", 3000), 
        ("Kumar", "Marketing", 2000), 
        ("Saif", "Sales", 4100)]

df = spark.createDataFrame(data, ["Employee Name", "Department", "Salary"])

# Pivot DataFrame
pivotDF = df.groupBy('Department').pivot('Employee Name').sum('Salary')
pivotDF.show()
```

## Exercise 2

Use roll up to create a hierarchical summary report. We will use the order by command to sort the results lowest to highest

```python
rollupDF = df.rollup('Department', 'Employee Name').sum('Salary').orderBy('sum(salary)', ascedning=False)
rollupDF.show()
```

Remember to replace `'numeric_column'`, `'column1'`, and `'column2'` with your actual column names. Happy coding!
