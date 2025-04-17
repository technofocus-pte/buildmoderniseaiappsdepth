# 用例 04 - 构建 ASP.NET 应用的 TODO 列表，将其部署到连接到 SQL 数据库的 Azure 应用服务

**预计持续时间** 40 分钟

**实验类型** 讲师指导

**目的**

Azure 应用服务提供高度可缩放的自修补 Web
托管服务。在本实验中，你将了解如何在应用服务中部署数据驱动的 ASP.NET
应用并将其连接到 Azure SQL 数据库。完成后，你将在 Azure 中运行一个
ASP.NET 应用并连接到 SQL 数据库。

## 练习 0：了解 VM 和凭据

在此任务中，我们将识别并了解我们将在整个实验室中使用的凭证。

1.  **Instructions**选项卡包含实验室指南，其中包含在整个实验室中要遵循的说明。

2.  **Resources** 选项卡已获取执行实验室所需的凭证。

    - **URL** – Azure portal的 URL

    - **Subscription** – 这是分配给你的订阅的 ID

    - **Username** – 登录 Azure 服务时需要使用的用户 ID。

    - **Password** – Azure 登录名的密码。让我们将此用户名和密码称为
      Azure 登录凭据。我们将在提及 Azure
      登录凭据的任何地方使用这些凭据。

    - **Resource Group** – 分配给您的**Resource group**。

>[！Alert] **重要提示** 请确保在此资源组下创建所有资源

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  **Help** 选项卡包含 **Support** 信息。此处的 **ID** 值是
    将在实验室执行期间使用的实验室实例 **ID**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## 练习 1：使用 Azure SQL 数据库将 ASP.NET 应用程序部署到 Azure

### 任务 1：设置 Visual Studio 2022 并运行应用程序

1.  在 Windows **Search**栏中，键入 +++**Visual Studio**+++ 并选择
    Visual Studio 2022。如果系统要求您登录，请继续执行步骤 2 和
    3，或从步骤 4 继续。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.jpeg)

2.  单击 **Sign in** ，然后使用 VM 的 Resources 选项卡中 **User
    Credentials** 部分下的 **Username** 和**Password**登录。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

3.  选择 **Start Visual Studio**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  选择 **Open a local folder**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

5.  在 **C：\Labfiles** 中选择 **webappwithsqldb** 文件夹 ，然后单击
    **Select Folder。**

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

6.  打开文件夹后，双击 **Solution Explorer** 中的
    **DotNetAppSqlDb.sln**。

    **注意** 如果 Solution Explorer 没有自动打开，请单击 **View -\> Solution Explorer**

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

7.  单击 **Build** -> **Build Solution**。

    ![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image10.jpeg)

8.  构建完成后，选择 **Debug -\> Start Debugging**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

9.  这将打开一个运行 **Todos Web App的**浏览器。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.jpeg)

10. 通过单击将一些项目添加到应用程序中 **Create New**的
    如下面的屏幕截图所示。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.jpeg)

    ![A screenshot of a application AI-generated content may be
incorrect.](./media/image15.jpeg)

11. 向列表中添加更多项。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.jpeg)

12. 在 Visual Studio 2022 中，单击 **Debug -\> Stop Debugging**。

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

### 任务 2：将 ASP.NET 应用程序发布到 Azure

1.  在**Solution Explorer**中，右键单击 **DotNetAppSqlDb**
    项目，然后选择 **Publish**。

               ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

2.  选择 **Azure** ，然后单击 **Next**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.jpeg)

3.  在**Which Azure service would you like to use to host your
    application? **中选择**“Azure App Service(Windows)** 屏幕，然后单击
    **Next**。

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image21.jpeg)

4.  在 Publish 对话框中，单击 **Sign In**并登录到您的 Azure
    订阅（如果尚未登录）。

**注意：**如果您已登录 Microsoft 帐户，请确保该帐户持有您的 Azure
订阅。如果已登录的 Microsoft 帐户没有您的 Azure
订阅，请单击该帐户以添加正确的帐户。

5.  单击 **Create new** 以创建新的 App 服务。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.jpeg)

6.  输入以下详细信息。

[TABLE]

7.  点击 New 上**Hosting Plan**。

8.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image23.png)

9.  点击 新建托管计划 选项并输入以下详细信息，然后单击 OK。

[TABLE]

10. ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image24.png)

11. 单击 App Service 窗口上的**Create** ，然后等待创建 Azure resources。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image25.png)

12. **Publish** 对话框显示您配置的资源。单击 **Finish**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

