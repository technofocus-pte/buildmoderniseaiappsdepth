# ユース ケース 11 - Azure OpenAI、Azure Cosmos DB for NoSQL を使用したCopilotの構築

このユース ケースでは、.NET ソフトウェア開発キットを使用して、Blazor Web
アプリケーションを Azure Cosmos DB for NoSQL と Azure OpenAI
に接続します。コードは、API for NoSQL
コンテナ内の項目を管理およびクエリします。また、コードでは Azure OpenAI
にプロンプトが送信され、応答が解析されます。

**ラボの期間:** 45分

**ラボタイプ**: インストラクター主導

**目的**

- Blazor、PostgreSQL、OpenAI の開発環境を設定する。

&nbsp;

- Blazor プロジェクトを作成し、対応的なチャット
  インターフェイスを設計　　する。

&nbsp;

- Azure で PostgreSQL データベースを構成し、Blazor アプリに接続する。

&nbsp;

- Azure OpenAI を統合してチャット機能を強化する。

&nbsp;

- Blazor アプリケーションと PostgreSQL データベースを Azure 上展開する。

- コンポーネント間のシームレスな相互作用を確実するためにアプリケーションをテストする.

- Azure
  上デプロイされたアプリケーションを監視およびトラブルシューティングする。

**使用した主なテクノロジー:** Azure Cosmos DB for NoSQL, Azure OpenAI

## 手順0: VM と資格情報を理解する

このタスクでは、ラボ全体で使用する資格情報を特定して理解します.

1.  **「Instructions」タブには、ラボ全体を通して従うべき支持が記載されたラボ
    ガイドが含まれています。**.

&nbsp;

2.  **\[Resources**\] タブには、ラボの実行に必要な資格情報があります。

    - **URL** – AzureポータルへのURL

    - **Subscription** – これはアサインされたサブスクリプションのIDです

    - **Username** –Azureサービスにログインするための必要のユーザID

    - **Password** –
      これはAzureログインのパスワードです。このユーザー名とパスワードを
      Azure ログイン資格情報と呼ばれます。Azure
      ログイン資格情報を記載する時には常に、これらの資格情報を使用します。

    - **Resource Group** – アサインされた**Resource groupです。**

\[!注意\] **重要: すべてのリソースをこのResource
Group下に作成するよう確実する。**![A screenshot of a computer
AI-generated content may be incorrect.](./media/image1.png)

3.  **\[Help\]** タブには、サポート情報が含まれています。ここでの ID
    値は、ラボの実行中に使用される**Lab Instance ID** です。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## 手順 1: インフラストラクチャーを展開して初期設定を完了する

このプロジェクトを完了するには、Azure Cosmos DB for NoSQL アカウントと
Azure OpenAI
アカウントが必要です。このプロセスを効率化するには、これらの両方のアカウントを使用して
Bicep テンプレートを Azure にデプロイする。

### タスク 1: テンプレートからインフラストラクチャを展開する

1.  **C:\Labfiles\Build and Test a custom chat application Using Azure
    Cosmos DB and AzureOpenAI**パスからファイルを開き96行目のAzure
    OpenAIバージョンを+++0125+++に更新します。ファイルを保存します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

2.  新しいブラウザを開き、アドレスバーに次の URL
    を入力する：Azureぽーたすを開くために+++<https://portal.azure.com/+++> 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  Azure ポータルで、ページ上部の検索ボックスの右側にある \[\>\_\]
    **(Cloud Shell)** ボタンをクリックします。ポータルの下部に Cloud
    Shell 画面が開きます。初めて Cloud Shell
    を開くと、使用するシェルの種類 (**Bash または PowerShell**)
    を選択するように要求する場合があります。**Bash**
    を選択します。このオプションが表示されない場合は、この手順をスキップしてください。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  In the **Getting StartedダイアログでMount storage
    accountを選択し、サブスクリプションしてからApplyをクリックします**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

5.  **Mount storage account**ダイアログで **we will create a storage
    account for youを選択して、Next**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

