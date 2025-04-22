# ユース ケース 09 - Azure Cosmos DB for MongoDB と Azure OpenAI Service を使用したチャット ボット エクスペリエンスの構築

**目的：**

このユースケースでは、MongoDB ベクトル検索とドキュメント取得のための
vCore ベースの Azure Cosmos DB と Azure OpenAI
サービスを組み合わせて、チャットボット
エクスペリエンスを構築するインテリジェントなソリューションを作成します。

![A diagram of a software application AI-generated content may be
incorrect.](./media/image1.jpeg)

**使用した主なテクノロジー**-- Azure OpenAI Service, Azure Cosmos DB,
ChatGPT model

**推定時間**—60分

**ラボタイプ** -- インストラクター主導

**重要：PowerShellにコマンドが貼り付けられない場合は、メモ帳を開き、メモ帳の空白部分にカーソルを置いたまま、貼り付けたいコマンドのTボタンをクリックする。内容がメモ帳にコピーされ、メモ帳からPowerShellにコピー＆ペーストできるようになります。**

## 手順 0: VM と資格情報を理解

このタスクでは、ラボ全体で使用する資格情報を特定して理解します。

1.  **\[Instructions\]**
    タブには、ラボ全体に従うべき指示が記載されたラボ
    ガイドが含まれています。

2.  **\[リソース**\] タブには、ラボの実行に必要な資格情報があります。

    - **URL** – Azure portal の URL。

    - **Subscription** – アサインされてサブスクリプションの ID。

    - **Username** – Azure サービスにログインするために必要なユーザー
      ID。

    - **Password** – Azure
      ログインのパスワード。このユーザー名とパスワードを Azure
      ログイン資格情報と呼ぶことにします。これらのクレドは、Azure
      のログイン資格情報について言及するすべての場所で使用します。

    - **Resource Group** – アサインされた **Resource group**

\[!
アラート\]**重要:**このリソースグループの下にすべてのリソースを作成必要です。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  **\[ヘルプ**\] タブには、サポート情報が表示されます。ここでの **ID**
    値は、**ラボの実行中に使用される**ラボ インスタンス ID です。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

## 手順 1: Provision Azure resources

### タスク 1: スクリプトを使用して Azure リソースを作成する