13. 单击 **Close**。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image27.jpeg)

14. 向下滚动到 Server Dependencies 部分，然后单击 **+**
    sign以添加依赖项。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

15. 在 **Add dependency**页面中选择 **Azure SQL Database**，然后单击
    **Next**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.jpeg)

16. 在 **Connect to Azure SQL Database** 对话框中，单击 **SQL
    databases** 旁边的 **Create New**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

17. 在 **Azure SQL Database Create new **对话框中，单击
    数据库服务器旁边的**New**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

18. 填写以下详细信息，然后单击 OK。

[TABLE]

19. ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image32.png)

20. 单击 Create new 对话框中的 **Create。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

### 任务 3：配置数据库连接

1.  向导创建完数据库资源后，单击 **Next**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

2.  在 **Connect to Azure SQL Database**
    对话框中填写以下详细信息，然后单击 **Finish**。

[TABLE]

3.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image35.jpeg)

4.  单击 **Finish** 查看**summary of changes**后。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

5.  等待配置向导完成，然后单击 **Close**。Azure SQL 数据库现已
    **connected**到您的应用。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

6.  在 Publish 页面中，单击 **Publish** 在右上角。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

**注意：**这大约需要 5 分钟

7.  将 ASP.NET 应用程序部署到 Azure
    后，将启动默认浏览器，其中包含已部署应用程序的 URL Add a few to-do
    items。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

### 任务 4：在本地访问数据库

Visual Studio 允许你在 **SQL Server Object Explorer Azure**
中的新数据库。新数据库已向你创建的应用服务应用打开了防火墙。但是，要从本地计算机（例如从
Visual Studio）访问它，必须为本地计算机的公共 IP
地址打开防火墙。如果您的 Internet 服务提供商更改了您的公共 IP
地址，则需要重新配置防火墙以再次访问 Azure 数据库。

1.  从 Visual Studio 2022 的**View** 菜单中，选择**SQL Server Object
    Explorer**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

2.  在 **SQL Server Object Explorer**的顶部，单击 **Add SQL Server**
    按钮。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.jpeg)

### 任务 5：配置数据库连接

1.  在 **Connect** 对话框中，展开 **Azure** 节点。此处列出了 Azure
    中的所有 SQL 数据库实例。

2.  选择之前创建的数据库
    （**dotnetappsqldbdbserver98**）。您之前创建的连接会自动填充在底部。

3.  键入您之前创建的数据库管理员**password**
    （+++**PassWord98**+++），然后单击 Connect。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

### 任务 6：允许来自计算机的客户端连接

此时将打开 Create a new firewall rule 对话框。默认情况下，服务器仅允许从
Azure 服务（例如 Azure 应用程序）连接到其数据库。若要从 Azure
外部连接到数据库，请在服务器级别创建防火墙规则。防火墙规则允许使用本地计算机的公有
IP 地址。

该对话框已填充了您计算机的公共 IP 地址。

1.  确保已选中 Add my client IP，然后单击 OK。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.jpeg)

2.  在 Visual Studio 完成为 SQL
    数据库实例创建防火墙设置后，您的连接将显示在 **SQL Server Object
    Explorer**中。

3.  扩展您的**connection \> Databases \> \< YOUR DATABASE \> \>
    Tables**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

4.  右键单击 **Todo** 表，然后选择 **View Data**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

5.  查看表的内容。从应用程序 UI 添加的数据应在此处列出。

![](./media/image46.jpeg)

## 练习 2：使用 Code First 迁移更新应用程序

1.  在 **Solution Explorer** 中，打开代码编辑器中的
    **Models\Todo.cs**。将以下属性作为最后一行添加到 **ToDo**
    类中（在** public DateTime CreatedDate { get; set;** } 之后 行
    ），然后单击 **Save** 。

+++**public bool Done { get; set; }**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.jpeg)

### 任务 1：在本地运行 Code First 迁移

运行几个命令以更新您的本地数据库。

1.  在 **Tools** 菜单中，单击 **NuGet Package Manager** \> **Package
    Manager Console**。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image48.jpeg)

2.  在 Package Manager Console 窗口中，通过执行此命令启用 Code First
    迁移。

+++**Enable-Migrations**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.jpeg)

3.  通过执行以下命令添加迁移。

+++**Add-Migration AddProperty**+++

![](./media/image50.jpeg)

4.  通过执行以下命令更新本地数据库。

+++**Update-Database**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

5.  键入 **Ctrl+F5** 运行应用程序，或单击 **Debug -\> Start without
    Debugging**。测试编辑、详细信息并创建链接。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image52.jpeg)