![A close-up of a computer screen AI-generated content may be
incorrect.](./media/image9.jpeg)

6.  \[Cloud Shell\] 画面の左上に表示されているシェルの種類は \[Bash\]
    に切り替えていることを確認する。**PowerShell**
    の場合は、ドロップダウン メニューを使用して **Bash**
    に切り替えます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.jpeg)

7.  ターミナルが起動したら、**\[ファイルの管理\] -\>\[アップロード\]
    \>**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

8.  パス**C:\Labfiles\Build and Test a custom chat application Using
    Azure Cosmos DB and AzureOpenAI** からファイル**azuredeploy.JSON** 
    を選択し**Openを選択します**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

ファイルのアップロードが成功したことのメッセージが表示されます。

![A white background with black text AI-generated content may be
incorrect.](./media/image13.jpeg)

9.  作成 するAzure リソース グループの名前 (mslearn-cosmos-openai)
    を使用して、**resourceGroupName**
    という名前の新しいシェル変数を作成する

+++resourceGroupName="ResourceGroup1"+++(\[リソース\] タブからリソース
グループ名を取得する)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

10. az group deploy create を使用して、**azuredeploy.json** テンプレート
    ファイルをリソース
    グループに展開する。次に、下記のコマンドを実行する。

+++az deployment group create --resource-group $resourceGroupName --name
zero-touch-deployment --template-file azuredeploy.json+++

**注:** この展開には約 5 分から 10 分かかる場合があります。

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image15.jpeg)

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image16.jpeg)

### タスク2: Get Azure Cosmos DB for NoSQL 及びAzure OpenAI accountの　資格情報を入手

上記のデプロイでは、Azure Cosmos DB for NoSQL と Azure OpenAI
アカウントがデプロイされ、それらの資格情報が Azure App Service Web
アプリの構成に保存されました。これで、Azure Portal または Azure CLI
を使用して各サービスの資格情報を取得すること選択できます。

1.  AzureポータルのHPから**Resource groupsをクリックする。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

2.  Resource Groupを選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

3.  **Resource
    Group**ページで、**Essentials**パネルを拡張し、**Deployment**ヘッダーを確認する。この時点で、Deploymentのステータスは**Succeeded**になっているはずです。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

4.  次に、**Azure Cosmos DB**アカウントを選択しResource
    Groupページに移動する。![A screenshot of a computer AI-generated
    content may be incorrect.](./media/image20.png)

5.  Resourceナビゲーションメニュの**Settings**セクションで**Keys**オプションを選択する。**URIとPRIMARY
    KEYフィールドの値をレコードする。これらの値を以降に使用することになります。**![A
    screenshot of a computer AI-generated content may be
    incorrect.](./media/image21.jpeg)

6.  **Resource Groupsページへ**に戻る。**Azure
    OpenAI**アカウントを選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

7.  **Azure Open AI** 画面で**Resource
    Managementセクションへ移動して** section, **Keys and
    Endpoints**にクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.jpeg)

8.  **Keys and
    Endpointsページで** **KEY1、**(KEY1又はKEY2のどちらも選択)と**Endpoint**をコーピしてノートパッドで**Saveし、次のタスクにこの情報を使用になります。**![A
    screenshot of a computer AI-generated content may be
    incorrect.](./media/image24.jpeg)

### タスク3: Dockerを実行する

1\. Windows の検索ボックスに「+++Docker+++」と入力し、**Docker Desktop**
をクリックします。![A screenshot of a desktop AI-generated content may
be incorrect.](./media/image25.jpeg)

2.  Docker Desktopを実行する

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.jpeg)

## Exercise 2 - スターターアプリケーションの設定と構成する

1.  From the VM検索バーから+++Visual Studio+++ を検索してから**Visual
    Studio Code**を選択する。

2.  **File** -\> **Open Folderをクリックする**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

3.  **C:\LabFilesから** **cosmosdb-chatgpt**  を選択し、**Select
    Folderをクリックする**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

