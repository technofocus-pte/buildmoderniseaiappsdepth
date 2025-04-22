# ユース ケース 12 - ジェネレーティブ AI 機能を Azure Database for PostgreSQL Flexible Callと統合して、特定の AI リストのレビューを評価する

**ラボ期間 --** 40分間

**ラボタイプ --** インストラクター主導

**紹介**

このラボでは、Azure AI サービスを PostgreSQL と統合して、高度な AI
機能でデータベースを強化する方法を習います。Azure OpenAI と PostgreSQL
拡張機能 (pgvector や PostGIS など)
を活用することで、高度なテキスト分析、ベクトル類似性検索、地理空間クエリをデータベース内で直接行うことができます。このラボでは、必要な
Azure リソースの提供、データベースの構成、AI
主導の分析情報と地理空間データを組み合わせた複雑なクエリの実行について説明します。

**目標**

- Azure Database for PostgreSQL フレキシブル
  サーバーをプロビジョニングして構成します。

- Azure OpenAI
  サービスを使用してベクター埋め込みを作成および管理します。

- 意味的に類似したテキストデータを見つけるためにベクトル類似性検索を実行する。

- PostGIS拡張機能を地理空間データ分析に利用する。

- Azure AI Language
  サービスを統合して、感情分析やその他の認知機能を実現する。

- インデックス作成ツールとクエリ計画ツールを使用して、クエリのパフォーマンスを最適化および分析します。

**重要:CloudShellに貼り付**け**られないコマンドがある場合は**、メモ帳を開き、カーソルをメモ帳の空きスペースに置いてから、貼り付けるコマンドのTボタンをクリックしてください。内容がメモ帳にコピーされ、メモ帳からCloudShellにコピーして貼り付けることができます。

## 手順 0: VM と資格情報の理解

このタスクでは、ラボ全体で使用する資格情報を特定して理解します。

1.  **\[指示\]** タブには、ラボ全体に従うべき指示が記載されたラボ
    ガイドがあります。

&nbsp;

2.  **\[リソース**\] タブには、ラボの実行に必要な資格情報があります。

    - **URL** – Azure portal の URL

    - **サブスクリプション** –
      これは、お客様に割り当てられたサブスクリプションの ID です

    - **ユーザー名** – Azure サービスにログインするために必要なユーザー
      ID。

    - **パスワード** – Azure
      ログインのパスワード。このユーザー名とパスワードを Azure
      ログイン資格情報と呼ぶことにします。これらの資格情報は、Azure
      のログイン資格情報について記載するすべての場合に使用します。

    - **リソースグループ** – **自分に割り当てられた**リソースグループ。

**\[!アラート\]** 重要: すべてのリソースをこのリソース
グループの下に作成するよう確認する。

![](./media/image1.png)

3.  \[ヘルプ\] タブには、サポート情報が表示されます。ここでの **ID**
    値は、 **ラボの実行中に使用される**ラボ インスタンス **ID** です。

![](./media/image2.png)

## 手順 1: Azure Database for PostgreSQL Flexible Server提供する

### タスク 0: リソース プロバイダーの登録

1.  1\. Azure ログイン資格情報を使用して**Azure
    ポータル**‐+++https://portal.azure.com+++にログインする.

2.  \[**サブスクリプション\] をクリックし**、左側の画面の **\[設定**\]
    で **\[リソース プロバイダー**\] を選択します .

3.  **+++Microsoft.DBforPostgreSQL+++**
    を検索し、**登録**をクリックしてこのリソース
    プロバイダーを登録する。

![](./media/image3.png)

### タスク 1: PostgreSQL Flexible ServerためにAzure Databaseを提供

