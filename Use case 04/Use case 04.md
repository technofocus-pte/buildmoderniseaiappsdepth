# 사용 사례 04 - TODO List ASP.NET 앱을 구축하고 SQL Database에 연결하는 Azure App Service에 배포하기

**예상 소요 시간:** 40분

**실습 유형:** 강사 진행

**목표:**

Azure App Service는 확장성이 뛰어난 자체 패치 웹 호스팅 서비스를
제공합니다. 이 실습에서는 App Service에서 데이터 기반 ASP.NET 앱을
배포하고 Azure SQL Database에 연결하는 방법을 배울 것입니다. 완료되면,
Azure에서 실행 중인 ASP.NET앱이 있고 SQL Database에 연결되어 있을
것입니다.

## 연습 0: VM 및 자격 증명을 이해하기

이 작업에서는 실습 전체에서 사용할 자격 증명을 식별하고 이해할 것입니다.

1.  **Instructions** 탭에는 실습 전체에서 따라야 할 지침이 있는 실습
    가이드가 있습니다.

2.  **Resources** 탭에는 실습을 실행하는 데 필요한 자격 증명이 있습니다.

    1.  **URL** – Azure 포털에 대한 URL

    2.  **Subscription** – 사용자에게 할당돤 구독의 ID

    3.  **Username** – Azure 서비스에 로그인하는 데 사용하는 사용자 ID.

    4.  **Password** – Azure 로그인에 대한 비밀번호. 이 사용자 이름과
        비밀번호를 Azure 로그인 자격 증명이라고 하겠습니다. Azure 로그인
        자격 증명을 언급할 때마다 이러한 자격 증명을 사용할 것입니다.

    5.  **Resource Group** – 사용자에게 할당돤 **Resource group**.

\[!경고\] **중요:** 이 리소스 그룹 아래에 모든 리소스를 생성해야 합니다

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

1.  **Help** 탭에는 Support 정보가 있습니다. **ID** 값은 실습 중에
    사용되는**Lab instance ID**입니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## 연습 1: Azure SQL Database를 사용하여 Azure에 ASP.NET 앱을 배포하기

### 작업 1: Visual Studio 2022를 설정하고 애플리케이션을 실행하기

3.  Windows **Search** 바에 +++**Visual studio**+++를 입력하고Visual
    Studio 2022를 선택하세요. 로그인하라는 메시지가 표시되면 2단계와 3
    단계를 계속하거나 4단계부터 계속하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.jpeg)

4.  Click on **Sign in**을 클릭하고 VM의 리소스 탭에 있는 **User
    Credentials** 섹션에서 **Username** 및 **password** 를 사용하여
    **sign in** 하새요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  **Start Visual Studio**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

6.  **Open a local folder**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

7.  **C:\Labfiles**의 **webappwithsqldb** 폴더를 선택하고 **Select
    Folder**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

8.  폴더가 열리면**Solution Explorer**에서**DotNetAppSqlDb.sln**를 두 번
    클릭하세요.

**참고:** Solution Explorer가 자동으로 열리지 않으면 **View -\> Solution
Explorer**를 클릭하세요**.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

9.  **Build** -\> **Build Solution**를 클릭하세요.

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image10.jpeg)

10. 빌드가 완료되면 **Debug -\>** **Start Debugging**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

11. 그러면 **Todos web app**이 실행 중인 브라우저가 열립니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.jpeg)

12. 아래 스크인샷과 같이 **Create New**를 클릭하여 앱에 몇 가지 항목을
    추가하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.jpeg)

![A screenshot of a application AI-generated content may be
incorrect.](./media/image15.jpeg)

13. 목록에 몇 가지 항목을 추가하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.jpeg)

14. Visual Studio 2022에서 **Debug -\>** **Stop Debugging**를
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

### 작업 2: Azure에 ASP.NET 애플리케이션을 개시하기

