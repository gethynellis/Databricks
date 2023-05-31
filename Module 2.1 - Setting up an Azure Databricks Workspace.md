# Lab Exercise: Setting up an Azure Databricks Workspace

## Objective: Learn how to create and configure an Azure Databricks Workspace.

### Tools Required:

Azure subscription
Estimated Time: 30 minutes

### Instructions:

#### Step 1: Sign into Azure Portal.

Navigate to the Azure portal (portal.azure.com) and sign in with your Azure account.

#### Step 2: Create a Databricks workspace.

In the Azure portal, click on "Create a resource" in the left-hand menu.
In the "New" window, search for "Databricks."
In the search results, select "Azure Databricks" and then click "Create."

#### Step 3: Configure your Databricks workspace.

In the "Workspace name" field, provide a name for your workspace.
Choose your Azure "Subscription" and "Resource Group" where you want to deploy the workspace.
Select the "Location" of your workspace.
For the "Pricing Tier," choose between Standard and Premium. For this exercise, select "Standard."
Enable "Managed Private Network" if you want a secure, private network. This can be disabled for now for simplicity.
Step 4: Deploy the workspace.

Review your configuration settings, then click "Review + Create."
After the validation passed, click "Create" to deploy your Databricks workspace.

#### Step 5: Access your Databricks workspace.

After the deployment is completed, go to the resource you just created.
Click on the "Launch Workspace" button to open your Databricks workspace.
Congratulations, you have successfully created an Azure Databricks Workspace!

Remember, the workspace is the first step to developing big data solutions. The next step would be to create clusters and notebooks within the workspace to run your big data workloads.

**Note:** Databricks services will incur charges, so remember to delete the resources once you're done with your work to avoid unnecessary costs.