4.  **Do you trust the authors dialog中でYes, I trust the
    authors** オプションをクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.jpeg)

5.  In the **Visual Studio CodeエディターでTerminal**をクリックし、**New
    Terminalを開く。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

6.  6\. .NET
    アプリケーションでは、構成プロバイダーを使用してアプリケーションに新しい設定を挿入するのが一般的です。このアプリケーションでは、appsettings.Development.json
    ファイルを使用して、Azure OpenAI
    エンドポイントとキーの最新の値を提供します。

7.  **appsettings.Development.JSONファイルを開く。**ファイル内の **Azure
    Cosmos DB** および **Azure OpenAI** リソースの uri
    とキー値のプレースホルダーを、先ほど保存した値に置き換えます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

**8. 以下のコマンドを実行して .NET プロジェクトを構築する。**

**+++dotnet build+++**![A screen shot of a computer AI-generated content
may be incorrect.](./media/image32.jpeg)

## Exercise 3: コードを理解

### タスク 1: 必要なメンバーとクライアントインスタンスを追加する

1.  **Services/OpenAiService.csファイルを開く。**本ファイルがAzure
    OpenAIクライアントを使用するために必要となるクラス変数を実地します。これはいくつかの静的プロンプトを実装し、OpenAIClient
    クラスの新しいインスタンスを作成します。

2.  このコード ブロックは、 \_systemPromptText
    という名前の新しい文字列変数と一緒、静的テキスト
    ブロックを作成して各プロンプトの前に AI アシスタントに送信します。

> private readonly string \_systemPrompt = @"
>
> あなたは人々が情報を見つけるのを手助けする AI アシスタントです。
>
> 丁寧でプロフェッショナルな簡潔な回答を提供してください。" +
> Environment.NewLine;

3.  このコード ブロックは、静的テキスト ブロックを含む \_summarizePrompt
    という名前の別の新しい文字列変数を作成して、会話を要約する方法を含む支持をAIアシスタントに送付します。

> private readonly string \_summarizePrompt = @"
>
> このプロンプトを 1 つか 2 つの単語で要約し、Web
> ページのボタンのラベルとして使用します。
>
> 句読点は使用することしません。" + Environment.NewLine;

4.  このコードブロックは、OpenAIClient
    クラスの新しいインスタンスを作成するために、エンドポイントを使用して
    Uri を構築し、キーを使用して AzureKeyCredential を構築します。

> Uri uri = new(endpoint);
>
> AzureKeyCredential credential = new(key);
>
> \_client = new(
>
> endpoint: uri,
>
> keyCredential: credential
>
> );

**タスク 2: AI モデルに質問する**

> まず、システム プロンプト、質問、セッション ID
> を送信して、質問回答の会話を実装し、AI
> モデルが現在の会話のコンテキストで回答できるようにします。プロンプトを解析して応答
> (このコンテキストでは完了)
> を返すために必要なトークン数を必ず測定しる。

1.  このブロックコードは、タイプのオプションと言う変数を新しく作成します。ChatCompletionsOptions.
    2 つのメッセージ変数をメッセージ リストに追加し、User の値を
    sessionId コンストラクター パラメーターに設定します。

> ChatCompletionsOptions options = new()
>
> {
>
> DeploymentName = "chatmodel",
>
> Messages = {
>
> new ChatRequestSystemMessage(\_systemPrompt),
>
> new ChatRequestUserMessage(userPrompt)
>
> },
>
> User = sessionId,
>
> MaxTokens = 4000,
>
> Temperature = 0.3f,
>
> NucleusSamplingFactor = 0.5f,
>
> FrequencyPenalty = 0,
>
> PresencePenalty = 0
>
> };

2.  Azure OpenAI クライアント変数 (\_client) の GetChatCompletionsAsync
    メソッドが非同期的に呼び出されます。結果は ChatCompletions 型の
    completions という名前の変数に格納されます。

