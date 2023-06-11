**Exercise Steps**

**Step 1: Accessing Azure Databricks**

1. Log in to your Azure account at [Azure Portal](https://portal.azure.com/).
2. In the left-hand menu, click on "All services".
3. In the "All services" search bar, type "Databricks", and select "Azure Databricks".

**Step 2: Create a New Databricks Workspace**

1. If you don't have an existing Databricks workspace, click on the "+ Add" button to create a new one.
2. Fill out the form with the necessary details like subscription, resource group, workspace name, location, and pricing tier.
3. Click "Review + Create" then "Create" to start the deployment process.
4. Once the deployment is complete, go to the resource.

**Step 3: Launch Databricks Workspace and Create a New Notebook**

1. Click "Launch Workspace" to open your Databricks workspace.
2. In the Databricks portal, go to "Workspace" -> "Users" -> your_username.
3. Click on the downward arrow next to your username, select "Create" -> "Notebook".
4. Name the notebook, select your desired language (Python or Scala), and click "Create".

**Step 4: Load a Dataset**

1. In the notebook, use Spark's data loading functions to load a large public dataset. 
    ```
    df = spark.read.format('csv').option('header', 'true').load('<dataset_url>')
    ```

**Step 5: Write a Data Processing Pipeline**

1. Implement a data processing pipeline using Spark transformations and actions. For example, a pipeline could filter rows, group data, and calculate aggregate statistics.

**Step 6: Run the Pipeline**

1. Run the pipeline by executing the notebook cells.
2. Note the time it takes to complete and the resources it uses.

**Step 7: Open Spark UI and Inspect the Application**

1. Go to the "Clusters" menu in the Databricks sidebar.
2. Click on the cluster running your notebook, and then click on "Apps".
3. Click on "Spark UI" next to your active application.

**Step 8: Identify Performance Bottlenecks**

1. In Spark UI, go to the "Stages" tab to inspect the stages and tasks of the application.
2. Look for stages with high task durations, high shuffle read/write, or high I/O, which could indicate performance bottlenecks.

**Step 9: Optimize the Pipeline**

1. Based on the identified bottlenecks, optimize the pipeline. This could involve:
    - Changing transformations to more efficient ones.
    - Using broadcast variables for small datasets in a join operation.
    - Adjusting the resources allocated to the Spark application.

**Step 10: Run the Optimized Pipeline and Compare**

1. Run the optimized pipeline in the Databricks notebook.
2. Compare the runtime, resource usage, and cost to the original pipeline. 

**Note:** Be sure to stop the Databricks cluster after the exercise to avoid unnecessary costs.
