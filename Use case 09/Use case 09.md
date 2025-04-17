# 사용 사례 09 - MongoDB용 Azure Cosmos DB 및 Azure OpenAI Service를 사용하여 Chat bot 경험을 구축하기 

**목표:**

이 사용 사례는 vCore 기반MongoDB용 Azure Cosmos DB 벡터 검색 및 문서
검색을 Azure OpenAI 서비스와 결합하여 챗봇 경험을 구축하는 지능형
솔루션을 생성할 것입니다.

![A diagram of a software application AI-generated content may be
incorrect.](./media/image1.jpeg)

**사용된 핵심 기술** -- Azure OpenAI Service, Azure Cosmos DB, ChatGPT
모델

**사용된 소요 시간** -- 60분

**실습 유형** -- 강사 진행

**중요: PowerShell**에 **붙여넣지** 못한 명령이 있으면 메모장을 열고
커서를 메모장의 빈 공간에 유지한 후 붙여넣을 명령의 T 버튼을 클릭하세요.
내용이 메모장에 복사된 후 메모장에서 복사하여 PowerShell에 붙여넣을 수
있습니다.

## 연습 0: VM 및 자격 증명을 이해하기

이 작업에서는 실습 전체에서 사용할 자격 증명을 식별하고 이해할 것입니다.

3.  **Instructions** 탭에는 실습 전체에서 따라야 할 지침이 있는 실습
    가이드가 있습니다.

4.  **Resources** 탭에는 실습을 실행하는 데 필요한 자격 증명이 있습니다.

    1.  **URL** – Azure 포털에 대한 URL

    2.  **Subscription** – 사용자에게 할당돤 구독의 ID

    3.  **Username** – Azure 서비스에 로그인하는 데 사용하는 사용자 ID.

    4.  **Password** – Azure 로그인에 대한 비밀번호. 이 사용자 이름과
        비밀번호를 Azure 로그인 자격 증명이라고 하겠습니다. Azure 로그인
        자격 증명을 언급할 때마다 이러한 자격 증명을 사용할 것입니다.

    5.  **Resource Group** – 사용자에게 할당돤 **Resource group**.

\[!경고\] **중요:** 이 리소스 그룹 아래에 모든 리소스를 생성해야 합니다

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

1.  **Help** 탭에는 Support 정보가 있습니다. **ID** 값은 실습 중에
    사용되는**Lab instance ID**입니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

## 연습 1: Azure 리소스 프로비전하기

### 작업 1: 스크립트를 사용하여 Azure 리소스를 생성하기

2.  +++\*\* 에서 Azure 포털로 로그인하여 **Resources** 탭에소 Azure
    로그인 자격 증명으로 로그인하세요.

3.  Azure 포털에서 구독을 선택하세요. 왼쪽 창에서 Settings의 Resource
    providers를 선택하고 +++**Microsoft.Alertsmanagement**+++를 선택하여
    **Register**를 클릭하세요.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image4.jpeg)

4.  VM에서 +++**power shell**+++를 검색하고**Windows PowerShell**을
    마우스 오른쪽 버튼으로 클릭하고 **Run as administrator**을
    선택하세요. 확인 대화 상자에서 **Yes**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image6.jpeg)

5.  아래 명령을 실행하여 PowerShell에 Az를 설치하세요.

+++**Install-Module Az**+++

메시지가 표시되면 **A** (모두 Yes)를 선택하세요.

**참고:** 완료하는 데 최대 5분이 소요됩니다.

![A computer screen with white text AI-generated content may be
incorrect.](./media/image7.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

6.  Az 모듈을 가져오기 위해 완료되면 아래 명령을 실행하세요.

+++**Import-Module Az**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

7.  아래 명령을 실행하여 브라우저 기반 로그인을 사용하세요

+++Update-AzConfig -EnableLoginByWam $false+++

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image10.jpeg)

8.  아래 명령을 실행하고 메시지가 표시되면 Azure 로그인을 선택하여
    Azure에 로그인하세요.