> Response\<ChatCompletions\> completionsResponse =
> await_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;

3.  最後に、以下のコード ブロックは、GetChatCompletionAsync
    メソッドの結果としてタプルを返し、完了の内容を文字列として、プロンプトに関連付けられたトークンの数、および応答のトークンの数を含むます。あ

> return (
>
> completionText: completions.Choices\[0\].Message.Content,
>
> completionTokens: completions.Usage.CompletionTokens
>
> );

**タスク 3: AIモデルに会話を要約するよう依頼する**

1.  次に、AI モデルに別のシステム プロンプト、現在の会話、セッション ID
    を送信して、AI モデルが会話を数語で要約できるようにします。

2.  次のコードでは、messages リスト内の 2 つのメッセージ変数 (User:
    sessionId コンストラクター パラメーターに設定、MaxTokens を 200
    に設定)、残りのプロパティを使用して、options という名前の
    ChatCompletionsOptions 変数を作成します.

> ChatCompletionsOptions options = new()
>
> {
>
> DeploymentName = "chatmodel",
>
> Messages = {
>
> new ChatRequestSystemMessage(\_systemPrompt),
>
> new ChatRequestUserMessage(conversationText)
>
> },
>
> User = sessionId,
>
> MaxTokens = 200,
>
> Temperature = 0.0f,
>
> NucleusSamplingFactor = 1.0f,
>
> FrequencyPenalty = 0,
>
> PresencePenalty = 0
>
> };

3.  3\. 以下のコードは、モデル名 (\_modelName)
    とオプション変数をパラメーターとして
    \_client.GetChatCompletionsAsync を非同期的に呼び出し、その結果を
    ChatCompletions 型の completions
    という名前の変数に格納します。SummarizeAsync
    メソッドの結果として、補完の内容を文字列として返します。

> Response\<ChatCompletions\> completionsResponse = await
> \_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;
>
> string completionText = completions.Choices\[0\].Message.Content;
>
> return completionText;

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image33.jpeg)

**タスク 4 - Azure Cosmos DB for NoSQLに接続する**

CosmosDbService クラスには、このモジュールで以前に作業した OpenAiService
クラスに似たサービスのスタブ実装が含まれています。対照的に、このクラスは
Azure Cosmos DB 用の .NET SDK を使用しますが、動作が少し異なります。

1.  このセクションでは、クライアントを使用して Azure Cosmos DB for NoSQL
    にアクセスするために必要なクラス変数とクライアントの実装について説明します。

2.  **Services/CosmosDbService.csファイルを開く**。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image34.jpeg)

3.  以下のコードは、CosmosSerializationOptions 型の options
    という名前の変数を作成し、変数の PropertyNamingPolicy プロパティを
    CosmosPropertyNamingPolicy.CamelCase に設定します。

> CosmosSerializationOptions options = new()
>
> {
>
> PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
>
> };

**注: このプロパティを設定すると、.NET
クラス内の対応するプロパティの大文字と小文字に関係なく、SDK
によって生成された JSON がキャメル
ケースでシリアル化および逆シリアル化されるようになります。**

4.  以下のコードは、CosmosClientBuilderクラス、エンドポイント、キー、およびシリアル化オプションを使用して、clientという名前のCosmosClient型の新しいインスタンスを作成します。

> CosmosClient client = new CosmosClientBuilder(endpoint, key)
>
> .WithSerializerOptions(options)
>
> .Build();

5.  以下のコードは、クライアント変数の GetDatabase
    メソッドを呼び出して、database という名前の Database 型の新しい null
    変数を作成します。

**Database? database = client?.GetDatabase(databaseName);**

6.  以下のコードは、コンストラクターのコンテナ変数が null
    でない場合にのみ、その変数をクラスの \_container
    変数に割り当てます。null の場合は、ArgumentException
    をスローします。

> \_container = container ??
>
> throw new ArgumentException("Unable to connect to existing Azure
> Cosmos DB container or database.");

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.jpeg)

