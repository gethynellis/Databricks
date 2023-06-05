## Lab/Workshop:

### Exercise 1: Ingesting Data from Azure Blob Storage

**Task:** Connect to Azure Blob Storage and ingest a sample dataset into Spark. Verify that the data has been loaded correctly.

**Instructions:**

1. Set up your Azure Blob Storage account if you haven't already. Remember to note your account name and account key.

2. Configure the connection to your Azure Blob Storage account by providing the account name and account key:

    ```python
    spark.conf.set(
        "spark.storageAccountName", "<your-storage-account-name>",
        "spark.storageAccountKey", "<your-storage-account-key>"
    )
    ```

3. Load a sample dataset from Azure Blob Storage into a Spark DataFrame:

    ```python
    df = spark.read.format("csv").option("header", "true").load("wasbs://<your-container>@<your-storage-account-name>.blob.core.windows.net/<your-file>")
    ```

4. Verify that the data has been loaded correctly by displaying the first few rows of the DataFrame:

    ```python
    df.show()
    ```

### Exercise 2: Ingesting Data from Azure Data Lake

**Task:** Connect to Azure Data Lake, ingest a different sample dataset into Spark and verify its successful ingestion.

**Instructions:**

1. Set up your Azure Data Lake Store account if you haven't already. Remember to note your account name.

2. Configure the connection to your Azure Data Lake Store account:

    ```python
    spark.conf.set("spark.datalakeAccountName", "<your-datalake-account-name>")
    ```

3. Load a sample dataset from Azure Data Lake Store into a Spark DataFrame:

    ```python
    df2 = spark.read.format("csv").option("header", "true").load("adl://<your-datalake-account-name>.azuredatalakestore.net/<your-file>")
    ```

4. Verify that the data has been loaded correctly by displaying the first few rows of the DataFrame:

    ```python
    df2.show()
    ```

### Exercise 3: Data Transformation using Spark DataFrames

**Task:** Perform various transformation operations on the ingested data using Spark DataFrames, such as filtering, aggregating, and joining.

**Instructions:**

1. Use the `filter` operation to filter out rows based on a specific condition:

    ```python
    df_filtered = df.filter(df["<column>"] == "<value>")
    ```

2. Use the `groupBy` and `count` operations to count the number of occurrences of each unique value in a specific column:

    ```python
    df_grouped = df.groupBy("<column>").count()
    ```

3. Use the `join` operation to join `df` with `df2` on a common column:

    ```python
    df_joined = df.join(df2, df["<common-column>"] == df2["<common-column>"])
    ```

### Exercise 4: Data Cleaning using Spark DataFrames and Datasets

**Task:** Identify and rectify quality issues in the dataset using Spark DataFrames and Datasets, such as dealing with missing values, duplicates, and outliers.

**Instructions:**

1. Use the `na.drop` function to remove rows with missing values:

    ```python
    df_clean = df.na.drop()
    ```

2. Use the `dropDuplicates` function to remove duplicate rows:

    ```python
    df_clean = df_clean.dropDuplicates()
    ```

3. Use the `approxQuantile` function to identify outliers:

    ```python
    quantiles = df_clean.approxQuantile("<column>", [0.25, 0.75], 0
