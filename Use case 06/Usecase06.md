# 用例 06 - 使用 PostgreSQL 靈活服務器在 Azure 容器應用上部署聊天應用

**目標:**

- 若要通過安裝 Azure CLI 在 Windows 上配置開發環境，Node.js、分配 Azure
  訂閱角色、啟動 Docker Desktop 以及啟用帶有開發容器擴展的 Visual Studio
  Code。

- 在 Azure 上使用 PostgreSQL 和 OpenAI 部署和測試自定義聊天應用程序。

![A screenshot of a computer Description automatically
generated](./media/image1.jpeg)

在此用例中，您將設置一個全面的開發環境，部署與 PostgreSQL
集成的聊天應用程序，並驗證其在 Azure 上的部署。這涉及安裝 Azure
CLI、Docker 和 Visual Studio Code
等基本工具（我們已經在主機環境中為您完成了）、在 Azure
中配置用戶角色、使用 Azure 開發人員 CLI
部署應用程序以及與部署的資源交互以確保功能。

**使用的關鍵技術** -- Python, FastAPI, Azure OpenAI models, Azure
Database for PostgreSQL and azure-container-apps,ai-azd-templates.

**預計持續時間**-- 45 分鐘

**實驗室類型:** 講師指導

**先決條件：**

GitHub 帳戶 -- 您應該擁有自己的 GitHub
登錄憑證。如果您沒有，請從此處創建一個
- **https://github.com/signup?user_email=&source=form-home-signupobjectives**

## 練習 1 ：配置、部署應用程序並從瀏覽器對其進行測試

### 任務 1：複製現有資源組名稱

