# 用例 06 - 使用 PostgreSQL 灵活服务器在 Azure 容器应用上部署聊天应用

**目标:**

- 若要通过安装 Azure CLI 在 Windows 上配置开发环境，Node.js、分配 Azure
  订阅角色、启动 Docker Desktop 以及启用带有开发容器扩展的 Visual Studio
  Code。

- 在 Azure 上使用 PostgreSQL 和 OpenAI 部署和测试自定义聊天应用程序。

![A screenshot of a computer Description automatically
generated](./media/image1.jpeg)

在此用例中，您将设置一个全面的开发环境，部署与 PostgreSQL
集成的聊天应用程序，并验证其在 Azure 上的部署。这涉及安装 Azure
CLI、Docker 和 Visual Studio Code
等基本工具（我们已经在主机环境中为您完成了）、在 Azure
中配置用户角色、使用 Azure 开发人员 CLI
部署应用程序以及与部署的资源交互以确保功能。

**使用的关键技术** -- Python, FastAPI, Azure OpenAI models, Azure
Database for PostgreSQL and azure-container-apps,ai-azd-templates.

**预计持续时间**-- 45 分钟

**实验室类型:** 讲师指导

**先决条件：**

GitHub 帐户 -- 您应该拥有自己的 GitHub
登录凭证。如果您没有，请从此处创建一个
- **https://github.com/signup?user_email=&source=form-home-signupobjectives**

## 练习 1 ：配置、部署应用程序并从浏览器对其进行测试

### 任务 1：复制现有资源组名称

