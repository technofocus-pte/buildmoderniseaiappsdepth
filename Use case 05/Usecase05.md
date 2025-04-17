# 用例 05 - 使用 Azure Database for PostgreSQL 部署数据驱动的 Python 餐厅 Web 应用

**目标:**

此用例使用 Flask 框架和 Azure Database for PostgreSQL 关系数据库服务部署
Python Web 应用。Flask 应用托管在完全托管的 Azure
应用服务中。此应用程序旨在在本地运行，然后部署到 Azure你将使用 Azure
Database for PostgreSQL **关系数据库服务将数据驱动的 Python Web
应用程序（Django** 或 **Flask**）部署到 **Azure 应用程序服务**。Azure
应用服务在 Linux 服务器环境中支持 Python。

![A diagram of a service plan Description automatically
generated](./media/image1.jpeg)

**使用的关键技术**-- Java 17, Azure Database for PostgreSQL

**预计持续时间** -- 45 分钟

**实验类型：** 讲师指导

**先决条件：**

GitHub 帐户 -- 您应该拥有自己的 GitHub
登录凭证。如果您没有，请从此处创建一个
- **https://github.com/signup?user_email=&source=form-home-signupobjectives**

requirements.txt 具有以下包，全部由典型的数据驱动型 Flask 应用程序使用：

[TABLE]

### 任务 1 ：注册服务提供商

1.  打开浏览器并转到 <https://portal.azure.com> 并使用 VM
    的“资源”选项卡中提供的云切片帐户登录。

> ![](./media/image2.png)

2.  在 Azure 门户的主页上，单击“ **资源组** ”磁贴。

![](./media/image3.png)

3.  复制资源组名称并将其保存在记事本中，以便使用下一个任务在此资源组中部署所需的资源。

![](./media/image4.png)

4.  在顶部导航栏上，单击 主页 。

![](./media/image5.png)

5.  单击 **Subscriptions** 磁贴。

![](./media/image6.png)

6.  单击订阅名称。

![](./media/image7.png)

7.  展开 个人设置 从左侧导航菜单。单击 **Resource providers**，输入
    Microsoft.AlertsManagement 并选择它，然后单击 **Register**。

> ![](./media/image8.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image9.png)

### 任务 2：创建 Github Codespace 以启动 Azure 开发人员 CLI 模板

此用例具有开发容器配置，可以更轻松地在本地开发应用程序、将其部署到 Azure
并对其进行监视。我们使用 Azure 开发 CLI 模板来部署应用程序

1.  打开浏览器并转到 ''https：\\github.com'' 并使用您的 Github
    帐户登录。

2.  单击 Fork （复刻） 将此存储库
    https://github.com/technofocus-pte/msdocs-flask-postgresql-sample-app
    **分叉到您的帐户** ，如下图所示。

![A screenshot of a computer Description automatically
generated](./media/image10.jpeg)

3.  输入唯一名称，然后单击 **Create repo**。

![A screenshot of a computer Description automatically
generated](./media/image11.jpeg)

4.  从复刻的存储库根目录中，选择 **Code** \> **Codespaces** \> **+**。

![A screenshot of a computer Description automatically
generated](./media/image12.jpeg)

5.  等待工作区设置完成。