**タスク 5 - Azure Cosmos DB for NoSQL サービスを実装する**

Azure Cosmos DB サービス (CosmosDbService) は、AI アシスタント
アプリケーションでのセッションとメッセージのクエリ、作成、削除、更新を管理します。これらの操作をすべて管理するには、サービスで
.NET SDK
のさまざまな機能を使用して、潜在的な操作ごとに複数のメソッドを実装する必要があります。

この手順では、取り組むべき重要な要件が複数あります。

- • セッションまたはメッセージを作成する操作を実装する

- • 複数のセッションまたはメッセージを取得するためのクエリを実装する

- •
  単一のセッションを更新する操作、または複数のメッセージを一括更新する操作を実装する

- •
  複数の関連するセッションとメッセージをクエリして削除する操作を実装する

Azure Cosmos DB for NoSQL は、JSON 形式でデータを保存するため、1
つのコンテナーにさまざまな種類のデータを保存できます。このアプリケーションは、AI
アシスタントとのチャット「セッション」と、各セッション内の個々の「メッセージ」の両方を保存します。NoSQL
用 API
を使用すると、アプリケーションは両方の種類のデータを同じコンテナーに保存し、単純な型フィールドを使用してこれらの型を区別できます。

1.  **Services/CosmosDbService.csファイルを開く**。

2.  以下のコードは、現在のセッションの SessionId
    プロパティをパラメーターとして使用して、PartitionKey
    型のpartitionKey という名前の新しい変数を作成します。

**PartitionKey partitionKey = new(session.SessionId);**

3.  以下のコードは、セッション
    パラメーターとパーティションキー変数を渡してコンテナーの
    CreateItemAsync メソッドを呼び出します。InsertSessionAsync
    メソッドの結果として応答を返します。

> return await \_container.CreateItemAsync\<Session\>(
>
> item: session,
>
> partitionKey: partitionKey
>
> );

4.  以下のコードは、パーティション キーの値として session.SessionId
    を使用して PartitionKey 変数を作成します。Timestamp
    プロパティが現在の UTC タイムスタンプに更新された、newMessage
    という名前の新しいメッセージ変数を作成します。新しいメッセージとパーティション
    キー変数の両方を渡して CreateItemAsync
    を呼び出します。InsertMessageAsync の結果として応答を返します。

> PartitionKey partitionKey = new(message.SessionId);
>
> Message newMessage = message with { TimeStamp = DateTime.UtcNow };
>
> return await \_container.CreateItemAsync\<Message\>(
>
> item: newMessage,
>
> partitionKey: partitionKey
>
> );

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image36.jpeg)

**タスク 6: 複数のセッションまたはメッセージを取得**

アプリケーションがコンテナーから複数のアイテムを取得する必要がある主なユースケースは
2 つあります。まず、アプリケーションは、type = Session
のアイテムにフィルター処理して、現在のユーザーのすべてのセッションを取得します。次に、アプリケーションは、type
= Session & sessionId =
の同様のフィルターを実行して、セッションのすべてのメッセージを取得します。ここでは、両方のクエリが
.NET SDK とフィード反復子を使用して実装されています。

1.  以下のコードは、QueryDefinition 型の query
    という名前の新しい変数を作成します。このコードは、Fluent
    WithParameter メソッドを使用して、Session
    クラスの名前をパラメーターの値として割り当てます。次に、汎用型の
    Session と query 変数をパラメーターとして渡して、\_container
    変数で汎用 GetItemQueryIterator\<\>
    メソッドを呼び出します。結果を、FeedIterator 型の response
    という名前の変数に格納します。

