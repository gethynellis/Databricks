
**Lab Exercise - Apache Spark, Spark Core, Spark SQL, and Spark Streaming**

**Exercise 1: Spark Core & RDDs**

**Step 1: Initializing a Spark Context**

1. Open your Python IDE and start a new project.
2. Import the SparkContext from pyspark, initialize it and test it with a simple operation.

```python
from pyspark import SparkContext
sc = SparkContext("local", "First App")
data = sc.parallelize([1,2,3,4,5])
print(data.count())
```

**Step 2: Understanding RDD operations**

1. Create an RDD from a list of words.
2. Use map() transformation to create a new RDD that contains the length of each word.

```python
words = sc.parallelize(["scala", "java", "hadoop", "spark", "akka"])
wordLengths = words.map(lambda s: len(s))
print(wordLengths.collect())
```

**Exercise 2: Spark SQL & DataFrames**

**Step 1: Initializing a Spark Session**

1. Import the SparkSession from pyspark.sql, and initialize it.

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName('sql_demo').getOrCreate()
```

**Step 2: Creating a DataFrame**

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

1. Import the StreamingContext from pyspark.streaming, and initialize it with a batch interval of 1 second.

```python
from pyspark.streaming import StreamingContext
ssc = StreamingContext(sc, 1)
```

**Step 2: Creating a DStream**

1. For this demo, we will create a DStream that receives data from a local TCP source (localhost and port 9999). To simulate a data source, open a new terminal window, start netcat (a simple utility for reading from and writing to network connections using TCP) by typing `nc -lk 9999`.

```python
lines = ssc.socketTextStream("localhost", 9999)
```

**Step 3: Processing the DStream**

1. Define a simple processing logic - split each line into words.

```python
words = lines.flatMap(lambda line: line.split(" "))
```

**Step 4: Start Streaming**

1. Before you start streaming, you need to set up an action for the processed data.
