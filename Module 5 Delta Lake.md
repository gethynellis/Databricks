**Lab Exercise**

**Exercise 1: Setting up the Environment**
Objective: To set up a working environment using Apache Spark and Delta Lake.
Tasks: Install Apache Spark and Delta Lake on the system. Verify the installation by running a simple command.

**You only need to create a new cluster if you don't one that exists already. If you have an Azure Databricks space and cluster created only need to open the Notebook**

1. Open Azure Databricks in your browser and navigate to your workspace.
2. Start a new cluster or use an existing one. *You should have a cluster provisioned. You may need to start it from the **Compute Section** of the workspace.*
3. Install the required libraries for Delta Lake (if they're not already installed). To do this, navigate to your cluster settings, select "Libraries" and click "Install New". Choose "Maven" and enter the coordinates for Delta Lake, then click "Install".
4. Open a new notebook and attach it to your cluster.
5. Run the following command to verify that Delta Lake is installed correctly: `spark.sql("SELECT 'Hello, Delta Lake!'").show()`. If everything is set up correctly, this should print 'Hello, Delta Lake!' in your notebook.

**Exercise 2: Creating a Delta Table**
Objective: To understand how to create Delta tables in Delta Lake.
Tasks: Create a Delta table using sample data. Write a command to view the schema and the contents of the table. You will use a Notebook and the language should be Scala

1. First, let's create some sample data. Run the following commands:
```scala
import spark.implicits._

val data = Seq(("James", "Smith", "USA"), ("Michael", "Rose", "USA"), ("Robert", "Williams", "USA"))
val df = data.toDF("firstname", "lastname", "country")
```
2. Now, let's create a Delta Table from this DataFrame. Run the following command:
```scala
df.write.format("delta").save("/delta-table")
```
3. Finally, let's view the schema and the contents of the table. Run the following commands:
```scala
val df = spark.read.format("delta").load("/delta-table")
df.printSchema()
df.show()
```

**Exercise 3: Working with Delta Table**
Objective: To get familiar with operations on Delta tables.
Tasks: Perform CRUD operations (Create, Read, Update, Delete) on the created Delta table. Write commands to carry out these operations.

1. Let's insert a new row into the table. Run the following commands:
```scala
import org.apache.spark.sql.functions._

val newData = Seq(("John", "Doe", "USA")).toDF("firstname", "lastname", "country")
newData.write.format("delta").mode("append").save("/delta-table")
```
2. Now, let's update a row in the table. We'll update John Doe's country to Canada. Run the following commands:
```scala
val deltaTable = DeltaTable.forPath(spark, "/delta-table")

deltaTable.update(
  condition = expr("firstname = 'John' and lastname = 'Doe'"),
  set = Map("country" -> lit("Canada"))
)
```
3. Let's read the table again to see the changes:
```scala
val df = spark.read.format("delta").load("/delta-table")
df.show()
```
4. Now, let's delete a row from the table. We'll delete the row for James Smith. Run the following commands:
```scala
deltaTable.delete(condition = expr("firstname = 'James' and lastname = 'Smith'"))
```
5. Read the table again to see the changes:
```scala
val df = spark.read.format("delta").load("/delta-table")
df.show()
```

**Exercise 4: Exploring Time Travel**
Objective: To understand the concept of Time Travel in Delta Lake.
Tasks: Make changes to the Delta table and revert back to the previous version of the table using the Time Travel feature.



1. Let's make another change to the table. We'll add a row for Jane Smith. Run the following commands:
```scala
val newData = Seq(("Jane", "Smith", "Canada")).toDF("firstname", "lastname", "country")
newData.write.format("delta").mode("append").save("/delta-table")
```
2. Let's revert back to the previous version of the table using Time Travel. Run the following command:
```scala
val df = spark.read.format("delta").option("versionAsOf", 0).load("/delta-table")
df.show()
```

**Exercise 5: Problem-solving**
Objective: To apply the knowledge gained to solve a real-world problem.
Tasks: Given a problem statement, use the learned Delta Lake features to solve it.

For this exercise, let's say the problem statement is as follows: We have a dataset of customer purchases, and we want to create a Delta Table from this dataset. The dataset is updated daily with new purchases. We want to create a system that can handle these updates and allow us to view the dataset as it was on any given day.

Your task is to design and implement a solution to this problem using Delta Lake. Here are some hints:

- You'll need to create a Delta Table from the dataset.
- To handle daily updates, you can append the new data to the Delta Table.
- To view the dataset as it was on any given day, you can use the Time Travel feature.
