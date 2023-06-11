Sure, here's a step-by-step guide on how to use MLlib for preparing a dataset and building a simple linear regression model using Azure Databricks:

**Step 1: Set Up Azure Databricks Environment**

1. Log in to the Azure portal.
2. Create a new Databricks workspace.
3. Once the workspace is ready, launch it, and create a new notebook.

**Step 2: Import the Data**

We will use the Boston Housing dataset available in the sklearn datasets for this exercise.

```python
from sklearn.datasets import load_boston
import pandas as pd

# Load the dataset
boston = load_boston()
df = pd.DataFrame(boston.data, columns = boston.feature_names)

# Add target variable to the DataFrame
df['PRICE'] = boston.target

# Convert the pandas DataFrame into a Spark DataFrame
spark_df = spark.createDataFrame(df)
spark_df.show()
```

**Step 3: Data Preparation**

In this step, we convert all the features into a single feature vector which will be passed into the model.

```python
from pyspark.ml.feature import VectorAssembler

# Define the input columns
input_cols = spark_df.columns[:-1]

# Initialize the VectorAssembler
vec_assembler = VectorAssembler(inputCols=input_cols, outputCol="features")

# Transform the data
df_kmeans = vec_assembler.transform(spark_df)

df_kmeans.show()
```

**Step 4: Split the Data into Training and Test sets**

```python
# Split the data
train_data, test_data = df_kmeans.randomSplit([0.7, 0.3])

train_data.show()
test_data.show()
```

**Step 5: Build the Linear Regression Model**

```python
from pyspark.ml.regression import LinearRegression

# Initialize the Linear Regression model
lr = LinearRegression(featuresCol='features', labelCol='PRICE')

# Fit the model on the training data
lr_model = lr.fit(train_data)

# Print the coefficients and intercept for linear regression
print("Coefficients: " + str(lr_model.coefficients))
print("Intercept: " + str(lr_model.intercept))
```

**Step 6: Evaluate the Model**

```python
# Get the predictions
predictions = lr_model.transform(test_data)

from pyspark.ml.evaluation import RegressionEvaluator

# Initialize the RegressionEvaluator
evaluator = RegressionEvaluator(predictionCol="prediction", labelCol="PRICE", metricName="rmse")

# Calculate RMSE
rmse = evaluator.evaluate(predictions)
print("Root Mean Squared Error (RMSE) on test data = %g" % rmse)
```

This lab exercise provides a basic understanding of how to use MLlib for machine learning tasks. Remember to explore additional steps and techniques such as data cleaning, feature engineering, hyperparameter tuning, and cross-validation for a more robust machine learning workflow.
