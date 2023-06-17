## Lab/Workshop:

### Exercise 1: Ingesting Data from Azure Blob Storage

**Task:** Connect to Azure Blob Storage and ingest a sample dataset into Spark. Verify that the data has been loaded correctly.

**Instructions:**

1. Set up your Azure Blob Storage account if you haven't already. Remember to note your account name and account key.

   You can follow these steps to set up the account up your Azure Blob Storage account:

 - Visit the Azure Portal: Go to the Azure portal website (https://portal.azure.com) and sign in with your Azure account credentials.
- Create a new storage account: Once you're logged in, navigate to the Azure portal dashboard and click on the "Create a resource" button.

- Search for "Storage account": In the search bar at the top of the Azure portal, type "Storage account" and select the "Storage account - blob, file, table, queue" option from the search results.

- Choose subscription and resource group: In the "Create storage account" page, select the appropriate subscription from the drop-down menu and choose or create a resource group to contain your storage account.

- Specify storage account settings: Provide a unique name for your storage account, keeping in mind that the name must be globally unique across all Azure accounts. Select the appropriate location for your storage account and choose the performance tier (Standard or Premium).

- Configure advanced settings: Expand the "Advanced" tab to configure additional settings. Here you can choose the account kind, replication option, and access tier. For general purpose storage, the default options are typically sufficient.

- Secure your storage account: In the "Networking" tab, you can configure network access and firewall rules for your storage account. Adjust these settings based on your specific requirements.

- Review and create the storage account: Double-check all the settings you've specified, ensuring that they match your requirements. Once you're satisfied, click on the "Review + create" button.

-  Create the storage account: After clicking "Review + create," Azure will validate your settings and create the storage account. Wait for the deployment process to complete.

-  Retrieve your account name and account key: Once the storage account is successfully created, navigate to the account's overview page. You can find the account name and account key in the "Settings" section of the overview.

- Note your account name and account key: Make sure to write down or securely store the account name and account key.
- **These credentials are essential for accessing and managing your Azure Blob Storage account programmatically.**

With these steps, you should be able to set up your Azure Blob Storage account successfully and have the necessary account name and account key information readily available.

3. Configure the connection to your Azure Blob Storage account by providing the account name and account key:

    ```python
    spark.conf.set(
        "spark.storageAccountName", "<your-storage-account-name>",
        "spark.storageAccountKey", "<your-storage-account-key>"
    )
    ```

4. Load a sample dataset from Azure Blob Storage into a Spark DataFrame:

    ```python
    df = spark.read.format("csv").option("header", "true").load("wasbs://<your-container>@<your-storage-account-name>.blob.core.windows.net/<your-file>")
    ```

5. Verify that the data has been loaded correctly by displaying the first few rows of the DataFrame:

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
