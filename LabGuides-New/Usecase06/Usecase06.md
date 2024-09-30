**Usecase 06 -** [**Deploying chat app on Azure Container Apps with
PostgreSQL Flexible
Server.**](https://github.com/Azure-Samples/rag-postgres-openai-python/)

**Objective:**

- To configure the development environment on Windows by installing
  Azure CLI, Node.js, assigning Azure subscription roles, starting
  Docker Desktop, and enabling Visual Studio Code with Dev Containers
  extension.

- To deploy and test Custom Chat Application with PostgreSQL and OpenAI
  on Azure.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

In this use case, you will set up a comprehensive development
environment, deploy a chat application integrated with PostgreSQL, and
verify its deployment on Azure. This involves installing essential tools
like Azure CLI, Docker, and Visual Studio Code ( we have already done it
for you on host env ), configuring user roles in Azure, deploying the
application using Azure Developer CLI, and interacting with the deployed
resources to ensure functionality.

**Key technologies used** – Python, FastAPI, Azure OpenAI models, Azure
Database for PostgreSQL and azure-container-apps,ai-azd-templates.

**Estimated duration** – 45 minutes

**Lab Type:** Instructor Led

**Pre-requisites:**

GitHub account – You are expected to have your own GitHub login
credentials. If you do not have, please create one from here -
**<https://github.com/signup?user_email=&source=form-home-signupobjectives>**

## Exercise 1: Set up environment

## Task 1: Install Azure Cli and set the policy scope to Local machine

1.  In your windows search bar, search for +++**PowerShell+++**. Open as
    **Run as administrator**. If you see the dialog box - **Do you want
    to allow this app to make changes to your device?** then click on
    the **Yes** button.

> ![A screenshot of a computer Description automatically
> generated](./media/image2.png)
>
> ![A screenshot of a computer error Description automatically
> generated](./media/image3.png)

2.  Run below commands to install stable version of winget.

$progressPreference = 'silentlyContinue'

Write-Information "Downloading WinGet and its dependencies..."

Invoke-WebRequest -Uri https://aka.ms/getwinget -OutFile
Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle

Invoke-WebRequest -Uri
https://aka.ms/Microsoft.VCLibs.x64.14.00.Desktop.appx -OutFile
Microsoft.VCLibs.x64.14.00.Desktop.appx

Invoke-WebRequest -Uri
https://github.com/microsoft/microsoft-ui-xaml/releases/download/v2.8.6/Microsoft.UI.Xaml.2.8.x64.appx
-OutFile Microsoft.UI.Xaml.2.8.x64.appx

Add-AppxPackage Microsoft.VCLibs.x64.14.00.Desktop.appx

Add-AppxPackage Microsoft.UI.Xaml.2.8.x64.appx

Add-AppxPackage Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle

> ![](./media/image4.png)

3.  Run the following command to install Azure Cli on the PowerShell

> **+++winget install microsoft.azd+++**

![A screen shot of a computer Description automatically
generated](./media/image5.png)

4.  Run the below command to set the policy to **Unrestricted** and
    enter **A** when asked to change the execution policy.

> **+++Set-ExecutionPolicy Unrestricted+++**

![A screenshot of a computer screen Description automatically
generated](./media/image6.png)

## Task 2: Assign a user as an owner of an Azure subscription

1.  Open your browser, open Azure portal
    +++<https://portal.azure.com/>*+++* Sign in with your Azure
    subscription account.

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

2.  On Home page, click on **Subscriptions** tile.

![](./media/image8.png)

3.  Click on your **subscription name**.

![](./media/image9.png)

4.  From the left menu, click on the **Access control(IAM).**Click o
    **Add -\> Add role assignment**

![](./media/image10.png)

5.  In the Role tab, select the **Privileged administrator roles** and
    select **Owner** . Click **Next**

![](./media/image11.png)

6.  Select Assign access to User group or service principal. Under
    **Members**, click **+Select members** and search for your Azure
    subscription account, select it and then click on **Select** button.

![](./media/image12.png)

7.  Click on **Next**.

![](./media/image13.png)

8.  Select **Allow user to assign all roles** radio button and then
    click on **Review +assign** .

![](./media/image14.png)

## **Task 3:** **Install Dev Containers extension**

1.  In your Windows search box, type **Visual Studio**, then click on
    **Visual Studio Code**.You can also open it form **Desktop**.

> ![](./media/image15.png)

2.  Click on **Extensions** , search for +++**Dev container+++**, select
    it and click on **Install**.

![](./media/image16.png)

## Exercise 2: Deploy the application and test it from the browser

## Task 1 : Run the Docker

1.  On the Desktop, double click on **Docker Desktop**.

+++Microsoft.AlertsManagement+++

![A screenshot of a computer Description automatically
generated](./media/image17.png)

2.  Run the Docker Desktop.

![A screenshot of a computer Description automatically
generated](./media/image18.png)

## Task 2 : Register Service provider

1.  Open a browser and go to <https://portal.azure.com> and sign in with
    your Azure subscription account.

2.  Click on the **Subscription** tile.

![](./media/image19.png)

3.  Click on subscription name.

![](./media/image20.png)

4.  Click on Resource provider from left navigation menu, type
    +++Microsoft.AlertsManagement+++ and press enter. Select it and then
    click on **Register**.

![](./media/image21.png)

![A screenshot of a computer Description automatically
generated](./media/image22.png)

## Task 3: Open development environment

1.  Open browser and go to
    <https://github.com/technofocus-pte/rag-postgres-openai-python.git>
    . Sign in with your Github account.

![A screenshot of a computer Description automatically
generated](./media/image23.png)

2.  Click on **fork** to fork the repo. Give unique name to the repo and
    click on **Create repo** button.

> ![](./media/image24.png)
>
> ![](./media/image25.png)

3.  Click on **Code -\> Codespaces -\> codespace+**

![](./media/image26.png)

4.  Wait for the codespace environment to setup .It takes few minutes to
    setup completely

![A screenshot of a computer Description automatically
generated](./media/image27.png)

![A screenshot of a computer Description automatically
generated](./media/image28.png)

## Task 4: Deploy chat app to Azure

1.  Sign in to Azure with the Azure Developer CLI. Run the following
    command on the Terminal

> **+++azd auth login+++**

![A screenshot of a computer Description automatically
generated](./media/image29.png)

2.  Copy the code and press Enter.

![](./media/image30.png)

3.  Default browser , enter the code and then click on **Next**.

![](./media/image31.png)

4.  Sign in with your Azure subscription account.

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)
>
> ![A screenshot of a login page Description automatically
> generated](./media/image33.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image34.png)

