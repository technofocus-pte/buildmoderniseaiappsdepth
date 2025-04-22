# ユースケース02 - Linux と PostgreSQL 上の Azure App Service を使用して Fruits List Quarkus Web アプリを作成する

**推定時間:** 40分

**ラボタイプ:** インストラクター主導

**目的：**

このユースケースでは、PostgreSQL データベースに接続されている Azure App
Service で安全な Quarkus
アプリケーションを構築、構成、デプロイする方法を示します (Azure Database
for PostgreSQL を使用)。Azure App Service は、Windows または Linux
にアプリを簡単にデプロイできる、高度にスケーラブルな自己修正プログラム
Web ホスティング サービスです。完了すると、Linux 上の Azure App Service
で Quarkus アプリが実行されます。

**前提条件:**

**GitHubアカウント** --
GitHubのログイン認証情報をお持ちである必要があります。お持ちでない場合は、こちらから作成できましす。
- +++<https://github.com/signup?user_email=&source=form-home-signup+++>

## 手順 0: VM と資格情報を理解する

このタスクでは、ラボ全体で使用する資格情報を特定して理解します。

1.  **\[Instructions\]** タブ
    には、ラボ全体に従うべき指示が記載されたラボ
    ガイドが含まれています。

&nbsp;

2.  **\[リソース**\] タブには、ラボの実行に必要な資格情報があります。

    - **URL** – Azure portal の URL

    - **Subscription** –割り当てられたサブスクリプションの ID です

    - **Username** – Azure サービスにログインするために必要なユーザー
      ID。

    - **Password** – Azure ログインのパスワード。

このユーザー名とパスワードをAzureログイン資格情報と呼びます。Azureログイン資格情報について記載する際には、必ずこの資格情報を使用します。

- **Resource Group** – **割り当てられた**リソース グループ

\[!アラート\]**重要:**このリソースグループの下にすべてのリソースを作成します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  **\[ヘルプ**\] タブには、サポート情報が表示されます。ここでの **ID**
    値は、 ラボの実行中に使用される**ラボ インスタンス ID** です。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## 手順 1: サンプルを実行する

まず、出発点としてサンプルのデータ駆動型アプリをセットアップします。ここで使用するサンプルリポジトリには、開発コンテナの設定が含まれています。開発コンテナには、データベース、キャッシュ、サンプルアプリケーションに必要なすべての環境変数など、アプリケーション開発に必要なものがすべて揃っています。開発コンテナはGitHubのコードスペースで実行できるため、Webブラウザであらゆるコンピュータでサンプルを実行できます。

1.  ブラウザーから、GitHub にサインインする
    account +++\*\*<https://github.com/login**+++>.

2.  新しいタブからこのURLを開く+++\*\*<https://github.com/technofocus-pte/msdocs-quarkus-postgresql-sample-app**+++>.

3.  **Fork -\> Create a new fork**を選択する

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.jpeg)

4.  Create a new forkページで**Create fork** をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

5.  リポジトリのフォークされたページで、 \[**Code\]** \> **\[Create
    codespace on main\] を選択します**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

**注:** メイン オプションで \[Create Codespace\]
が表示されない場合は、Codespaces の横にある \[+\] 記号をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

**注:** codespace の作成には、設定に約 10 分かかります。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

5.  ターミナルで+++mvn
    quarkus:dev+++を実行する。ポップアップで**Allowをクリックする。**

![A screenshot of a browser AI-generated content may be
incorrect.](./media/image8.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

6.  **Your application running on port 8080** is
    availableとの通知が表示される場合**Open in Browser**を選択する**。**
    新しいブラウザ タブにサンプル アプリケーションが表示されます。

port **5005**との通知が表示さるたら**skip**する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

1.  Quarkus 開発サーバーを停止するには、 **Codespace ターミナルで**
    **Ctrl+C** を入力する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

## 手順 2: App Service と PostgreSQL を作成する

まず、Azure リソースを作成します。このラボで使用する手順では、App
Service や Azure Database for PostgreSQL
など、既定でセキュリティが確保されたリソースのセットを作成します。

1.  Azure portal を +++<https://portal.azure.com/+++> で開き、 **VM の
    \[Resources**\] タブから Azure
    ログイン資格情報を使用して**ログイン**する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

2.  \[Welcome\] ページで \[キャンセル\] または閉じるボタンを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.jpeg)

