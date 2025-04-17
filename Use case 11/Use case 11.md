# 사용 사례 11 - Azure OpenAI, NoSQL용 Azure Cosmos DB을 사용하여 Copilot구축하기 

이 사용사례에서는 .NET 소프트웨어 개발 키트를 사용하여 Blazor웹
애플리케이션을NoSQL용 Azure Cosmos DB 및 Azure OpenAI에 연결할 것입니다.
코드는NoSQL용 API 커테이너의 항목을 관리하고 쿼리합니다. 또한 코드는
Azure OpenAI에 프롬프트를 보내고 응답을 구문 분석합니다.

**실습 시간:** 45분

**실습 유형 –** 강사 진행

**소개**

- Blazor, PostgreSQL 및 OpenAI에 대한 개발 환경을 설정하기.

- Blazor 프로젝트를 생성하고 반응형 채팅 인터페이스를 설계하기.

- Azure에서 PostgreSQL 데이터베이스를 구성하고 Blazor 앱에 연결하기.

- 향상된 채팅 기능을 위해 Azure OpenAI를 통합하기.

- Azure에 Blazor 애플리케이션 및 PostgreSQL 데이터베이스를 배포하기.

- 구성 요소 간의 원활한 상호 작용을 보장하기 위해 애플리케이션을
  테스트하기.

- Azure에 배포된 애플리케이션을 모니터링하고 문제를 해결하기.

**사용된 핵심 기술:**  NoSQL용 Azure Cosmos DB, Azure OpenAI

## 연습 0: VM 및 자격 증명을 이해하기

이 작업에서는 실습 전체에서 사용할 자격 증명을 식별하고 이해할 것입니다.

1.  **Instructions** 탭에는 실습 전체에서 따라야 할 지침이 있는 실습
    가이드가 있습니다.

2.  **Resources** 탭에는 실습을 실행하는 데 필요한 자격 증명이 있습니다.

    - **URL** – Azure 포털에 대한 URL

    - **Subscription** – 사용자에게 할당돤 구독의 ID

    - **Username** – Azure 서비스에 로그인하는 데 사용하는 사용자 ID.

    - **Password** – Azure 로그인에 대한 비밀번호. 이 사용자 이름과
      비밀번호를 Azure 로그인 자격 증명이라고 하겠습니다. Azure 로그인
      자격 증명을 언급할 때마다 이러한 자격 증명을 사용할 것입니다.

    - **Resource Group** – 사용자에게 할당돤 **Resource group**.

\[!경고\] **중요:** 이 리소스 그룹 아래에 모든 리소스를 생성해야 합니다

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  **Help** 탭에는 Support 정보가 있습니다. **ID** 값은 실습 중에
    사용되는**Lab instance ID**입니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## 연습 1: 인프라 배포 및 초기 설정을 완료하기

이 프로젝트를 완료하려면NoSQL용 Azure Cosmos DB 계정 및 Azure OpenAI
계정이 필요합니다. 이 프로세스를 간소화하려면 이러한 두 계정을 모두
사용하여 Azure에 Bicep 템플릿을 배포합니다.

### 작업 1: 템플릿에서 인프라를 배포하기

1.  **C:\Labfiles\Build and Test a custom chat application Using Azure
    Cosmos DB and AzureOpenAI** 경로에서 파일을 열고 96줄에서Azure
    OpenAI 버전을 +++0125+++로 업데이트하세요. 파일을 **Save**하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

2.  새 브라우저를 열고 주소 표시줄에 다음 URL을 입력하세요:
    +++<https://portal.azure.com/+++> 로Azure Portal을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  Azure 포털에서 검색창 오른쪽 페이지 상단의 **\[\>\_\] (Cloud
    Shell)** 버튼을 클릭하세요. Cloud Shell 창이 포털 아래 쪽에
    열립니다. Cloud Shell을 처음 열면 사용할 셸 유형
    (**Bash** 또는 **PowerShell**)을 선택하라는 메시지가 표시될 수
    있습니다. **Bash**를 선택하세요. 이 옵션이 표시되지 않으면 이 단계를
    건너뛰세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  **Getting Started** 대화상자에서 **Mount storage account**를
    선택하고**subscription**을 선택하고**Apply**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

