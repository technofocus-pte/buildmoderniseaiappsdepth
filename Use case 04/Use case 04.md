# ユースケース 04 - TODO リスト ASP.NET アプリを構築し、SQL データベースに接続する Azure App Service にデプロイする

**推定時間:** 40分

**ラボタイプ:** インストラクター主導

**目的:**

Azure App Service は、高度にスケーラブルで自己修正可能な Web
ホスティング サービスを提供します。このラボでは、データ駆動型 ASP.NET
アプリを App Service にデプロイし、Azure SQL Database
に接続する方法を学習します。完了すると、Azure で実行され、SQL Database
に接続された ASP.NET アプリが完成します。

## 手順 0: VM と資格情報を理解する

このタスクでは、ラボ全体で使用する資格情報を特定して理解します。

1.  **\[Instructions**\]
    タブには、ラボ全体に従うべき指示が記載されたラボ ガイドがあります。

&nbsp;

1.  **\[Resources**\] タブには、ラボの実行に必要な資格情報があります。

    - **URL** – Azure portalへのURL

    - **Subscription** – アサインされたサブスクリプションのID

    - **Username** –Azure servicesへログインするために必要となるユーザID

    - **Password** –
      Azureログインのパスワード。このユーザー名とパスワードをAzureログイン資格情報と呼びます。Azureログイン資格情報について記載する場合は必ずこの資格情報を使用します。

    - **Resource Group** – アサインされた**Resource group**

\[!注意\] 重要: すべてのリソースをこのリソース グループの下に作成する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  **「ヘルプ**」タブにはサポート情報が表示されます。ここで表示される**ID**は、　ラボ実行時に使用される**ラボインスタンスID**です。.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## 手順 1: Azure SQL Database を使用して ASP.NET アプリを Azure にデプロイする

### タスク 1: Set up Visual Studio 2022を設定してアプリを実行する

1.  1\. Windows 検索バーに「+++Visual studio+++」と入力し、「Visual
    Studio 2022」を選択します。サインインを求められた場合は、手順 2 と 3
    に進むか、手順 4 から続行します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.jpeg)

2.  **Sign
    inをクリックして、VMのResourcesタブ中にあるユーザ資格情報　　　セクションにあるUsername** と **passwordを使用してサインインする**![A
    screenshot of a computer AI-generated content may be
    incorrect.](./media/image4.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

3.  **Start Visual Studio**を選択する

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  **Open a local folder**を選択する

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

5.  **C:\Labfileの中にwebappwithsqldbフォルダを選択してSelect
    Folder**を　　クリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

1.  フォルダが開いたら、**ソリューションエクスプローラー**から**DotNetAppSqlDb.sln**をダブルクリックする。

**注意:** ソリューションエクスプローラーが自動的に開かない場合**View -\>
Solution Explorer** をクリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

6.  **Build** -\> **Build Solution**をクリックする

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image10.jpeg)

7.  ビルドが完了したら**Debug -\>** **Start Debugging**を選択する

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

8.  これが**Todos web app** を実行されているブラウザを開きます

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.jpeg)

9.  Add few items into the app by clicking
    on 以下のスクリーンショットのようアプリにいくつかのアイテムを追加するために**Create
    Newをクリックする** ![A screenshot of a computer AI-generated
    content may be incorrect.](./media/image14.jpeg)

![A screenshot of a application AI-generated content may be
incorrect.](./media/image15.jpeg)

10. リストにさらにいくつかの項目を追加します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.jpeg)

11. Visual Studio 2022から**Debug -\>** **Stop Debugging**をクリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

### タスク 2: ASP.NET アプリケーションを Azure に公開する

1.  **Solution
    Explorer**で**DotNetAppSqlDb**プロジェクトに右クリックして**Publish**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

2.  **Azureを選択し、Next**をクリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.jpeg)

3.  **Which Azure service would you like to use to host your
    application?** 画面に**Azure App
    Service(Windows)** を選択し、**Next**をクリックする

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image21.jpeg)

4.  Publishダイアログの中に**Sign
    Inをクリックし、Azureサブスクリプションにサインインする（まだサインインしていない場合）。注意：既にMicrosoftアカウントにサインインしている場合、そのアカウントにAzureサブスクリプションが設定されていることを確認する。サインインしているMicrosoftアカウントにAzureサブスクリプションがない場合は、正しいアカウントを追加するためにそれをクリックする。**

5.  Click on **Create
    newをクリックして新しいアプリサービスを作成する。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.jpeg)

6.  以下の詳細を入力する。

[TABLE]

7.  **New** on **Hosting Plan**上に**New**をクリックする。

8.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image23.png)

9.  **Hosting
    Planオプション上にNewをクリックして以下の情報を入力しOKをクリック**。

[TABLE]

10. ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image24.png)