3.  Azure portal の上部にある検索バーに「**+++web app
    database+++**」と入力します。**\[Marketplace\]**
    の見出し**の下にある** \[**Web App + Database**\]
    というラベルの付いた項目を選択します.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.jpeg)

4.  **Create Web App + Databaseで次の情報を記入してReview +
    createを選択する。**

[TABLE]

5.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image16.png)

6.  ![A screenshot of a web application AI-generated content may be
    incorrect.](./media/image17.jpeg)

7.  検証に合格したら、「作成」をクリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

**注:** アプリの作成には約 15 分かかります。

8.  デプロイメントが完了したら**Go to resource**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

1.  App Service ページ**に直接移動します**。
    左上隅にある**\[ホーム**\]をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.jpeg)

9.  ポータルメニューをクリックし、**Resource Groupsをクリックする。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.jpeg)

10. アサインされたResource
    groupを選択し、実行したデプロイからつぃぎの resourceが作成さっれていることを確認する。

> \- アプリサービスプラン
>
> \- アプリサービス
>
> \- 仮想ネットワーク
>
> \- Azure Database for PostgreSQL flexible server
>
> \- プライベートDNSゾーン

![A screenshot of a group AI-generated content may be
incorrect.](./media/image22.png)

## 手順 3: 接続設定の確認

作成ウィザードでは、接続変数がアプリ設定として既に生成されています。この手順では、アプリ設定の場所と、独自の設定を作成する方法について説明します。

1.  Resource Group内のリソースの一覧から **App Service**
    をクリックする。

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image23.jpeg)

2.  App Serviceページの左側メニューで**Settings**の下の **Environment
    variables**  を選択する。

3.  **Environment variablesページのApp
    settings**タブで **AZURE_POSTGRESQL_CONNECTIONSTRINGが存在することを確認する。　**これは、実行時に環境変数として挿入されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

4.  **+ Add**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.jpeg)

1.  設定に +++**PORT**+++ という名前を付け、その値を Quarkus
    アプリケーションのデフォルトポートである +++**8080**+++
    に設定します。\[**適用\]** を選択します。

![A screenshot of a login AI-generated content may be
incorrect.](./media/image26.jpeg)

5.  **Apply**を選択する

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

6.  **Confirm**を選択する

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

7.  アプリの設定が更新されことを示す通知が届きます。

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image29.jpeg)

## 手順 4: Deploy sample code

この手順ではGitHub
Actionsを使用してGitHubの展開を構成します。これはアプリサービスにデプロイする多くの方法の一つであるとデプロイプロセスで継続的の統合を実現するための優れた方法でもあります。デフォルトでは、GitHub
リポジトリへのすべての git
プッシュによって、ビルドとデプロイのアクションが開始されます

1.  アプリサービスページで左側のメニューから**Deployment**
    の下に**Deployment Center** を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

2.  Sourceの中で**GitHub**を選択する。デフォルトでは、ビルド
    プロバイダーとして GitHub Actions が選択されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

3.  **Authorizeをクリックすることで**GitHubアカウントにサインし、プロンプトに伴ってAzureを承認する。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image32.jpeg)

4.  下記のほうに詳細を記入し、残りはデフォルトのままにして**Save**をクリックする。他をデフォルトでとして残り

[TABLE]

5.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image33.jpeg)

6.  **\[保存**\] をクリックすると、App Service はワークフロー
    ファイルを選択した GitHub リポジトリの .github/workflows
    ディレクトリにコミットします.

7.  サンプル フォークの GitHub codespace に戻り、+++git pull origin
    main+++ を実行します。これにより、新しくコミットされたワークフロー
    ファイルが codespace に取り込まれます。

\[!注\]**注:**ターミナルでテストケースがまだ実行されている場合は、Ctrl +
Cを押してから上記のコマンドを実行できます.

![A screenshot of a computer code AI-generated content may be
incorrect.](./media/image34.jpeg)

7.  エクスプローラーで src/main/resources/application.properties
    **を開きます** 。Quarkus は、このファイルを使用して Java
    プロパティをロードします.