> QueryDefinition query = new QueryDefinition("SELECT DISTINCT \* FROM c
> WHERE c.type = @type")
>
> .WithParameter("@type", nameof(Session));
>
> FeedIterator\<Session\> response =
> \_container.GetItemQueryIterator\<Session\>(query);

2.  while ループ内の以下のコードは、応答変数で ReadNextAsync
    を呼び出して次のページの結果を取得し、その結果を output
    という名前のリスト変数に追加します。while
    ループの外では、GetSessionsAsync
    メソッドの結果として、セッションのリストが output 変数に返されます。

> FeedResponse\<Session\> results = await response.ReadNextAsync();
>
> output.AddRange(results);
>
> return output;

3.  以下のコードでは、Fluent
    WithParameterメソッドを使用して、パラメータとして渡されたセッション識別子に@sessionIdパラメータを割り当て、Messageクラスの名前に@typeパラメータを割り当てます。

> QueryDefinition query = new QueryDefinition("SELECT \* FROM c WHERE
> c.sessionId = @sessionId AND c.type = @type")
>
> .WithParameter("@sessionId", sessionId)
>
> .WithParameter("@type", nameof(Message));

4.  クエリ変数とGetItemQueryIterator\<\>メソッドを使用してFeedIterator\<
    Message \>を作成します。

FeedIterator response = \_container.GetItemQueryIterator(query);

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image37.jpeg)

## Exercise 4: アプリを実施する

これで、アプリケーションに Azure OpenAI と Azure Cosmos DB
が完全に実装されました。ソリューションをデバッグすることで、アプリケーションをエンドツーエンドでテストできます。

1.  From the **Visual Studio Code
    Terminal**から下記コマンドを使用することによりプロジェクトを構築します。

+++**dotnet build**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.jpeg)

2.  dotnet watch を使用して有効されたホット
    リロードでアプリケーションを起動します。

+++**dotnet watch run --non-interactive**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

3.  Visual Studio Code は、Web
    アプリケーションが実行されている状態でツール内のシンプルなブラウザーを起動します。Web
    アプリケーションで、**\[+ Create New Chat**\]
    をクリックして新しいチャット セッションを作成し、AI
    アシスタントに質問する。次に、実行中の Web
    アプリケーションを閉じる。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image40.jpeg)

4.  テキストボックスに次のテキストを貼り付けて、**送信**アイコンをクリックする。

+++How many wins does it take to promote to the Premier League?+++

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image41.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

5\.
テキストボックスに次のテキストを貼り付けて、送信アイコンをクリックする。+++What
is Azure OpenAI?+++

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image43.jpeg)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image44.jpeg)

5.  ターミナルを閉じる。

## Exercise 5: Resource Groupを削除する

1.  新しいブラウザを開き、アドレスバーに次のURLを入力する: Azure Portal
    Azure Portal を開くために+++<https://portal.azure.com/+++> 

2.  From the Resource groupページから**アサインされたResource
    group**を選択。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  すべての**リソース**を選択してからDeleteを選択する。![A screenshot
    of a computer AI-generated content may be
    incorrect.](./media/image46.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

4.  テキストボックス中に+++**delete**+++
    を入力して**Delete**をクリックする。![A screenshot of a computer
    AI-generated content may be incorrect.](./media/image48.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image49.png)

5.  Deleteされたリソースに関する成功通知によっての削除が確認されます。

6.  リソースが削除されたら、Azure ポータルのホーム ページから **Azure AI
    サービス**を検索して選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.jpeg)

7.  左側の画面から **Azure OpenAI**
    を選択し、**削除されたリソースの管理**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

8.  そこにリストされているリソースを選択し、「**クリア**」をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.jpeg)

9.  **Yesをクリックする**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

**概要**

**このラボでは、Blazor、PostgreSQL、Azure OpenAI を使用してカスタム
チャット
アプリケーションを構築、展開、テストするための包括的なガイドを提供しました。このラボでは、必要な開発環境の設定、Blazor
ベースのチャット インターフェイスの作成と設計、Azure での PostgreSQL
データベースの構成と接続、Azure OpenAI
の統合による機能強化、そして最後に Azure
でのアプリケーションの展開とテストすることについて学びました。この実践的な経験により、最先端のテクノロジとクラウド
サービスを使用して最新の Web
アプリケーションを開発および管理するスキルを身に付けることができます。** 