2.  **Solution Explorer**에서 **DotNetAppSqlDb** 프로젝트를 마우스
    오른쪽 버튼으로 클릭하고**Publish**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

3.  **Azure**를 선택하고**Next**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.jpeg)

4.  **Azure App Service(Windows)** in **Which Azure service would you
    like to use to host your application?** 화면을 선택하고 **Next**를
    클릭하세요.

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image21.jpeg)

5.  아직 로그인 하지 않은 경우 Publish 대화상자에서 **Sign In**을
    클릭하고  Azure 구독에 로그인하세요.

**참고:** Microsoft 계정에 이미 로그인한 경우 해당 Azure 구독이 있는지
확인하세요. 로그인한 Microsoft 계정에 Azure 구독이 없는 경우 클릭하여
올바른 계정을 추가하세요.

6.  새로운 App 서비스를 생성하기 위해 **Create new**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.jpeg)

7.  다음 세부 정보를 입력하세요.

[TABLE]

8.  **Hosting Plan**에서 **New**를 클릭하세요.

9.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image23.png)

10. **Hosting Plan** 옵션에서 **New**를 클릭하고 다음 세부 정보를
    입력하고 **OK**를 클릭하세요.

[TABLE]

11. ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image24.png)

12. App Service 창에서 **Create**를 클릭하고 Azure 리소스가 생성될
    때까지 기다리세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image25.png)

13. **Publish** 대화 상자에는 구성한 리소스가 표시됩니다. **Finish**를
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

14. **Close**를 클릭하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image27.jpeg)

15. Server Dependencies 섹션까지 아래로 스크롤하고 **+** 기호를 클릭하여
    종속성을 추가하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

16. **Add dependency** 페이지에서 **Azure SQL Database**를 선택하고
    **Next**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.jpeg)

17. **Connect to Azure SQL Database** 대화 상자에서 SQL데이터베이스 옆에
    있는 **Create New** 를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

18. **Azure SQL Database Create new** 대화 상자에서 데이터베이스 서버
    옆에 있는 **New**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

19. 다음 세부 정보를 입력하고 **OK**를 클릭하세요.

[TABLE]

20. ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image32.png)

21. Create new 대화 상자에서 **Create** 를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

### 작업 3: 데어터베이스 연결을 구성하기

1.  Wizard가 데이터베이스 리소스를 생성을 미치면 **Next**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

2.  **Connect to Azure SQL Database** 대화 상자에서 다음 세부 정보를
    입력하고 **Finish**를 클릭하세요.

[TABLE]

3.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image35.jpeg)

4.  **Summary of changes**을 검토한 후 **Finish**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

5.  구성 wizard가 완료될 때까지 기다렸다가 **Close**를 클릭하세요. 이제
    Azure SQL 유가 앱에**connected**되었습니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

6.  Publish 페이지에서 오른쪽 상단의 **Publish**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

**참고:** 약 5분 정도 소요됩니다

7.  ASP.NET 앱이 Azure에 배포되면 배포된 앱의 URL과 함께 기본 브라우저가
    시작됩니다. **Add a few to-do items**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

### 작업 4: 로컬에서 데이터베이스에 액세스하기

Visual Studio를 사용하면 **SQL Server Object Explorer**에서 Azure의 새
데이터베이스를 쉽게 탐색하고 관리할 수 있습니다. 새 데이터베이스는
사용자가 생성된 App Service 앱에 대한 방화벽을 이미 열었습니다. 그러나
로컬 컴퓨터 (예: Visual Studio)에서 액세스하려면 로컬 컴퓨터의 공용 IP
주소에 대한 방화벽을 열어야합니다. 인터넷 서비스 공급자가 공용 IP 주소를
변경하는 경우 Azure 데이터베이스에 다시 액세스하도록 방화벽을 다시
구성해야 합니다.

1.  Visual Studio 2022의 **View** 메뉴에서 **SQL Server Object
    Explorer**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