1.  打開瀏覽器，打開 Azure 門戶\`\`https:\\portal.azure.com\`\`.
     使用主機環境的說明/資源部分下提供的 ***Azure 切片帳戶（Azure
    憑據***）登錄。

![A screenshot of a computer Description automatically
generated](./media/image2.jpeg)

2.  在主頁上，單擊“**資源組** ”磁貼。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

3.  確保您已經創建了一個資源組供您使用。切勿刪除此資源組。相反，您可以刪除資源組中的資源，但不能刪除資源組本身。

4.  單擊 Resource Group Name

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  複製資源組名稱並將其保存在記事本中，以用於將所有資源部署到此資源組

![A screenshot of a computer Description automatically
generated](./media/image5.png)

### 任務 2：運行 Docker

1.  在 Desktop 上，雙擊 **Docker Desktop**。

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

2.  運行 Docker Desktop。

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

### 任務 3 ： 註冊服務提供商

1.  切換回 Azure 門戶選項卡，單擊“ **訂閱** ”磁貼。

![](./media/image8.png)

2.  單擊訂閱名稱。

![](./media/image9.png)

3.  單擊 **左側導航菜單中的** 設置 - \> 資源提供程序。

![](./media/image10.png)

4.  類型\`\`**Microsoft.AlertsManagement**\`\` ，然後按 Enter
    鍵。選擇它，然後單擊**註冊**。

![](./media/image11.png)

![A screenshot of a computer Description automatically
generated](./media/image12.png)

### 任務 4 ：開放開發環境

1.  打開瀏覽器，導航到地址欄，鍵入或粘貼以下
    URL：“https://github.com/technofocus-pte/rag-postgres-openai-python.git”選項卡打開，並要求您在
    Visual Studio Code 中打開。選擇 **Open Visual Studio Code。**

![](./media/image13.jpeg)

2.  單擊 **fork** 以分叉存儲庫。為存儲庫指定唯一名稱，然後單擊 **Create
    repo** 按鈕。

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

3.  單擊**“代碼 - \> Codespaces”-\> Codespaces+**

![A screenshot of a computer Description automatically
generated](./media/image16.jpeg)

4.  等待 Codespaces 環境設置 。完全設置需要幾分鐘

![A screenshot of a computer Description automatically
generated](./media/image17.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

### 任務 5：預配服務並將應用程序部署到 Azure

1.  在終端上運行以下命令。它會生成要複製的代碼。複製代碼並按 Enter。

\`\`azd auth login\`\`

![](./media/image19.png)

2.  默認瀏覽器將打開，以輸入生成的代碼進行驗證。輸入代碼，然後單擊
    **Next（下一步**）。

![](./media/image20.png)

3.  使用 Azure 憑據登錄。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

4.  若要為 Azure 資源創建環境，請運行以下 Azure 開發人員 CLI
    命令。它要求您輸入 環境名稱 。輸入您選擇的任何名稱，然後按
    Enter（例如 ：**ragpgpy**）

**注：** 創建環境時，請確保名稱由小寫字母組成。

\`\`azd env new\`\`

![A screenshot of a computer Description automatically
generated](./media/image22.png)

5.  運行以下 Azure Developer CLI 命令以預配 Azure 資源並部署代碼。

\`\`azd provision \`\`

![A screenshot of a computer Description automatically
generated](./media/image23.png)

6.  出現提示時，選擇一個**訂閱**以創建資源，然後選擇離你最近的區域;在本實驗中，我們選擇了美國**東部
    2** 區域。

![](./media/image24.png)

7.  它會提示你 “**Enter a value for the 'existingResourceGroupName'
    infrastructure parameter:**” 輸入在任務 1 中複製的資源組 (eg :
    **ResourceGroup1 用於開發切片) .**您可以從 Resources
    **部分複製資源組名稱** ，如下圖所示

> ![](./media/image25.png)

8.  出現提示時，**為“openAILocation”基礎設施參數輸入一個值，**選擇離你最近的區域;在本練習中，我們選擇了**美國中北部**區域

![](./media/image26.png)

9.  預置資源大約需要 5 到 10 分鐘。如果 **出現提示**，請單擊 Yes。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

10. 等待模板成功預置所有資源。

![A screenshot of a computer Description automatically
generated](./media/image28.png)

11. 執行以下命令設置資源組

\`\`azd env set AZURE_RESOURCE_GROUP {your resource group name}\`\`

![](./media/image29.png)

12. 運行以下命令，將應用部署到 Azure。

\`\`azd deploy\`\`

![](./media/image30.png)

13. 等待部署完成。部署需要 \<5

![A screenshot of a computer Description automatically
generated](./media/image31.png)

14. 單擊已部署的 Web 應用程序終端節點鏈接。

![](./media/image32.png)

15. 點擊 **Open**。它打開帶有應用程序的新標簽頁

![](./media/image33.png)

16. 應用程序隨即打開。

![A screenshot of a chat Description automatically
generated](./media/image34.png)

### 任務 6：使用聊天應用程序從文件中獲取答案

1.  在 **RAG on database |OpenAI+PoastgreSQL** Web
    應用程序頁面，**點擊最適合遠足的鞋子？** 按鈕並觀察輸出

![](./media/image35.png)

![A screenshot of a computer Description automatically
generated](./media/image36.png)

2.  點擊 **clear chat。**

![](./media/image37.png)

3.  在 **RAG on database |OpenAI+PoastgreSQL** Web 應用頁面，點擊
    **Climbing gear cheap than $30** 按鈕並觀察輸出

![](./media/image38.png)

![A screenshot of a computer Description automatically
generated](./media/image39.png)

4.  點擊 **clear chat。**

### 任務 7：在 Azure 門戶中驗證已部署的資源

1.  在 Azure 門戶的主頁上，單擊**“資源組**”。

![](./media/image40.png)

2.  單擊您的資源組名稱

![](./media/image41.png)

3.  確保已成功部署以下資源

    - 容器應用程序

    - 應用程序洞察

    - 容器應用程序環境

    - Log Analytics 工作區

    - Azure OpenAI

    - Azure Database for PostgreSQL 靈活服務器

    - 容器註冊表

![](./media/image42.png)

4.  單擊 **Azure OpenAI** 資源名稱。

![](./media/image43.png)

5.  在 左側導航菜單中的 Overview 上，單擊 **Go to Azure AI Foundry
    portal** ，然後選擇以打開新選項卡。

![](./media/image44.png)

6.  單擊左側導航菜單中**的共享資源** **-\> 部署**，並確保**應成功部署**
    gpt-35-turbo**、**text-embedding-ada-002

![](./media/image45.png)

### 任務 8 ： 清理所有資源

要清理此示例創建的所有資源，請執行以下作：

1.  切換回 **Azure portal -\> Resource group- \> Resource group name.**

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2.  選擇所有資源，然後單擊 Delete （刪除），如下圖所示。 (**不要刪除**
    資源組）

![](./media/image47.png)

3.  在文本框中鍵入 '' **delete** '' ，然後單擊 **Delete**.

![](./media/image48.png)

4.  單擊 **Delete** 確認刪除。

![](./media/image49.png)

5.  切換回 Github 門戶選項卡並刷新頁面。

![A screenshot of a computer Description automatically
generated](./media/image50.png)

6.  單擊 Code ，選擇為此實驗室創建的分支，然後單擊 **Delete** 。

![A screenshot of a computer Description automatically
generated](./media/image51.png)

7.  單擊 **Delete** 按鈕確認分支刪除。

![A screenshot of a computer Description automatically
generated](./media/image52.png)