1.  Azure portal (+++\*\* にログインし、\[Resources**\] タブから Azure
    ログイン資格情報を使用してログイン**する。

2.  Azure portal から、サブスクリプションを選択します。左側の画面で、
    \[Settings\] の \[Resource provider\] を選択し、
    \[+++Microsoft.Alertsmanagement+++\] を選択して、 \[Register\]
    をクリックします。

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  VMから**+++power shell+++**を検索し、**Windows
    PowerShell**を右クリックして\[**管理者として実行\]**を選択します。
    確認ダイアログで\[**はい**\]をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  次のコマンドを実行して、PowerShell に Az をインストールします。

+++**Install-Module Az**+++

プロンプトが表示されたら、\[A**\] (\[Yes to All\])** を選択します。

**注:** この処理には最大 5 分かかります。

![A computer screen with white text AI-generated content may be
incorrect.](./media/image7.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

5.  完了したら、以下のコマンドを実行して Az
    モジュールをインポートします。

+++**Import-Module Az**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

6.  以下のコマンドを実行して、ブラウザベースのサインインを使用します

+++Update-AzConfig -EnableLoginByWam $false+++

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image10.jpeg)

7.  以下のコマンドを実行し、プロンプトが表示されたらAzureログインを選択してAzureにログインします。

+++Connect-AzAccount+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

8.  次のコマンドを実行して、**LabFiles**フォルダに移動します。

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![A blue rectangular sign with white text AI-generated content may be
incorrect.](./media/image12.jpeg)

9.  以下のコマンドを実行して、**winget**を使用して**MicrosoftBicep**をインストールします。

+++winget install -e --id Microsoft.Bicep+++

プロンプトが表示されたら**、Y** を入力します。

![A computer screen with white text AI-generated content may be
incorrect.](./media/image13.jpeg)

10. **PowerShell を閉じ**てから **、もう一度**開きます。

11. 以下のコマンドを実行し、プロンプトが表示されたらAzureログインを選択してAzureにログインします。

+++Connect-AzAccount+++

12. 次のコマンドを実行して、**LabFiles**フォルダに移動します。

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![A blue rectangular sign with white text AI-generated content may be
incorrect.](./media/image12.jpeg)

13. 以下のコマンドを実行して、サブスクリプションIDを設定します。

+++Set-AzContext -SubscriptionId @lab.CloudSubscription.Id+++

![A computer screen shot of a blue screen AI-generated content may be
incorrect.](./media/image14.jpeg)

14. **C:\LabFiles\Build a Chat bot\Labs\deploy** にあるファイル
    **azuredeploy.bicep** を開き、35 行目の **dgxxxxxxx** を
    +++dg@lab.LabInstance.Id+++ に置き換えます。**74**
    行目のバージョン**を +++0125+++** に更新します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

![](./media/image16.png)

15. 次のコマンドを実行して、Azure Cosmos DB ワークスペース、Azure OpenAI
    などのリソースを Azure にデプロイします。

New-AzResourceGroupDeployment -ResourceGroupName
@lab.CloudResourceGroup(ResourceGroup1).Name -TemplateFile
.\azuredeploy.bicep -TemplateParameterFile .\azuredeploy.parameters.json
-c \`\`\`

\>\[!注\] \*\*注:\*\* デプロイには約 10 分から 15 分かかります。

デプロイメントに問題があり、失敗した場合は、ステップ 14
の名前を別の名前に更新して、もう一度やり直します。

\>\[!注\] \*\*注:\*\* プロンプトが表示されたら「Y」と入力します。

![A computer screen shot of a blue screen AI-generated content may be
incorrect.](./media/image17.jpeg)

![](./media/image18.jpeg)

\>\[!注\] \*\*注:\*\* 15 分から 20 分後に PowerShell
に更新がない場合は、Azure ポータルの \[Resource group -\> Deploy\]
で確認するか、\*\*PowerShell\*\* ウィンドウで \*\*Enter\*\*
キーを押します。

### タスク 2: Azureで作成したリソースを確認する

1.  Azure ログイン資格情報**を使用して**
    、+++[https://portal.azure.com/+++ で](https://portal.azure.com/+++)
    Azure portal **にログインします**。 \[**Resource Group\]**
    を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

2.  \[リソース グループ\] の一覧から、**アサインされたResource
    Groupを選択します**。

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image20.png)

3.  Azure OpenAI リソース**、**App Service**、**Azure Cosmos DB for
    MongoDB **などの一連のリソース** が作成されていることに注意する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

4.  Azure OpenAI **リソース**をクリックします。

![](./media/image22.png)

5.  \[**リソース管理\] で** \[**キーとエンドポイント\]** を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.jpeg)

6.  後で参照できるように、キー 1
    とエンドポイントをメモ帳にコピーして保存します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

7.  リソース グループ ページに戻り、**Azure Cosmos DB for Mongo DB**
    リソースを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

8.  \[**Settings**\] の下の \[**接続文字列**\] をクリックします。Self
    の値をコピーします (常にこのクラスターです。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.jpeg)

9.  接続文字列をコピーして、メモ帳に貼り付けます。
    コピーした接続文字列の\<**パスワード**\>を**+++myMongoDB98+++**に置き換えて、メモ帳に保存します。

## 手順 2: コードから Azure OpenAI モデルを探索して使用する

### タスク 1: 環境のセットアップ

1.  ラボ VM ウィンドウの検索バーで、+++Visual Studio code+++
    を検索し、**Visual Studio Code を開きます**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

2.  \[フォルダを開く\]**をクリックします**。(ポップアップしない場合は、
    **ファイル -\> フォルダ**を開く)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

3.  C:\Labfiles **に移動し、\[Build a Chat bot\]
    をクリックして、\[Select folder**\] **を選択します。**

![A screenshot of a chat bot AI-generated content may be
incorrect.](./media/image29.jpeg)

4.  ポップアップでは**Yes, I trust the Authorsをクリックする**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

5.  Visual Studio Code で、**Labs** フォルダー**から**
    \[**lab_0_explore_and_use_models.ipynb**\] を開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

6.  **Select Kernel**をクリックする。

7.  **Do you want to install the recommended extensions for
    PythonポップアップでInstallを選択する。**

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image32.jpeg)

8.  プロンプトが表示されたら**、\[Allow access\]**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.jpeg)

9.  **Select Kernel**をクリックする**。** \[Python
    Environmentsを選択して**\]を選択し**
    、\[**Suggested**\]または**\[Recommended**\]オプション**としてリストされる**Python
    3.12.3以降を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.jpeg)

10.  **.env**ファイルを開く

11. タスク 2 の手順 1で以前**notepad中に保存されたDB_CONNECTION
    STRING**, **AOAI_KEY** and **AOAI_Endpoint** を置き換える。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

これで、環境変数は、既に作成した Azure
リソースを指すように設定されました。

### タスク 2: コードを実行する

1.  ラボ 0 の ipynb **ファイル**に戻り**、\[Play\]
    ボタンをクリックして**最初のセル**を実行し**、最新の OpenAI
    クライアント ライブラリをインストールします。

![A black screen with a black background AI-generated content may be
incorrect.](./media/image37.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image38.jpeg)

2.  **次のセル**を実行して**Python-dotenvをインストールします**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image39.jpeg)

3.  Ctrl+Shift+P **を押し**、+++Reload Window+++
    と入力して、リストに表示される Developer:Reload Window
    オプションを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

4.  **最初のセルから**再度実行します。

5.  次のセルを**実行**して必要なOpenAIライブラリをインポートし、osを実行して環境変数にアクセスし、dotenvを実行して.envファイルから環境変数をロードします。

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image41.jpeg)

6.  **次のセル**を実行して、**Azure OpenAI チャット完了 API を呼び出す**
    Azure OpenAI クライアントを作成します。

![A computer screen with text AI-generated content may be
incorrect.](./media/image42.jpeg)

7.  次のセルを実行して、クライアント側で**.chat.completions.create()**メソッドを呼び出し、**チャット補完を実行**します。チャットレスポンスが返されるはずです。

![A computer screen with text on it AI-generated content may be
incorrect.](./media/image43.jpeg)

## 手順 3: 最初の Cosmos DB for MongoDB API アプリケーション

この手順では、最初の Cosmos DB
プロジェクトを作成する方法について説明します。ノートブックを使用して、基本的な
CRUD 操作を示します。

1.  Labs **フォルダ**から **lab_1_first_application.ipynb**
    ファイルを開きます。

2.  \[カーネルの選択**\]をクリックし**
    、**Pythonバージョンを選択します**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

3.  **最初のセル**を実行して**pymongoをインストールします**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

4.  次のセルを**実行**して、必要な**インポート**を実行します

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image46.jpeg)

5.  次のセルを実行して**データベースを作成します。**

\[!【ノート】**ノート:** これは、.env
ファイルで更新した接続文字列を使用します

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image47.jpeg)

6.  次のセルを実行してCollectionを作成します。

![A black screen with white text AI-generated content may be
incorrect.](./media/image48.jpeg)

7.  **次のセル**を**実行**して**ドキュメントを作成**します。ドキュメントを作成する方法の
    1 つは、insert_one メソッドを使用することです。このメソッドは、1
    つのドキュメントを取得し、それをデータベースに挿入します.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image49.jpeg)

8.  **次のセル**を実行して、データベースから **1
    つのドキュメントを取得します**。ここでは、**この目的のためにfind_one**方法を使用します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.jpeg)

9.  **find_one_and_update**メソッドを使用してデータベース内の 1
    つのドキュメントを更新する**次のセル**を**実行**します。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

10. **delete_one**メソッドを使用してデータベースから 1
    つのドキュメントを削除する**次のセル**を**実行**します。

![](./media/image52.jpeg)

11. **find**
    メソッドは、データベース内の複数のドキュメントをクエリするために使用されます。
    次の**3つのセルを** 1つずつ**実行**して、動作を確認します。

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image53.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image55.jpeg)

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image56.jpeg)

12. 次のセルは、
    この手順で作成したデータベースとコレクションを**削除**します。これは、
    **データベースオブジェクトで** **drop_database**
    メソッドを使用して行われます

![A computer screen with text AI-generated content may be
incorrect.](./media/image57.jpeg)

## 手順 4: MongoDB API を使用して Cosmos DB にデータを読み込む

前の手順では、コレクションにデータを個別に追加する方法を示しました。この手順では、一括操作を使用してデータを複数のコレクションにロードする方法を示します.
このデータは、後続のラボで、AI に関する MongoDB 用 Azure Cosmos DB API
の機能についてさらに説明するために使用されます。

このノートブックでは、MongoDB API を使用して、Cosmic Works JSON
ファイルからデータベースにデータを Cosmos DB
にロードする方法を示します。

1.  ラボフォルダから**lab_2_load_data.ipynbファイルを開く。**Click
    on **Select Kernelをclickし、Python version**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

2.  **requests**をインストールするために最初のセルを実行する

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

3.  次のセルを実行して、必要な**インポートを実行します**。

![A computer screen with green text AI-generated content may be
incorrect.](./media/image60.jpeg)

4.  **database**と**接続**を確立するための次のセルを**実行**する**。**

![A computer screen with text AI-generated content may be
incorrect.](./media/image61.jpeg)

![A computer screen shot of text AI-generated content may be
incorrect.](./media/image62.jpeg)

5.  **次のセル**を実行して**製品を**ロード**します**。

![A screen shot of a computer screen AI-generated content may be
incorrect.](./media/image63.jpeg)

6.  次のセルを**実行**して、**顧客データ**と**売上データ**を**読み込みます**。このリポジトリでは、顧客データと売上データは同じファイルに保存されています。typeフィールドは、2種類のドキュメントを区別するために使用されます。

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image64.jpeg)

![](./media/image65.jpeg)

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image66.jpeg)

7.  **clean up**するために次のセルを実行.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image67.jpeg)

## 手順 5: vCore ベースの Azure Cosmos DB for MongoDBを使用したベクトル検索

1.  **Labs**フォルダから**lab_3_mongodb_vector_search.ipynb**ファイルを開**く** 。

2.  **Select Kernelクリックし、Python version**を選択する。

![](./media/image68.jpeg)

3.  **tenacityをインストールするために最初のセルを実施する。**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image69.jpeg)

4.  **必要なImportsを実施するために次のセルを実施する**

![A computer screen with text AI-generated content may be
incorrect.](./media/image70.jpeg)

5.  次のセルを**実行**して**、** .env
    ファイルから**設定を読み込みます**。

![A computer screen with text AI-generated content may be
incorrect.](./media/image71.jpeg)

6.  次のセルを実行して、**データベース**への**接続**を確立**します**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image72.jpeg)

7.  **Azure OpenAI connectivity**を確立するために次のセルを実施する。

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image73.jpeg)

8.  各ドキュメントにベクトル埋め込みフィールドを作成するプロセスは、一度だけ実行する必要があります。ただし、ドキュメントが変更された場合は、ベクトル埋め込みフィールドを更新されたベクトルで更新する必要があります。これは、次の
    2 つのセルで行われます。 次の 2 つのセルを**実行**し、 2
    番目のセルで出力として得られた**埋め込みを**観察します.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image74.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image75.jpeg)

9.  次のセルを実施して**Vectorize and update all documents in the Cosmic
    Works databaseを行う。**

![A computer screen shot of a program code AI-generated content may be
incorrect.](./media/image76.jpeg)

10. **products, customer and sales documentsにvector
    fields** を**追加するよう次のセルを3つを実施する。**

注: 最初のセルは約 5 分、2 番目のセルは約 3 分、3 番目のセルは約 20
分をかかり実行をで完了します。

\![\](./media/image77.jpeg)

11. 次のセルを**実行**して**products vector indexを作成します。**

![A screen shot of a computer screen AI-generated content may be
incorrect.](./media/image77.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image78.jpeg)

12. 各ドキュメントに関連付けられたベクター埋め込みと、各コレクションにベクターインデックスが作成されたので、vCore
    ベースの Azure Cosmos DB for MongoDB
    のベクター検索機能を使用できるようになりました。次の **3
    つのセルを実行**する。

![](./media/image79.jpeg)

![](./media/image80.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image81.jpeg)

13. 次のセルを**実行**して、Chat
    GPT-3.5を使用した**RAGパターンでのベクトル検索結果**の使用を観察します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image82.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image83.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image84.jpeg)

14. 次のセルの出力を観察します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image85.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image86.jpeg)

## 手順 6: デプロイされたリソースの削除

1.  Azure
    portal(+++[https://portal.azure.com+++](https://portal.azure.com+++/))から、アサインされたresource
    groupを選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image87.png)

2.  その下にあるすべてのリソースを選択し、メニューの**3つのドットをクリックして**\[削除**\]を選択し**
    、すべてのリソースを削除します.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image88.png)

3.  テキストボックスで +++delete+++を入力してDeleteボタンうをclickする
    。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image89.png)

4.  リソースが削除されたら、Azure portal のホーム ページで **+++Azure AI
    Services+++** を検索して選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.jpeg)

5.  左側の画面から **\[Azure OpenAI**\] を選択し、 \[**Manage deleted
    resources\]** を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image91.jpeg)

6.  そこにリストされるリソースを選択し、\[**パージ\]
    をクリックします**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image92.jpeg)

7.  **Yesをクリックする。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image93.jpeg)

**要約**

Azure Cosmos DB for MongoDBベクター検索とAzure OpenAI
でのドキュメント取得でソリューションを正常に作成しました。