5.  Switch back to Github code space tab.

![](./media/image35.png)

6.  To create an environment for Azure resources, run the following
    Azure Developer CLI command.It asks you to enter environment name
    .Enter any name of your choice and press enter (eg
    :+++**ragpgpyXXX+++**)(XXX can be unique number)

**Note:** When creating an environment, ensure that the name consists of
lowercase letters.

> **+++azd env new+++**

![A screenshot of a computer program Description automatically
generated](./media/image36.png)

7.  Run the following Azure Developer CLI command to provision the Azure
    resources and deploy the code.

> **+++azd up+++**

![A screenshot of a computer program Description automatically
generated](./media/image37.png)

8.  When prompted, select a **subscription** to create the resources and
    select the region closest to your location; in this lab, we have
    chosen the **East US2** region.

![](./media/image38.png)

9.  When prompted, **enter a value for the ‘openAILocation’
    infrastructure parameter** select the region closest to your
    location; in this lab, we have chosen the **North Central US**
    region

![](./media/image39.png)

10. Deployment will take around 19-20 min. While the deployment is going
    on, You can go next Task 3 and verify deployed resources.

![A screenshot of a computer program Description automatically
generated](./media/image40.png)

11. Deployment completed and front end hosted successfully. Click on the
    generated URL

![](./media/image41.png)

![](./media/image42.png)

## Task 5: Verify deployed resources in the Azure portal

1.  On Home page of Azure portal, click on **Resource Groups**.

> ![](./media/image43.png)

2.  Click on your resource group name

![](./media/image44.png)

3.  Make sure the below resource got deployed successfully

- Container App

- Application Insights

- Container Apps Environment

- Log Analytics workspace

- Azure OpenAI

- Azure Database for PostgreSQL flexible server

- Container registry

> ![](./media/image45.png)

4.  Click on **Azure OpenAI** resource name.

> ![](./media/image46.png)

5.  On **Overview** in the left navigation menu, right click **Go to
    Azure OpenAI Studio** and select to open a new tab.

![](./media/image47.png)

6.  Click on **Deployments** from left navigation menu and make sure
    **gpt-35-turbo**, **text-embedding-ada-002** should be deployed
    successfully

![](./media/image48.png)

## Task 6: Use chat app to get answers from files

1.  In the **RAG on database |OpenAI+PoastgreSQL** web app page, **click
    on Best shoe for hiking?** button and observe the output

![](./media/image49.png)

![](./media/image50.png)

![](./media/image51.png)

2.  Click on the **clear chat.**

![](./media/image52.png)

3.  In the **RAG on database |OpenAI+PoastgreSQL** web app page, click
    on **Climbing gear cheaper than $30** button and observe the output

![A screenshot of a chat Description automatically
generated](./media/image53.png)

![A screenshot of a computer Description automatically
generated](./media/image54.png)

4.  Click on the **clear chat.**

## Task 7: Clean up all the resources

To clean up all the resources created by this sample:

1.  Goback Visual Studio terminal and run **+++azd down –purge+++**

> ![A close-up of a computer code Description automatically
> generated](./media/image55.png)

2.  When asked if you are sure you want to continue, enter y

> ![A screenshot of a computer program Description automatically
> generated](./media/image56.png)

3.  When asked if you want to permanently delete the resources,
    enter **y**

![A screenshot of a computer Description automatically
generated](./media/image57.png)

**Summary**:

This use case walks you through deploying a chat application with
PostgreSQL and OpenAI on Azure, focusing on cloud-based application
deployment and management. you’ve set up the development environment,
installed necessary tools like Azure CLI, configured Azure resources
using Azure Developer CLI, and deployed the application to Azure
Container Apps.