5.  **Mount storage account** 대화상자에서 **we will create a storage
    account for you**를 선택하고**Next**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

![A close-up of a computer screen AI-generated content may be
incorrect.](./media/image9.jpeg)

6.  Cloud Shell 창의 왼쪽 상단에 표시된 셸 유형이 **Bash**로
    전환되었는지 확인하세요. **PowerShell**인 경우 드롭다운 메뉴를
    사용하여 **Bash**로 전환하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.jpeg)

7.  터미널이 시작되면 **Manage files -\> Upload**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

8.  **C:\Labfiles\Build and Test a custom chat application Using Azure
    Cosmos DB and AzureOpenAI**에서**azuredeploy.JSON** 파일을
    선택하고**Open**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

파일 업로드를 위해 성공 메시지가 표시되어야 합니다.

![A white background with black text AI-generated content may be
incorrect.](./media/image13.jpeg)

9.  생성한 Azure 리소스 그룹 (mslearn-cosmos-openai)의 이름을 사용하여
    **resourceGroupName**이라는 새 셸 변수를 생성하세요.

+++resourceGroupName="ResourceGroup1"+++(리소스 탭에서 리소스 그룹
이름을 가져오세요.)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

10. az group deployment create를 사용하여**azuredeploy.json** 템플릿
    파일을 리소스 그룹에 배포하세요. 다음 명령을 실행하세요.

+++az deployment group create --resource-group $resourceGroupName --name
zero-touch-deployment --template-file azuredeploy.json+++

**참고:** 이 배포에는 약 5-10분이 걸릴 수 있습니다.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image15.jpeg)

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image16.jpeg)

### 작업 2: NoSQL용 Azure Cosmos DB 및Azure OpenAI 계정 자격 증명을 받기

위의 배포에서는 NoSQL 및 Azure OpenAI 계정용 Azure Cosmos DB를 배포한 후
Azure App Service웹앱의 구성에 자격 증명을 저장했습니다. 이제 Azure
portal 또는 Azure CLI를 사용하여 각 서비스에 대한 자격 증명을 검색할 수
있습니다.

1.  Azure portal Home 페이지에서 **Resource groups**를 클릭하세요**.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

2.  Resource group을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

3.  **Resource Groups** 페이지에서**Essentials** 패널을 확장하고
    **Deployments** 헤더를 확인하세요. 이 시점에서 배포
    상태는**Succeeded**이어야 합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

4.  이제, 리소스의 페이지로 이동하기 위해**Azure Cosmos DB** 계정을
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

5.  리소스 탐색 메뉴의 **Settings** 섹션에서**Keys** 옵션을 선택하세요.
    **URI** 및 **PRIMARY KEY** 필드의 값을 기록하세요. 이러한 값은
    나중에 사용할 수 있습니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.jpeg)

6.  **Resource Groups** 페이지로 돌아가세요. **Azure OpenAI** 계정을
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

7.  **Azure Open AI** 창에 **Resource Management** 섹셔으로 이동하고
    **Keys and Endpoints**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.jpeg)

8.  **Keys and Endpoints** 페이지에서**KEY1** (*KEY1 또는 KEY2를 사용할
    수 있습니다)* 및**Endpoint**를 복사하여 예정된 작업의 정보를
    사용하기 위해 메모장을 **Save**하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

### 작업 3: Docker 실행하기

1.  Windows 검색 상자에서 +++Docker+++ 를 입력하고 **Docker Desktop**를
    클릭하세요.

![A screenshot of a desktop AI-generated content may be
incorrect.](./media/image25.jpeg)

2.  Docker Desktop을 실행하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.jpeg)

