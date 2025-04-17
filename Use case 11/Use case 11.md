# 用例 11 - 使用 Azure OpenAI、Azure Cosmos DB for NoSQL 构建 Copilot

在此用例中，你将使用 .NET 软件开发工具包将 Blazor Web 应用程序连接到
Azure Cosmos DB for NoSQL 和 Azure OpenAI。您的代码管理和查询 API for
NoSQL 容器中的项目。您的代码还会向 Azure OpenAI 发送提示并分析响应。

**实验时长** 45 分钟

**实验类型**：讲师指导

**目的**

- 为 Blazor、PostgreSQL 和 OpenAI 设置开发环境。

- 创建 Blazor 项目并设计响应式聊天界面。

- 在 Azure 上配置 PostgreSQL 数据库并将其连接到 Blazor 应用。

- 集成 Azure OpenAI 以增强聊天功能。

- 在 Azure 上部署 Blazor 应用程序和 PostgreSQL 数据库。Azure.

- 测试应用程序以确保组件之间的无缝交互。

- 监视 Azure 上部署的应用程序并对其进行故障排除。

**使用的关键技术：**Azure Cosmos DB for NoSQL、Azure OpenAI

## 练习 0：了解 VM 和凭据

在此任务中，我们将识别并了解我们将在整个实验室中使用的凭证。

1.  **Instructions**选项卡包含实验室指南，其中包含在整个实验室中要遵循的说明。

2.  **Resources** 选项卡已获取执行实验室所需的凭证。

    - **URL** – Azure 门户的 URL

    - **Subscription** – 这是分配给你的订阅的 ID

    - **Username** – 登录 Azure 服务时需要使用的用户 ID。

    - **Password** – Azure 登录名的密码。让我们将此用户名和密码称为
      Azure 登录凭据。我们将在提及 Azure
      登录凭据的任何地方使用这些凭据。

    - **Resource Group** – 分配给您的**Resource Group** 。

>[!Alert] **重要提示：**请确保在此资源组下创建所有资源

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.**Help** 选项卡包含 Support 信息。此处的 **ID** 值是
将在实验室执行期间使用的**Lab instance ID**。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## 练习 1：部署基础设施并完成初始设置

若要完成此项目，需要一个 Azure Cosmos DB for NoSQL 帐户和一个 Azure
OpenAI 帐户。若要简化此过程，请使用这两个帐户将 Bicep 模板部署到 Azure。

### 任务 1：从模板部署基础设施

1.  从路径 **C：\Labfiles\Build and Test a custom chat application Using
    Azure Cosmos DB and AzureOpenAI** 打开文件，并将第 96 行的 Azure
    OpenAI 版本更新为 +++0125+++。 **Save**文件。

  ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

2.  打开新浏览器，并在地址栏中输入以下 URL：

    +++https://portal.azure.com/+++ 以打开 Azure 门户。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  在 Azure 门户中，单击页面顶部搜索框右侧的 **\[\>\_\] （Cloud
    Shell）** 按钮。Cloud Shell 窗格将在门户底部打开。首次打开 Cloud
    Shell 时，系统可能会提示您选择要使用的 shell 类型（**Bash** 或
    **PowerShell**）。选择
    **Bash**。如果您没有看到此选项，请跳过此步骤。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  在**Getting Started** 对话框中，选择**Mount storage account**
    ，选择**subscription** ，然后单击**Apply**。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

5.  在 **Mount storage account** 对话框中，选择 **we will create a
    storage account for you** ，然后单击 **Next**。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

  ![A close-up of a computer screen AI-generated content may be
incorrect.](./media/image9.jpeg)

6.  确保 Cloud Shell 窗格左上角指示的 shell 类型已切换到
    **Bash**。如果是 **PowerShell**， 请使用下拉菜单切换到 **Bash**。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.jpeg)

7.  终端启动后，单击 **Manage files -\> Upload**。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

8.  选择 **azuredeploy。JSON** 文件，来自路径 ** C:\Labfiles\Build and
    Test a custom chat application Using Azure Cosmos DB and
    AzureOpenAI，**然后选择**Open**。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

  您应该会收到文件上传的成功消息。

  ![A white background with black text AI-generated content may be
incorrect.](./media/image13.jpeg)

