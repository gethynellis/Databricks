So we'll try some machine learning

The example is using a dataset called the "Boston Housing dataset", which contains information about various housing blocks in the city of Boston. Each row in the dataset represents a different housing block and contains various features about the block like crime rate, average number of rooms per dwelling, full-value property tax rate per $10,000, etc.

The goal of the model in this example is to predict the median value of owner-occupied homes in those blocks. This is represented by the 'PRICE' column in the dataset. The model is trained using the various features provided to predict this price.

This is a regression problem because the target variable, 'PRICE', is a continuous variable. Linear Regression is a common algorithm used for such problems. It tries to fit a line that best represents the relationship between the features and the target variable, in this case, the 'PRICE'. The performance of the model is then evaluated by using the Root Mean Squared Error (RMSE) metric on the test data.

**Step 1: Set Up Azure Databricks Environment**

In this step, you are logging in to the Azure portal and creating a Databricks workspace. Azure Databricks is an Apache Spark-based analytics platform optimized for Azure's cloud services. It provides a workspace environment where you can run analysis and create notebooks for collaborative coding and data science. The output of this step will be a Databricks workspace ready for you to launch and start creating a new notebook.

1. Log in to the Azure portal.
2. Create a new Databricks workspace.
3. Once the workspace is ready, launch it, and create a new notebook.

**Step 2: Import the Data**

Here, the Boston Housing dataset is being loaded from the sklearn datasets library, then converted into a pandas DataFrame. The DataFrame df contains the features of the dataset (like crime rate, average number of rooms per dwelling, etc.) as columns. An additional column 'PRICE' is appended which is the target variable (median value of owner-occupied homes). This pandas DataFrame is then converted to a Spark DataFrame using spark.createDataFrame(df). The show() function is used to print the first 20 records of the DataFrame.


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

In this step, all the features are combined into a single feature vector using the VectorAssembler from PySpark's ML library. This is necessary as MLlib requires data to be passed in this format where each row is a tuple of a label and features. df_kmeans.show() will display the transformed data, with an additional column named "features" that contains the feature vector.

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

The data is split into training and test sets, with 70% of the data going to the training set and 30% going to the test set. The purpose of this is to provide a separate dataset (test set) to evaluate the model's performance after it has been trained on the training set. The show() function is again used to print the first 20 records of each DataFrame.

```python
# Split the data
train_data, test_data = df_kmeans.randomSplit([0.7, 0.3])

train_data.show()
test_data.show()
```

**Step 5: Build the Linear Regression Model**

Here, a Linear Regression model is being initialized and then trained (fitted) on the training data. The featuresCol and labelCol parameters specify the names of the features and label columns respectively. After training, the coefficients and intercept of the regression line are printed. These give insight into the learned model. The coefficients represent the relationship between the features and the target variable, and the intercept is the predicted value when all feature values are zero.

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

In this final step, the trained model is used to make predictions on the test data. The RegressionEvaluator is initialized and used to compute the Root Mean Squared Error (RMSE) of the model. RMSE is a popular metric to evaluate regression models, which tells you how close the observed data points are to the model’s predicted values. A lower value of RMSE is better as it means the error is less. This RMSE value is printed as the final output of this exercis

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
