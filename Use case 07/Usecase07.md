# 用例 07 - 在 Azure Database for PostgreSQL 灵活服务器中启用 semantic 搜索，以使用 Azure OpenAI 生成向量嵌入。

**目标**:

在此用例中，你将实现语义搜索以生成和存储嵌入，在 Azure Database for
PostgreSQL 灵活服务器中安装向量和azure_ai扩展，然后应用扩展来存储 Azure
OpenAI 生成的嵌入向量

**使用的关键技术**-- Azure OpenAI, Azure Database for PostgreSQL, Azure
AI extension

**预计持续时间**-- 45 minutes

**实验室类型:** Instructor Led

## 练习 1 ：使用 Azure OpenAI 生成向量嵌入

要执行语义搜索，您必须首先从模型生成嵌入向量，将它们存储在向量数据库中，然后查询嵌入向量。您将创建一个数据库，使用示例数据填充该数据库，然后针对这些列表运行语义搜索。

在本练习结束时，你将拥有一个启用了矢量和azure_ai扩展的 Azure Database
for PostgreSQL 灵活服务器实例。您将为 Seattle Airbnb Open Data
数据集的房源表生成嵌入向量。您还将通过生成查询的嵌入向量并执行向量余弦距离搜索来针对这些列表运行语义搜索。