9.  使用创建的 Azure 资源组 （mslearn-cosmos-openai） 的名称创建名为
    **resourceGroupName** 的新 shell 变量。

  +++resourceGroupName="ResourceGroup1"+++(Get the Resource Group name
from the Resources tab)

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

10. 使用 az group deployment create 将 **azuredeploy.json**
    模板文件部署到资源组。然后，执行以下命令。

  +++az deployment group create --resource-group $resourceGroupName --name
zero-touch-deployment --template-file azuredeploy.json+++

**注意：**此部署可能需要大约 5-10 分钟。

  ![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image15.jpeg)

  ![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image16.jpeg)

### 任务 2：获取 Azure Cosmos DB for NoSQL 和 Azure OpenAI 帐户凭据

上述部署部署已部署适用于 NoSQL 的 Azure Cosmos DB 和 Azure OpenAI
帐户，然后将其凭据存储在 Azure 应用服务 Web
应用的配置中。现在，您可以选择使用 Azure 门户或 Azure CLI
来检索每个服务的凭据。

1.  在 Azure 门户主页中，单击**Resource groups。**

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

2.  选择您的resource group。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

3.  在 **Resource Groups** 页面上，展开 **Essentials** 面板并观察
    **Deployments** 标头。此时，部署的状态应为 **Succeeded** 。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

4.  现在，选择 **Azure Cosmos DB** 帐户以导航到资源页面。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

5.  在 资源导航菜单的 **Settings** 部分中选择 **Keys** 选项。记录
    **URI** 和 **PRIMARY KEY** 字段的值。稍后将使用这些值。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.jpeg)

6.  返回到 **Resource Groups** 页面。选择 **Azure OpenAI** 帐户。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

7.  在 **Azure Open AI** 窗口中，导航到 **Resource Management**
    部分，然后单击 **Keys and Endpoints**。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.jpeg)

8.  在 **Keys and Endpoints** 页面中，复制 **KEY1（***您可以使用 KEY1 或
    KEY2）*和
    **Endpoint**，然后**Save** 记事本以在即将执行的任务中使用这些信息。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

### 任务 3：运行 Docker

1.  在 Windows 搜索框中，键入 +++Docker+++，然后单击 **Docker
    Desktop**。

  ![A screenshot of a desktop AI-generated content may be
incorrect.](./media/image25.jpeg)

2.  运行 Docker Desktop。

  ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.jpeg)

## 练习 2 - 设置和构建 Starter 应用程序

1.  在 VM 搜索栏中，搜索 +++Visual Studio+++，然后选择 **Visual Studio
    Code**。

2.  单击 **File** -\> **Open Folder**

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

3.  从 **C：\LabFiles** 中选择 **cosmosdb-chatgpt**，然后单击 **Select
    Folder。**

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

4.  单击 **Do you trust the authors 对话框中**的 Yes， I trust the
    authors **选项**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.jpeg)

5.  在 **Visual Studio Code** 编辑器中，单击 **Terminal**，打开 **New
    Terminal**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

6.  在 .NET
    应用程序中，通常使用配置提供程序将新设置注入应用程序。对于此应用程序，请使用
    **appsettings.Development.json**文件提供 Azure OpenAI
    终结点和密钥的最新值。

7.  打开 **appsettings。Development.JSON** 文件。将文件中 **Azure Cosmos
    DB** 和 **Azure OpenAI** 资源的 uri 和 key
    值的占位符替换为我们之前保存的值。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

8.  通过执行以下命令**Build** .NET 项目。

    +++dotnet build+++

    ![A screen shot of a computer AI-generated content may be
incorrect.](./media/image32.jpeg)

## 练习 3：了解代码

### 任务 1：添加所需的成员和客户端实例

1.  打开 **Services/OpenAiService.cs** 文件。此文件实现使用 Azure OpenAI
    客户端所需的类变量。它实现了一些静态提示，并创建了 OpenAIClient
    类的新实例。

2.  此代码块创建一个名为 \_systemPromptText
    的新字符串变量，其中包含一个静态文本块，以便在每个提示之前发送到 AI
    助手。

    ```nocopy
    private readonly string _systemPrompt = @"
    You are an AI assistant that helps people find information.
    Provide concise answers that are polite and professional." + Environment.NewLine;
    ```

