# 사용 사례 10 : 사용자의 질문에 답변하고 대화 전반의 채팅 기록을 추적하기 위해 채팅 애플리케이션을 배포하기

**목표:**

이 사용 사례에서는 기존 Blazor 애플리케이션을 NoSQL용 Azure Cosmos DB
계정 및 Azure OpenAI 계정에 연결하는 단계를 안내합니다. 애플리케이션은
Azure OpenAI의 모델에 프롬프트를 보내고 응답을 구문 분석합니다. 또한
애플리케이션은 다양한 대화 세션과 해당 메시지를 NoSQL용 Azure Cosmos DB
내의 단일 컨테이너에 배치된 항목으로 저장합니다.

즉, 애플리케이션은 다음을 수행합니다:

- **.**NET SDK를 사용하여 Azure OpenAI의 모델에 **연결**

- 모델에 프롬프트를 보내고 완료 응답을 구문 **분석**

- .NET SDK를 사용하여 NoSQL용 Azure Cosmos DB에 **연결**

- 개별 작업, 쿼리 및 트랜잭션 일괄 처리로 항목 **관리**

이 샘플 채팅 애플리케이션은 사용자의 질문에 답변하고 대화 전반의 채팅
기록을 추적합니다.

![](./media/image1.jpeg)

**사용된 핵심 기술** --, Csharp, nosql ,asp-net,blazor,azure-cosmos-db,

**예상 소요 시간** -- 45분

**실습 유형:** 강사 진행

**사전 요구 사항:**