1.  打开浏览器，打开 Azure 门户\`\`https:\\portal.azure.com\`\`.
     使用主机环境的说明/资源部分下提供的 ***Azure 切片帐户（Azure
    凭据***）登录。

![A screenshot of a computer Description automatically
generated](./media/image2.jpeg)

2.  在主页上，单击“**资源组** ”磁贴。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

3.  确保您已经创建了一个资源组供您使用。切勿删除此资源组。相反，您可以删除资源组中的资源，但不能删除资源组本身。

4.  单击 Resource Group Name

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  复制资源组名称并将其保存在记事本中，以用于将所有资源部署到此资源组

![A screenshot of a computer Description automatically
generated](./media/image5.png)

### 任务 2：运行 Docker

1.  在 Desktop 上，双击 **Docker Desktop**。

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

2.  运行 Docker Desktop。

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

### 任务 3 ： 注册服务提供商

1.  切换回 Azure 门户选项卡，单击“ **订阅** ”磁贴。

![](./media/image8.png)

2.  单击订阅名称。

![](./media/image9.png)

3.  单击 **左侧导航菜单中的** 设置 - \> 资源提供程序。

![](./media/image10.png)

4.  类型\`\`**Microsoft.AlertsManagement**\`\` ，然后按 Enter
    键。选择它，然后单击**注册**。

![](./media/image11.png)

![A screenshot of a computer Description automatically
generated](./media/image12.png)

### 任务 4 ：开放开发环境

1.  打开浏览器，导航到地址栏，键入或粘贴以下
    URL：“https://github.com/technofocus-pte/rag-postgres-openai-python.git”选项卡打开，并要求您在
    Visual Studio Code 中打开。选择 **Open Visual Studio Code。**

![](./media/image13.jpeg)

2.  单击 **fork** 以分叉存储库。为存储库指定唯一名称，然后单击 **Create
    repo** 按钮。

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

3.  单击**“代码 - \> Codespaces”-\> Codespaces+**

![A screenshot of a computer Description automatically
generated](./media/image16.jpeg)

4.  等待 Codespaces 环境设置 。完全设置需要几分钟

![A screenshot of a computer Description automatically
generated](./media/image17.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

### 任务 5：预配服务并将应用程序部署到 Azure

1.  在终端上运行以下命令。它会生成要复制的代码。复制代码并按 Enter。

\`\`azd auth login\`\`

![](./media/image19.png)

2.  默认浏览器将打开，以输入生成的代码进行验证。输入代码，然后单击
    **Next（下一步**）。

![](./media/image20.png)

3.  使用 Azure 凭据登录。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

4.  若要为 Azure 资源创建环境，请运行以下 Azure 开发人员 CLI
    命令。它要求您输入 环境名称 。输入您选择的任何名称，然后按
    Enter（例如 ：**ragpgpy**）

**注：** 创建环境时，请确保名称由小写字母组成。

\`\`azd env new\`\`

![A screenshot of a computer Description automatically
generated](./media/image22.png)

5.  运行以下 Azure Developer CLI 命令以预配 Azure 资源并部署代码。

\`\`azd provision \`\`

![A screenshot of a computer Description automatically
generated](./media/image23.png)

6.  出现提示时，选择一个**订阅**以创建资源，然后选择离你最近的区域;在本实验中，我们选择了美国**东部
    2** 区域。

![](./media/image24.png)

7.  它会提示你 “**Enter a value for the 'existingResourceGroupName'
    infrastructure parameter:**” 输入在任务 1 中复制的资源组 (eg :
    **ResourceGroup1 用于开发切片) .**您可以从 Resources
    **部分复制资源组名称** ，如下图所示

> ![](./media/image25.png)

8.  出现提示时，**为“openAILocation”基础设施参数输入一个值，**选择离你最近的区域;在本练习中，我们选择了**美国中北部**区域

![](./media/image26.png)

9.  预置资源大约需要 5 到 10 分钟。如果 **出现提示**，请单击 Yes。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

10. 等待模板成功预置所有资源。

![A screenshot of a computer Description automatically
generated](./media/image28.png)

11. 执行以下命令设置资源组

\`\`azd env set AZURE_RESOURCE_GROUP {your resource group name}\`\`

![](./media/image29.png)

12. 运行以下命令，将应用部署到 Azure。

\`\`azd deploy\`\`

![](./media/image30.png)

13. 等待部署完成。部署需要 \<5

![A screenshot of a computer Description automatically
generated](./media/image31.png)

14. 单击已部署的 Web 应用程序终端节点链接。

![](./media/image32.png)

15. 点击 **Open**。它打开带有应用程序的新标签页

![](./media/image33.png)

16. 应用程序随即打开。

![A screenshot of a chat Description automatically
generated](./media/image34.png)

### 任务 6：使用聊天应用程序从文件中获取答案

1.  在 **RAG on database |OpenAI+PoastgreSQL** Web
    应用程序页面，**点击最适合远足的鞋子？** 按钮并观察输出

![](./media/image35.png)

![A screenshot of a computer Description automatically
generated](./media/image36.png)

2.  点击 **clear chat。**

![](./media/image37.png)

3.  在 **RAG on database |OpenAI+PoastgreSQL** Web 应用页面，点击
    **Climbing gear cheap than $30** 按钮并观察输出

![](./media/image38.png)

![A screenshot of a computer Description automatically
generated](./media/image39.png)

4.  点击 **clear chat。**

### 任务 7：在 Azure 门户中验证已部署的资源

1.  在 Azure 门户的主页上，单击**“资源组**”。

![](./media/image40.png)

2.  单击您的资源组名称

![](./media/image41.png)

3.  确保已成功部署以下资源

    - 容器应用程序

    - 应用程序洞察

    - 容器应用程序环境

    - Log Analytics 工作区

    - Azure OpenAI

    - Azure Database for PostgreSQL 灵活服务器

    - 容器注册表

![](./media/image42.png)

4.  单击 **Azure OpenAI** 资源名称。

![](./media/image43.png)

5.  在 左侧导航菜单中的 Overview 上，单击 **Go to Azure AI Foundry
    portal** ，然后选择以打开新选项卡。

![](./media/image44.png)

6.  单击左侧导航菜单中**的共享资源** **-\> 部署**，并确保**应成功部署**
    gpt-35-turbo**、**text-embedding-ada-002

![](./media/image45.png)

### 任务 8 ： 清理所有资源

要清理此示例创建的所有资源，请执行以下作：

1.  切换回 **Azure portal -\> Resource group- \> Resource group name.**

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2.  选择所有资源，然后单击 Delete （删除），如下图所示。 (**不要删除**
    资源组）

![](./media/image47.png)

3.  在文本框中键入 '' **delete** '' ，然后单击 **Delete**.

![](./media/image48.png)

4.  单击 **Delete** 确认删除。

![](./media/image49.png)

5.  切换回 Github 门户选项卡并刷新页面。

![A screenshot of a computer Description automatically
generated](./media/image50.png)

6.  单击 Code ，选择为此实验室创建的分支，然后单击 **Delete** 。

![A screenshot of a computer Description automatically
generated](./media/image51.png)

7.  单击 **Delete** 按钮确认分支删除。

![A screenshot of a computer Description automatically
generated](./media/image52.png)