3.  此代码块创建另一个名为 \_summarizePrompt
    的新字符串变量，其中包含一个静态文本块，以发送到 AI
    助手，其中包含有关如何汇总对话的说明。

    ```nocopy
    private readonly string _summarizePrompt = @"
    Summarize this prompt in one or two words to use as a label in a button on a web page.
    Do not use any punctuation." + Environment.NewLine;
    ```

4.  此代码块使用终端节点创建 OpenAIClient 类的新实例，并使用密钥生成
    AzureKeyCredential。

    ```nocopy
      Uri uri = new(endpoint);
          AzureKeyCredential credential = new(key);
          _client = new(
              endpoint: uri,
              keyCredential: credential
          );
    ```

**任务 2：向 AI 模型提问**

首先，通过发送系统提示、问题和会话 ID 来实现问答对话，以便 AI
模型可以在当前对话的上下文中提供答案。确保测量解析提示并返回响应（或在此上下文中的完成）所需的令牌数量。

1.  此代码块将创建一个名为 options 的新变量，该变量的类型为
    ChatCompletionsOptions。将两个 message 变量添加到消息列表中，并将
    User 的值设置为 sessionId 构造函数参数。

    ```nocopy
    ChatCompletionsOptions options = new()
        {
            DeploymentName = "chatmodel",
            Messages = {
                new ChatRequestSystemMessage(_systemPrompt),
                new ChatRequestUserMessage(userPrompt)
            },
            User = sessionId,
            MaxTokens = 4000,
            Temperature = 0.3f,
            NucleusSamplingFactor = 0.5f,
            FrequencyPenalty = 0,
            PresencePenalty = 0
        };
    ```

2.  Azure OpenAI 客户端变量 （\_client） 的 GetChatCompletionsAsync
    方法是异步调用的。结果存储在名为 completions 的 ChatCompletion
    类型的变量中。

    ```nocopy
    Response<ChatCompletions> completionsResponse = await_client.GetChatCompletionsAsync(options);
    ChatCompletions completions = completionsResponse.Value;
    ```

3.  最后，下面的代码块返回一个元组作为 GetChatCompletionAsync
    方法的结果，其中完成的内容以字符串形式显示，与提示关联的令牌数以及响应的令牌数。

    ```nocopy
    return (
            completionText: completions.Choices[0].Message.Content,
            completionTokens: completions.Usage.CompletionTokens
        );
    ```

**任务 3：要求 AI 模型总结对话**

现在，向 AI 模型发送不同的系统提示、您当前的对话和会话 ID，以便 AI
模型可以用几个词来总结对话。

2.下面的代码创建一个名为 options 的 ChatCompletionsOptions
变量，其中包含 Messages 列表中的两个 message 变量，User 设置为 sessionId
构造函数参数，MaxTokens 设置为 200，其余属性。

    ```nocopy
    ChatCompletionsOptions options = new()
        {
             DeploymentName = "chatmodel",
            Messages = {
                 new ChatRequestSystemMessage(_systemPrompt),
                new ChatRequestUserMessage(conversationText)
            },
            User = sessionId,
            MaxTokens = 200,
            Temperature = 0.0f,
            NucleusSamplingFactor = 1.0f,
            FrequencyPenalty = 0,
            PresencePenalty = 0
        };
    ```
3.  下面的代码调用 _client。GetChatCompletionsAsync，并将模型名称 （_modelName） 和 options 变量作为参数，并将结果存储在名为 completions 的 ChatCompletion 类型的变量中。它将完成的内容作为 SummarizeAsync 方法的结果作为字符串返回。

    ```nocopy
    Response<ChatCompletions> completionsResponse = await _client.GetChatCompletionsAsync(options);
    ChatCompletions completions = completionsResponse.Value;
    string completionText = completions.Choices[0].Message.Content;
    return completionText;
    ```

    ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image33.jpeg)

**任务 4 - 连接到 Azure Cosmos DB for NoSQL**

CosmosDbService 类包含类似于本模块前面使用的 OpenAiService
类的服务的存根实现。相比之下，此类使用适用于 Azure Cosmos DB 的 .NET
SDK，其工作方式略有不同。