GitHub 계정 -- 고유한 GitHub 로그인 자격 증명이 있어야 합니다. 가지고
있지 않은 경우 여기에서 생성하세요.
-\`\` **https://github.com/signup?user_email=&source=form-home-signupobjectives\`\`**

### 작업 1: Docker를 실행하기

1.  Windows 검색 상자에서 **Docker**를 입력하고 **Docker Desktop**를
    클릭하세요.

![](./media/image2.jpeg)

### 작업 2: Service 공급자를 등록하기

1.  브라우저를 열고 <https://portal.azure.com>로 이동하여 VM의
    **Resource** 탭에서 사용할 수 있는Azure 자격 증명으로 로그인하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

2.  Azure 포털의 홈페이지에서 **Resource groups** 타일을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

3.  이 리소스 그룹에 필요한 리소스를 배포하기 위해 리소스 그룹 이름을
    복사하고 메모장에 저장하여 다음 작업을 사용하세요.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

4.  홈페이지로 이동하고 **Subscription** 타일을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

5.  구독 이름을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

6.  왼쪽 탐색 메뉴에서 **Settings - \> Resource provider**를 클릭하세요.

![](./media/image8.png)

7.  \`\`**Microsoft.AlertsManagement**\`\`를 입력하고enter를 입력하세요.
    그것을 선택하고**Register**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

![A screenshot of a computer Description automatically
generated](./media/image10.png)

### 작업 3: Azure에 서비스 및 애플리케이션을 프로비저닝하기

1.  브라우저를 열고 \`\`https:\\github.com\`\` 로 이동하고 Github
    계정으로 로그인하세요.아래 repo를 검색하세요

![](./media/image11.jpeg)

2.  아래 repo를 검색하고 **Fork**를 클릭하세요.

> \`\`https://github.com/technofocus-pte/chat-csharp-cosmos-db-nosql-openai\`\`

![](./media/image12.jpeg)

3.  리포지토리 이름을 입력한 후 **Create repository**를 클릭하세요.

![](./media/image13.jpeg)

4.  **Code -\> Code space -\> Open Code space**를 클릭하세요**.**

![](./media/image14.jpeg)

5.  Dev 컨테이너가 설정될 때까지 기다리세요. 3-5분 정도 걸립니다.

![](./media/image15.jpeg)

6.  아래 명령어를 실행하여 AZD에 로그인하세요. 생성된 코드를 복사하고
    Enter 키를 누르세요. 

> \`\`**azd auth login\`\`**

![](./media/image16.jpeg)

7.  생성된 코드를 붙여넣고 Azure 자격 증명으로 로그인하세요.

![](./media/image17.jpeg)

![](./media/image18.jpeg)

8.  현재 디렉토리에서 프로젝트를 초기화하기 위해 아래 명령을 실행하세요.
    Environment 이름을 \`\`**cosmoschatapp\`\`**로 입력하고Enter를
    클릭하세요.

\`\`azd init \`\`

![A screenshot of a computer Description automatically
generated](./media/image19.png)

9.  Azure에 서비스를 배포하고 컨테이너를 빌드하기 위해 다음 명령을
    실행하세요. 다음 값을 선택하세요.

> \`\`azd provision\`\`
>
> **Select an Azure Subscription to use**: 구독을 선텍하세요
>
> **Select an Azure location to use** : **East us/west us** (경우 따라
> East US를 사용할 수 없는 경우 다른 위치를 선택하고 배포합니다.)
>
> **Enter a value for the 'existingResourceGroupName' infrastructure
> parameter:** **ResourceGroup1**

![](./media/image20.png)

![A screenshot of a computer Description automatically
generated](./media/image21.png)

10. 리소스가 완전히 프로비저닝될 때까지 기다리세요. 이 프로세스는 필요한
    모든 리소스를 만드는 데 5-10분이 걸립니다.

![](./media/image22.png)

### 작업 4: Azure로 애플리케이션을 배포하기

1.  Azure 포털로 다시 이동하고 홈페이지에서 리소스 그룹 타일을
    클릭하세요.

![](./media/image23.png)

2.  리소스 그룹 이름을 클릭하세요.

![](./media/image24.png)

3.  아래 리소스가 표시되어야 합니다

- **Container**

- **Container Registry**

- **Azure Cosmos Db account**

- **AureOpenAI**

![A screenshot of a computer Description automatically
generated](./media/image25.png)

4.  **Container registry** 이름을 클릭하세요.

![](./media/image26.png)

5.  왼쪽 탐색 메누에서 **Setting**를 확장하고 **Access keys**를
    클릭하세요**. Admin user check box**를 클릭하세요**.** 앱을 배포하는
    데 사용하기 위해**Login server**, **user name** 및 **password**를
    메모장에 복사하세요.

![](./media/image27.png)

6.  탭을 복제하여 새 탭에서 Azure 포털을 여세요.

![](./media/image28.png)

7.  위쪽 탐색 메뉴에서 리소스 그룹 이름을 클릭하세요.

![](./media/image29.png)

8.  Container App 이름을 클릭하세요.

![](./media/image30.png)

9.  GitHub 계정으로 인증하기 위해 Github-Sign in에서 **Authorize**
    버튼을 클릭하세요. Github 계정 권한 부여하세요.

10. 다음 값을 선택하세요

> **Organization : your Github organization**
>
> **Repository:** chat-csharp-cosmos-db-nosql-openai
>
> **Branch :** main

![](./media/image31.png)

11. **Registry settings**까지 아래로 스크롤하고 아래 값을 입력한 후
    **Start continuous deployment** 버튼을 클릭하세요.

- Repository source: **Docker Hub 또는 다른 레지스트리.**

- Login server URL: 로그인 서버가 컨테이너 레지스트리에서 복사되었습니다
  (#5단계).

- Username : Container Registry의 비밀번호 (#5단계)

- Password : Container Registry의 비밀번호 (#5단계)

![](./media/image32.png)

12. 워크플로 파일 링크를 클릭하세요. Github과 함께 새 탭이 열립니다.

![](./media/image33.png)

13. **Actions** 탭을 클릭하세요.

![](./media/image34.png)

14. 배포가 완료될 때까지 기다리세요.

![](./media/image35.png)

15. 탭을 닫지 마세요.

### 작업 5: 채팅 앱에 액세스하기

1.  Azure Portal로 다시 전환하고 왼쪽 탐색 영역에서 **Overview**를
    클릭한 다음 **Application URL**을 클릭하세요. 새로운 로드 앱이
    열립니다.

![](./media/image36.png)

2.  **Create New Chat** 버튼을 클릭하세요.

![](./media/image37.png)

3.  다음 프롬프트를 입력하세요.

\`\`What is the seating capacity for Lumen in Seattle?\`\`

![](./media/image38.jpeg)

4.  다음 프롬프트를 입력하세요. 다양한 프롬프트 앱 탐색하세요.

\`\`is that bigger than Dogger stadium??\`\`

![](./media/image39.jpeg)

### 작업 6: 리소스를 정리하기

이 샘플에서 생성한 모든 리소스를 정라하기:

1.  Github 포털 탭으로 다시 전환하고 페이지를 새로 고치세요.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

2.  Code를 클릭하고 이 실습에 대해 생성된 브랜치를 선택하고**Delete**를
    클릭하세요.

![](./media/image41.png)

3.  **Delete** 버튼을 클릭하여 브랜치 삭제를 확인합니다.

![A screenshot of a computer Description automatically
generated](./media/image42.png)

5.  **Azure portal -\> Resource group- \> Resource group name**로 다시
    전환하세요**.**

![](./media/image43.png)

6.  모든 리소스를 선택한 후 아래 이미지와 같이 Delete를 클릭하세요.
    (리소스 그룹을 **DO NOT DELETE**)

![](./media/image44.png)

7.  텍스트 상자에서 \`\`**delete**\`\` 를 입력하고**Delete**를
    클릭하세요.

> ![](./media/image45.png)

8.  **Delete**를 클릭하여 삭제를 확인합니다.

![](./media/image46.png)

**요약:**

NuGet에서 Microsoft.Azure.Cosmos 및 Azure.AI.OpenAI 패키지를 사용하여
서비스 클래스를 구현했습니다. 컨텍스트 접두사와 함께 Azure OpenAI 대화형
인터페이스에 프롬프트를 보내고 응답의 usage 및 body 속성을 구문
분석했습니다. 또한 NoSQL용 Azure Cosmos DB를 사용하여 대화 세션 및
메시지를 단일 컨테이너 내에 저장했습니다.
