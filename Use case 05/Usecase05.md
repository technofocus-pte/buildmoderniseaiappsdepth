# 用例 05 - 使用 Azure Database for PostgreSQL 部署數據驅動的 Python 餐廳 Web 應用

**目標:**

此用例使用 Flask 框架和 Azure Database for PostgreSQL 關系數據庫服務部署
Python Web 應用。Flask 應用託管在完全託管的 Azure
應用服務中。此應用程序旨在在本地運行，然後部署到 Azure你將使用 Azure
Database for PostgreSQL **關系數據庫服務將數據驅動的 Python Web
應用程序（Django** 或 **Flask**）部署到 **Azure 應用程序服務**。Azure
應用服務在 Linux 服務器環境中支持 Python。

![A diagram of a service plan Description automatically
generated](./media/image1.jpeg)

**使用的關鍵技術**-- Java 17, Azure Database for PostgreSQL

**預計持續時間** -- 45 分鐘

**實驗類型：** 講師指導

**先決條件：**

GitHub 帳戶 -- 您應該擁有自己的 GitHub
登錄憑證。如果您沒有，請從此處創建一個
- **https://github.com/signup?user_email=&source=form-home-signupobjectives**

requirements.txt 具有以下包，全部由典型的數據驅動型 Flask 應用程序使用：

[TABLE]

### 任務 1 ：註冊服務提供商

1.  打開瀏覽器並轉到 <https://portal.azure.com> 並使用 VM
    的“資源”選項卡中提供的雲切片帳戶登錄。

> ![](./media/image2.png)

2.  在 Azure 門戶的主頁上，單擊“ **資源組** ”磁貼。

![](./media/image3.png)

3.  複製資源組名稱並將其保存在記事本中，以便使用下一個任務在此資源組中部署所需的資源。

![](./media/image4.png)

4.  在頂部導航欄上，單擊 主頁 。

![](./media/image5.png)

5.  單擊 **Subscriptions** 磁貼。

![](./media/image6.png)

6.  單擊訂閱名稱。

![](./media/image7.png)

7.  展開 個人設置 從左側導航菜單。單擊 **Resource providers**，輸入
    Microsoft.AlertsManagement 並選擇它，然後單擊 **Register**。

> ![](./media/image8.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image9.png)

### 任務 2：創建 Github Codespace 以啟動 Azure 開發人員 CLI 模板

此用例具有開發容器配置，可以更輕鬆地在本地開發應用程序、將其部署到 Azure
並對其進行監視。我們使用 Azure 開發 CLI 模板來部署應用程序

1.  打開瀏覽器並轉到 ''https：\\github.com'' 並使用您的 Github
    帳戶登錄。

2.  單擊 Fork （複刻） 將此存儲庫
    https://github.com/technofocus-pte/msdocs-flask-postgresql-sample-app
    **分叉到您的帳戶** ，如下圖所示。

![A screenshot of a computer Description automatically
generated](./media/image10.jpeg)

3.  輸入唯一名稱，然後單擊 **Create repo**。

![A screenshot of a computer Description automatically
generated](./media/image11.jpeg)

4.  從複刻的存儲庫根目錄中，選擇 **Code** \> **Codespaces** \> **+**。

![A screenshot of a computer Description automatically
generated](./media/image12.jpeg)

5.  等待工作區設置完成。

