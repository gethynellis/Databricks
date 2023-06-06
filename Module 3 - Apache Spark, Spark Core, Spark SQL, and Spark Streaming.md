
**Lab Exercise - Apache Spark, Spark Core, Spark SQL, and Spark Streaming**

**Exercise 1: Spark Core & RDDs**

**Step 1: Initializing a Spark Context**

This step is about setting up a SparkContext. In Spark, communication occurs between a driver and executors. The driver has Spark jobs that it needs to run and these jobs are split into tasks that are submitted to the executors for completion. The results from these tasks are delivered back to the driver. Here, we're initiating a SparkContext object, which tells Spark how to access a cluster. The "local" argument means that we're running it on a local machine.

1. Open your Python IDE and start a new project.
2. Import the SparkContext from pyspark, initialize it and test it with a simple operation.

```python
from pyspark import SparkContext
sc = SparkContext("local", "First App")
data = sc.parallelize([1,2,3,4,5])
print(data.count())
```

**Step 2: Understanding RDD operations**

Resilient Distributed Datasets (RDDs) are a fundamental data structure in Spark. They are an immutable distributed collection of objects, which can be processed in parallel. This step is about creating an RDD (using sc.parallelize) from a list of words, then using a map operation to create a new RDD which contains the length of each word.

1. Create an RDD from a list of words.
2. Use map() transformation to create a new RDD that contains the length of each word.

```python
words = sc.parallelize(["scala", "java", "hadoop", "spark", "akka"])
wordLengths = words.map(lambda s: len(s))
print(wordLengths.collect())
```

**Exercise 2: Spark SQL & DataFrames**

**Step 1: Initializing a Spark Session**

This step involves creating a SparkSession - the entry point to any Spark functionality. When you create a SparkSession, SparkContext will be automatically created.

1. Import the SparkSession from pyspark.sql, and initialize it.

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName('sql_demo').getOrCreate()
```

**Step 2: Creating a DataFrame**

Here we create a DataFrame, which is a distributed collection of data organized into named columns. It is conceptually equivalent to a table in a relational database. We create the DataFrame from a list of tuples, where each tuple represents a row of data.

1. Create a simple DataFrame from a list of tuples.
2. Show the DataFrame.

```python
data = [("James","Smith","USA",30),
        ("Michael","Rose","USA",33),
        ("Robert","Williams","USA",24)]
columns = ["FirstName","LastName","Country","Age"]
df = spark.createDataFrame(data=data, schema = columns)
df.show()
```

**Step 3: Running SQL Queries**

DataFrames can be registered as temporary tables in Spark SQL, and SQL queries can be executed against them. In this step, we register our DataFrame as a temporary table and then run a simple SQL query to select records where the age is greater than 25.

1. Register the DataFrame as a SQL temporary view.
2. Use the sql() method of your SparkSession to execute SQL queries.

```python
df.createOrReplaceTempView("PEOPLE")
result = spark.sql("SELECT * FROM PEOPLE where Age > 25")
result.show()
```

**Exercise 3: Spark Streaming**

This part of the lab requires a real-time data source for a meaningful demonstration, which is beyond the scope of this setup. However, we'll use a simple example of processing a data stream.

**Step 1: Initializing a StreamingContext**

The StreamingContext is the main entry point for all streaming functionality. We create a local StreamingContext with two execution threads, and a batch interval of one second.

1. Import the StreamingContext from pyspark.streaming, and initialize it with a batch interval of 1 second.

```python
from pyspark.streaming import StreamingContext
ssc = StreamingContext(sc, 1)
```

**Step 2: Creating a DStream**

A DStream (Discretized Stream) is a sequence of data arriving over time. Here, we're creating a DStream that connects to a TCP source on localhost on port 9999.

1. For this demo, we will create a DStream that receives data from a local TCP source (localhost and port 9999). To simulate a data source, open a new terminal window, start netcat (a simple utility for reading from and writing to network connections using TCP) by typing `nc -lk 9999`.

```python
lines = ssc.socketTextStream("localhost", 9999)
```

**Step 3: Processing the DStream**

This step defines the actual processing logic. We want to split each line into multiple words. The flatMap operation is a transformation operation that applies the lambda function to each element of the DStream to generate multiple output elements.



1. Define a simple processing logic - split each line into words.

```python
words = lines.flatMap(lambda line: line.split(" "))
```

**Step 4: Start Streaming**

In this step, you would typically start the computation with ssc.start() and await termination with ssc.awaitTermination(). Before starting the stream, we'd set up an action for the processed data. Actions trigger the execution of the data processing, and in a real-world scenario, this might be saving the data out to a database or filesystem. However, this part is omitted here due to the scope of the setup.

1. Before you start streaming, you need to set up an action for the processed data.