## 연습 2 – 시작 애플리케이션을 설정하고 구축하기

1.  VM searchbar에서 +++Visual Studio+++를 검색하고 **Visual Studio
    Code**를 선택하세요.

2.  **File** -\> **Open Folder**를 클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

3.  **C:\LabFiles**에서**cosmosdb-chatgpt**를 선택하고**Select
    Folder**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

4.  **Do you trust the authors dialog**의 **Yes, I trust the
    authors** 옵션을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.jpeg)

5.  **Visual Studio Code** 편집자에서 **Terminal**를 클릭하고 **New
    Terminal**를 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

6.  .NET 애플리케이션에서는 구성 공급자를 사요하여 애플리케이션에 새
    설정을 삽입하는 것이 일반적입니다. Azure OpenAI endpoint and key의
    최신 값을 제공하기 위해 이 애플리케이션의 경우
    **appsettings.Development.json** 파일을 사용하세요.

7.  **appsettings.Development.JSON** 파일을 여세요. 파일에서 **Azure
    Cosmos DB** 및 **Azure OpenAI** 리소스의 uri 및 키 값에 대한 자리
    표시자를 이전에 저장한 값으로 바꾸세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

8.  아래 명령을 실행하여 .NET 프로젝트를 **Build**하세요.

+++dotnet build+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image32.jpeg)

## 연습 3: 코드를 이해하기

### 작업 1: 필요한 구성원 및 클라이언트 인스턴스를 추가하기

1.  **Services/OpenAiService.cs** 파일을 여세요. 이 파일은 Azure OpenAI
    클라이언트를 사용하는 데 필요한 클래스 변수를 구현합니다. 몇 가지
    정적 프롬프트를 구현하고 OpenAIClient 클래스의 새 인스턴스를
    생성합니다.

2.  이 코드 블록은 각 프롬프트 전에 AI 어시스턴트에게 보낼 정적 텍스트
    블록이 있는 \_systemPromptText라는 새 문자열 변수를 생성합니다.

> private readonly string \_systemPrompt = @"
>
> You are an AI assistant that helps people find information.
>
> Provide concise answers that are polite and professional." +
> Environment.NewLine;

3.  이 코드 블록은 대화를 요약하는 방법에 대한 지침과 함께 AI
    어시스턴트에게 보낼 정적 텍스트 블록이 있는 \_summarizePrompt라는 또
    다른 새 문자열 변수를 만듭니다.

> private readonly string \_summarizePrompt = @"
>
> 이 프롬프트를 하나 또는 두 개의 단어로 요약하여 웹 페이지의 버튼에서
> 레이블로 사용합니다.
>
> Do not use any punctuation." + Environment.NewLine;

4.  이 코드 블록은 엔드포인트를 사용하여 Uri를 빌드하고 키를 사용하여
    AzureKeyCredential을 빌드하는 OpenAIClient 클래스의 새 인스턴스를
    생성합니다.

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

**작업 2: AI 모델에 질문하기**

먼저 AI 모델이 현재 대화의 컨텍스트에서 답변을 제공할 수 있도록 시스템
프롬프트, 질문 및 세션 ID를 전송하여 질문-답변 대화를 구현합니다.
프롬프트를 구문 분석하고 응답 (또는 이 컨텍스트에서 완료)을 반환하는 데
필요한 토큰 수를 측정해야 합니다.

1.  이 코드 블록은 ChatCompletionsOptions 형식의 options라는 새 변수를
    생성합니다. 두 개의 메시지 변수를 Messages 목록에 추가하고 User의
    값을 sessionId 생성자 매개 변수로 설정합니다.

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

2.  Azure OpenAI 클라이언트 변수(\_client)의 GetChatCompletionsAsync
    메서드는 비동기적으로 호출됩니다. 결과는 ChatCompletions 형식의
    completions라는 변수에 저장됩니다.

> Response\<ChatCompletions\> completionsResponse =
> await_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;

