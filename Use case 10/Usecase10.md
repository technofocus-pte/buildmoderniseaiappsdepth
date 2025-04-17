# 

# 用例 10 ：部署聊天应用程序以回答用户的问题并跟踪对话中的聊天历史记录

**目的:**

此用例将指导你完成将现有 Blazor 应用程序连接到 Azure Cosmos DB for NoSQL
帐户和 Azure OpenAI 帐户的步骤。应用程序向 Azure OpenAI
中的模型发送提示并分析响应。应用程序还将各种对话会话及其相应的消息存储为项，并置在
Azure Cosmos DB for NoSQL 中的单个容器中。

简而言之，该应用程序将：

- 使用 .NET SDK 连接到 Azure OpenAI 的模型

- **向模型发送**提示并解析完成响应

- 使用 .NET SDK 连接到 Azure Cosmos DB for NoSQL

- **使用单个作、查询和事务批处理**管理项目

此示例聊天应用程序回答用户的问题并跟踪对话中的聊天历史记录。

![](./media/image1.jpeg)

**使用的关键技术**--, Csharp, nosql ,asp-net,blazor,azure-cosmos-db,

**预计持续时间** -- 45 分钟

**实验类型：** 讲师指导

**先决条件：**

GitHub 帐户 -- 您应该拥有自己的 GitHub
登录凭证。如果您没有，请从此处创建一个
-\`\` **https://github.com/signup?user_email=&source=form-home-signupobjectives\`\`**

### 任务 1：运行 Docker

1.  在 Windows 搜索框中，键入 **Docker** ，然后单击 **Docker Desktop**。

![](./media/image2.jpeg)

### 任务 2 ：注册服务提供商

1.  打开浏览器并转到 <https://portal.azure.com> 并使用 VM
    的“资源**”选项卡中**提供的 Azure 凭据登录。

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

2.  在 Azure 门户的主页上，单击“ **资源组** ”磁贴。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

3.  复制资源组名称并将其保存在记事本中，以便使用下一个任务在此资源组中部署所需的资源。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

4.  导航回 主页 ，单击 **订阅** 拼贴。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

5.  单击订阅名称。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

6.  单击 **左侧导航菜单中的** 设置 - \> 资源提供程序。

![](./media/image8.png)

7.  输入 \`\`**Microsoft.AlertsManagement**\`\` ，然后按 Enter
    键。选择它，然后单击**注册**。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

![A screenshot of a computer Description automatically
generated](./media/image10.png)

### 任务 3：将服务和应用程序预配到 Azure

1.  打开浏览器并转到\`\`https:\\github.com\`\` 并使用您的 Github
    帐户登录。搜索以下存储库

![](./media/image11.jpeg)

2.  搜索以下存储库，然后单击 **Fork**。

> \`\`https://github.com/technofocus-pte/chat-csharp-cosmos-db-nosql-openai\`\`

![](./media/image12.jpeg)

3.  输入存储库名称，然后单击 **Create repository** .

![](./media/image13.jpeg)

4.  单击 **Code -\> Code space -\> Open Code space。**

![](./media/image14.jpeg)

5.  等待 Dev 容器设置 。需要 3-5 分钟

![](./media/image15.jpeg)

6.  运行以下命令以登录到 AZD。复制生成的代码，然后按 Enter 键. 

> \`\`**azd auth login\`\`**

![](./media/image16.jpeg)

7.  粘贴生成的代码并使用您的 Azure 凭据登录。

![](./media/image17.jpeg)

![](./media/image18.jpeg)

8.  运行以下命令以初始化当前目录中的项目。将 Environment name
    （环境名称） 输入为\`\`**cosmoschatapp\`\`** ，然后按 Enter 键。

\`\`azd init \`\`

![A screenshot of a computer Description automatically
generated](./media/image19.png)

9.  运行以下命令将服务部署到 Azure，构建容器。选择以下值。

> \`\`azd provision\`\`
>
> **选择要使用的 Azure 订阅** ：选择您的订阅
>
> **选择要使用的 Azure 位置** ： **美国东部/美国西部**
> （有时，美国东部可能不可用，请选择其他位置并进行部署。
>
> **为“existingResourceGroupName”基础架构参数输入一个值:**
> **ResourceGroup1**

![](./media/image20.png)

![A screenshot of a computer Description automatically
generated](./media/image21.png)

10. 等待资源完全预置。此过程需要 5-10 分钟才能创建所有必需的资源。

![](./media/image22.png)

### 任务 4：将应用程序部署到 Azure

1.  切换回 Azure 门户，然后单击主页上的“资源组”磁贴。

![](./media/image23.png)

2.  单击 资源组名称 。

![](./media/image24.png)

3.  您应该看到以下资源

- **容器**

- **容器注册表**

- **Azure Cosmos Db 账户**

- **AureOpenAI**

![A screenshot of a computer Description automatically
generated](./media/image25.png)

4.  单击 **Container registry name**。

![](./media/image26.png)

5.  从 左侧导航菜单中展开 设置 ，单击 **访问键。** 选中 **Admin user
    复选框。** 将**登录服务器**、**用户名和密码**
    复制到记事本，以使用它来部署应用程序。

![](./media/image27.png)

6.  复制选项卡以在新选项卡中打开 Azrue 门户。

![](./media/image28.png)

7.  单击顶部导航菜单中的资源组名称。

![](./media/image29.png)

8.  单击 Container App name 。

![](./media/image30.png)

9.  单击 Github-Sign in 下的 Authorize 按钮，以使用您的 GitHub
    帐户进行身份验证。授权您的 Github 帐户。

10. 选择以下值

> **组织 : 您的 Github 组织**
>
> **存储 库:** chat-csharp-cosmos-db-nosql-openai
>
> **分支 ：** main

![](./media/image31.png)

11. 向下滚动到 **Registry settings** 并输入以下值，然后单击 **Start
    continuous deployment** 按钮。

- 存储库源 ： **Docker Hub 或其他注册表。**

- 登录服务器 URL：您的登录服务器从 Container registry 复制（步骤 \#5）

- 用户名 ：来自容器注册表的密码（步骤 \#5）

- 密码 ：来自容器注册表的密码（步骤 \# 5）

![](./media/image32.png)

12. 单击 Workflow file 链接。它会打开带有 Github 的新标签页。

![](./media/image33.png)

13. 单击 **Actions** 选项卡。

![](./media/image34.png)

14. 等待部署完成。

![](./media/image35.png)

15. 不要关闭任何选项卡。

### 任务 5 ： 访问聊天应用程序

1.  切换回 Azure 门户，单击
    左侧导航栏中的“**概述**”，然后单击**“**应用程序
    **URL**”。它会打开一个新的 to load 应用程序。

![](./media/image36.png)

2.  点击 **Create New Chat** 按钮。

![](./media/image37.png)

3.  输入以下提示。

\`\`What is the seating capacity for Lumen in Seattle?\`\`

![](./media/image38.jpeg)

4.  输入以下提示 。使用不同的提示浏览应用程序。

\`\`is that bigger than Dogger stadium??\`\`

![](./media/image39.jpeg)

### 任务 6 ： 清理所有资源

要清理此示例创建的所有资源，请执行以下作：

1.  切换回 Github 门户选项卡并刷新页面。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

1.  单击 Code ，选择为此实验室创建的分支，然后单击 **Delete** 。

![](./media/image41.png)

2.  单击 Delete **按钮**确认分支删除。

![A screenshot of a computer Description automatically
generated](./media/image42.png)

3.  切换回 **Azure portal -\> Resource group- \> Resource group name.**

![](./media/image43.png)

4.  选择所有资源，然后单击 Delete （删除），如下图所示。（**DO NOT
    DELETE** 资源组）

![](./media/image44.png)

5.  在文本框上输入\`\`**delete**\`\`，然后单击 **Delete** 。

> ![](./media/image45.png)

6.  单击 Delete 确认删除。

![](./media/image46.png)

**总结:**

你已在 NuGet 上使用 Microsoft.Azure.Cosmos 和 Azure.AI.OpenAI
包实现了服务类。您向 Azure OpenAI
对话界面发送了提示以及上下文前缀，并分析了响应的用法和正文属性。你还使用了
Azure Cosmos DB for NoSQL 将对话会话和消息存储在单个容器中。