本部分简要介绍了使用客户端访问 Azure Cosmos DB for NoSQL
所需的类变量和客户端的实现。

1.  打开 **Services/CosmosDbService.cs** 文件。

    ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image34.jpeg)

2.  下面的代码创建一个名为 options 的 CosmosSerializationOptions
    类型的变量，并将该变量的 PropertyNamingPolicy 属性设置为
    CosmosPropertyNamingPolicy.CamelCase。

    ```nocopy
    CosmosSerializationOptions options = new()
    {
        PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
    };
    ```

**注意：**设置此属性将确保 SDK 生成的 JSON
以驼峰式大小写进行序列化和反序列化，而不管其对应的属性在 .NET
类中如何大小写。

3.  下面的代码使用之前指定的 CosmosClientBuilder
    类、终结点、密钥和序列化选项创建一个名为 client 的 CosmosClient
    类型的新实例。

    ```nocopy
    CosmosClient client = new CosmosClientBuilder(endpoint, key)
        .WithSerializerOptions(options)
        .Build();
    ```

4.  下面的代码通过调用 client 变量的 GetDatabase 方法，创建一个名为
    database 的 Database 类型的新可为 null 变量。

    **Database? database = client?.GetDatabase(databaseName);**

5.  下面的代码仅在 class 不为 null 时将构造函数的 container
    变量分配给类的 \_container 变量。如果为 null，则引发
    ArgumentException。

    ```nocopy
    _container = container ??
    throw new ArgumentException("Unable to connect to existing Azure Cosmos DB container or database.");
    ```

    ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.jpeg)

**任务 5 - 实现 Azure Cosmos DB for NoSQL 服务**

Azure Cosmos DB 服务 （CosmosDbService） 管理在 AI
助理应用程序中查询、创建、删除和更新会话和消息。为了管理所有这些作，该服务需要使用
.NET SDK 的各种功能为每个潜在作实现多种方法。

在本练习中，有多个关键要求需要解决:

- 实施创建会话或消息的作

- 实施查询以检索多个会话或消息

- 实现更新单个会话或批量更新多条消息的作

- 实现查询和删除多个相关会话和消息的作

Azure Cosmos DB for NoSQL 以 JSON
格式存储数据，允许我们在单个容器中存储多种类型的数据。此应用程序存储与
AI 助手的聊天“会话”和每个会话中的单个“消息”。借助 API for
NoSQL，应用程序可以将这两种类型的数据存储在同一个容器中，然后使用简单的类型字段区分这些类型。

1.  打开 **Services/CosmosDbService.cs** 文件。

2.  下面的代码使用当前会话的 SessionId 属性作为参数，创建一个名为
    partitionKey 的新变量，该变量的类型为 PartitionKey。

**PartitionKey partitionKey = new(session.SessionId);**

3.  下面的代码调用容器的 CreateItemAsync 方法，并传入 session 参数和
    partitionKey 变量。返回作为 InsertSessionAsync 方法结果的响应。

    ```nocopy
    return await _container.CreateItemAsync<Session>(
        item: session,
        partitionKey: partitionKey
    );
    ```

4.  下面的代码使用 session 创建一个 PartitionKey 变量。SessionId
    作为分区键的值。创建一个名为 newMessage 的新消息变量，并将 Timestamp
    属性更新为当前 UTC 时间戳。调用传入新消息和分区键变量的
    CreateItemAsync。

返回响应作为 InsertMessageAsync 的结果。

    ```nocopy
    PartitionKey partitionKey = new(message.SessionId);
    Message newMessage = message with { TimeStamp = DateTime.UtcNow };
    return await _container.CreateItemAsync<Message>(
        item: newMessage,
        partitionKey: partitionKey
    );
    ```

    ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image36.jpeg)

**任务 6：检索多个会话或消息**

在两个主要用例中，应用程序需要从我们的容器中检索多个项目。首先，应用程序通过将项目筛选为
type = Session
的项目来检索当前用户的所有会话。其次，应用程序通过执行类似的过滤器来检索会话的所有消息，其中type
= Session & sessionId = 。此处的两个查询都是使用 .NET SDK
和源迭代器实现的。