2.  **SQL Server Object Explorer** 위에서 **Add SQL Server** 버튼을
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.jpeg)

### 작업 5: 데이터베이스 연결을 구성하기

1.  **Connect** 대화상자에서 **Azure** 노드를 확장하세요. Azure의 SQL
    Database인스턴스가 여기에 나열됩니다.

2.  이전에 생성한 데이터베이스 (**dotnetappsqldbdbserver98**)를
    선택하세요. 이전에 생성한 연결은 맨 아래에 자동으로 채워집니다.

3.  이전에 생성한 데이터베이스 관리자 **password**
    (+++**PassWord98**+++)를 입력하고 Connect를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

### 작업 6: 컴퓨터에서 클라이언트 연결을 허용하기

Create a new firewall rule 대화 상자가 열립니다. 기본적으로 서버는 Azure
앱과 같은 Azure 서비스에서만 데이터베이스에 대한 연결을 허용합니다.
Azure 외부에서 데이터베이스에 연결하려면 서버 수준에서 방화벽 규칙을
생성합니다. 방화벽 규칙은 로컬 컴퓨터의 공용 IP 주소를 허용합니다.

대화 상자는 이미 컴퓨터의 공용 IP 주소로 채워져 있습니다.

1.  Add my client IP기 선택되어 있는지 확인하고 OK를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.jpeg)

2.  Visual Studio에서 SQL Database 인스턴스에 대한 방화벽 설정 생성을
    완료하면 **SQL Server Object Explorer**에 연결이 표시됩니다.

3.  **Connection \> Databases \> \< YOUR DATABASE \> \> Tables**를
    확장하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

4.  **Todoes** 테이블을 마우스 오른쪽 클릭하고 **View Data**를
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

5.  테이블의 내용을 봅니다. 앱 UI에서 추가된 데이터는 여기에 나열되어야
    합니다.

![](./media/image46.jpeg)

## 연습 2: Code First Migrations로 앱을 업데이트하기

1.  **Solution Explorer**에서 코드 편집기에 **Models\Todo.cs**를 여세요.
    다음 속성을 **ToDo** 클래스의 마지막 줄 (**public DateTime
    CreatedDate { get; set; }** 줄뒤)로 추가하고**Save**를 클릭하세요.

+++**public bool Done { get; set; }**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.jpeg)

### 작업 1: Code First Migrations 로컬로 실행하기

로컬 데이터베이스를 업데이트하기 위해 몇 가지 명령을 실행하세요.

1.  **Tools** 메뉴에서 **NuGet Package Manager** \> **Package Manager
    Console**를 선택하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image48.jpeg)

2.  Package Manager Console 창에서 이 명령을 실행하여Code First
    Migrations을 사용하도록 설정하세요.

+++**Enable-Migrations**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.jpeg)

3.  아래 명령을 실행하여 마이그레이션을 추가하세요.

+++**Add-Migration AddProperty**+++

![](./media/image50.jpeg)

4.  아래 명령을 실행하여 로컬 데이터베이스를 업데이트하세요.

+++**Update-Database**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

5.  앱을 실행하기 위해 **Ctrl+F5**를 입력하여 **Debug -\> Start without
    Debugging**를 클릭하세요. 편집, 세부 정보 테스트하고 링크를
    생성하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image52.jpeg)

6.  애플리케이션 페이지가 열리고 애플리케이션 로직이 아직 이 새 속성을
    사용하지 않기 때문에 여전히 동일하게 보입니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

### 작업 2: 새 속성을 사용하기

Done 속성을 사용하도록 코드를 약간 변경하세요.

1.  Visual Studio에서 **Controllers\TodosController.cs**를 여세요.
    52행에서 **Create()** 방법을 찾아 Bind 속성의 속성 목록에
    +++**Done**+++ 를 추가하세요. 완료되면 Create() 메서드 서명이 다음
    코드와 같이 표시됩니다:

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