1.  Webブラウザを開き、+++[https://portal.azure.com+++に移動します](https://portal.azure.com+++/)

2.  Azure portal **ツール バーで Cloud Shell
    アイコンを選択して、ブラウザー ウィンドウの上部に新しい Cloud Shell
    ウィンドウを開きます。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  Cloud Shell を初めて開くときに、使用するシェルの種類 (**Bash**
    または **PowerShell**)
    を選択するように求められる場合があります。\[**Bash\]**
    を選択します。

![](./media/image5.jpeg)

4.  \[**Getting started**\] ダイアログ ボックスで、 \[**Mount storage
    account**\] を選択し、Azure
    サブスクリプションを選択します。「**Apply」**ボタンをクリックします。

![](./media/image6.png)

5.  **Mount storage account**ダイアログボックスで**we will create a
    storage account for youを選択してNextボタンをクリックする。**

![](./media/image7.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

6.  クラウド・シェル・プロンプトで、次のコマンドを実行して、リソースを作成するための変数を定義します。変数は、リソース
    グループとデータベースに割り当てる名前を表し、リソースをデプロイする
    Azure 領域を指定します。

&nbsp;

7.  以下のコマンドのリソースグループ名を、割り当てたリソースグループに置き換えて、コマンドを実行する。

+++RG_NAME= \< Resource group Name \>+++

![](./media/image9.png)

8.  データベース名で、{SUFFIX} トークンをイニシャルなどの**ラボ
    インスタンス ID** に置き換えて、データベース
    サーバー名がグローバルに一意になるようにします。

+++DATABASE_NAME=<pgsql-flex-@lab.LabInstance.Id>+++

![](./media/image10.jpeg)

9.  以下のコマンドを実行して、Region値を設定する。

+++REGION=@lab.CloudResourceGroup(ResourceGroup1).Location+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

10. 次の Azure CLI コマンドを実行して、割り当てられたリソース
    グループ内で Azure Database for PostgreSQL データベース
    インスタンスを提供します (このコマンドの完了には 10
    分かかります)\`\`\`

> az postgres flexible-server create --name $DATABASE_NAME --location
> $REGION --resource-group $RG_NAME \\
>
> --admin-user s2admin --admin-password Seattle123Seattle123
> --database-name airbnb \\
>
> --public-access 0.0.0.0-255.255.255.255 --version 16 \\
>
> --sku-name Standard_D2s_v3 --storage-size 32 --yes
>
> \`\`\`

![](./media/image12.jpeg)

### タスク 2: Azure Cloud Shell で psql を使用してデータベースに接続する

このタスクでは、Azure Cloud Shell から psql コマンドライン
ユーティリティを使用してデータベースに接続します。

1.  ブラウザーを開き、+++[https://portal.azure.com++++](https://portal.azure.com+++/)
    に移動して、Azure サブスクリプション アカウントでサインインします。

2.  **ホームページで**、「**リソース・グループ」をクリックする**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.jpeg)

3.  **割り当てられたリソースグループ名**をクリックする。

![](./media/image14.png)

4.  リソース グループで、**PostgreSQL フレキシブル サーバー**
    リソースを選択する。

![](./media/image15.png)

5.  左側のナビゲーション メニューで、**Settings** の下の **Connect**
    を選択する。

![](./media/image16.jpeg)

6.  Azure portal の**データベースの \[Connect\]**
    ページで、\[**データベース名**\] に **\[airbnb**\]
    を選択し、\[**Connection details**\]
    ブロックをコピーしてメモ帳に貼り付け、今後のタスクで情報を使用されます。

![](./media/image17.jpeg)

7.  Azure Database for PostgresSQL のホーム
    ページで、左側のナビゲーション メニューの \[**Overview**\]
    をクリックし、サーバー名をコピーしてメモ帳に貼り付け、
    メモ帳をSaveして、次のラボに情報を使用します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.jpeg)

8.  Azure Database for
    PostgreSQLのホームページでSettings　下の**Networking**を選択して、**Allow
    public access from any Azure service within Azure to this
    serverを選択する。Save**ボタンをクリックする。

![](./media/image19.jpeg)

![](./media/image20.jpeg)

9.  Azure portal のツール バーで **Cloud Shell**
    アイコンを選択すると、ブラウザー ウィンドウの上部に新しい Cloud
    Shell ウィンドウが開きます。

&nbsp;

10. **Connection Detailsを** Cloud Shell に貼り付けます。

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image21.jpeg)

11. Cloud Shell プロンプトで、**{your_password}
    トークンを**データベースの作成時に **s2admin
    ユーザーに割り当てたパスワードに**置き換え、パスワードは
    **+++Seattle123Seattle123+++** にする必要があります。

![](./media/image22.jpeg)

12. psql
    コマンドラインユーティリティを使用してデータベースに接続するには、プロンプトで次のように入力します。

+++psql+++

![](./media/image23.jpeg)

Cloud Shell からデータベースに接続するには、データベースの
**\[ネットワーキング\]** ページで \[Azure 内の任意の Azure
サービスからサーバーへのパブリック アクセスを許可する\] チェック
ボックスがオンになっている必要があります
。接続できないというメッセージが表示された場合は、これがオンになっていることを確認して、もう一度お試しください。

### タスク 3: データベースへのデータの追加

psql
コマンド・プロンプトを使用して、テーブルを作成し、ラボで使用するデータをテーブルに入力します。

1.  次のコマンドを実行して、パブリック BLOB ストレージ アカウントから
    JSON 　データをインポートするための一時テーブルを作成します。

> CREATE TABLE temp_calendar (data jsonb);
>
> CREATE TABLE temp_listings (data jsonb);
>
> CREATE TABLE temp_reviews (data jsonb);

![](./media/image24.jpeg)

2.  COPY
    コマンドを使用して、各一時テーブルにパブリックストレージアカウントの
    JSON ファイルのデータを移入します。

+++\COPY temp_calendar (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/calendar.json'+++>

+++\COPY temp_listings (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/listings.json'+++>

+++\COPY temp_reviews (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/reviews.json'+++>

![](./media/image25.jpeg)

![](./media/image26.jpeg)

3.  次のコマンドを実行して、このラボで使用する形状にデータを格納するためのテーブルを作成します:

> CREATE TABLE listings (
>
> listing_id int,
>
> name varchar(50),
>
> street varchar(50),
>
> city varchar(50),
>
> state varchar(50),
>
> country varchar(50),
>
> zipcode varchar(50),
>
> bathrooms int,
>
> bedrooms int,
>
> latitude decimal(10,5),
>
> longitude decimal(10,5),
>
> summary varchar(2000),
>
> description varchar(2000),
>
> host_id varchar(2000),
>
> host_url varchar(2000),
>
> listing_url varchar(2000),
>
> room_type varchar(2000),
>
> amenities jsonb,
>
> host_verifications jsonb,
>
> data jsonb
>
> );

![](./media/image27.jpeg)

> CREATE TABLE reviews (
>
> id int,
>
> listing_id int,
>
> reviewer_id int,
>
> reviewer_name varchar(50),
>
> date date,
>
> comments varchar(2000)
>
> );
>
> CREATE TABLE calendar (
>
> listing_id int,
>
> date date,
>
> price decimal(10,2),
>
> available boolean
>
> );

![](./media/image28.jpeg)

4.  最後に、次の **INSERT INTO**
    ステートメントを実行して、一時テーブルからメイン
    テーブルにデータを読み込み、JSON データ
    フィールドから個々の列にデータを抽出します。

> INSERT INTO listings
>
> SELECT
>
> data\['id'\]::int,
>
> replace(data\['name'\]::varchar(50), '"', ''),
>
> replace(data\['street'\]::varchar(50), '"', ''),
>
> replace(data\['city'\]::varchar(50), '"', ''),
>
> replace(data\['state'\]::varchar(50), '"', ''),
>
> replace(data\['country'\]::varchar(50), '"', ''),
>
> replace(data\['zipcode'\]::varchar(50), '"', ''),
>
> data\['bathrooms'\]::int,
>
> data\['bedrooms'\]::int,
>
> data\['latitude'\]::decimal(10,5),
>
> data\['longitude'\]::decimal(10,5),
>
> replace(data\['description'\]::varchar(2000), '"', ''),
>
> replace(data\['summary'\]::varchar(2000), '"', ''),
>
> replace(data\['host_id'\]::varchar(50), '"', ''),
>
> replace(data\['host_url'\]::varchar(50), '"', ''),
>
> replace(data\['listing_url'\]::varchar(50), '"', ''),
>
> replace(data\['room_type'\]::varchar(50), '"', ''),
>
> data\['amenities'\]::jsonb,
>
> data\['host_verifications'\]::jsonb,
>
> data::jsonb
>
> FROM temp_listings;
>
> INSERT INTO reviews
>
> SELECT
>
> data\['id'\]::int,
>
> data\['listing_id'\]::int,
>
> data\['reviewer_id'\]::int,
>
> replace(data\['reviewer_name'\]::varchar(50), '"', ''),
>
> to_date(replace(data\['date'\]::varchar(50), '"', ''), 'YYYY-MM-DD'),
>
> replace(data\['comments'\]::varchar(2000), '"', '')
>
> FROM temp_reviews;
>
> INSERT INTO calendar
>
> SELECT
>
> data\['listing_id'\]::int,
>
> to_date(replace(data\['date'\]::varchar(50), '"', ''), 'YYYY-MM-DD'),
>
> data\['price'\]::decimal(10,2),
>
> replace(data\['available'\]::varchar(50), '"', '')::boolean
>
> FROM temp_calendar;

![](./media/image29.jpeg)

## 手順 2: Azure AI と Vector の拡張機能を**allowlist**に追加する

このラボでは、azure_ai 拡張機能と pgvector
拡張機能を使用して、PostgreSQL データベースに生成 AI
機能を追加します。この手順では、*PostgreSQL
拡張機能の使用方法で説明されているように、*これらの拡張機能をサーバーの*allowlist*に追加します。

1.  ホームページで、「**Resource Groups」**をクリックする。

![](./media/image30.jpeg)

2.  Resource group名をクリックする![](./media/image14.png)

3.  Resource Group中で**PostgreSQL Flexible Server**リソースを選択する

![](./media/image15.png)

4.  データベースの左側のナビゲーション メニューから、 **\[Settings\]**
    の **\[Server parameters\]**
    を選択し、検索ボックスに**+++azure.extensions+++**を入力する。\[VALUE\]
    ドロップダウン
    リストを拡張し、次の各拡張機能のにあるチェックボックスをオンにする。

    - AZURE_AI

    - POSTGIS

    - VECTOR

![](./media/image31.jpeg)

![](./media/image32.jpeg)

![](./media/image33.jpeg)

5.  ツールバーの \[保存**\] を選択すると**
    、データベースへの展開がトリガーされます。

![](./media/image34.jpeg)

## 手順 3: Azure OpenAI リソースを作成する

azure_ai 拡張機能では、ベクター埋め込みを作成するために、基になる Azure
OpenAI サービスが必要です。この手順では、Azure portal で Azure OpenAI
リソースをプロビジョニングし、そのサービスに埋め込みモデルをデプロイします。

### タスク 1: Azure OpenAI サービスをプロビジョニングする

このタスクでは、新しい Azure OpenAI サービスを作成します。

1.  Azure ポータルのホーム ページで、 **次の図に示すように、Microsoft
    Azure コマンド バーの左側にある 3 本の水平バーで表される** Azure
    ポータル メニューをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.jpeg)

2.  **+ Create a resource**へ移動してクリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

3.  On **Create a resourceページで** **Search services and
    marketplace**検索バー中に+++**Azure
    OpenAI**+++を入力して**Enterボタンをクリックする。**![A screenshot
    of a computer AI-generated content may be
    incorrect.](./media/image37.jpeg)

&nbsp;

4.  **Marketplaceページで** page, navigate to the **Azure
    OpenAIセクションへ移動して**Createのドロップダウンボタンをクリックして、図に示すよう**Azure
    OpenAIを選択する。** (すでに**Azure
    OpenAI**タイルをクリックしている場合は、**Azure
    OpenAIページのCreateボタンをクリックします**)。

![A screenshot of a software page AI-generated content may be
incorrect.](./media/image38.png)

5.  On the Create Azure
    OpenAI **Basics**タブで次の情報を入力して**Nextボタンをクリックする。**

[TABLE]

6.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image39.png)

7.  **\[Network\]**
    タブで、すべてのラジオボタンをデフォルトの状態のままにして、\[**Next\]**ボタンをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

8.  In
    the **Tagsタブで全てのフィールドをディフォルト状態のままにして、Nextボタンをクリックする。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

9.  In
    the **Review+submit** タブでヴァリデーションが成功になる場合**Create**ボタンをクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

10. デプロイが完了するまで待ちます。デプロイには約 2 分から 3
    分かかります。

\[!注\]**注:**Azure OpenAI
Serviceが現在、アプリケーションフォームを介して顧客に利用可能であるというメッセージが表示された場合。選択したサブスクリプションはサービスに対して有効になっておらず、価格レベルのクォータはありません。Azure
OpenAI
サービスへのアクセスをリクエストするには、リンクをクリックし、リクエスト
フォームに記入する必要があります。

### タスク 2: Azure OpenAI サービスのキーとエンドポイントを読み出す

1.  リソースの \[**Overview**\] ページで、 \[**Go to resource**\]
    ボタンを選択します。プロンプトされたら、ラボの資格情報を選択します:

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.jpeg)

2.  **Azure OpenAI ホーム** ウィンドウで、 \[**リソース管理**\]
    セクションに移動し、 **\[Keys and Endpoint\]** をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

3.  \[**キーとエンドポイント\]** ページで、次の画像に示すように
    **KEY1、KEY 2、**および **Endpoint**
    の値をコピーしてメモ帳に貼り付け、
    メモ帳を保存して今後のタスクで情報を使用します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.jpeg)

**注:** KEY1 または KEY2 のいずれかを使用できます。常に 2
つのキーを持つことで、サービスを中断することなく、キーを安全にローテーションして再生成できます。

### タスク 3: Deploy an embedding model

azure_ai
拡張機能を使用すると、テキストからベクター埋め込みを作成できます。これらの埋め込みを作成するには、Azure
OpenAI サービス内に text-embedding-ada-002 (バージョン 2)
モデルをデプロイする必要があります。このタスクでは、Azure OpenAI Studio
を使用して、採用できるモデル のデプロイを作成します。

1.  **Azure
    OpenAI**ページで、左側のナビゲーションメニューの**\[Overview**\]をクリックし、下にスクロールして、
    **次の画像に示すように\[Go to Azure OpenAI
    Studio**\]ボタンをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

2.  On the **Azure AI Foundry | Azure Open AI
    Serviceのホームページで** **Components**
    セクションに移動して**Deployments**をクリックする。

3.  **Deployments画面で** window, drop down the **+Deploy
    model** をドロップダウンして**Deploy base modelを選択する。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

4.  In the **Select a model**ダイアログボックスで dialog box, navigate
    and carefully
    select **text-embedding-ada-002**へナビゲートして慎重に選択してから**Confirm**ボタンをクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

5.  In the **Deploy
    model** ダイアログでモデルをデプロイするために次を設定して**Createを選択する**。

    - **Select a
      model**:リストから**text-embedding-ada-002**を選択する。

    - **Model version**: **2 (Default)が選択することを確認する。**

    - **Deployment name**: +++**embeddings**+++を入力する

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

6.  **Deployments画面で** **Deployment
    nameをコピーしてメモ帳に貼り付ける（** 画像に示すよう）それからメモ帳をSaveして次のタスクに情報を使用できます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

## 手順 4: azure_ai 拡張機能をインストールして構成する

この手順では、azure_ai 拡張機能をデータベースにインストールし、Azure
OpenAI サービスに接続するように構成します。

### タスク 1: Azure Cloud Shell中にpsqlを使用してデータベースへ接続する。

このタスクでは、Azure Cloud Shell の psql コマンドライン
ユーティリティを使用してデータベースに接続します。

1.  Azure portal のツール バーで **Cloud Shell**
    アイコンを選択すると、ブラウザー 外面の上部に新しい **Cloud Shell**
    外面が開きます.

2.  Cloud Shellの中に**Connection details**を貼り付ける

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image21.jpeg)

3.  Cloud Shell プロンプトで、**{your_password}**
    トークンをデータベースの作成時に **s2admin**
    ユーザーに割り当てたパスワードに置き換え、パスワードは
    **Seattle123Seattle123 にする必要があります**。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.jpeg)

4.  psql
    コマンドラインユーティリティを使用してデータベースに接続するには、プロンプトで次を入力する：

+++**psql**+++

![A black background with a black square AI-generated content may be
incorrect.](./media/image23.jpeg)

### タスク 2: azure_ai 拡張機能をインストールする

azure_ai 拡張機能を使用すると、Azure OpenAI と Azure Cognitive Services
をデータベースに統合できます。データベースで拡張機能を有効にするには、次の手順に従います。

1.  拡張機能が*allowlist*に正常に追加されたことを確認するには、psql
    コマンド プロンプトから次のコマンドを実行します:

+++SHOW azure.extensions;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

2.  CREATE EXTENSION コマンドを使用して、azure_ai
    拡張機能をインストールする。

+++CREATE EXTENSION IF NOT EXISTS azure_ai;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.jpeg)

### タスク 3: azure_ai拡張機能内に保存されるオブジェクトを確認する。 

azure_ai
拡張機能内のオブジェクトを確認すると、その機能について理解を深めることができます。このタスクでは、拡張機能によってデータベースに追加されたさまざまなスキーマ、ユーザー定義関数
(UDF)、および複合型を検査します。

1.  psql コマンドプロンプト**から \dx メタコマンドを使用して**
    、拡張機能に含まれるオブジェクトを一覧表示できます。

\[!注\] 注: クラウド シェルが \[More...\]
とプロンプトされたら、任意のキーをクリックして続行する。

+++\dx+ azure_ai+++

![](./media/image56.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image57.jpeg)

メタコマンドの出力は、azure_ai 拡張機能がデータベース内に 3
つのスキーマ、複数のユーザー定義関数
(UDF)、およびいくつかの複合型を作成することを示しています。次の表に、拡張機能によって追加されたスキーマと、それぞれについて説明します。

[TABLE]

1.  1\.
    関数と型はすべて、いずれかのスキーマに関連付けられています。azure_ai
    スキーマで定義されている関数を確認するには、\df
    メタコマンドを使用して、表示する関数のスキーマを指定します。\df
    の前に \x auto
    コマンドを付けると、必要に応じて展開表示が自動的に適用され、Azure
    Cloud Shell でコマンドの出力を見やすくすることができます。

+++\x auto+++ +++\df+ azure_ai.\*+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

azure_ai.set_setting() 関数を使用すると、Azure AI
サービスのエンドポイントとキーの値を設定できます。キー と
それを割り当てる値を受け入れます。azure_ai.get_setting()
関数は、set_setting() 関数で設定した値を取得する方法を提供します。
表示したい設定のキーを受け入れます。どちらの方法でも、キーは次のいずれかである必要があります：

### タスク 4: Azure OpenAI エンドポイントとキーを設定する

azure_openai 関数を使用する前に、Azure OpenAI サービス
エンドポイントとキーへ拡張機能を構成します。

1.  次のコマンドでは、**{endpoint}** トークンと **{api-key}** トークンを
    Azure portal から取得した値に置き換え、Cloud Shell ウィンドウの psql
    コマンド
    プロンプトからコマンドを実行して、値を構成テーブルに追加する。

2.  SELECT azure_ai.set_setting('azure_openai.endpoint','{endpoint}');

3.  SELECT azure_ai.set_setting('azure_openai.subscription_key',
    '{api-key}');

![A computer screen with white text AI-generated content may be
incorrect.](./media/image59.jpeg)

4.  次のクエリを使用して、構成テーブルに記述された設定を確認します：

&nbsp;

5.  SELECT azure_ai.get_setting('azure_openai.endpoint');

6.  SELECT azure_ai.get_setting('azure_openai.subscription_key');

azure_ai 拡張機能が Azure OpenAI
アカウントに接続され、ベクター埋め込みを生成する準備が整っています。

![A computer screen with white text AI-generated content may be
incorrect.](./media/image60.jpeg)

## 手順 5: Azure OpenAI を使用してベクター埋め込みを生成する

azure_ai 拡張機能の azure_openai スキーマにより、Azure OpenAI
はテキスト値のベクトル埋め込みを作成できます。このスキーマを使用すると、Azure
OpenAI
でデータベースから直接埋め込みを生成し、入力テキストのベクトル表現を作成できます。このベクトル表現は、ベクトル類似度検索に使用できるだけでなく、機械学習モデルでも利用できます。

埋め込みとは、機械学習と自然言語処理（NLP）における概念であり、単語、文書、エンティティなどのオブジェクトを多次元空間内のベクトルとして表現するものです。埋め込みにより、機械学習モデルは情報間の関連性を評価できます。この手法は、データ間の関係性と類似性を効率的に識別し、アルゴリズムがパターンを識別して正確な予測を行うことを可能にします。

### タスク 1: pgvector 拡張機能によりベクトルサポートを有効

azure_ai
拡張機能を使用すると、入力テキストの埋め込みを生成できます。生成されたベクトルをデータベース内の他のデータと共に保存できるようにするには、データベースドキュメントのベクトルサポートの有効化のガイダンスに従って、pgvector
拡張機能をインストールする必要があります。

1.  CREATE EXTENSION コマンドを使用して pgvector
    拡張機能をインストールする。

+++CREATE EXTENSION IF NOT EXISTS vector; +++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image61.jpeg)

2.  データベースに vector supported を追加したら、vector
    データ型を使用して listings
    テーブルに新しい列を追加し、テーブル内に埋め込みを格納します。text-embedding-ada-002
    モデルは 1536 次元のベクトルを生成するため、ベクトル サイズとして
    1536 を指定する必要があります。

3.  ALTER TABLE listings

4.  ADD COLUMN description_vector vector(1536);

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image62.jpeg)

### タスク 2: ベクトル埋め込みの生成と保存

これで、listings
テーブルに埋め込みを格納する準備が整いました。azure_openai.create_embeddings()
関数を使用して、description フィールドのベクトルを作成し、listings
テーブルの新しく作成した description_vector 列に挿入します。

1.  create_embeddings()
    関数を使用する前に、次のコマンドを実行して検査し、必要な引数を確認する:

+++\df+ azure_openai.\* +++

![A computer screen with white text AI-generated content may be
incorrect.](./media/image63.jpeg)

\df+ azure_openai.\* コマンドの出力にある Argument
データ型プロパティは、関数が想定する引数の一覧を示します。

[TABLE]

2.  デプロイメント名を使用して、次のクエリを実行して listings
    テーブルの各レコードを更新し、azure_openai.create_embeddings()
    関数を使用して description フィールドに生成されたベクトル埋め込みを
    description_vector 列に挿入します。{your-deployment-name}
    を、**Azure OpenAI Studio** の \[**デプロイ\] ページ**からコピーした
    **\[デプロイ名**\] の値に置き換えます。このクエリは完了するまでに約
    5 分かかることに注意してください.

> DO $$
>
> DECLARE counter integer := (SELECT COUNT(\*) FROM listings WHERE
> description \<\> '' AND description_vector IS NULL);
>
> DECLARE r record;
>
> BEGIN
>
> RAISE NOTICE 'Total descriptions to embed: %', counter;
>
> WHILE counter \> 0 LOOP
>
> BEGIN
>
> FOR r IN
>
> SELECT listing_id FROM listings WHERE description \<\> '' AND
> description_vector IS NULL
>
> LOOP
>
> BEGIN
>
> UPDATE listings
>
> SET description_vector =
> azure_openai.create_embeddings('{your-deployment-name}', description)
>
> WHERE listing_id = r.listing_id;
>
> EXCEPTION
>
> WHEN OTHERS THEN
>
> RAISE NOTICE 'Waiting 1 second before trying again...';
>
> PERFORM pg_sleep(1);
>
> END;
>
> counter := (SELECT COUNT(\*) FROM listings WHERE description \<\> ''
> AND description_vector IS NULL);
>
> IF counter % 25 = 0 THEN
>
> RAISE NOTICE 'Remaining descriptions to embed: %', counter;
>
> END IF;
>
> END LOOP;
>
> END;
>
> END LOOP;
>
> END;
>
> $$;

![A computer screen with white text AI-generated content may be
incorrect.](./media/image64.jpeg)

上記のクエリは、WHILEループを使用して、listingsテーブルから、description_vectorフィールドがnullで、descriptionフィールドが空文字列ではないレコードを取得します。次に、azure_openai.create_embeddings関数を使用して、description_vector列をdescription列のベクトル表現で更新しようとします。この更新を実行する際にループを使用するのは、createembeddings関数の呼び出しがAzure
OpenAIサービスの呼び出しレート制限を超えないようにするためです。呼び出しレート制限を超えた場合、出力に次のような警告が表示されます。

\[!注\] 注: 再試行する前に 1 秒待機します....

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image65.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image66.jpeg)

3.  すべてのリスト レコードに対して description_vector
    列が入力されたことを確認するには、次のクエリを実行します:

+++SELECT COUNT(\*) FROM listings WHERE description_vector IS NULL AND
description \<\> '';+++

クエリの結果は 0 のカウントである必要があります.

![A black screen with white text AI-generated content may be
incorrect.](./media/image67.jpeg)

### タスク 3: ベクトル類似性検索の実行

ベクトル類似度は、2 つの項目の類似性をベクトル (一連の数値)
として表すことによって、類似性を測定するために使用される方法です。ベクトルは、LLM
を使用して検索を実行するためによく使用されます。ベクトル類似度は、通常、ユークリッド距離やコサイン類似度などの距離メトリックを使用して計算されます。ユークリッド距離は
n 次元空間内の 2 つのベクトル間の直線距離を測定し、コサイン類似度は 2
つのベクトル間の角度のコサインを測定します。各埋め込みは浮動小数点数のベクトルであるため、ベクトル空間内の
2 つの埋め込み間の距離は、元の形式の 2
つの入力間の意味的類似性と相関します。

1.  ベクトル類似性検索を実行する前に、ILIKE
    句を使用して次のクエリを実行し、ベクトル類似性を使用せずに自然言語クエリを使用してレコードを検索した結果を観察します。

+++SELECT listing_id, name, description FROM listings WHERE description
ILIKE '%Properties with a private room near Discovery Park%';+++

![A black background with white text AI-generated content may be
incorrect.](./media/image68.jpeg)

クエリは、説明フィールドのテキストと提供された自然言語クエリを一致させようとしているため、結果が
0 件返されます。

2.  リスティングテーブルに対してコサイン類似度検索クエリを実行し、リスティングの説明に対するベクトル類似度検索を実行します。入力された質問に対して埋め込みが生成され、ベクトル配列
    (::vector)
    にキャストされます。これにより、リスティングテーブルに格納されているベクトルと比較できるようになります。{your-deployment-name}
    を、Azure OpenAI Studio
    の**デプロイメントページ**からコピーした**デプロイメント名**の値に置き換えます。

+++SELECT listing_id, name, description FROM listings ORDER BY
description_vector \<=\>
azure_openai.create_embeddings('{your-deployment-name}', 'Properties
with a private room near Discovery Park')::vector LIMIT 3;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image69.jpeg)

このクエリでは、多次元空間内の 2
つのベクトル間の距離を計算するために使用される \cosine distance\\
演算子を表す \<=\> ベクトル演算子を使用します。

3.  EXPLAIN ANALYZE
    句を使用して同じクエリを再度実行し、クエリの計画時間と実行時間を表示します。**{your-deployment-name}**
    を**、Azure OpenAI Studio** の \[**デプロイ**\] ページからコピーした
    **\[デプロイ名\] の値に置き換えます**。

> EXPLAIN ANALYZE
>
> SELECT listing_id, name, description FROM listings
>
> ORDER BY description_vector \<=\>
> azure_openai.create_embeddings('{your-deployment-name}', 'Properties
> with a private room near Discovery Park')::vector
>
> LIMIT 3;

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image70.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image71.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image72.jpeg)

出力では、次のような内容と同じようなことで始まるクエリ
プランに注意する：

Limit (cost=1098.54..1098.55 rows=3 width=261) (actual
time=10.505..10.507 rows=3 loops=1) -\> Sort (cost=1098.54..1104.10
rows=2224 width=261) (actual time=10.504..10.505 rows=3 loops=1)

…

Sort Method: top-N heapsort Memory: 27kB -\> Seq Scan on listings
(cost=0.00..1069.80 rows=2224 width=261) (actual time=0.005..9.997
rows=2224 loops=1) クエリは、シーケンシャル スキャン
ソートを使用してルックアップを実行しています。計画時間と実行時間は結果の最後にリストされ、次のようになります:
Planning Time: 62.020 ms Execution Time: 10.530 ms

4.  ベクトルフィールド上でより効率的な検索を可能にするために、コサイン距離とHNSW（Hierarchical
    Navigable Small
    Worldの略）を使用してリストのインデックスを作成します。HNSWにより、pgvectorは最新のグラフベースのアルゴリズムを利用して最近傍クエリを近似することができます。

+++CREATE INDEX ON listings USING hnsw (description_vector
vector_cosine_ops);+++

![](./media/image73.jpeg)

5.  5\. hnswインデックスがテーブルに与える影響を確認するには、EXPLAIN
    ANALYZE句を指定してクエリを再度実行し、クエリの計画時間と実行時間を比較します。{your-deployment-name}を、Azure
    OpenAI
    Studioの「デプロイメント」ページからコピーしたデプロイメント名の値に置き換えます。

> EXPLAIN ANALYZE
>
> SELECT listing_id, name, description FROM listings
>
> ORDER BY description_vector \<=\>
> azure_openai.create_embeddings('{your-deployment-name}', 'Properties
> with a private room near Discovery Park')::vector
>
> LIMIT 3;

![A computer screen with white text AI-generated content may be
incorrect.](./media/image74.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image75.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image76.jpeg)

出力では、クエリ プランに、より効率的なインデックス
スキャンが含まれていることに注意する。

Limit (cost=116.48..119.33 rows=3 width=261) (actual time=1.112..1.130
rows=3 loops=1) -\> Index Scan using listings_description_vector_idx on
listings (cost=116.48..2228.28 rows=2224 width=261) (actual
time=1.111..1.128 rows=3 loops=1)

クエリ実行時間は、クエリの計画と実行にかかる時間を大幅に短縮するはずです:

計画時間: 56.802 ms

実行時間: 1.167 ms

## 手順 6: Azure AI Servicesを統合

azure_ai 拡張機能の azure_cognitive スキーマに含まれる Azure AI
サービス統合は、データベースから直接アクセスできる豊富な AI
言語機能を提供します

機能には、感情分析、言語検出、キーフレーズ抽出、エンティティ認識、テキスト要約などがあります。これらの機能は、Azure
AI Language サービスを通じて有効化されます。

拡張機能を通じて利用可能な Azure AI
機能の完全な一覧を確認するには、「Azure Database for PostgreSQL Flexible
Serverと Azure Cognitive Services の統合」ドキュメントをご覧ください。

### タスク 1: Azure AI 言語サービスをプロビジョニングする

Azure AI Languageservice
は、azure_ai拡張機能の認知機能を利用するために必要です。この手順では、Azure
AI Language Serviceを作成します。

1.  Azure ポータルのホーム ページで、 **次の図に示すように、Microsoft
    Azure コマンド バーの左側にある 3 本の水平バーで表される** Azure
    ポータル メニューをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image77.jpeg)

2.  \[**リソースの作成\] ページで、左側のメニューから** \[**AI + Machine
    Learning\] を選択し** 、 \[**Language Service\] を選択します**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image78.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image79.jpeg)

3.  **Select additional featuresダイアログでContinue to create your
    resource**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image80.jpeg)

4.  On the Create Languageの**Basicsタブで次を入力する：**

[TABLE]

5.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image81.png)

6.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image82.jpeg)

&nbsp;

7.  既定の設定はLanguage service構成の残りのタブに使用されるため、
    \[**Review + create\]** ボタンを選択します。

&nbsp;

8.  \[Review + Create**\] タブの \[Create**\] ボタン**を選択して**
    、Language Serviceをプロビジョニングする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image83.png)

9.  Language Serviceの展開が完了した場合deploymentページの **Go to
    resource groupを選択する。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image84.jpeg)

### タスク 2: Azure AI Language サービスのエンドポイントとキーを設定する

azure_openai 関数と同様に、azure_ai 拡張機能を使用して Azure AI
サービスに対して正常に呼び出しを行うには、Azure AI Language
サービスのエンドポイントとキーを指定する必要があります。

1.  言語のホームページで、
    左側のナビゲーションメニューから**「リソース管理」の下の**「**キーとエンドポイント**」項目を選択します。

2.  **\[キーとエンドポイント\]** ページで、次の図に示すように
    **KEY1、KEY 2、**エンドポイント
    の値をコピーしてメモ帳に貼り付け、メモ帳**を保存し**て今後のタスクで情報を使用します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image85.jpeg)

3.  エンドポイントとアクセス
    キーの値をコピーし、次のコマンドで、{endpoint} トークンと {api-key}
    トークンを Azure portal から取得した値に置き換えます。Cloud Shell の
    psql コマンド
    プロンプトからコマンドを実行して、構成テーブルに値を追加します。

\[注意!\]注**:**以下のコマンドを実行する前に、psqlコマンドプロンプトに接続する。

SELECT azure_ai.set_setting('azure_cognitive.endpoint','{endpoint}');

SELECT azure_ai.set_setting('azure_cognitive.subscription_key',
'{api-key}');

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image86.jpeg)

### タスク 3: レビューのセンチメントを分析する

このタスクでは、azure_cognitive.analyze_sentiment 関数を使用して、Airbnb
のリスティングのレビューを評価します。

1.  azure_ai 拡張機能の azure_cognitive
    スキーマを使用して感情分析を実行するには、analyze_sentiment
    関数を使用します。次のコマンドを実行して、その機能を確認します：

+++\df azure_cognitive.analyze_sentiment+++

![A computer screen with white text AI-generated content may be
incorrect.](./media/image87.jpeg)

出力には、関数のスキーマ、名前、結果のデータ型、および引数のデータ型が表示されます。この情報は、機能の使用方法を理解するのに役立ちます。

2.  関数が出力する結果データ型の構造を理解し、戻り値を正しく処理することも重要です。sentiment_analysis_result型を調べるには、次のコマンドを実行します:

+++\dT+ azure_cognitive.sentiment_analysis_result+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image88.jpeg)

3.  上記のコマンドの出力は、sentiment_analysis_result型がタプルであることを示しています。そのタプルの構造を理解するには、次のコマンドを実行して、sentiment_analysis_result複合型に含まれる列を確認します:

+++\d+ azure_cognitive.sentiment_analysis_result+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image89.jpeg)

このコマンドの出力は、次のようになります：複合型
"azure_cognitive.sentiment_analysis_result"

Column | Type | Collation | Nullable | Default | Storage | Description
----------------+------------------+-----------+----------+---------+----------+-------------

sentiment | text | | | | extended |

positive_score | double precision | | | | plain |

neutral_score | double precision | | | | plain |

negative_score | double precision | | | | plain |

azure_cognitive.sentiment_analysis_result
は、入力テキストのセンチメント予測を含む複合型です。これには、肯定的、否定的、中立的、または混合の感情と、テキストで見つかった肯定的、中立的、否定的な側面のスコアが含まれます。スコアは
0 から 1 までの実数で表されます。たとえば、(neutral,0.26,0.64,0.09)
では、センチメントは中立で、正のスコアは 0.26、中立のスコアは
0.64、負のスコアは 0.09 です.

## 手順 7: すべてをまとめるために最後のクエリを実行します

### この手順では、pgAdmin でデータベースに接続し、ラボ 3 と 4 全体の azure_ai、postgis、pgvector 拡張機能を使用した作業をまとめる最終クエリを実行します。タスク 1: pgAdminをインストール

1.  Webブラウザーを開き、<https://www.pgadmin.org/download/pgadmin-4-windows/>に移動する

&nbsp;

2.  pgAdminの最新バージョンをクリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.jpeg)

3.  Select **pgadmin4-8.9-x64.exe**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image91.jpeg)

4.  ダウンロードしたファイルを実行してインストールする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image92.jpeg)

5.  Select Setup Install Modeタブで**Install for me
    only(recommended)を選択する**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image93.jpeg)

6.  **Nextボタンをクリックする**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image94.jpeg)

7.  **I accept the agreementを選択してNextボタンをクリック**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image95.jpeg)

8.  パスを選択して**Nextボタンをクリック**

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image96.jpeg)

9.  **Setup-pgAdmin 4**画面で**Next**ボタンをクリックする

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image97.jpeg)

10. **Installボタンをクリックする**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image98.jpeg)

11. **Setup-pgAdmin 4** 画面で**Finishボタンをクリックする**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image99.jpeg)

### タスク 2: pgAdminを使用してデータベースへ接続する

このタスクでは、pgAdminを開き、データベースに接続します。

1.  Windowsの検索ボックスに「**+++pgAdmin+++**」と入力し、\[**pgAdmin\]をクリックします**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image100.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image101.jpeg)

2.  サーバーを登録するには **、**オブジェクト エクスプローラーで
    \[**サーバー**\] を右クリックし、\[**Register\>Server\]**
    を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image102.jpeg)

3.  \[Register‐ Server**\] ダイアログで** 、Azure Database for
    PostgreSQL Flexible Serverサーバー名 (手順 1\> タスク 1
    で保存したもの) を \[**General**\] タブの **\[Name**\]
    フィールドに貼り付けます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image103.jpeg)

4.  次に、\[**Connection**\]タブを選択し、サーバー名を下記**Hostname/address**フィールドに張り付ける。**Username**
    フィールドに+++**s2admin**+++を入力して **Password** 
    ボックス+++**Seattle123Seattle123**+++
    を入力して、必要に応じて**Save password**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image104.jpeg)

5.  最終に**Parametersタブを選択してSSL modeをrequireに設定する**.
    サーバを登録するために**Saveをクリックする。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image105.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image106.jpeg)

6.  サーバーに接続したら、\[**データベース**\]ノードを展開し、**airbnb**データベースを選択します。**airbnb**データベースを右クリックし、
    **コンテキストメニューからQuery tool**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image107.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image108.jpeg)

### タスク 3: PostGIS 拡張機能がデータベース内にインストールされていることを確認する

データベースに postgis 拡張機能をインストールするには、CREATE EXTENSION
コマンドを使用します。

1.  上記で開いたクエリウィンドウで、CREATE EXTENSION コマンドを IF NOT
    EXISTS 句とともに実行し、データベースに postgis
    拡張機能をインストールします。

+++CREATE EXTENSION IF NOT EXISTS postgis;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image109.jpeg)

PostGIS
拡張機能が読み込まれたので、データベース内の地理空間データの操作を開始する準備が整いました。上記で作成して入力したリスティング
テーブルには、リスティングされたすべての宿泊施設の緯度と経度が含まれています。これらのデータを地理空間分析に使用するには、listings
テーブルを変更して、point データ型を受け入れる geometry
列を追加する必要があります。これらの新しいデータ型は、postgis
拡張機能に含まれています.

2.  ポイント データを格納するには、ポイント
    データを受け入れる新しいジオメトリ列をテーブルに追加します。次のクエリをコピーして、開いているpgAdminクエリウィンドウに貼り付けます:

3.  ALTER TABLE のリスティング

+++ADD COLUMN listing_location geometry(point, 4326); +++

4.  次に、経度と緯度の値を geometry
    列に追加して、各リストに関連付けられた地理空間データでテーブルを更新します。

&nbsp;

5.  UPDATEのリスティング

+++SET listing_location = ST_SetSRID(ST_Point(longitude, latitude),
4326);+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image110.jpeg)

### タスク 4: クエリを実行し、マップ上に結果を表示する

1.  次のクエリをコピーして開いているクエリ
    エディターに貼り付け、それを実行して listing_location
    列に格納されているデータを表示します 。

+++SELECT listing_id, name, listing_location FROM listings LIMIT 50;+++

\[データ出力\] パネルで、 **クエリ結果の** listing_location
列**に表示される \[View all geometries**\] ボタンを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image111.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image112.jpeg)

2.  次に、次のクエリを実行して**geospatial proximity
    query**を実行し、2016 年 1 月 13 日の週に利用可能で、1 泊あたり
    $75.00 未満で、シアトルのディスカバリー
    パークから近い距離内にあるプロパティを返します。このクエリは、PostGIS
    拡張機能によって提供される ST_DWithin
    関数を使用して、公園から一定の距離 (経度 -122.410347、緯度
    47.655598) のリストを識別します。

> SELECT name, listing_location, summary
>
> FROM listings l
>
> INNER JOIN calendar c ON l.listing_id = c.listing_id
>
> WHERE ST_DWithin(
>
> listing_location,
>
> ST_GeomFromText('POINT(-122.410347 47.655598)', 4326),
>
> 0.025
>
> )
>
> AND c.date = '2016-01-13'
>
> AND c.available = 't'
>
> AND c.price \<= 75.00;

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image113.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image114.jpeg)

**要約**

このラボでは、Azure AI サービスと PostgreSQL を正常に統合して、強力な AI
対応データベース環境を作成しました。まず、Azure
リソースをプロビジョニングし、必要な拡張機能を使用して PostgreSQL
データベースを構成しました。次に、テキスト
データのベクトル埋め込みを生成し、ベクトル類似性検索を実行して、意味的に類似したレコードを見つけました。さらに、地理空間データ分析には
PostGIS 拡張機能を、感情分析には Azure AI Language
サービスを利用しました。最後に、インデックス作成を使用してクエリを最適化し、そのパフォーマンスを分析し、高度なデータ分析のためのこの統合ソリューションの効率性と機能を実証しました。

 