3.  마지막으로, 아래 코드 블록은 GetChatCompletionAsync 메서드의 결과로
    완료 내용, 프롬프트와 연결된 토큰 수 및 응답에 대한 토큰 수가 포함된
    튜플을 반환합니다.

> return (
>
> completionText: completions.Choices\[0\].Message.Content,
>
> completionTokens: completions.Usage.CompletionTokens
>
> );

**작업 3: AI 모델에 대화 요약을 요청하기**

이제 AI 모델이 대화를 몇 단어로 요약할 수 있도록 AI 모델에 다른 시스템
프롬프트, 현재 대화 및 세션 ID를 보내세요.

2.  아래 코드는 메시지 목록에 있는 두 개의 메시지 변수, sessionId 생성자
    매개 변수로 설정된 User, 200으로 설정된 MaxTokens 및 나머지 속성을
    사용하여 options라는 ChatCompletionsOptions 변수를 생성합니다.

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

3.  아래 코드는 \_client 호출합니다. GetChatCompletionsAsync는 모델
    이름(\_modelName) 및 options 변수를 매개 변수로 사용하여
    비동기적으로 동기화하고 결과를 ChatCompletions 형식의
    completions라는 변수에 저장합니다. 완료 내용을 SummarizeAsync
    메서드의 결과로 문자열로 반환합니다.

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

**작업 4 - NoSQL용 Azure Cosmos DB for과 연결하기**

CosmosDbService 클래스에는 이 모듈에서 이전에 작업한 OpenAiService
클래스와 유사한 서비스의 스텁 구현이 포함되어 있습니다. 반면, 이
클래스는 약간 다르게 작동하는 Azure Cosmos DB용 .NET SDK를 사용합니다.

이 섹션에서는 클라이언트를 사용하여 NoSQL용 Azure Cosmos DB에 액세스하는
데 필요한 클래스 변수 및 클라이언트의 구현을 설명합니다.

1.  **Services/CosmosDbService.cs** 파일을 여세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image34.jpeg)

2.  아래 코드는 CosmosSerializationOptions 형식의 options라는 변수를
    생성하여 변수의 PropertyNamingPolicy 속성을
    CosmosPropertyNamingPolicy.CamelCase로 설정합니다.

> CosmosSerializationOptions options = new()
>
> {
>
> PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
>
> };

**참고:** 이 속성을 설정하면 SDK에서 생성된 JSON이 .NET 클래스에서 해당
속성의 대/소문자 구분 방식에 관계없이 카멜 대/소문자로 직렬화 및
역직렬화됩니다.

3.  아래 코드는 앞에서 지정한 CosmosClientBuilder 클래스, 엔드포인트, 키
    및 직렬화 옵션을 사용하여 client라는 CosmosClient 형식의 새
    인스턴스를 생성합니다.

> CosmosClient client = new CosmosClientBuilder(endpoint, key)
>
> .WithSerializerOptions(options)
>
> .Build();

4.  아래 코드는 클라이언트 변수의 GetDatabase 메서드를 호출하여
    database라는 Database 형식의 새 nullable 변수를 생성합니다.

**Database? database = client?.GetDatabase(databaseName);**

5.  아래 코드는 null이 아닌 경우에만 생성자의 컨테이너 변수를 클래스의
    \_container 변수에 할당합니다. null인 경우 ArgumentException을
    throw하세요.

> \_container = container ??
>
> throw new ArgumentException("Unable to connect to existing Azure
> Cosmos DB container or database.");

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.jpeg)

**작업 5 - NoSQL 용 Azure Cosmos DB 서비스를 구현하기**

Azure Cosmos DB 서비스(CosmosDbService)는 AI 도우미 애플리케이션에서
세션 및 메시지의 쿼리, 생성, 삭제 및 업데이트를 관리합니다. 이러한 모든
작업을 관리하려면 서비스에서 .NET SDK의 다양한 기능을 사용하여 각 잠재적
작업에 대해 여러 방법을 구현해야 합니다.

