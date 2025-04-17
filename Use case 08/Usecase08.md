# 用例 08 - 构建和部署 Contoso Real Estate 聊天应用程序以支持客户。

**目标**

此用例演示了使用 Retrieval Augmented Generation
模式在您自己的数据上创建类似 ChatGPT 的体验的几种方法。它使用 Azure
OpenAI 服务访问 ChatGPT 模型 （gpt-35-turbo），并使用 Azure AI
搜索进行数据索引和检索。

![A diagram of a software process Description automatically
generated](./media/image1.jpeg)

该用例包含示例数据，因此可以端到端尝试。在此示例应用程序中，我们使用一家名为
Contoso Real Estate
的虚构公司，该体验允许其客户询问有关其产品使用情况的支持问题。示例数据包括一组文档，用于描述其服务条款、隐私政策和支持指南。

该应用程序由多个组件组成，包括：

- **搜索服务**: 提供搜索和检索功能的后端服务。

- **Indexer service**：对数据进行索引并创建搜索索引的服务。

- **Web 应用程序**：提供用户界面并协调用户与后端服务之间交互的前端 Web
  应用程序。

![A diagram of a software system Description automatically
generated](./media/image2.jpeg)

- 聊天和 Q&A 界面

- 探索各种选项，以帮助用户评估带有引文、跟踪源内容等的响应的可信度。

- 展示模型 （ChatGPT） 和检索器之间交互的数据准备、提示构建和编排
  （Azure AI 搜索） 的可能方法

- 直接在 UX 中进行调整，以调整行为并试验选项

- 使用 Application Insights 进行可选的性能跟踪和监控

**使用的关键技术**-- Azure OpenAI Service, ChatGPT model (gpt-35-turbo),
and Azure AI Search

**预计持续时间 --** 40 分钟

## 练习 1 ：部署应用程序并从浏览器对其进行测试

### 任务 1：开放开发环境

1.  打开浏览器，导航到地址栏，键入或粘贴以下
    URL： \`\`https://github.com/technofocus-pte/azure-search-openai-javascript\`\` 并使用您的
    Github 帐户登录。

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

2.  单击 **Fork**。

![A screenshot of a web page Description automatically
generated](./media/image4.jpeg)

3.  输入存储库名称，然后单击 **Create fork**.

![A screenshot of a computer Description automatically
generated](./media/image5.jpeg)

4.  点击 **Code -\> Codespaces -\> +**

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

5.  等待环境设置完成。需要 5-10 分钟。

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

### 任务 2：预配所需的服务以生成聊天应用并将其部署到 Azure

1.  在终端上运行以下命令。复制代码并按 Enter。

\`\`azd auth login\`\`

![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  默认浏览器打开以输入代码。输入复制的代码，然后单击 **Next**。

![](./media/image9.png)

3.  使用 Azure 凭据登录。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

![A screenshot of a computer error Description automatically
generated](./media/image11.png)

4.  切换回 Github Codespace
    选项卡。执行以下命令，初始化当前目录下的工程环境。将 Environment
    name （环境名称） 输入为 \`\`**ragpgpy \`\`** ，然后按 Enter 键。

注意：env name 应该是唯一的

\`\` azd env new\`\`

![](./media/image12.png)

5.  运行以下命令将服务预配到 Azure，构建容器。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

6.  选择以下值。

> \`\`azd provision\`\`

- **选择要使用的 Azure 订阅** ：选择您的订阅

- **选择要使用的 Azure 位置** ： **East us2/west us2** （有时，East US
  可能不可用，请从下面提到的列表中选择位置。

&nbsp;

- 选择现有资源组：您现有的资源组（例如 :**ResourceGroup1 )**

![](./media/image14.png)

7.  等待资源完全预置。此过程需要 5-10 分钟才能创建所有必需的资源。

> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)

### 任务 3：部署聊天应用程序并浏览它

8.  运行以下命令以部署应用程序。

\`\`azd deploy\`\`

![](./media/image16.png)

9.  等待部署 。此过程\< 5 分钟。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

10. 单击生成的端点 URL。

![](./media/image18.png)

11. 点击 **Open**。

![](./media/image19.png)

12. 它会在新选项卡中打开应用程序。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

13. 选择**How to search and book rental?** 容器，然后单击文本框旁边的
    enter 按钮。

![](./media/image21.png)

### 任务 4 ：清理所有资源

1.  切换回 **Azure portal -\> Resource group- \> Resource group name.**

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  选择所有资源，然后单击 Delete （删除），如下图所示。（**DO NOT
    DELETE** 资源组）

![](./media/image23.png)

3.  在文本框中键入 '' **delete**'' ，然后单击 **Delete**。

![](./media/image24.png)

4.  单击 Delete 确认删除。

![](./media/image25.png)

5.  切换回 Github 门户选项卡并刷新页面。

![A screenshot of a computer Description automatically
generated](./media/image26.png)

6.  单击 Code ，选择为此实验室创建的分支，然后单击 **Delete** 。

![](./media/image27.png)

7.  单击 Delete **按钮**确认分支删除。

![](./media/image28.png)

### 总结：

此用例认为您，为在 Azure 上运行的检索增强一代模式部署聊天应用程序，使用
Azure AI 搜索进行检索，并使用 Azure OpenAI 和 LangChain 大型语言模型
（LLM） 来支持 ChatGPT 风格和 Q&A 体验