+++Connect-AzAccount+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

9.  아래 명령을 실행하여 **LabFiles** 폴더로 이동하세요.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![A blue rectangular sign with white text AI-generated content may be
incorrect.](./media/image12.jpeg)

10. **winget**을 사용하여 **Microsoft Bicep**을 설치하려면 아래 명령을
    실행하세요.

+++winget install -e --id Microsoft.Bicep+++

Type **Y** if prompted.

![A computer screen with white text AI-generated content may be
incorrect.](./media/image13.jpeg)

11. PowerShell을 **닫았다**가 다시 **여세요.**

12. 아래 명령을 실행하고 메시지가 표시되면 Azure 로그인을 선택하여
    Azure에 로그인하세요.

+++Connect-AzAccount+++

12. 아래 명령을 실행하여 **LabFiles** 폴더로 이동하세요.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![A blue rectangular sign with white text AI-generated content may be
incorrect.](./media/image12.jpeg)

13. 아래 명령을 실행하여 구독 ID를 설정하세요.

+++Set-AzContext -SubscriptionId @lab.CloudSubscription.Id+++

![A computer screen shot of a blue screen AI-generated content may be
incorrect.](./media/image14.jpeg)

14. **C:\LabFiles\Build a Chat bot\Labs\deploy** 경로에서
    **azuredeploy.bicep** 파일을 열고 35줄의
    **dgxxxxxxx** 문자를 <+++dg@lab.LabInstance.Id>+++로 바꾸세요.
    **74**행에서 버전을 +++**0125**+++로 업데이트하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

![](./media/image16.png)

15. Azure Cosmos DB 작업 영역, Azure의 Azure OpenAI와 같은 리소스를
    배포하기 위해 아래 명령을 실행하세요.