이 연습에서는 여러 가지 주요 요구 사항을 해결해야 합니다:

- 세션이나 메시지를 만드는 작업을 구현

- 쿼리를 구현하여 여러 세션 또는 메시지를 검색

- 단일 세션을 업데이트하거나 여러 메시지를 일괄 업데이트하는 작업을 구현

- 여러 관련 세션 및 메시지를 쿼리하고 삭제하는 작업을 구현

NoSQL용 Azure Cosmos DB는 데이터를 JSON 형식으로 저장하므로 단일
컨테이너에 여러 유형의 데이터를 저장할 수 있습니다. 이 애플리케이션은 AI
비서와의 채팅 "세션"과 각 세션 내의 개별 "메시지"를 모두 저장합니다.
NoSQL용 API를 사용하면 애플리케이션에서 두 가지 유형의 데이터를 동일한
컨테이너에 저장한 다음 단순 유형 필드를 사용하여 이러한 유형을 구별할 수
있습니다.

1.  **Services/CosmosDbService.cs** 파일을 여세요.

2.  아래 코드는 현재 세션의 SessionId 속성을 매개 변수로 사용하여
    PartitionKey 형식의 partitionKey라는 새 변수를 생성합니다.

**PartitionKey partitionKey = new(session.SessionId);**

3.  아래 코드는 세션 매개 변수 및 partitionKey 변수를 전달하는
    컨테이너의 CreateItemAsync 메서드를 호출합니다. InsertSessionAsync
    메서드의 결과로 응답을 반환합니다.

> return await \_container.CreateItemAsync\<Session\>(
>
> item: session,
>
> partitionKey: partitionKey
>
> );

4.  아래 코드는 세션을 사용하여 PartitionKey 변수를 ㅊ. SessionId를
    파티션 키의 값으로 사용합니다. Timestamp 속성이 현재 UTC
    타임스탬프로 업데이트된 newMessage라는 새 메시지 변수를
    애플리케이션. CreateItemAsync를 호출하여 새 메시지와 파티션 키
    변수를 모두 전달합니다. InsertMessageAsync의 결과로 응답을
    반환합니다.

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

**작업 6: 여러 세션이나 메시지를 검색하기**

애플리케이션이 컨테이너에서 여러 항목을 검색해야 하는 두 가지 주요 사용
사례가 있습니다. 먼저 애플리케이션은 항목을 type = Session인 항목으로
필터링하여 현재 사용자의 모든 세션을 검색합니다. 둘째, 애플리케이션은
type = Session & sessionId = 인 유사한 필터를 수행하여 세션에 대한 모든
메시지를 검색합니다. 여기서는 두 쿼리 모두 .NET SDK 및 피드 반복기를
사용하여 구현됩니다.

1.  아래 코드는 QueryDefinition 유형의 query라는 새 변수를 생성합니다.
    fluent WithParameter 메서드를 사용하여 Session 클래스의 이름을 매개
    변수의 값으로 할당합니다. \_container 변수에 대해 제네릭
    GetItemQueryIterator\<\> 메서드를 호출하고 제네릭 형식 Session 및
    쿼리 변수를 매개 변수로 전달합니다. response라는 FeedIterator 유형의
    변수에 결과를 저장합니다.