8.  コード（10～11行目）を見つけてください。このコードは、プロダクション変数
    %prod.quarkus.datasource.jdbc.url
    を、作成ウィザードで指定されたアプリ設定に設定します。quarkus.package.type
    は、App Service で実行する必要がある Uber-Jar
    をビルドするように設定されています。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.jpeg)

9.  エクスプローラーで .github/workflows/main_quarkuwebapp\[lab instance
    id\].yml **を開きます** 。このファイルは、App Service
    作成ウィザードによって作成されました.

10. Build with Mavenを使用したステップでMaven commandを+++**mvn clean
    install -DskipTests**+++に変更する。

**-DskipTests** は、GitHub
ワークフローが途中で失敗するのを避けるために、Quarkus
プロジェクトのテストをスキップします.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

11. **Source Control拡張機能を選択する。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

12. テクストボックスの中に+++**Configure DB and deployment
    workflow**+++のようなコミットメセッジを入力する。**Commitを選択し、Yesで確認する。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.jpeg)

13. **Sync changes 1**を選択し**、OK**で確認する**。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

14. Back in the Deployment Center page in the Azureポータルの Deployment
    Center
    ページに戻り、**Logs**を選択する。コミットされた変更からすでに新規のデプロイ実行が開始される。

15. デプロイ実行のログ項目で、 **最新のタイムスタンプを持つ**
    Build/Deploy Logs エントリを選択します.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.jpeg)

1.  GitHub リポジトリに移動し、GitHub
    アクションが実行されていることを確認します。ワークフロー
    ファイルでは、ビルドとデプロイの 2
    つのステージが定義されています。GitHub の実行で \[完了\]
    の状態が表示されるまで待ちます。所要時間は約5分です。

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image42.jpeg)

## 手順 5: アプリをブラウズする

1.  Azureポータル(+++[https://portal.azure.com+++](https://portal.azure.com+++/))から、リソースグループ**ResourceGroup1**を開き**、App
    Service**リソースを選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

2.  左側のメニューから**Overview**を選択して**Default
    domainの下のアプリの**URL**を選択する。**![A screenshot of a
    computer AI-generated content may be
    incorrect.](./media/image44.jpeg)

3.  コーピしたurlを新しいブラウザに張り付けて、アプリを開く。

![A screenshot of a fruit list AI-generated content may be
incorrect.](./media/image45.jpeg)

4.  リストにFruitをいくつか追加します。これで、Azure Database for
    PostgreSQL へのセキュアあな接続を備えた Web アプリを Azure App
    Service で実行できるようになりました。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.jpeg)

## 手順 6: 診断ログのストリーム

Azure App
Serviceがコンソールに出力された全てのメセッジをキャプチャーして、アプリの問題を診断するのに役立ちます。以下のようサンプルアプリにはこの機能を示す標準のJBossロギングステートメントが含まります。

1.  Azure portal の \[App Service\] ページで、左側のメニューから
    \[**Monitoring**\] の下の **\[App Service Logs\] を選択します**。

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image48.jpeg)

1.  **Application logging**の下に **File System**を選択する**。**
    上部のメニューで、\[Save\] を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.jpeg)

2.  左側のメニューから**Log
    stream**を選択する。プラットフォームログやコンテナ内のログを含まりアプリのログが表示されます。

![A computer screen shot of a computer screen AI-generated content may
be incorrect.](./media/image50.jpeg)

## 手順 7: リソースの削除

1.  AzureポータルのホームページでResource groupsを選択する

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

2.  **NetworkWatcherRGを選択してDelete resource group**をクリックする

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

3.  テキストボックスの中に+++NetworkWatcherRG+++を選択して**Delete**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image54.png)

4.  次に、Resource groupページからアサインされてResource
    groupを選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

5.  全ての**resourcesを選択して、Delete**を選択する。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

6.  テキストボックスの中に+++**delete**+++を入力して**Delete**をクリックする。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image58.png)

7.  削除されたリソースの正常通知が削除を確認します。

8.  GitHub ワークスペースに戻り、 \[**コード**\]
    の横にあるドロップダウンをクリックし、codespace 名の横にある 3
    つのドットを選択して、 \[**削除**\] をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

**要約：**

Azure App
Service中でセキュアなQuarkusアプリをデプロイして、それをPostgreSQLデータベースに接続して,
アプリのUIからFruit名を追加する方法を学びました。