1.  打开 Web 浏览器并导航到\`\`https:\\portal.azure.com/\`\`并使用您的
    Azure 凭据登录。

2.  选择 Azure 门户工具栏中的 Cloud Shell 图标，在浏览器窗口底部打开新的
    Cloud Shell 窗格。选择 **Bash**。

![A screenshot of a computer Description automatically
generated](./media/image1.jpeg)

3.  选择**“无需存储帐户”**单选按钮，选择订阅，然后单击**“应用**”。

![](./media/image2.png)

4.  在 Cloud Shell 提示符下，运行以下命令以克隆项目

\`\`git clone https://github.com/technofocus-pte/postgresql-case\`\`

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

5.  导航到项目文件夹。

**\`\`cd postgresql-case\`\`**

![A screenshot of a computer Description automatically
generated](./media/image4.jpeg)

6.  接下来，运行三个命令来定义变量，以减少使用 Azure CLI 命令创建 Azure
    资源时的冗余键入。这些变量表示要分配给资源组的名称
    （RG_NAME）、将要将资源部署到的 Azure 区域 （REGION） 以及随机生成的
    PostgreSQL 管理员登录名密码 （ADMIN_PASSWORD）。

7.  在第一个命令中，分配给相应变量的区域是 eastus 或 westus， **\[but
    you can replace it with a location of your
    preference.\]{.mark}** 但是，如果替换默认值，则必须选择另一个
    \[支持抽象摘要的 Azure
    区域\]{.underline}，以确保可以完成此学习路径中模块中的所有任务。

\`\`REGION=westus\`\`

8.  以下命令分配要用于资源组的现有资源组名称，该资源组将容纳本练习中使用的所有资源。

> \`\`RG_NAME=Your existing resource group name\`\`

![](./media/image5.png)

9.  最后一个命令会随机生成 PostgreSQL 管理员登录的密码。
    **请确保将其复制到**安全的位置，以便以后用于连接到 PostgreSQL
    灵活服务器。

> a=()
>
> for i in {a..z} {A..Z} {0..9};
>
> do
>
> a\[$RANDOM\]=$i
>
> done
>
> ADMIN_PASSWORD=$(IFS=; echo "${a\[\*\]::18}")
>
> echo "Your randomly generated PostgreSQL admin user's password is:"

echo $ADMIN_PASSWORD

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

### 任务 1：分配认知服务参与者

1.  打开新标签页并转到\`\`**https://portal.azure.com\`\`** .使用 Azure
    凭据登录，然后单击“**订阅**”磁贴。

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

2.  单击订阅名称 .

![](./media/image8.png)

3.  单击左侧导航菜单中的 Access control （IAM）。单击 **Add** 并选择
    **Add role assignment。**

![](./media/image9.png)

4.  寻找 \`\`Cognitive Services Contributor\`\` 并选择它，然后单击
    **下一页** 按钮。

![A screenshot of a service assignment Description automatically
generated](./media/image10.jpeg)

5.  选择 **User， group or service
    principal（用户、组或服务主体**），然后单击 **select member
    （选择成员**） 链接。搜索 Azure 订阅帐户并选择它。最后，点击
    **Select** 按钮。

![](./media/image11.png)

6.  单击 **Review + assign** 按钮。

![](./media/image12.png)

7.  再次单击 **Review + assign** 按钮。

> ![](./media/image13.png)

### 任务 2：运行 Bicep 部署脚本以预配 Azure 资源

1.  使用 Azure CLI 切换回 Azure 门户的第 1 个选项卡，以执行 Bicep
    部署脚本以在资源组中预配 Azure 资源：部署需要 3 - 5 分钟

\`\`cd\`\`

\`\`az deployment group create --resource-group $RG_NAME --template-file
"postgresql-case/Allfiles/Labs/Shared/deploy.bicep" --parameters
restore=false adminLogin=pgAdmin adminLoginPassword=$ADMIN_PASSWORD\`\`

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

![A computer screen shot of a black screen Description automatically
generated](./media/image15.jpeg)

2.  Bicep 部署脚本将完成此练习所需的 Azure
    服务预配到资源组中。部署的资源包括 Azure Database for PostgreSQL
    灵活服务器。您可以检查资源组中的资源。

- **Azure OpenAI,**

- **Azure AI 语言服务.**

![](./media/image16.png)

3.  单击 Open AI resource

![](./media/image17.png)

4.  单击左侧导航菜单中 **Resource Management** 下的 **Keys and
    Endpoint**。记下 Key 1 和 endpoint 以在任务 5 中使用它们

![](./media/image18.png)

5.  Bicep 脚本还会执行一些配置步骤，例如将 azure_ai 和 vector 扩展添加到
    PostgreSQL 服务器的*允许列表*（通过 azure.extensions
    服务器参数），在服务器上创建名为 rentals 的数据库，以及使用
    **text-embedding-ada-002** 模型将名为 embedding 的部署添加到 Azure
    OpenAI 服务。

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

6.  部署通常需要几分钟才能完成。您可以从 Cloud Shell
    监控它，也可以导航到您在上面创建的资源组的 **Deployments （部署）**
    页面，然后观察其中的部署进度。

7.  资源部署完成后，关闭 Cloud Shell 窗格。

### 任务 3：使用 Azure Cloud Shell 中的 psql 连接到数据库

在此任务中，您将使用 Azure Cloud Shell 中的 psql 命令行实用程序连接到
Azure Database for PostgreSQL 服务器上的租赁数据库。

1.  在 Azure 门户 （https://portal.azure.com/） 中，导航到新创建的 Azure
    Database for PostgreSQL 灵活服务器。

![](./media/image20.png)

2.  在侧边栏上，选择 **Server Parameters** 。搜索 **azure.extensions**
    参数。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

3.  选择扩展 **Vector** 和 **AZURE_AI** 如果尚未选择。

![A screenshot of a computer Description automatically
generated](./media/image22.png)

4.  在资源菜单中的 **Settings** 下，选择 **Databases** 选择 **Connect**
    作为 rentals 数据库。

![](./media/image23.png)

5.  在 Cloud Shell 的“Password for user pgAdmin”提示符下，输入随机生成的
    **pgAdmin** 登录密码。

登录后，将显示 rentals 数据库的 psql 提示符。

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

6.  在本练习的其余部分，您将继续在 Cloud Shell
    中工作，因此通过选择窗格右上角的 Maximize （最大化）
    按钮来扩展浏览器窗口中的窗格可能会有所帮助 。

![A screenshot of a computer Description automatically
generated](./media/image25.jpeg)

### 任务 4 ：配置扩展

若要存储和查询向量以及生成嵌入向量，需要为 Azure Database for PostgreSQL
灵活服务器设置允许列表并启用两个扩展：向量和azure_ai。

1.  使用 Azure cli 切换回 Azure 门户选项卡，并运行以下 SQL
    命令以启用矢量扩展。有关详细说明

\`\`CREATE EXTENSION vector;\`\`

![A screenshot of a computer Description automatically
generated](./media/image26.jpeg)

2.  要启用 azure_ai 扩展， **请更新并运行** 以下 SQL 命令。需要 Azure
    OpenAI 资源的终结点和 API 密钥。

> \`\`CREATE EXTENSION azure_ai;\`\`
>
> \`\`SELECT azure_ai.set_setting('azure_openai.endpoint',
> 'https://\<endpoint\>.openai.azure.com');\`\`

\`\`SELECT azure_ai.set_setting（'azure_openai.subscription_key', '\<API
Key\>');\`\`

![A screenshot of a computer program Description automatically
generated](./media/image27.jpeg)

### 任务 5 ：使用示例数据填充数据库

在浏览 azure_ai 扩展之前，请向 rentals
数据库添加几个表，并使用示例数据填充它们，以便在查看扩展的功能时可以使用信息。

1.  运行以下命令以创建列表和评论表，用于存储出租物业列表和客户评论数据：

DROP TABLE IF EXISTS listings;

CREATE TABLE listings (

id int,

name varchar(100),

description text,

property_type varchar(25),

room_type varchar(30),

price numeric,

weekly_price numeric

);

![A screenshot of a computer Description automatically
generated](./media/image28.jpeg)

DROP TABLE IF EXISTS reviews;

CREATE TABLE reviews (

id int,

listing_id int,

date date,

comments text

);

![A screenshot of a computer Description automatically
generated](./media/image29.jpeg)

2.  接下来，使用 COPY 命令将 CSV
    文件中的数据加载到您上面创建的每个表中。首先，运行以下命令以填充
    listings 表：

\`\`\COPY listings FROM
'postgresql-case/Allfiles/Labs/Shared/listings.csv' CSV HEADER\`\`

命令输出应为 COPY 50，表示已将 50 行从 CSV 文件写入表中。

![A screenshot of a computer program Description automatically
generated](./media/image30.jpeg)

3.  最后，运行以下命令，将客户评论加载到 reviews 表中：

\`\`\COPY reviews FROM
'postgresql-case/Allfiles/Labs/Shared/reviews.csv' CSV HEADER\`\`

命令输出应为 COPY 354，表示已将 354 行从 CSV 文件写入表中。

![](./media/image31.jpeg)

4.  要重置示例数据，您可以执行 DROP TABLE 列表，然后重复这些步骤。

### 任务 6：创建和存储嵌入向量

现在我们已经有一些样本数据，是时候生成和存储嵌入向量了。azure_ai
扩展使调用 Azure OpenAI 嵌入 API 变得容易。

1.  添加 embedding vector 列。

text-embedding-ada-002 模型配置为返回 1,536
个维度，因此请将其用于向量列大小。

\`\`ALTER TABLE listings ADD COLUMN listing_vector vector(1536);\`\`

![A computer screen shot of a black screen Description automatically
generated](./media/image32.jpeg)

2.  通过create_embeddings用户定义的函数调用 Azure
    OpenAI，为每个列表的描述生成嵌入向量，该函数由 azure_ai 扩展实现：

> UPDATE listings SET listing_vector =
> azure_openai.create_embeddings('embedding', description, max_attempts
> =\> 5, retry_delay_ms =\> 500) WHERE listing_vector IS NULL;

请注意，这可能需要几分钟时间，具体取决于可用配额。

![A screenshot of a computer screen Description automatically
generated](./media/image33.png)

### 任务 7 ：执行 Semantic 搜索查询

现在，您已经使用嵌入向量扩充了列表数据，是时候运行语义搜索查询了。为此，请获取查询字符串
embedding
vector，然后执行余弦搜索以查找其描述在语义上与查询最相似的列表。

1.  在余弦搜索中使用嵌入（\\=\> 表示余弦距离运算），获取与查询最相似的前
    10 个列表。

SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;

您将得到与此类似的结果。结果可能会有所不同，因为不能保证嵌入向量是确定性的：

![A screenshot of a computer Description automatically
generated](./media/image34.jpeg)

2.  您还可以投影 description 列，以便能够读取其 description
    在语义上相似的匹配行的文本。例如，此查询返回最佳匹配项：

SELECT id, description FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 1;

这将打印出如下内容：

![A screenshot of a computer Description automatically
generated](./media/image35.jpeg)

要直观地理解语义搜索，请注意描述实际上不包含术语 “bright” 或
“natural”。但它确实突出了 “夏天” 和 “阳光”、“窗户” 和 “天花板窗户”。

### 任务 8 ： 检查您的工作

执行上述步骤后，房源表包含来自 Kaggle 上西雅图 Airbnb Open Data
的示例数据。这些列表通过嵌入向量进行扩充，以执行语义搜索。

1.  确认商品表有四列：id、name、description 和 listing_vector。

\`\`\d listings\`\`

它应该打印类似:

![A screenshot of a computer Description automatically
generated](./media/image36.jpeg)

2.  确认至少有一行具有填充的 listing_vector 列.

\`\`SELECT COUNT(\*) \> 0 FROM listings WHERE listing_vector IS NOT
NULL;\`\`

结果必须显示 t，表示 true。指示至少有一行包含其相应 description
列的嵌入：

![A screen shot of a computer Description automatically
generated](./media/image37.jpeg)

3.  确认嵌入向量有 1536 个维度：

\`'选择 vector_dims(listing_vector) FROM listing_vector 不为 NULL 的商品
LIMIT 1;\`\`

Yielding:

![A screen shot of a computer Description automatically
generated](./media/image38.jpeg)

4.  确认语义搜索返回结果。

在余弦搜索中使用 embedding，获取与查询最相似的前 10 个列表。

\`\`SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;\`\`

![A screenshot of a computer program Description automatically
generated](./media/image39.jpeg)

5.  请返回同一页面以继续下一个任务。

## 练习 2 - 为推荐系统创建搜索函数

让我们将向量嵌入逻辑和 API 调用包装在一个函数中。在本练习中，你将在
Azure Database for PostgreSQL 灵活服务器中安装 vector 和 azure_ai
扩展，并探索该扩展将 [Azure
OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/overview)
集成到 database.

### 任务 1 ：为推荐系统创建搜索函数

让我们使用语义搜索构建一个推荐系统。系统将根据提供的示例列表推荐多个列表。该示例可能来自用户正在查看的列表或其首选项。我们将利用
azure_openai 扩展将系统实现为 PostgreSQL 函数。

在本练习结束时，您将定义一个函数recommend_listing，该函数最多提供与提供的
sampleListingId 最相似的 numResults
列表。您可以使用这些数据来推动新的机会，例如将推荐商品与打折商品合并。

将资源部署到 Azure 订阅中

此步骤将指导您使用 Azure Cloud Shell 中的 Azure CLI 命令创建资源组并运行
Bicep 脚本，以将完成此练习所需的 Azure 服务部署到 Azure 订阅中。

**注意：**如果您在此学习路径中学习多个模块，则可以在它们之间共享 Azure
环境。在这种情况下，您只需完成此资源部署步骤一次。

### 任务 2 ：创建推荐函数

1.  推荐函数采用 sampleListingId 并返回 numResults
    最相似的其他列表。为此，它会创建示例列表的名称和描述的嵌入，并针对列表嵌入运行该查询向量的语义搜索。

> CREATE FUNCTION
>
> recommend_listing(sampleListingId int, numResults int)
>
> RETURNS TABLE(
>
> out_listingName text,
>
> out_listingDescription text,
>
> out_score real)
>
> AS $$
>
> DECLARE
>
> queryEmbedding vector(1536);
>
> sampleListingText text;
>
> BEGIN
>
> sampleListingText := (
>
> SELECT
>
> name || ' ' || description
>
> FROM
>
> listings WHERE id = sampleListingId
>
> );
>
> queryEmbedding := (
>
> azure_openai.create_embeddings('embedding', sampleListingText,
> max_attempts =\> 5, retry_delay_ms =\> 500)
>
> );
>
> RETURN QUERY
>
> SELECT
>
> name::text,
>
> description,
>
> -- cosine distance:
>
> (listings.listing_vector \<=\> queryEmbedding)::real AS score
>
> FROM
>
> listings
>
> ORDER BY score ASC LIMIT numResults;
>
> END $$
>
> LANGUAGE plpgsql;

![A screenshot of a computer Description automatically
generated](./media/image40.jpeg)

### 任务 3 ：查询推荐函数

1.  要查询推荐函数，请向其传递列表 ID 和它应提出的推荐数量。

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommendations
closest to a listing

结果将类似于:

![A screenshot of a computer Description automatically
generated](./media/image41.jpeg)

2.  若要查看函数运行时，请确保 **在 Azure
    门户的**“服务器参数**”部分中启用了 track_functions（可以使用 PL 或
    ALL）：**

![](./media/image42.png)

![A screenshot of a computer Description automatically
generated](./media/image43.png)

### 任务 4 ： 检查您的工作

1.  确保该函数存在且签名正确：

\`\`\df recommend_listing\`\`

You should see the following:

![A screenshot of a computer Description automatically
generated](./media/image44.jpeg)

2.  确保您可以使用以下查询来查询它：

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommendations
closest to a listing

![A screenshot of a computer Description automatically
generated](./media/image45.jpeg)

### 任务 5 ： 清理

完成本练习后，请删除您创建的 Azure
资源。您需要为配置的容量付费，而不是为数据库的使用量付费。按照这些说明删除您的资源组以及您为此实验室创建的所有资源。

1.  在主页上，搜索 **Azure Open AI** 并选择它。

![](./media/image46.png)

2.  选择 Open AI 资源，然后单击 **Delete** 。

![](./media/image47.png)

3.  在文本框中**键入** delete **，然后单击 \*\*Delete。** 确认删除。

![](./media/image48.png)

![A screenshot of a computer Description automatically
generated](./media/image49.png)

4.  单击 **Manage deleted resources**，选择资源，然后单击 **Purge**
    按钮，如下图所示。

![](./media/image50.png)

5.  单击 Yes 确认清除。

![](./media/image51.png)

6.  在主页上，选择“ **Azure 服务”下的**“资源组”。

![A screenshot of a computer Description automatically
generated](./media/image52.jpeg)

7.  单击 Resource group name。

![](./media/image53.png)

8.  在 资源组的 Overview 页面上，选择**所有资源，**然后单击 **Delete
    。请勿删除 RESOURCE Group。**

> ![](./media/image54.png)

9.  键入 **Delete** 并单击 Delete。单击 Delete 按钮确认删除资源 。

![](./media/image55.png)

**总结**

你了解了如何在 Azure Database for PostgreSQL
灵活服务器中使用语义搜索，以使用 Azure OpenAI
生成的嵌入进行查询。您通过以下方式完成了此搜索：

- 启用 vector 和 azure_ai 扩展。

- 创建向量列以存储嵌入向量。

- 生成和存储嵌入。

- 使用查询向量查询数据库。