6.  应用程序页面将打开，它看起来仍然相同，因为您的应用程序逻辑尚未使用此新属性。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

### 任务 2：使用 new 属性

在代码中进行一些更改以使用 Done 属性。

1.  在 Visual Studio 中，打开 **Controllers\TodosController.cs**。在第
    52 行找到 **Create（）** 方法，并将 +++**Done**+++ 添加到 Bind
    属性的属性列表中。完成后，您的 Create（） 方法签名将类似于以下代码：

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

2.  打开 **Views\Todos\Create.cshtml**。在 **CreatedDat**e 的 \< div
    class=“form-group” \>后添加以下代码。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image55.jpeg)

\<div class="form-group"\>

@Html.LabelFor(model =\> model.Done, htmlAttributes: new { @class =
"control-label col-md-2" })

\<div class="col-md-10"\>

\<div class="checkbox"\>

@Html.EditorFor(model =\> model.Done)

@Html.ValidationMessageFor(model =\> model.Done, "", new { @class =
"text-danger" })

\</div\>

\</div\>

\`\`\`

3.  打开 **Views\Todos\Index.cshtml**。在空的 **th** 元素中，在
    **CreatedDate** 的 **th** 元素之后添加以下代码。

<+++@Html.DisplayNameFor>(model =\> model.Done)+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image56.jpeg)

4.  将此代码添加到 html 的正上方。ActionLink（） 辅助方法。

5.  \<td\>

6.  @Html.DisplayFor(modelItem =\> item.Done)

\`\`\`

\![\](./media/image53.jpeg)

7.  键入 **Ctrl+F5** 以运行应用程序。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.jpeg)

### 任务 3：在 Azure 中启用 Code First 迁移

1.  右键单击项目，然后选择 **Publish**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

2.  单击 **More actions** \> **Edit** 以打开发布**settings**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

3.  在 **MyDatabaseContext** 下拉列表中，选择 Azure SQL
    数据库的数据库连接。

4.  选择 **Execute Code First Migrations** （runs on
    application，然后单击 **Save**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image60.jpeg)

5.  在 Publish 页面中，单击 **Publish**。

![A black rectangular object with white text AI-generated content may be
incorrect.](./media/image61.jpeg)

6.  更新后的应用现已在 Azure 上提供。

7.  再次尝试添加待办事项并选择
    **Done**，它们应该会在您的主页上显示为已完成的项目。

![A screenshot of a application AI-generated content may be
incorrect.](./media/image62.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image63.jpeg)

## 练习 3：流式传输应用程序日志

1.  在发布页面中，向下滚动到 **Hosting** 部分。在右上角，单击
    **... \> View Streaming Logs**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image64.jpeg)

2.  日志现在已流式传输到 Output 窗口中。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image65.jpeg)

3.  你还没有看到任何跟踪消息，因为当你首次选择 View Streaming Logs
    （查看流式日志） 时，Azure 应用会将跟踪级别设置为
    Error（错误），这只会记录错误事件。

\[！注意\] **注意：**如果您还没有看到日志记录流，请从 Visual Studio
重新启动它们。

### 任务 1：更改跟踪级别

1.  转到发布页面。在 Hosting 部分，单击 ** … \> Open in Azure portal**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image66.jpeg)

2.  在 Azure 门户 - 应用页面中，
    从**Monitoring **部分下的左窗格中选择**App Service Logs**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image67.jpeg)

3.  在 **Application Logging** （File System） 下，在 Level 中选择
    **Verbose**。单击 **Save** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image68.jpeg)

4.  在浏览器中，访问 Azure 上的 Web 应用程序并执行一些活动。

![A screenshot of a application AI-generated content may be
incorrect.](./media/image69.jpeg)

5.  跟踪消息现在流式传输到 Visual Studio 中的 Output 窗口。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image70.jpeg)

6.  要停止日志流式处理服务，请单击 Output 窗口中的 **Stop monitoring**
    按钮。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image71.jpeg)

7.  关闭 Visual Studio。

## 练习 4：清理资源

1.  在 Azure 门户中，打开分配的Resourcegroup。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image72.png)

2.  选择所有资源，然后单击 Delete。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image73.png)

3.  在文本框中键入 +++delete+++，然后选择 Delete。在确认对话框中选择
    Delete 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image74.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image75.png)

**总结**

在本实验中，你学习了如何在应用服务中部署数据驱动的 ASP.NET
应用并将其连接到 Azure SQL 数据库。