1.  以下代码创建一个名为 query、类型为 QueryDefinition 的新变量。它使用
    Fluent WithParameter 方法将 Session 类的名称分配为参数的值。然后对
    \_container 变量调用泛型 GetItemQueryIterator\<\> 方法，将泛型类型
    Session 和查询变量作为参数传入。将结果存储在名为 response 的
    FeedIterator 类型的变量中。

    ```nocopy
    QueryDefinition query = new QueryDefinition("SELECT DISTINCT * FROM c WHERE c.type = @type")
    .WithParameter("@type", nameof(Session));
    FeedIterator<Session> response = _container.GetItemQueryIterator<Session>(query);
    ```

2.  在 while 循环中，以下代码通过对响应变量调用 ReadNextAsync
    异步获取下一页结果，然后将这些结果添加到名为 output 的列表变量中。在
    while 循环之外，output 变量将返回一个会话列表，作为 GetSessionsAsync
    方法的结果。

    ```nocopy
    FeedResponse<Session> results = await response.ReadNextAsync();
    output.AddRange(results);
    return output;

3.  下面的代码使用 fluent WithParameter 方法将 @sessionId
    参数分配给作为参数传入的会话标识符，并将 @type 参数分配给 Message
    类的名称。

    ```nocopy
    QueryDefinition query = new QueryDefinition("SELECT * FROM c WHERE  c.sessionId = @sessionId AND c.type = @type")
        .WithParameter("@sessionId", sessionId)
        .WithParameter("@type", nameof(Message));
    ```

4.  使用 query 变量和 GetItemQueryIterator\<\> 方法创建 FeedIterator \<
    Message \>。

    FeedIterator response = \_container.GetItemQueryIterator(query);

    ![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image37.jpeg)

## 练习 4：执行应用程序

现在，应用程序已实现 Azure OpenAI 和 Azure Cosmos
DB。您可以通过调试解决方案来端到端测试应用程序。

1.  在 **Visual Studio Code Terminal**中，使用以下命令构建项目。

    +++**dotnet build**+++

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.jpeg)

2.  使用 dotnet watch 启用热重载启动应用程序。

    +++**dotnet watch run --non-interactive**+++

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

3.  Visual Studio Code 将启动工具内简单浏览器，并运行 Web 应用程序。在
    Web 应用程序中，通过单击 **+ Create New
    Chat**来创建新的聊天会话，然后向 AI 助手提问。然后，关闭正在运行的
    Web 应用程序。

    ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image40.jpeg)

4.  将以下文本粘贴到文本框中，然后单击 ** Send** 图标。

    +++How many wins does it take to promote to the Premier League?+++

    ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image41.jpeg)

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

5.  将以下文本粘贴到文本框中，然后单击 ** Send** 图标。

    +++What is Azure OpenAI?+++

    ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image43.jpeg)

    ![A screenshot of a chat AI-generated content may be
incorrect.](./media/image44.jpeg)

6.  关闭terminal。

## 练习 5：清理资源组

1.  打开新浏览器，并在地址栏中输入以下
    URL：+++https://portal.azure.com/+++ 以打开 Azure 门户。

2.  在 Resource group 页面中，选择您 ** assigned Resource group**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  选择所有 **resources**，然后选择 **Delete**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

4.  在文本框中输入 +++**delete**+++，然后单击 **Delete**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

    ![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image49.png)

5.  已删除资源上的成功通知确认删除。

6.  删除资源后，在 Azure 门户主页中搜索 **Azure AI Services**并选择它。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.jpeg)

7.  从左侧窗格中选择 **Azure OpenAI，**然后选择 **Manage deleted
    resources**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

8.  选择此处列出的资源，然后单击 **Purge**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.jpeg)

9.  单击 **Yes**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

**总结**

此实验室提供了使用 Blazor、PostgreSQL 和 Azure OpenAI
构建、部署和测试自定义聊天应用程序的全面指南。在本实验中，你学习了如何设置必要的开发环境，创建和设计了基于
Blazor 的聊天界面，在 Azure 上配置和连接了 PostgreSQL 数据库，集成了
Azure OpenAI 以增强功能，最后在 Azure
上部署和测试了应用程序。这种实践经验使您具备了使用尖端技术和云服务开发和管理现代
Web 应用程序的技能