11. App Service
    windowで**Create**をクリックしてAzureリソースを作成されるのを待つ 。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image25.png)

12. The **Publishダイアログには設定したリソースが表示される。Finishをクリックする** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

13. **Closeをクリックする**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image27.jpeg)

14. Server
    Dependenciesセクションまでスコロールし、＋記号をクリックして依存関係を追加する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

15. Select **Azure SQL Database** in the **Add dependencyページ中にAzure
    SQL Database**  を選択して**Next**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.jpeg)

16. **Connect to Azure SQL
    Databaseダイアログボックス中に、SQLデータベースの横にあるCreate
    New** をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

17. **Azure SQL Database Create
    newダイアログボックス中にデータベースサーバーの横にあるNew**をクリックする。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

18. 以下の詳細を入力し、OKをクリックする

[TABLE]

19. ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image32.png)

20. Create newダイアログ中に**Create**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

### タスク 3: データベース接続の構成

1.  ウィザードでデータベースリソースの作成を完了したら**Nextをクリックする**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

2.  **Connect to Azure SQL
    Databaseに以下の詳細を入力し、Finish**をクリックする 

[TABLE]

3.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image35.jpeg)

4.  **Summary of changes**を確認した後**Finish**をクリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

5.  構成ウィザードが完了するまで待ち、**Close**
    をクリックする。これでAzure SQL Dbがアプリに接続されました。 is
    now **connected** to your app.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

6.  Publishページで右上隅にある**Publish**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

**注意：**これは約5分かかります

> 7\. ASP.NET アプリが Azure にデプロイされると、デプロイされたアプリの
> URL が表示された既定のブラウザーが起動します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

### タスク 4: データベースにローカルでアクセスする

Visual Studioでは**SQL Server Object
Explorer**を使用してAzureの新しいデータベースを簡単に探索と管理にできます。新しいデータベースでは作成したAppServiceアプリに対するファイヤーウォールを既にオープンしました。ただし、ローカル
コンピュータ (Visual Studio など) からアクセスするには、ローカル
マシンのパブリック IP
アドレスに対してファイアウォールを開く必要があります。インターネットサービスプロバイダーがパブリックIPアドレスを変更した場合Azureデータベースに再度アクセスするようにファイヤーウォールを再構成する必要です

1.  From the Visual Studio 2022の**View**メニュから**SQL Server Object
    Explorer**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

2.  At the top of **SQL Server Object Explorerの上、Add SQL
    Server** ボタンをクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.jpeg)

### タスク 5: データベース接続を構成する

1.  In the **Connect** dialog, expand the **Azure** node. All your SQL
    Database instances in Azure are listed here.

> **コネクトダイアログ**で**Azureノード**を展開する。Azure内のすべてのSQLデータベースがここに一覧表示されます。

2.  以前作成したデータベースを選択する(**dotnetappsqldbdbserver98**)。前作成した接続は下部に自動的に入力されます。

3.  前作成したデータベース管理者パスワードを入力し(+++**PassWord98**+++)、Connectをクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

### タスク 6: コンピュータからのクライアント接続を許可する

Create a new
firewallルールダイアログが開きます。デフォルトでは、サーバーがAzureアプリなどのAzureサービスからのみデータベースへの接続を許可します。Azureの外部からデータベースへーに接続するには、サーバーレベルでファイアウォールルールを作成する。ファイアウォールルールはローカルコンピューターのIPアドレスを許可しています。

ダイアログには、コンピューターのパブリックIPアドレスが既に入力されています。

1.  Add my client IPが選択されていることを確認し、OKをクリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.jpeg)

2.  Visual
    StudioでSQLデータベースインスタンスのファイアウォール設定の作成が完了すると、接続が**SQL
    Server Object Explorer**に表示されます**。**

3.   **connection \> Databases \> \< YOUR DATABASE \> \>
    Tables**を展開する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

4.  **Todoes表で右クリックし、** **View Data**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

5.  テーブルの内容を表示する。アプリのUIから追加されたデータは、ここに一覧表示されます。

![](./media/image46.jpeg)

## 手順 2: Code First Migrations でアプリを更新する

1.  ソリューション エクスプローラー**で、コード エディターで**
    Models\Todo.cs **を開きます** 。次のプロパティを **ToDo**
    クラスの最後の行として追加します (**public DateTime CreatedDate {
    get; set; }** 行)をクリックし、\[Save\]をクリックします .

+++**public bool Done { get; set; }**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.jpeg)

### タスク 1: Code First Migrations をローカルで実行する

いくつかのコマンドを実行して、ローカルデータベースを更新します。

1.  **Tools**メニューから**NuGet Package Manager** \> **Package Manager
    Console**をクリックする

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image48.jpeg)