![A screenshot of a computer Description automatically
generated](./media/image13.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

6.  在 codespace 終端中，運行以下命令：

> \# Install requirements

\`\`python3 -m pip install -r requirements.txt\`\`

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

7.  運行以下命令以創建環境變量

> \# Create .env with environment variables

\`\`cp .env.sample.devcontainer .env\`\`

![A screenshot of a computer program Description automatically
generated](./media/image16.jpeg)

8.  運行以下命令進行數據遷移

> \# Run database migrations

\`\`python3 -m flask db upgrade\`\`

![A screenshot of a computer program Description automatically
generated](./media/image17.jpeg)

9.  運行以下命令

> \# Start the development server

\`\`python3 -m flask run\`\`

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

10. 當您看到消息 Your application running on port is
    available.（您的應用程序在端口上運行可用）時，單擊 **Open in
    Browser（在瀏覽器中打開**）。

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image20.jpeg)

11. 點擊 **Add new restaurant** 按鈕。

![A white screen with black text Description automatically
generated](./media/image21.jpeg)

12. 在下面輸入詳細信息，然後單擊 **Submit** 按鈕。

名稱 : \`\`**Contoso Rica\`\`**

街道地址 - \`\`**3A ,8th cross, Ferns street , Singapore\`\`**

描述 - \`\`**這是一家位於城市購物中心的中高價位餐廳\`\`**

![A screenshot of a restaurant Description automatically
generated](./media/image22.jpeg)

13. 點擊 **Add new review** 按鈕。

![A screenshot of a computer Description automatically
generated](./media/image23.jpeg)

14. 輸入您的評論，然後單擊按鈕。 **保存更改**

**您的姓名 ： 您的姓名**

**評分 ： 您的評分**

\`\`這是一家位於城市購物中心的中高檔餐廳。服務有點混亂，因為我們至少有 6
個服務員來問我們事情。食物需要一些時間才能來。我們有 2
個菜單：一個印度菜單和一個泰國菜單。泰國菜便宜
30%，所以我們點了一些開胃菜和泰國紅咖喱。食物需要一些時間，但這是值得的。它很好吃，而且準備得非常好。總的來說，這是一頓不錯的飯菜。

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

![A white card with black text Description automatically
generated](./media/image25.jpeg)

1.  Add some more reviews and new restaurant with comments.

![A screenshot of a computer Description automatically
generated](./media/image26.jpeg)

### 任務 3：在 Azure 中預配所需的資源。

此項目旨在與 Azure Developer CLI
配合使用，從而更輕鬆地在本地開發應用程序、將其部署到 Azure
並對其進行監視。

1.  切換回 Github 代碼空間選項卡，運行以下命令來初始化一個新的 azd
    環境：

\`\`azd init\`\`

![](./media/image27.jpeg)

2.  它將提示您提供環境名稱 (例如 **flask-app**XXXX (XXXX
    可以是唯一的數字)), 稍後將用於已部署資源的名稱。

![](./media/image28.jpeg)

3.  如果需要，請登錄 '\`**azd auth login\`\`** .複製代碼並按 Enter。

![A screenshot of a computer Description automatically
generated](./media/image29.jpeg)

4.  輸入代碼，然後使用 Azure 憑據登錄。

![A screenshot of a computer error Description automatically
generated](./media/image30.jpeg)

![](./media/image31.jpeg)

![A screenshot of a computer error Description automatically
generated](./media/image32.jpeg)

![A screenshot of a computer program Description automatically
generated](./media/image33.jpeg)

5.  切換回 Gtihub codespace
    選項卡並運行以下命令來配置和部署所有資源。它將提示選擇您的 Azure
    訂閱。輸入 **1** 以選擇您的訂閱，然後按 Enter。

**\`\`azd provision\`\`**

![A computer screen shot of a computer code Description automatically
generated](./media/image34.png)

6.  選擇位置作為
    **WestUS/eastus**。然後，它將預置您賬戶中的資源並部署最新代碼。如果您在部署時遇到錯誤，更改位置（如更改為“westus”）可能會有所幫助，因為某些資源可能存在可用性限制。

![A screenshot of a computer program Description automatically
generated](./media/image35.png)

7.  輸入 Azure 門戶中的資源組名稱（在上一個任務中複製），然後按 Enter。

![](./media/image36.png)

8.  部署需要 **20 到 30 分鐘**。還可以在生成的鏈接或 **Azure
    門戶\>資源組\>部署中檢查部署狀態**。

![](./media/image37.png)

![A screenshot of a computer Description automatically
generated](./media/image38.png)

![A screenshot of a computer Description automatically
generated](./media/image39.png)

![A screenshot of a computer Description automatically
generated](./media/image40.png)

### 任務 4：從 Github 部署應用程序

1.  運行以下命令以設置資源組環境變量。

\`\`azd env set AZURE_RESOURCE_GROUP {Name of existing resource group}
\`\`

注意：將 {Name of existing resource group} 替換為 VM
中“資源”部分下可用的資源組名稱。

![](./media/image41.png)

2.  運行以下命令以部署所有資源並等待部署成功完成。

\`\`azd deploy\`\`

![](./media/image42.png)

3.  單擊生成的 Endpoint URL

![](./media/image43.png)

4.  單擊 **Open** 以打開外部網站。

![](./media/image44.png)

5.  應用程序將在新選項卡中打開。

![A screenshot of a computer Description automatically
generated](./media/image45.png)

### 任務 5 ：流式傳輸診斷日誌

Azure
應用服務捕獲輸出到控制台的所有消息，以幫助你診斷應用程序的問題。該應用程序包含
print（） 語句來演示此功能，如下所示。

@app.route('/', methods=\['GET'\])

def index():

print('Request for index page received')

restaurants = Restaurant.query.all()

return render_template('index.html', restaurants=restaurants)

1.  切換回 **Azure 門戶 - \> 資源組** “，然後單擊 **”應用服務**”。

![](./media/image46.png)

2.  在 App Service 頁面中。從左側菜單中，選擇 **Monitoring -\> App
    Service logs。**

![](./media/image47.png)

2.  在 Application logging **下**，確保 **File System**
    處於選中狀態。如果需要，請選擇它。在頂部菜單中，選擇 **Save**
    （保存）。

![](./media/image48.png)

3.  從左側菜單中，選擇 **Log
    stream**。您可以看到應用程序的日誌，包括平臺日誌和來自容器內部的日誌。

![](./media/image49.png)

### 任務 6：清理 Github 中的資源。

1.  切換回 Github，單擊 **repo -\> Code -\> Codespaces。**
    選擇正確的分支

![](./media/image50.png)

2.  選擇分支，然後單擊 **Delete （刪除**）。

![](./media/image51.png)

3.  點擊 **Delete**。

![](./media/image52.png)

4.  切換回 **Azure 門戶 -\> 資源組。**

![A screenshot of a computer Description automatically
generated](./media/image53.png)

5.  選擇所有資源，然後單擊 **Delete** （ 不刪除資源組）

![](./media/image54.png)

6.  輸入 ''Delete'' 然後點擊 **Delete**。

![](./media/image55.png)

7.  單擊 **Delete** 以確認刪除。

![](./media/image56.png)