![A screenshot of a computer Description automatically
generated](./media/image13.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

6.  在 codespace 终端中，运行以下命令：

> \# Install requirements

\`\`python3 -m pip install -r requirements.txt\`\`

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

7.  运行以下命令以创建环境变量

> \# Create .env with environment variables

\`\`cp .env.sample.devcontainer .env\`\`

![A screenshot of a computer program Description automatically
generated](./media/image16.jpeg)

8.  运行以下命令进行数据迁移

> \# Run database migrations

\`\`python3 -m flask db upgrade\`\`

![A screenshot of a computer program Description automatically
generated](./media/image17.jpeg)

9.  运行以下命令

> \# Start the development server

\`\`python3 -m flask run\`\`

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

10. 当您看到消息 Your application running on port is
    available.（您的应用程序在端口上运行可用）时，单击 **Open in
    Browser（在浏览器中打开**）。

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image20.jpeg)

11. 点击 **Add new restaurant** 按钮。

![A white screen with black text Description automatically
generated](./media/image21.jpeg)

12. 在下面输入详细信息，然后单击 **Submit** 按钮。

名称 : \`\`**Contoso Rica\`\`**

街道地址 - \`\`**3A ,8th cross, Ferns street , Singapore\`\`**

描述 - \`\`**这是一家位于城市购物中心的中高价位餐厅\`\`**

![A screenshot of a restaurant Description automatically
generated](./media/image22.jpeg)

13. 点击 **Add new review** 按钮。

![A screenshot of a computer Description automatically
generated](./media/image23.jpeg)

14. 输入您的评论，然后单击按钮。 **保存更改**

**您的姓名 ： 您的姓名**

**评分 ： 您的评分**

\`\`这是一家位于城市购物中心的中高档餐厅。服务有点混乱，因为我们至少有 6
个服务员来问我们事情。食物需要一些时间才能来。我们有 2
个菜单：一个印度菜单和一个泰国菜单。泰国菜便宜
30%，所以我们点了一些开胃菜和泰国红咖喱。食物需要一些时间，但这是值得的。它很好吃，而且准备得非常好。总的来说，这是一顿不错的饭菜。

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

![A white card with black text Description automatically
generated](./media/image25.jpeg)

1.  Add some more reviews and new restaurant with comments.

![A screenshot of a computer Description automatically
generated](./media/image26.jpeg)

### 任务 3：在 Azure 中预配所需的资源。

此项目旨在与 Azure Developer CLI
配合使用，从而更轻松地在本地开发应用程序、将其部署到 Azure
并对其进行监视。

1.  切换回 Github 代码空间选项卡，运行以下命令来初始化一个新的 azd
    环境：

\`\`azd init\`\`

![](./media/image27.jpeg)

2.  它将提示您提供环境名称 (例如 **flask-app**XXXX (XXXX
    可以是唯一的数字)), 稍后将用于已部署资源的名称。

![](./media/image28.jpeg)

3.  如果需要，请登录 '\`**azd auth login\`\`** .复制代码并按 Enter。

![A screenshot of a computer Description automatically
generated](./media/image29.jpeg)

4.  输入代码，然后使用 Azure 凭据登录。

![A screenshot of a computer error Description automatically
generated](./media/image30.jpeg)

![](./media/image31.jpeg)

![A screenshot of a computer error Description automatically
generated](./media/image32.jpeg)

![A screenshot of a computer program Description automatically
generated](./media/image33.jpeg)

5.  切换回 Gtihub codespace
    选项卡并运行以下命令来配置和部署所有资源。它将提示选择您的 Azure
    订阅。输入 **1** 以选择您的订阅，然后按 Enter。

**\`\`azd provision\`\`**

![A computer screen shot of a computer code Description automatically
generated](./media/image34.png)

6.  选择位置作为
    **WestUS/eastus**。然后，它将预置您账户中的资源并部署最新代码。如果您在部署时遇到错误，更改位置（如更改为“westus”）可能会有所帮助，因为某些资源可能存在可用性限制。

![A screenshot of a computer program Description automatically
generated](./media/image35.png)

7.  输入 Azure 门户中的资源组名称（在上一个任务中复制），然后按 Enter。

![](./media/image36.png)

8.  部署需要 **20 到 30 分钟**。还可以在生成的链接或 **Azure
    门户\>资源组\>部署中检查部署状态**。

![](./media/image37.png)

![A screenshot of a computer Description automatically
generated](./media/image38.png)

![A screenshot of a computer Description automatically
generated](./media/image39.png)

![A screenshot of a computer Description automatically
generated](./media/image40.png)

### 任务 4：从 Github 部署应用程序

1.  运行以下命令以设置资源组环境变量。

\`\`azd env set AZURE_RESOURCE_GROUP {Name of existing resource group}
\`\`

注意：将 {Name of existing resource group} 替换为 VM
中“资源”部分下可用的资源组名称。

![](./media/image41.png)

2.  运行以下命令以部署所有资源并等待部署成功完成。

\`\`azd deploy\`\`

![](./media/image42.png)

3.  单击生成的 Endpoint URL

![](./media/image43.png)

4.  单击 **Open** 以打开外部网站。

![](./media/image44.png)

5.  应用程序将在新选项卡中打开。

![A screenshot of a computer Description automatically
generated](./media/image45.png)

### 任务 5 ：流式传输诊断日志

Azure
应用服务捕获输出到控制台的所有消息，以帮助你诊断应用程序的问题。该应用程序包含
print（） 语句来演示此功能，如下所示。

@app.route('/', methods=\['GET'\])

def index():

print('Request for index page received')

restaurants = Restaurant.query.all()

return render_template('index.html', restaurants=restaurants)

1.  切换回 **Azure 门户 - \> 资源组** “，然后单击 **”应用服务**”。

![](./media/image46.png)

2.  在 App Service 页面中。从左侧菜单中，选择 **Monitoring -\> App
    Service logs。**

![](./media/image47.png)

2.  在 Application logging **下**，确保 **File System**
    处于选中状态。如果需要，请选择它。在顶部菜单中，选择 **Save**
    （保存）。

![](./media/image48.png)

3.  从左侧菜单中，选择 **Log
    stream**。您可以看到应用程序的日志，包括平台日志和来自容器内部的日志。

![](./media/image49.png)

### 任务 6：清理 Github 中的资源。

1.  切换回 Github，单击 **repo -\> Code -\> Codespaces。**
    选择正确的分支

![](./media/image50.png)

2.  选择分支，然后单击 **Delete （删除**）。

![](./media/image51.png)

3.  点击 **Delete**。

![](./media/image52.png)

4.  切换回 **Azure 门户 -\> 资源组。**

![A screenshot of a computer Description automatically
generated](./media/image53.png)

5.  选择所有资源，然后单击 **Delete** （ 不删除资源组）

![](./media/image54.png)

6.  输入 ''Delete'' 然后点击 **Delete**。

![](./media/image55.png)

7.  单击 **Delete** 以确认删除。

![](./media/image56.png)