2.  パッケージ マネージャー コンソール
    ウィンドウで、次のコマンドを実行して Code First Migrations
    を有効にします。

+++**Enable-Migrations**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.jpeg)

3.  次のコマンドを実行して、マイグレーションを追加する

> +++**Add-Migration Addプロパティ**+++

![](./media/image50.jpeg)

4.  次のコマンドを実行してローカルデータベースを更新する。

+++**Update-Database**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

5.  **Ctrl+F5キーを押してアプリを実行する、又はDebug -\> Start without
    Debuggingをクリックする。編集、と詳細をテストし、リンクを作成する**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image52.jpeg)

1.  アプリケーション ページが開きますが、アプリケーション
    ロジックでこの新しいプロパティがまだ使用されていないため、外観は同じままです。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

### タスク 2: 新しいプロパティを使用する

Make some changes in your code to use the Done プロパティ.

1.  Visual Studioから**Controllers\TodosController.csを開く。**52行目で
    **Create()** メソッドを見つけた後Bind属性のプロパティリストに
    +++**Done**+++ を追加する。完了するとCreate()
    メソッドのシグネチャーは次のコードのようになります。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

1.  **Views\Todos\Create.cshtmlを開きます。CreatedDate** **の** div
    class="form-group" \> \<の後に、次のコードを追加します。

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

3.  **Views\Todos\Index.cshtmlを開く。CreatedDate** の **th**
    要素の後の空の **th** 要素**に次のコードを追加します**。

<+++@Html.DisplayNameFor>(model =\> model.Done)+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image56.jpeg)

4.  html.ActionLink() helper
    methods.このコードをhtml.ActionLink()ヘルパメソッドのすぐ上に追加する。

5.  \<td\>

6.  @Html.DisplayFor(modelItem =\> item.Done)

\`\`\`

\![\](./media/image53.jpeg)

5.  **Ctrl+F5を入力し、アプリを実行する。** 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.jpeg)

### タスク 3: AzureでEnable Code First Migrationsを有効する in 

1.  プロジェクトを右クリックして**Publish**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

2.  Publish **More
    actions** \> **Edit**をクリックしてPublishの設定を開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

3.  In the **MyDatabaseContextドロップダウンで** Azure SQL Database
    のデータベース接続を選択する。

4.  Select **Execute Code First
    Migrations** (アプリの起動時に実行)を選択し、**Save**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image60.jpeg)

5.  Publishページで**Publishをクリックする。**

![A black rectangular object with white text AI-generated content may be
incorrect.](./media/image61.jpeg)

6.  これで更新されたアプリがAzureで利用できるようになりました。

7.  To-do項目をもう一度追加し、Doneを選択するとホームページで完了項目として、表示されます。

![A screenshot of a application AI-generated content may be
incorrect.](./media/image62.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image63.jpeg)

## 手順 3: アプリケーションログのストリーム

1.  PublishページでHostingセクションまでにスクロールする。右隅にある［…］**View
    Streaming Logs**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image64.jpeg)

2.  これで、ロッグがアウトプットウィンドウにストリーミングされます

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image65.jpeg)

3.  トレースメセッジはまだ表示されません。View Streaming
    Logsはじめに選択したときにAzureアプリによってトレースレベルがエラーに設定されエラーイベントのみログに記録されるためです

\[!注意\] **注意:** まだ表示されない場合Visual Studio
からログストリーミを再起度する。

### タスク 1: トレースレベルの変更

1.  Publishページへ移動して、ホスティングセクションで **… \> Open in
    Azure portal**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image66.jpeg)

2.  AzureポータルのアプリページでMonitoringセクションの左側画面から**App
    Service Logs** を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image67.jpeg)

3.  Under **Application Logging** (File System)
    の下にLevelでVerboseを選択する。Saveをクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image68.jpeg)

4.  ブラウザからAzure上のウェッブアプリにアクセスし、いくつかの活動を実行する。

![A screenshot of a application AI-generated content may be
incorrect.](./media/image69.jpeg)

5.  これで、トレースメセッジがVisual Studio
    のアウトプットウィンドウにストリーミングされるようになります。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image70.jpeg)

6.  ロッグストリーミングサービスを停止するにはアウトプットウィンドウで**Stop
    monitoring** ボタンをクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image71.jpeg)

7.  Visual Studioを閉じる

## 手順 4: リソースの削除

1.  Azure portalからアサインされたResourcegroupを開く。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image72.png)

2.  全てのリソースを選択してDeleteをクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image73.png)

3.  Type
    テキストボックスの中に+++delete+++を入力し、Deleteを選択する。確認ダイアログボックスでDeleteを選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image74.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image75.png)

**要約**

このラボではデータ駆動型ASP.NETアプリをApp
Serviceにデプロイし、それをAzure SQL
Databaseに接続する方法を学びました。