> QueryDefinition query = new QueryDefinition("SELECT DISTINCT \* FROM c
> WHERE c.type = @type")
>
> .WithParameter("@type", nameof(Session));
>
> FeedIterator\<Session\> response =
> \_container.GetItemQueryIterator\<Session\>(query);

2.  while 루프 내의 아래 코드는 응답 변수에서 ReadNextAsync를 호출하여
    결과의 다음 페이지를 비동기적으로 가져온 후 해당 결과를 output이라는
    목록 변수에 추가합니다. while 루프 외부에서 출력 변수는
    GetSessionsAsync 메서드의 결과로 세션 목록과 함께 반환됩니다.

> FeedResponse\<Session\> results = await response.ReadNextAsync();
>
> output.AddRange(results);
>
> return output;

3.  아래 코드는 fluent WithParameter 메서드를 사용하여 매개 변수로
    전달된 세션 식별자에 @sessionId 매개 변수를 할당하고 @type 매개
    변수를 Message 클래스의 이름에 할당합니다.

> QueryDefinition query = new QueryDefinition("SELECT \* FROM c WHERE
> c.sessionId = @sessionId AND c.type = @type")
>
> .WithParameter("@sessionId", sessionId)
>
> .WithParameter("@type", nameof(Message));

4.  쿼리 변수와 GetItemQueryIterator\<\> 메서드를 사용하여
    FeedIterator\< Message \> 생성하세요.

FeedIterator response = \_container.GetItemQueryIterator(query);

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image37.jpeg)

## 연습 4: 앱을 실행하기

이제 애플리케이션에 Azure OpenAI 및 Azure Cosmos DB가 완전히
구현되었습니다. 솔루션을 디버깅하여 애플리케이션을 종단 간 테스트할 수
있습니다.

1.  **Visual Studio Code Terminal**에서 다음 명령을 사용하여 프로젝트를
    구축하세요.

+++**dotnet build**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.jpeg)

2.  Dotnet watch를 사용하여 핫 다시 로드를 사용하도록 애플리케이션을
    시작하세요.

+++**dotnet watch run --non-interactive**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

3.  Visual Studio Code는 웹 애플리케이션이 실행 중인 도구 내 단순
    브라우저를 시작하세요. 웹 애플리케이션에서 **+ Create New Chat**를
    클릭하요 새 채팅 세션을 생성하고 AI 비서에게 질문하세요. 실행 중인
    웹 애플리케이션을 닫으세요.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image40.jpeg)

4.  텍스트 상자에 다음 텍스트를 붙여넣고 **Send** 아이콘을 클릭하세요.

+++How many wins does it take to promote to the Premier League?+++

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image41.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

5.  텍스트 상자에 다음 텍스트를 붙여넣고 **Send** 아이콘을 클릭하세요.

+++What is Azure OpenAI?+++

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image43.jpeg)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image44.jpeg)

6.  터미널을 닫으세요.

## 연습 5: 리소스 그룹을 정리하기

1.  새 브라우저를 열고 주소 표시줄에 다음 URL을 입력하세요:
    +++<https://portal.azure.com/+++> 로Azure Portal를 여세요.

2.  Resource group 페이지에서**assigned Resource group**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  모든 **resources**를 선택하고 **Delete**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

4.  텍스트 상자에서 +++**delete**+++를 입력하고**Delete**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image49.png)

5.  삭제된 리소스에 대한 성공 알림은 삭제를 확인합니다.

6.  리소스가 삭제되면 Azure 포털 홈페이지에서 **Azure AI Services**를
    검색하여 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.jpeg)

7.  왼쪽 창에서 **Azure OpenAI**를 선택하고**Manage deleted
    resources**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

8.  거기에 나열되는 리소스를 선택하고 **Purge**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.jpeg)

9.  **Yes**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

**요약**

이 랩에서는 Blazor, PostgreSQL 및 Azure OpenAI를 사용하여 사용자 지정
채팅 애플리케이션을 구축, 배포 및 테스트하는 방법에 대한 포괄적인
가이드를 제공했습니다. 이 랩에서는 필요한 개발 환경을 설정하고, Blazor
기반 채팅 인터페이스를 만들고 설계하고, Azure에서 PostgreSQL
데이터베이스를 구성 및 연결하고, 향상된 기능을 위해 Azure OpenAI를
통합하고, 마지막으로 Azure에서 애플리케이션을 배포 및 테스트하는 방법을
배웠습니다. 이 실습 경험을 통해 최첨단 기술과 클라우드 서비스를 사용하여
최신 웹 애플리케이션을 개발하고 관리할 수 있는 기술을 갖추게 되었습니다.
