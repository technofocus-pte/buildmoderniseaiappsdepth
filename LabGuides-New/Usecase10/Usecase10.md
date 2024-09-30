**Usecase 10 : [Deploying chat application to answer questions from the
user and tracks chat history across
conversations.](https://github.com/Azure-Samples/chat-csharp-cosmos-db-nosql-openai)**

**Objective:**

This usecase walks you through the steps to connect an existing Blazor
application to an Azure Cosmos DB for NoSQL account and an Azure OpenAI
account. Your application sends prompts to the model in Azure OpenAI and
parses the responses. Your application also stores various conversation
sessions and their corresponding messages as items collocated in a
single container within Azure Cosmos DB for NoSQL.

In short, the application will:

- **Connect** to Azure OpenAI's model using the .NET SDK

- **Send** prompts to the model and parse the completion response

- **Connect** to Azure Cosmos DB for NoSQL using the .NET SDK

- **Manage** items with individual operations, queries, and
  > transactional batches

This sample chat application answers questions from the user and tracks
chat history across conversations.

![Screenshot of the application with multiple conversation streams and
an active conversation on the seating capacity for a sports
stadium.](./media/image1.png)

**Key technologies used** –, Csharp, nosql
,asp-net,blazor,azure-cosmos-db,

**Estimated duration** – 45 minutes

**Lab Type:** Instructor Led

**Pre-requisites:**

GitHub account – You are expected to have your own GitHub login
credentials. If you do not have, please create one from here -
**<https://github.com/signup?user_email=&source=form-home-signupobjectives>**

## Task 1 : Run the Docker

1.  In your Windows search box, type **Docker** , then click on **Docker
    Desktop**.

![A screenshot of a computer Description automatically
generated](./media/image2.png)

**Task 2 : Deploy Services and application to Azure**

1.  Open a browser and go to
    +++https://github.com/technofocus-pte/chat-csharp-cosmos-db-nosql-openai+++
    and sign in with your Github account.Search for the below repo

> ![A screenshot of a login screen Description automatically
> generated](./media/image3.png)

2.  Search for the below repo and click on Fork.

+++<https://github.com/Azure-Samples/chat-csharp-cosmos-db-nosql-openai.git>+++

![A screenshot of a computer Description automatically
generated](./media/image4.png)

3.  Enter the repository name and then click on **Create repository**.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

4.  Click on **Code -\> Code space -\> Open Code space.**

![A screenshot of a computer Description automatically
generated](./media/image6.png)

5.  Wait for the Dev container to setup . it takes 3-5 min

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  Run below command to log in to AZD.Copy the generated code and press
    Enter.

> **+++azd auth login+++**

![A screenshot of a chat application Description automatically
generated](./media/image8.png)

7.  Paste the generated code and sign in with your Azure subscription.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

![A screenshot of a computer Description automatically
generated](./media/image10.png)

8.  Run below command to Initialize the project in the current
    directory.Say **yes** when asked to Continue initializing app

> +++azd init --template chat-csharp-cosmos-db-nosql-openai+++
>
> ![A screenshot of a computer Description automatically
> generated](./media/image11.png)

9.  Etner the Environment name as +++**cosmoschatapp+++** and press
    Enter.

> ![A screenshot of a computer code Description automatically
> generated](./media/image12.png)

10. Run below command to deploy the services to Azure, build your
    container, and deploy the application.

> +++azd up+++

![A screenshot of a computer program Description automatically
generated](./media/image13.png)

11. Select your Subscription and your nearest location . we have taken
    **East US** location for this usecase.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

12. Wait for the resource to deploy completely.

![A screenshot of a computer Description automatically
generated](./media/image15.png)

13. After deployment successful, service end point url gets generated.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

14. Open the web service endpoint url link.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

15. It opens the chat app.

> ![A screenshot of a computer Description automatically
> generated](./media/image18.png)

16. Click on **Create New Chat** button.Enter the below prompt.

> +++What is the seating capacity for Lumen in Seattle?+++
>
> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

17. Enter below prompt . Explore the app with different prompts.

> +++is that bigger than Dogger stadium??+++
>
> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

**Task 3 : Explore the code.**

18. Expand **src -\> services- \> ChatService.cs.** This file has a code
    returns list of chat session ids and name and returns the chat
    messages to display on the main web page when the user selects a
    chat from the left-hand nav

> ![](./media/image21.png)

19. Click on CosmosDbService.cs ‘s code .Code is to create Cosmos DB
    service and to create databases ,containers . create and get chat
    messages etc

> ![A screenshot of a computer Description automatically
> generated](./media/image22.png)

20. Click on OpenAIService.CS .Code is to create resource sand deploy
    modesl .Also, send user prompts to instruct the model for chat
    session,summarization

![A screenshot of a computer Description automatically
generated](./media/image23.png)

**Task 4: Varify the deployed resource in Azure portal**

1.  Switch back to Azure portal and click on your resource group
    name.You should see below resources

> Contaienr A\[\[
>
> Container Registry
>
> Azure Cosmos Db account
>
> AureOpenAI
>
> ![A screenshot of a computer Description automatically
> generated](./media/image24.png)

2.  Click on **Azure Cosmos DB account** name.

> ![](./media/image25.png)

3.  Click on Data explorer from left navigation menu, expand database
    and click on Items. You should see the chat completions.

> ![](./media/image26.png)

4.  Switch back to Chat app and enter some prompts .You can check
    completions under items in Cosmos DB database.

## Task 5: Clean up all the resources

To clean up all the resources created by this sample:

1.  Go back Github codespace tab and run +++azd down –purge+++

> ![A screenshot of a computer Description automatically
> generated](./media/image27.png)

2.  When asked if you are sure you want to continue, enter y

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

3.  When asked if you want to permanently delete the resources,
    enter **y**

![A screenshot of a computer Description automatically
generated](./media/image29.png)

Summary :

You have implemented service classes using
the Microsoft.Azure.Cosmos and Azure.AI.OpenAI packages on NuGet. You
sent prompts to the Azure OpenAI conversational interface along with
contextual prefixes and parsed the usage and body properties of the
response. You also used Azure Cosmos DB for NoSQL to store the
conversation sessions and messages within a single container.
