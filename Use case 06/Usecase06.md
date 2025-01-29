# Usecase 06 - Deploying chat app on Azure Container Apps with PostgreSQL Flexible Server

**Objective:**

- To configure the development environment on Windows by installing Azure CLI, Node.js, assigning Azure subscription roles, starting
  Docker Desktop, and enabling Visual Studio Code with Dev Containers extension.

- To deploy and test Custom Chat Application with PostgreSQL and OpenAI  on Azure.

![](./media/image1.jpeg)

In this use case, you will set up a comprehensive development environment, deploy a chat application integrated with PostgreSQL, and
verify its deployment on Azure. This involves installing essential tools like Azure CLI, Docker, and Visual Studio Code ( we have already done it for you on host env ), configuring user roles in Azure, deploying the application using Azure Developer CLI, and interacting with the deployed resources to ensure functionality.

**Key technologies used** -- Python, FastAPI, Azure OpenAI models, Azure Database for PostgreSQL and azure-container-apps,ai-azd-templates.

**Estimated duration** -- 45 minutes

**Lab Type:** Instructor Led

**Pre-requisites:**

GitHub account -- You are expected to have your own GitHub login credentials. If you do not have, please create one from here
- +++https://github.com/signup?user_email=&source=form-home-signupobjectives+++

## Exercise 1 : Provision, deploy the application ,and test it from the browser

### Task 1 : Run the Docker

1.  On the Desktop, double-click on **Docker Desktop**.

![](./media/image6.jpeg)

2.  Run the Docker Desktop.

![](./media/image7.jpeg)

### Task 2: Register Service provider

1.  Open your browser, open Azure   portal +++https://portal.azure.com+++.  Sign in with your Azure slice account(Azure Credentials) available under the instructions/Resources section of your host environment.

![](./media/image2.jpeg)

2.  Click on **Subscription** tile.

![](./media/image8.png)

2.  Click on the subscription name.

![](./media/image9.png)

3.  Click on **Settings - \> Resource provider** from left navigation
    menu.

![](./media/image10.png)

4.  Type +++Microsoft.AlertsManagement+++ and press enter. Select it and then click on **Register**.

![](./media/image11.png)

![](./media/image12.png)

### Task 3: Open development environment

1.  Open your browser, navigate to the address bar, and type or paste the following  URL: +++https://github.com/technofocus-pte/rag-postgres-openai-python-CSTesting.git+++ 

![](./media/image13.jpeg)

2.  Click on **fork** to fork the repo. Give a unique name to the repo and click on **Create repo** button.

![](./media/image14.jpeg)

![](./media/image15.jpeg)

3.  Click on **Code -> Codespaces -> Codespaces+**

![](./media/image16.jpeg)

4.  Wait for the Codespaces environment to set up.It takes a few minutes to setup completely

![](./media/image17.jpeg)

![](./media/image18.jpeg)

### Task 5: Provision Services and deploy application to Azure

1.  Run the following command on the Terminal. It generates the code to copy. Copy the code and press Enter.

+++azd auth login+++

![](./media/image19.png)

2.  The default browser opens to enter the generated code to verify. Enter the code and click **Next**.

![](./media/image20.png)

3.  Sign in with your Azure credentials.

![](./media/image21.png)

4.  To create an environment for Azure resources, run the following Azure Developer CLI command.It asks you to enter the environment name
    .Enter any name of your choice and press enter (eg :+++ragpgpy+++)

**Note:** When creating an environment, ensure that the name consists of lowercase letters.

+++azd env new+++

![](./media/image22.png)

5.  Run the following Azure Developer CLI command to provision the Azure resources and deploy the code.

+++azd provision+++

![](./media/image23.png)

6.  When prompted, select a **subscription** to create the resources and select the region closest to your location; in this lab, we have
    chosen the **East US2** region.

![](./media/image24.png)

7.  It will prompt you “**Enter a value for the 'existingResourceGroupName' infrastructure parameter:**” Enter the
    resource group - +++@lab.CloudResourceGroup(ResourceGroup1).Name+++

![](./media/image25.png)

8.  When prompted, **enter a value for the 'openAILocation' infrastructure parameter** select the region closest to your
    location; in this lab, we have chosen the **North Central US** region

![](./media/image26.png)

9.  Provisioning resources will take around 5 - 10 min. Click **Yes** if prompted.

![](./media/image27.png)

10. Wait for the template to provision all resources successfully.

![](./media/image28.png)

11. Run the below command to set the resource group

+++azd env set AZURE_RESOURCE_GROUP {your resource group name}+++

![](./media/image29.png)

12. Run the below command to deploy the app to Azure.

+++azd deploy+++

![](./media/image30.png)

13. Wait for the deployment to complete. Deployment takes \<5

![](./media/image31.png)

14. Click on the deployed web app endpoint link.

![](./media/image32.png)

15. Click on **Open**. It opens a new tab with the app

![](./media/image33.png)

16. The app opens.

![](./media/image34.png)

### Task 6: Use chat app to get answers from files

1.  In the **RAG on database |OpenAI+PoastgreSQL** web app page, **click on Best shoe for hiking?** button and observe the output

![](./media/image35.png)

![](./media/image36.png)

2.  Click on the **clear chat.**

![](./media/image37.png)

3.  In the **RAG on database |OpenAI+PoastgreSQL** web app page, click on **Climbing gear cheaper than \\$30** button and observe the
    output

![](./media/image38.png)

![](./media/image39.png)

4.  Click on the **clear chat.**

### Task 7: Verify deployed resources in the Azure portal

1.  On the Home page of the Azure portal, click on **Resource Groups**.

![](./media/image40.png)

2.  Click on your resource group name

![](./media/image41.png)

3.  Make sure the below resource got deployed successfully

    - Container App

    - Application Insights

    - Container Apps Environment

    - Log Analytics workspace

    - Azure OpenAI

    - Azure Database for PostgreSQL flexible server

    - Container registry

![](./media/image42.png)

4.  Click on **Azure OpenAI** resource name.

![](./media/image43.png)

5.  On **Overview** in the left navigation menu, click **Go to Azure AI Foundry portal** and select to open a new tab.

![](./media/image44.png)

6.  Click on **Shared resources -\>** **Deployments** from left navigation menu and make sure **gpt-35-turbo**, **text-embedding-ada-002** should be deployed  successfully

![](./media/image45.png)

### Task 8: Clean up all the resources

To clean up all the resources created by this sample:

1.  Switch back to **Azure portal -\> Resource group- \> Resource group name.**

![](./media/image46.png)

2.  Select all the resources and then click on Delete as shown inthe  below image. (**DO NOT DELETE** resource group)

![](./media/image47.png)

3.  Type ``delete`` on the text box and then click on **Delete**.

![](./media/image48.png)

4.  Confirm the deletion by clicking on **Delete**.

![](./media/image49.png)

5.  Switch back to Github portal tab and refresh the page.

![](./media/image50.png)

6.  Click on Code, select the branch created for this lab , and click on **Delete**.

![](./media/image51.png)

7.  Confirm the branch deletion by clicking on **Delete** button.

![](./media/image52.png)

>**Summary:**:This use case walks you through deploying a chat application with PostgreSQL and OpenAI on Azure, focusing on cloud-based application deployment and management. you’ve set up the development environment, installed necessary tools like Azure CLI, configured Azure resources using Azure Developer CLI, and deployed the application to Azure Container Apps.
