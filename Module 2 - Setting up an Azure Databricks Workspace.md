# Lab Exercise: Setting up an Azure Databricks Workspace

## Objective: Learn how to create and configure an Azure Databricks Workspace.

### Tools Required:

Azure subscription
Estimated Time: 30 minutes

### Instructions:

#### Step 1: Sign into Azure Portal.

Navigate to the Azure portal (portal.azure.com) and sign in with your Azure account.

#### Step 2: Create a Databricks workspace.

In the Azure portal, click on **Create a resource** in the left-hand menu.
In the **New** window, search for **Databricks.**
In the search results, select **Azure Databricks** and then click **Create Azure Databricks Service.**

#### Step 3: Configure your Databricks workspace.

In the **Workspace name** field, provide a name for your workspace.
Choose your Azure **Subscription** and **Resource Group** where you want to deploy the workspace.
Select the **Location** of your workspace.
For the **Pricing Tier,** choose between Standard and Premium. For this exercise, select **Standard.**
Enable **Managed Private Network** if you want a secure, private network. This can be disabled for now for simplicity.
Step 4: Deploy the workspace **Review and Create** then **Create**.

Review your configuration settings, then click **Review + Create.**
After the validation passed, click **Create** to deploy your Databricks workspace.

The provisioning process will take several minutes to complete. Be patient.

#### Step 5: Access your Databricks workspace.

After the deployment is completed, go to the resource you just created. Click **Go to Resource**
Click on the **Launch Workspace** button to open your Databricks workspace.
Congratulations, you have successfully created an Azure Databricks Workspace!

Remember, the workspace is the first step to developing big data solutions. The next step would be to create clusters and notebooks within the workspace to run your big data workloads.

# Lab Exercise: Setting up a Databricks Cluster

## Objective: Learn how to create and configure a Databricks cluster.

### Tools Required:

- Azure subscription
- Databricks workspace
- Estimated Time: 20 minutes

### Instructions:

#### Step 1: Log into your Azure Databricks workspace.

Navigate to the Azure portal (portal.azure.com) and sign in with your Azure account.
Locate your Databricks workspace and click on the **Launch Workspace** button.

#### Step 2: Create a new Databricks cluster.

From your workspace, go to the left-hand menu and click on **Clusters.**
Click on the **Create Cluster** button.

#### Step 3: Configure your new cluster.

Provide a name for your cluster in the **Cluster Name** field.
Select the Databricks Runtime Version from the dropdown. (As a beginner, you can use the latest version.)
Choose the Worker Type. For this lab, you can select **Standard_DS3_v2,** which is suitable for general purposes.
Set the number of Worker Nodes. You can specify a minimum and maximum. For this lab, you can start with a minimum of 2 and a maximum of 8. This allows the cluster to auto-scale based on demand.
Change the **terminate after** value to read **10** minutes

#### Step 4: Configure advanced options (optional).

In the advanced options, you can configure Spark settings, environment variables, and init scripts. However, for this basic lab, you can skip these advanced settings.
If required, you can enable auto-termination which will shut down your cluster after a period of inactivity. For example, setting this to '120' will automatically terminate the cluster if it's inactive for 120 minutes. This can help to manage costs.

#### Step 5: Create the cluster.

Review the cluster configuration and click on the **Create Cluster** button.

#### Step 6: Monitor your cluster.

Once the cluster is created, it should be listed under **Compute** in your workspace.
You can see the current state of your cluster. It might take a few minutes for the cluster to start.

####  Step 7: Test the cluster.

Create a new notebook (from the left-hand menu, select **New +**  > **Notebook**).
Attach your new notebook to the cluster you created (using the **Attached/Detached** option in the notebook).
Write some simple Spark code to test your cluster, such as a PySpark command to create a dataframe.


# Lab Exercise: Using a Notebook on an Azure Databricks Workspace and Cluster

## Objective: Learn how to create and use a notebook within an Azure Databricks Workspace and Cluster.

### Tools Required:

Azure subscription
Databricks workspace
Databricks cluster
Estimated Time: 30 minutes

### Instructions:

#### Step 1: Sign into Azure Portal and access your Databricks workspace.

Navigate to the Azure portal (portal.azure.com) and sign in with your Azure account.
Locate your Databricks workspace and click on the **Launch Workspace** button.

####  Step 2: Create a new notebook.

From your workspace, go to the left-hand menu and click on **Workspace.**
Click on the **Create** button, then select **Notebook.**
Provide a name for your notebook in the **Name** field.
Choose the language for your notebook (Python, Scala, SQL, or R).
Click **Create.**

#### Step 3: Attach your notebook to a cluster.

Open your notebook.
Click on the **Clusters** button in the upper left corner of the notebook.
Select your previously created cluster from the drop-down list.

#### Step 4: Write and run some code in your notebook.

Click on the first cell in your notebook and write some simple code. For example, if you selected Python as your language, you could write: print(**Hello, Databricks!**).
Run the cell by clicking the **Run Cell** button, or by pressing Shift + Enter.
Observe the output of your cell beneath it.

#### Step 5: Create and run additional cells.

Create a new cell by clicking the **+ Cell** button.
In the new cell, write some code and run the cell as before.
Repeat this process to add as many cells as you wish.

Congratulations, you have successfully created and used a notebook within your Azure Databricks Workspace and Cluster!

Remember to detach the notebook from the cluster when you are done, and to terminate the cluster to avoid unnecessary charges.


**Note:** Databricks services will incur charges, so remember to delete the resources once you're done with your work to avoid unnecessary costs.
