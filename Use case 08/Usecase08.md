# 用例 08 - 構建和部署 Contoso Real Estate 聊天應用程序以支持客戶。

**目標**

此用例演示了使用 Retrieval Augmented Generation
模式在您自己的數據上創建類似 ChatGPT 的體驗的幾種方法。它使用 Azure
OpenAI 服務訪問 ChatGPT 模型 （gpt-35-turbo），並使用 Azure AI
搜索進行數據索引和檢索。

![A diagram of a software process Description automatically
generated](./media/image1.jpeg)

該用例包含示例數據，因此可以端到端嘗試。在此示例應用程序中，我們使用一家名為
Contoso Real Estate
的虛構公司，該體驗允許其客戶詢問有關其產品使用情況的支持問題。示例數據包括一組文檔，用於描述其服務條款、隱私政策和支持指南。

該應用程序由多個組件組成，包括：

- **搜索服務**: 提供搜索和檢索功能的後端服務。

- **Indexer service**：對數據進行索引並創建搜索索引的服務。

- **Web 應用程序**：提供用戶界面並協調用戶與後端服務之間交互的前端 Web
  應用程序。

![A diagram of a software system Description automatically
generated](./media/image2.jpeg)

- 聊天和 Q&A 界面

- 探索各種選項，以幫助用戶評估帶有引文、跟蹤源內容等的響應的可信度。

- 展示模型 （ChatGPT） 和檢索器之間交互的數據準備、提示構建和編排
  （Azure AI 搜索） 的可能方法

- 直接在 UX 中進行調整，以調整行為並試驗選項

- 使用 Application Insights 進行可選的性能跟蹤和監控

**使用的關鍵技術**-- Azure OpenAI Service, ChatGPT model (gpt-35-turbo),
and Azure AI Search

**預計持續時間 --** 40 分鐘

## 練習 1 ：部署應用程序並從瀏覽器對其進行測試

### 任務 1：開放開發環境

1.  打開瀏覽器，導航到地址欄，鍵入或粘貼以下
    URL： \`\`https://github.com/technofocus-pte/azure-search-openai-javascript\`\` 並使用您的
    Github 帳戶登錄。

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

2.  單擊 **Fork**。

![A screenshot of a web page Description automatically
generated](./media/image4.jpeg)

3.  輸入存儲庫名稱，然後單擊 **Create fork**.

![A screenshot of a computer Description automatically
generated](./media/image5.jpeg)

4.  點擊 **Code -\> Codespaces -\> +**

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

5.  等待環境設置完成。需要 5-10 分鐘。

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

### 任務 2：預配所需的服務以生成聊天應用並將其部署到 Azure

1.  在終端上運行以下命令。複製代碼並按 Enter。

\`\`azd auth login\`\`

![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  默認瀏覽器打開以輸入代碼。輸入複製的代碼，然後單擊 **Next**。

![](./media/image9.png)

3.  使用 Azure 憑據登錄。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

![A screenshot of a computer error Description automatically
generated](./media/image11.png)

4.  切換回 Github Codespace
    選項卡。執行以下命令，初始化當前目錄下的工程環境。將 Environment
    name （環境名稱） 輸入為 \`\`**ragpgpy \`\`** ，然後按 Enter 鍵。

注意：env name 應該是唯一的

\`\` azd env new\`\`

![](./media/image12.png)

5.  運行以下命令將服務預配到 Azure，構建容器。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

6.  選擇以下值。

> \`\`azd provision\`\`

- **選擇要使用的 Azure 訂閱** ：選擇您的訂閱

- **選擇要使用的 Azure 位置** ： **East us2/west us2** （有時，East US
  可能不可用，請從下面提到的列表中選擇位置。

&nbsp;

- 選擇現有資源組：您現有的資源組（例如 :**ResourceGroup1 )**

![](./media/image14.png)

7.  等待資源完全預置。此過程需要 5-10 分鐘才能創建所有必需的資源。

> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)

### 任務 3：部署聊天應用程序並瀏覽它

8.  運行以下命令以部署應用程序。

\`\`azd deploy\`\`

![](./media/image16.png)

9.  等待部署 。此過程\< 5 分鐘。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

10. 單擊生成的端點 URL。

![](./media/image18.png)

11. 點擊 **Open**。

![](./media/image19.png)

12. 它會在新選項卡中打開應用程序。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

13. 選擇**How to search and book rental?** 容器，然後單擊文本框旁邊的
    enter 按鈕。

![](./media/image21.png)

### 任務 4 ：清理所有資源

1.  切換回 **Azure portal -\> Resource group- \> Resource group name.**

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  選擇所有資源，然後單擊 Delete （刪除），如下圖所示。（**DO NOT
    DELETE** 資源組）

![](./media/image23.png)

3.  在文本框中鍵入 '' **delete**'' ，然後單擊 **Delete**。

![](./media/image24.png)

4.  單擊 Delete 確認刪除。

![](./media/image25.png)

5.  切換回 Github 門戶選項卡並刷新頁面。

![A screenshot of a computer Description automatically
generated](./media/image26.png)

6.  單擊 Code ，選擇為此實驗室創建的分支，然後單擊 **Delete** 。

![](./media/image27.png)

7.  單擊 Delete **按鈕**確認分支刪除。

![](./media/image28.png)

### 總結：

此用例認為您，為在 Azure 上運行的檢索增強一代模式部署聊天應用程序，使用
Azure AI 搜索進行檢索，並使用 Azure OpenAI 和 LangChain 大型語言模型
（LLM） 來支持 ChatGPT 風格和 Q&A 體驗