2.  **Views\Todos\Create.cshtml**를 여세요. **CreatedDate**에 대한 \<
    div class="form-group" \> 뒤에 다음 코드를 추가하세요.

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

3.  **Views\Todos\Index.cshtml**를 여세요. **CreatedDate**에 대한 **th**
    요소 뒤의 빈 **th** 요소에 다음 코드를 추가하세요.

<+++@Html.DisplayNameFor>(model =\> model.Done)+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image56.jpeg)

4.  html ActionLink() 도우미 메서드 바로 위에이 코드를 추가하세요.

5.  \<td\>

6.  @Html.DisplayFor(modelItem =\> item.Done)

\`\`\`

\![\](./media/image53.jpeg)

5.  앱을 실행하기 위해 **Ctrl+F5**를 입력하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.jpeg)

### 작업 3: Azure에서 Code First Migrations를 활성화하기

1.  프로젝트를 마우스 오른쪽 버튼으로 클릭하고 **Publish**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

2.  개시 설정을 열기 위해 **More actions** \> **Edit**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

3.  **MyDatabaseContext** 드롭다운에서 Azure SQL Database에 대한
    데이터베이스 연결을 선택하세요.

4.  **Execute Code First Migrations** (애플리케이션 시작 시 실행)를
    선택하고 **Save**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image60.jpeg)

5.  Publish 페이지에서 **Publish**를 클릭하세요.

![A black rectangular object with white text AI-generated content may be
incorrect.](./media/image61.jpeg)

6.  이제 업데이트된 앱을 Azure에서 사용할 수 있습니다.

7.  할 일 항목을 다시 추가하고**Done**를 선택하면 홈페이지에 완료괸
    항목으로 표시되어야 합니다.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image62.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image63.jpeg)

## 연습 3: 애플리케이션 로그를 스트리밍하기

1.  Publish 페이지에서 **Hosting** 섹션까지 아래로 스크롤하세요. 오른쪽
    모서리에서 **...** \> **View Streaming Logs**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image64.jpeg)

2.  이제 로그가 Output 창으로 스트리밍됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image65.jpeg)

3.  View Streaming Logs를 처음 선택하면 Azure 앱이 추적 수준을 오류
    이벤트만 기록하는 오류로 설장하기 때문에 추적 메시지가 아직 표시되지
    않습니다.

\[!참고\] **참고:** 로깅 스티리밍이 아직 표시되지 않는 경우 Visual
Studio에서 다시 시작하세요.

### 작업 1: 추적 수준을 변경하기

1.  Publish 페이지로 이동하세요. Hosting 섹션에서 **… \> Open in Azure
    portal**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image66.jpeg)

2.  Azure 포털 – 앱 페이지에서**Monitoring** 섹션의 왼쪽 창에서 **App
    Service Logs**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image67.jpeg)

3.  **Application Logging** (File System)에서Level의 **Verbose**를
    선택하세요. **Save**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image68.jpeg)

4.  브라우저에서 Azure의 웹앱에 액세스하고 몇 가지 작업을 수행하세요.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image69.jpeg)

5.  이제 추적 메시지가 Visual Studio의 Output 창으로 스트리밍됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image70.jpeg)

6.  로그 스트리밍 서비스를 중지하려면 Output 창에서 **Stop
    monitoring** 버튼을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image71.jpeg)

7.  Visual Studio를 닫으세요.

## 연습 4: 리소스를 정리하기

1.  Azure 포털에서 할당된 Resourcegroup를 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image72.png)

2.  모든 리소스를 선택하고 Delete를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image73.png)

3.  텍스트 상자에 +++delete+++ 를 입력하고Delete를 선택하세요.
    확인란에서Delete를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image74.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image75.png)

**요약**

이 실습에서는 데이터 기반 ASP.NET 앱을 App Service에 배포하고 Azure SQL
Database에 연결하는 방법을 배웠습니다.