New-AzResourceGroupDeployment -ResourceGroupName
@lab.CloudResourceGroup(ResourceGroup1).Name -TemplateFile
.\azuredeploy.bicep -TemplateParameterFile .\azuredeploy.parameters.json
-c \`\`\`

\>\[!참고\] \*\*참고:\*\* 배포에는 약 10-15분이 소요됩니다.

배포에 문제가 있고 실패한 경우 14단계의 이름을 다른 이름으로
업데이트하고 다시 시도하세요.

\>\[!참고\] \*\*참고:\*\* 메시지가 표시되면 Y를 입력하세요.

![A computer screen shot of a blue screen AI-generated content may be
incorrect.](./media/image17.jpeg)

![](./media/image18.jpeg)

\>\[!참고\] \*\*참고:\*\* 15분에서 20분 후에도 PowerShell에 업데이트가
없으면 Azure Portal의 리소스 그룹 -\> 배포를 확인하거나
\*\*PowerShell\*\* 창에서 \*\*Enter\*\*를 누르세요.

### 작업 2: Azure에서 생성된 리소스를 확인하기

1.  **Azure login credentials**을 사용하여
    +++<https://portal.azure.com/+++>에서 **Azure portal** 에
    로그인하세요. **Resource groups**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

2.  Resource groups 목록에서 **assigned Resource Group**를 선택하세요.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image20.png)

3.  **Azure OpenAI resource, App Service, Azure Cosmos DB for
    MongoDB**를 포함한 리소스 집합이 생성됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

4.  **Azure OpenAI** 리소스를 클릭하세요.

![](./media/image22.png)

5.  **Resource Management**에서 **Keys and Endpoint**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.jpeg)

6.  나중에 참조하기 위해**Key 1** 및 **Endpoint**를 복사하여 메모장에
    저장하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

7.  리소스 그룹 페이지로 돌아가서 **Azure Cosmos DB for Mongo
    DB** 리소스를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

8.  **Settings**에서 **Connection strings**를 클릭하세요. Self의 값을
    복사하세요 (항상 이 클러스터.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.jpeg)

9.  연결 문자열을 복사하여 메모장에 붙여넣으세요. 복사된 연결 문자열에서
    \< **password** \> +++**myMongoDB98**+++로 바꾸고 메모장에
    저장하세요.

## 연습 2: 코드에서 Azure OpenAI 모델을 탐색하고 사용하기

### 작업 1: 환경을 설정하기

1.  실습 VM 창 검색 창에서 +++Visual Studio code+++를 검색하고 **Visual
    Studio Code**를 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

2.  **Open Folder**를 클릭하세요. (팝업되지 않는 경우 **File -\> Open
    Folder**를 선택하세요)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

3.  **C:\Labfiles**로 이동하여 **Build a Chat bot** 폴더를 클릭하고
    **Select Folder**를 선택하세요.

![A screenshot of a chat bot AI-generated content may be
incorrect.](./media/image29.jpeg)

4.  팝업에서 **Yes, I trust the Authors**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

5.  Visual Studio Code에, **Labs** 폴더에서
    **lab_0_explore_and_use_models.ipynb**를 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

6.  **Select Kernel**를 클릭하세요.

7.  **Do you want to install the recommended extensions for
    Python** 팝업에서**Install**를 선택하세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image32.jpeg)

8.  프롬프트되면**Allow access**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.jpeg)

9.  **Select Kernel**을 클릭하세요. **Python Environments**을 선택한 후
    **Suggested** or **Recommended** 옵션으로 나열되는 **Python
    3.12.3**이상을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.jpeg)

10. **.env** 파일을 여세요

11. 연습 1의 작업 2에서 **notepad**을 저장한 **DB_CONNECTION
    STRING**, **AOAI_KEY** 및 **AOAI_Endpoint**를 바꾸세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

이제 환경 변수가 이미 만든 Azure 리소스를 가리키도록 설정되었습니다.

### 작업 2: 코드를 실행하기

1.  **Lab 0 ipynb** 파일로 돌아가서 Play 버튼을 클릭하여 **first
    cell**을 **execute**하여 최신 OpenAI 클라이언트 라이브러리를
    설치하세요.

![A black screen with a black background AI-generated content may be
incorrect.](./media/image37.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image38.jpeg)

2.  다음 셀을 **Execute**하여 **Python-dotenv**를 설치하세요

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image39.jpeg)

3.  Press **Ctrl+Shift+P**를 누르고 +++Reload Window+++를 입력한 후
    나열된 Developer:Reload Window 옵션을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

4.  **First cell**에서 다시 실행하세요.

5.  다음 셀을 **Execute**하여 필요한 OpenAI 라이브러리를 가져오고, os를
    실행하여 환경 변수에 액세스하고, dotenv를 실행하여 .env 파일에서
    환경 변수를 로드하세요.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image41.jpeg)

6.  다음 셀을 **Execute**하여 Azure OpenAI 채팅 완료 API를 호출하는
    **Azure OpenAI client**를 생성하세요:

![A computer screen with text AI-generated content may be
incorrect.](./media/image42.jpeg)

7.  다음 셀을 **Execute**하여 클라이언트에서
    **.chat.completions.create()** 메소드를 호출하여 **chat
    completion**를 수행하세요. 채팅 응답을 받아야 합니다.

![A computer screen with text on it AI-generated content may be
incorrect.](./media/image43.jpeg)

## 연습 3: 첫 번째 MongoDB API용 Cosmos DB 애플리케이션

이 연습에서는 첫 번째 Cosmos DB 프로젝트를 생성하는 방법을 설명합니다.
Notebook을 사용하여 기본 CRUD 작업을 시연할 것입니다.

1.  **Labs** 폴더에서 **lab_1_first_application.ipynb** 파일을 여세요.

2.  **Select kernel**을 클릭하여**Python version**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

3.  **pymongo**를 설치하기 위해first cell을 **Execute**하세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

4.  다음 셀을 **Execute**하여 필요한 **imports**를 수행하세요

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image46.jpeg)

5.  **Create a database**를 위해 다음 셀을 실행하세요

\[!참고\] **참고:** 그러면 .env 파일에서 업데이트한 연결 문자열이
사용됩니다

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image47.jpeg)

6.  **Collection** 를 생성하기 위해 다음 셀을 **Execute** 하세요.

![A black screen with white text AI-generated content may be
incorrect.](./media/image48.jpeg)

7.  **Document**를 생성하기 위해 다음 셀을 **Execute** 하세요. 문서를
    생상하는 한 가지 방법은 insert_one 방법을 사용하는 것입니다. 이
    방법은 단일 문서를 가져 와서 데이터베이스에 삽입합니다.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image49.jpeg)

8.  데이터베이스에서 **retrieve a single document**하기 위해 다음 셀을
    **Execute**하세요. 이를 위해 **find_one** 방법이 여기에 사용됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.jpeg)

9.  **find_one_and_update** 메소드가 데이터베이스의 단일 문서를
    업데이트하는 데 사용되는 다음 셀을 **Execute**하세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

10. **delete_one** 메소드가 데이터베이스에서 단일 문서를 삭제하는 데
    사용되는 다음 셀을 **Execute**하세요.

![](./media/image52.jpeg)

11. **find** 메소드는 데이터베이스의 여러 문서를 쿼리하는 데
    사용됩니다.  **next 3 cells**의 셀을 하나씩 **Execute**하여 작동
    상태를 확인하세요.

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image53.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image55.jpeg)

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image56.jpeg)

12. 다음 셀은 이 연습에서 만든 데이터베이스와 컬렉션을 **delete**합니다.
    이 작업은 데이터베이스 개체에서 **drop_database** 메서드를 사용하여
    수행됩니다.

![A computer screen with text AI-generated content may be
incorrect.](./media/image57.jpeg)

## 연습 4: MongoDB API를 사용하여 Cosmos DB에 데이터를 로드하기

이전 연습에서는 컬렉션에 데이터를 개별적으로 추가하는 방법을
설명했습니다. 이 연습에서는 대량 작업을 사용하여 데이터를 여러 컬렉션에
로드하는 방법을 보여 줍니다. 이 데이터는 AI에 대한MongoDB용 Azure Cosmos
DB API의 기능을 자세히 설명하기 위해 후속 실습에서 사용됩니다.

이 Notebook은 MongoDB API를 사용하여 Cosmic Works JSON 파일에서
데이터베이스로 데이터를 Cosmos DB로 로드하는 방법을 보여 줍니다.

1.  **Labs** 폴더에서 **lab_2_load_data.ipynb**를 여세요. **Select
    Kernel**을 클릭하여**Python version**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

2.  첫 번째 셀을 실행하여 **requests**을 설치하세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

3.  필요한 **imports**를 수행하기 위해 다음 셀을 **Execute**하세요.

![A computer screen with green text AI-generated content may be
incorrect.](./media/image60.jpeg)

4.  **Database**와의 **connection**을 설정하는 다음 셀을
    **Execute**하세요.

![A computer screen with text AI-generated content may be
incorrect.](./media/image61.jpeg)

![A computer screen shot of text AI-generated content may be
incorrect.](./media/image62.jpeg)

5.  **Products**을 **load**하기 위해 **Execute**하세요.

![A screen shot of a computer screen AI-generated content may be
incorrect.](./media/image63.jpeg)

6.  **customers** 및 **sales** **raw data**를 **load**하기 위해 다음
    셀을 **Execute**하세요. 이 저장소에서 고객 및 판매 데이터는 동일한
    파일에 저장됩니다. type 필드는 두 가지 유형의 문서를 구별하는 데
    사용됩니다.

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image64.jpeg)

![](./media/image65.jpeg)

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image66.jpeg)

7.  **clean up**할 다음 셀을 **Execute**하세요.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image67.jpeg)

## 연습 5: vCore 기반 MongoDB용 Azure Cosmos DB를 사용한 벡터를 검색하기

1.  **Labs** 폴더에서**lab_3_mongodb_vector_search.ipynb**를 파일을
    여세요.

2.  **Select Kernel**을 클릭하고**Python version**을 선택하세요.

![](./media/image68.jpeg)

3.  **tenacity**를 설치하기 위해 첫 셀을 **Execute**하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image69.jpeg)

4.  필요한 **imports**를 수행하기 위해 다음 셀을 **Execute**하세요.

![A computer screen with text AI-generated content may be
incorrect.](./media/image70.jpeg)

5.  .env 파일에서 **settings** 을 **load** 하기 위해 다음 셀을
    **Execute**하세요.

![A computer screen with text AI-generated content may be
incorrect.](./media/image71.jpeg)

6.  **database**에 **connectivity** 수립하기 위해 다음 셀을 실행하세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image72.jpeg)

7.  **Azure OpenAI connectivity**를 수립하기 위해 다음 셀을
    **Execute**하세요.

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image73.jpeg)

8.  각 문서에 벡터 임베딩 필드를 생성하는 과정은 한 번만 수행하면
    됩니다. 그러나 문서가 변경되면 벡터 임베딩 필드를 업데이트된 벡터로
    업데이트해야 합니다. 다음 두 셀에서 수행됩니다. 두 번째 셀에서
    출력으로 얻은 **embeddings**을 관찰하기 위해 다음 두 셀을
    **Execute**하세요.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image74.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image75.jpeg)

9.  **Vectorize and update all documents in the Cosmic Works
    database**를 위해 다음 셀을 **Execute**하세요**.**

![A computer screen shot of a program code AI-generated content may be
incorrect.](./media/image76.jpeg)

10. **products, customer and sales documents**에 **vector fields**를
    추가하기 위해 다음 **3** 셀을 **Execute**하세요.

**참고:** 첫 번째 셀은 실행을 완료하는 데 약 5분, 두 번째 셀은 약 3분,
세 번째 셀은 약 20분이 소요됩니다.

\![\](./media/image77.jpeg)

11. **products vector index**를 생성하기 위해 다음 셀을
    **Execute**하세요.

![A screen shot of a computer screen AI-generated content may be
incorrect.](./media/image77.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image78.jpeg)

12. 이제 각 문서에 연결된 벡터 포함이 있고 각 컬렉션에서 벡터 인덱스가
    생성했으므로 이제 vCore 기반MongoDB용 Azure Cosmos DB의 벡터 검색
    기능을 사용할 수 있습니다. 다음 **3** 셀을 **Execute**하세요.

![](./media/image79.jpeg)

![](./media/image80.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image81.jpeg)

13. Chat GPT-3.5를 사용하여 RAG패턴에서 **vector search results**의
    사용을 관찰하기 위해 다음 셀을 **Execute**하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image82.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image83.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image84.jpeg)

14. 다음 셀의 출력을 관찰하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image85.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image86.jpeg)

## 연습 6: 배포된 리소스를 삭제하기

1.  Azure 포털
    (+++[https://portal.azure.com+++](https://portal.azure.com+++/))에서
    할당된 리소스 그룹을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image87.png)

2.  그 아래에 있는 모든 리소스를 선택하고 메뉴에서 **three dots**을
    클릭한 후 **delete**를 선택하여 모든 리소스를 삭제합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image88.png)

3.  텍스트 상자에 +++delete+++를 입력하고 Delete 버튼을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image89.png)

4.  리소스가 삭제되면 Azure Portal 홈페이지에서 **+++Azure AI
    Services+++**를 검색하여 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.jpeg)

5.  왼쪽 창에서 **Azure OpenAI**를 선택하고**Manage deleted
    resources**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image91.jpeg)

6.  거기에 나열되는 리소스를 선택하고 **Purge**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image92.jpeg)

7.  **Yes**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image93.jpeg)

**요약:**

Azure OpenAI 서비스를 사용하여MongoDB용 Azure Cosmos DB 벡터 검색 및
문서 검색을 사용하여 솔루션을 성공적으로 생성했습니다.
