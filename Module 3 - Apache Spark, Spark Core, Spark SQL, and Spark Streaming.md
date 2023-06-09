
## Lab Exercise - Apache Spark, Spark Core, Spark SQL, and Spark Streaming ##

We will use a Databricks notebook to run the code in this example, You need to create a Notebook before you can complete the exercises

### Create a Notebook
To create an Azure Databricks notebook, follow these steps, **(note it is very likely that most of these steps you will have completed already, if you have completed them simply move to the next step)**:


To create an Azure Databricks notebook, follow these steps:

**1. Sign in to the Azure portal**

Firstly, you need to have an Azure account and be able to sign in to the Azure portal. If you don't have an account, you can create one. 

**2. Create an Azure Databricks workspace**

- In the Azure portal, click on 'Create a resource'.
- In the search bar, type 'Azure Databricks'.
- Select 'Azure Databricks' from the search results.
- Click 'Create' and you'll be prompted to fill in details for your workspace. 
- Select your subscription.
- Create a new resource group or use an existing one.
- Name your workspace, select the region, and select the pricing tier.
- Click 'Review + Create', then click 'Create'.

**3. Launch Azure Databricks workspace**

- Go to the Azure portal's home page, click on 'Resource groups', then click on the resource group where you created your Databricks workspace.
- Click on the Databricks workspace.
- Click on 'Launch Workspace' at the top right corner of the screen.

**4. Create a notebook**

- Once you're in the Databricks workspace, on the left-hand menu, click 'Workspace'.
- Click on 'Users' and then click on your user.
- Right-click your user folder and select 'Create' > 'Notebook'.
- Enter a name for the notebook.
- Choose the language (Python, Scala, SQL, or R) from the drop-down menu.
- Click 'Create'.

Now you've created a notebook and you can begin writing code. Remember to attach your notebook to a cluster in order to execute commands. You can do this by clicking 'Detached' at the top-left corner of the notebook and then selecting a cluster.


**Lab Exercise - Azure Databricks with Spark, Spark Core, Spark SQL, and Structured Streaming**

## Exercise 1: Spark Core & RDDs ##

Step 1: Initializing a Spark Context

In Azure Databricks, a SparkContext is already created for you and is named "sc".

Use this existing SparkContext to run a simple operation.
```python
data = sc.parallelize([1,2,3,4,5])
print(data.count())
```

Step 2: Understanding RDD operations

This step is about creating an RDD from a list of words and using a map operation to create a new RDD which contains the length of each word.

```python
words = sc.parallelize(["scala", "java", "hadoop", "spark", "akka"])
wordLengths = words.map(lambda s: len(s))
print(wordLengths.collect())
```

Exercise 2: Spark SQL & DataFrames

Step 1: Initializing a Spark Session

In Azure Databricks, a SparkSession is also pre-configured for you, named "spark".

Step 2: Creating a DataFrame

Here we create a DataFrame from a list of tuples, each representing a row of data.

```python
data = [("James","Smith","USA",30),
        ("Michael","Rose","USA",33),
        ("Robert","Williams","USA",24)]
columns = ["FirstName","LastName","Country","Age"]
df = spark.createDataFrame(data=data, schema = columns)
display(df)
```

Step 3: Running SQL Queries

You can register the DataFrame as a temporary view and execute SQL queries against it. Here, we select records where the age is greater than 25.

```python
df.createOrReplaceTempView("PEOPLE")
result = spark.sql("SELECT * FROM PEOPLE where Age > 25")
display(result)
```

Exercise 3: Structured Streaming in Databricks

Step 1: Initializing a Streaming DataFrame

Instead of a StreamingContext, we're going to use Spark's structured streaming to create a streaming DataFrame. Here we read from a socket source on localhost and port 9999. Please note that Azure Databricks does not allow you to read from localhost, so you'll have to replace "localhost" with the correct IP address.

```python
lines = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load()
```

Step 2: Processing the Streaming DataFrame

This step defines the actual processing logic. We want to split each line into multiple words. This is achieved by using the `split` function from `pyspark.sql.functions`.

```python
from pyspark.sql.functions import split, explode
words = lines.select(explode(split(lines.value, " ")).alias("word"))
```

Step 3: Start Streaming

In this step, you start the computation with `writeStream.start()`. Before starting the stream, set up an action for the processed data, e.g., write the data to the console for this exercise.

```python
query = words.writeStream.outputMode("append").format("console").start()
query.awaitTermination()
```

In a real-world scenario, you might want to write the data out to a database or filesystem. The Structured Streaming programming guide provides more details on output sinks and how to write out data.
