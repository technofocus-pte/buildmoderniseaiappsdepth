# 사용 사례 12- Generative AI 기능을PostgreSQL Flexible Server용 Azure Database와 통합하여 지정된AI 목록에 대한 검토를 평가하기

**실습 기간 –** 40분

**실습 유형 –** 강사 진행

**소개**

이 실습에서는 Azure AI 서비스를 PostgreSQL과 통합하여 고급 AI 기능으로
데이터베이스를 향상시키는 방법을 알아볼 것입니다. pgvector 및 PostGIS와
같은 Azure OpenAI 및 PostgreSQL 확장의 기능을 활용하여 데이터베이스
내에서 직접 정교한 텍스트 분석, 벡터 유사성 검색 및 지리 공기적 쿼리를
사용하도록 설정할 수 있습니다. 이 실습에서는 필요한 Azure 리소스를
프로비저닝하고 데이터베이스를 구성하고 AI 기반 인사이트를 지리 공간적
데이터와 결합하는 복잡한 퀴리를 실행하는 방법을 안내합니다.

**목표**

- PostgreSQL Flexible Server용 Azure Database를 프로비저닝하고 구성하기.

- Azure OpenAI 서비스를 사용하여 벡터 임베딩을 생성하고 관리하기.

- 의미론적으로 유사한 텍스트 데이터를 찾기 위해 벡터 유사성 검색을
  수행하기.

- 지리 공간 데이터 분석을 위해 PostGIS 확장을 활용하기.

- 김정 분석 및 기타 인지 기능을 위해Azure AI Language 서비스를 통합하기.

- 인덱싱 및 퀴리 계획 도구를 사용하여 쿼리 성능을 최적화하고 분석하기.

**중요:** 명령 중 하나라도 **CloudShell**에 **붙여넣어**지지 않으면
메모장을 열고 커서를 메모장의 빈 공간에 유지한 후 붙여넣을 명령의 T
버튼을 클릭하세요. 내용이 메모장에 복사된 후 메모장에서 CloudShell로
복사하여 붙여넣을 수 있습니다.

## 연습 0: VM 및 작격 증명을 이해하기

이 작업에서는 실습 전체에서 사용할 자격 증명을 식별하고 이해할 것입니다.

1.  **Instructions** 탭에는 실습 전반에 걸쳐 따라야 할 지침이 포함된
    실습 가이드가 있습니다.

2.  **Resources** 탭에는 실습을 실행하는 데 필요한 자격 증명이 있습니다.

    - **URL** – Azure 포털에 대한 URL

    - **Subscription** – 사용자에게 할당된 구독의 ID입니다

    - **Username** – Azure 서비스에 로그인하는 데 사용하는 사용자
      ID입니다.

    - **Password** – Azure 로그인에 대한 암호입니다. 이 사용자 이름과
      암호를 Azure 로그인 자격 증명입니다. Azure 로그인 자격 증명을
      언급할 때마다 이러한 자격 증명을 사용할 것입니다.

    - **Resource Group** – 사용자에게 할당된 **Resource Group**입니다.

\[!경고\] **중요:** 이 Resource 그룹 아래에 모든 리소스를 생성해야
합니다

![](./media/image1.png)

3.  **Help** 탭에는Support information가 있습니다. 여기서 **ID** 값은
    실습 실행 중에 시용되는 **Lab instance ID**입니다.

![](./media/image2.png)

## 연습 1: PostgreSQL Flexible Server용 Azure Database를 프로비저닝하기

### 작업 0: 리소스 공급자를 등록하기

1.  Azure 로그인 자격 증명을 사용하여**Azure portal** -
    +++https://portal.azure.com+++ 로 로그인하세요.

2.  **Subscriptions**를 클릭하고  왼쪽 창의 **Settings**에서**Resource
    Providers**를 선택하세요.

3.  +++**Microsoft.DBforPostgreSQL**+++ 를 검색하여Resource Provider를
    등록하기 위해**Register**를 클릭하세요.

![](./media/image3.png)

### 작업 1: PostgreSQL Flexible Server용 Azure Database를 프로비저닝하기

1.  웹 브라우저를 열고 +++https://portal.azure.com+++ 로 이동하세요

2.  Azure 포털 도구 모음에서 **Cloud Shell** 아이콘을 선택하여 브라우저
    창 맨 위에 새 Cloud Shell 창을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  CloudShell을 처음 열면 사용할 셀 유형
    (**Bash** 또는 **PowerShell**)을 선택하라는 메시지가 표시될 수
    있습니다. **Bash**를 선택하세요.

![](./media/image5.jpeg)

4.  **Getting started** 대화상자에서 **Mount storage account**를
    선택하여 Azure 구독을 선택하세요. **Apply** 버튼을 클릭하세요.

![](./media/image6.png)

5.  **Mount storage account** 대화상자에서 **we will create a storage
    account for you**를 선택하여**Next** 버튼을 클릭하세요.

![](./media/image7.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

6.  CloudShell 프롬프트에서 리소스를 생성하기 위해 변수를 정의하려면
    다음 명령을 실행하세요. 변수는 리소스 그룹 및 데이터베이스에 할당할
    이름을 나타내고 리소스를 배포해야 하는 Azure 지역을 지정합니다.

7.  아래의 명령어의Resource group Name을 할당된 Resource group으로
    바꾸고 명령어를 실행하세요.

+++RG_NAME= \< Resource group Name \>+++

![](./media/image9.png)

8.  데이터베이스 이름에서 replace the {SUFFIX} 토큰을**Lab instance ID**
    (예를 들어: 이니셜)로 바꿔 데이터베이스 서버 이름이 글로벌적으로
    고유한지 확인합니다.

+++DATABASE_NAME=<pgsql-flex-@lab.LabInstance.Id>+++

![](./media/image10.jpeg)

9.  Region 값을 설정하기 위해 다음 명령을 실행하세요.

+++REGION=@lab.CloudResourceGroup(ResourceGroup1).Location+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

10. 다음 Azure CLI 명령을 실행하여 할당된 리소스 그룹 내에서PostgreSQL
    데이테베이스용 Azure Database 인스턴스를 프로비저닝하세요 (이 명령을
    완료하는 데 10분이 걸립니다)

> \`\`\`
>
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

### 작업 2: Azure Cloud Shell에서 psql을 사용하여 데이터베이스에 연결하기

이 작업에서는, Azure Cloud Shell의 psql 명령줄 유틸리티를 사용하여
데이터베이스에 연결할 것입니다.

1.  브라우저를 열고 Azure 구독 계정으로 +++https://portal.azure.com+++에
    로그인하세요.

2.  **Home** 페이지에서 **Resource Groups**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.jpeg)

3.  **Your assigned resource group** 이름을 클릭하세요

![](./media/image14.png)

4.  Resource group에서**PostgreSQL Flexible Server** 리소스를 선택하세요

![](./media/image15.png)

5.  왼쪽 탐색 메뉴에서**Settings**에서 **Connect**를 선택하세요.

![](./media/image16.jpeg)

6.  Azure 포털의 데이터베이스의 **Connect** 페이지에서**Database
    name**에**airbnb**를 선택하고 향후 작업에서 정보를 사용하기 위해
    **Connection details** 볼록을 복사하여 메모장에 붙여넣으세요.

![](./media/image17.jpeg)

7.  PostgresSQL용 Azure Database 홈 페이지에서 왼쪽 탐색 메뉴에서
    **Overview**를 클릭하여 서버 이름을 복사하여 메모장에 붙여넣고
    예정된 실습에서 정보를 사용하기 위해 메모장을 차례로 **Save**하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.jpeg)

8.  PostgreSQL용 Azure Database 홈 페이지에서 설정에서**Networking**을
    선택하고 **Allow public access from any Azure service within Azure
    to this server**를 선택하세요. **Save** 버튼을 클릭하세요.

![](./media/image19.jpeg)

![](./media/image20.jpeg)

9.  브라우저 창 상단에 있는 새 Cloud Shell 창을 열기 위해 **Cloud
    Shell** 아이콘을 선택하세요.

10. Cloud Shell에서 Paste **Connection details**을 붙여넣으세요.

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image21.jpeg)

11. Cloud Shell 프롬프트에서 **{your_password}** 토큰을 데이터베이스를
    생성할 때 **s2admin** 사용자에게 할당한 비밀번호로 바꾸면 비밀번호는
    +++**Seattle123Seattle123**+++이어야 합니다.

![](./media/image22.jpeg)

12. 프롬프트에 다음을 입력하여 psql 명령줄 유틸리티를 사용하여
    데이터베이스에 연결하세요:

+++psql+++

![](./media/image23.jpeg)

Cloud Shell에서 데이터베이스에 연결하려면 데이터베이스의 **Networking** 
페이지에서 Azure 내의 모든 Azure 서비스에서 서버로 공용 액세스 허용
상자를 선택해야 합니다 . 연결할 수 없다는 메시지가 표시되면 이 옵션이
선택되어 있는지 확인하고 다시 시도하세요..

### 작업 3: 데이터베이스에 데이터를 추가하기

psql 명령 프롬프트를 사용하여 테이블을 생성하고 실습에서 사용할 데이터로
채웁니다.

1.  다음 명령을 실행하여 공용 Blob Storage 계정에서 JSON 데이터를
    가져오기 위한 임시 테이블을 생성하세요.

> CREATE TABLE temp_calendar (data jsonb);
>
> CREATE TABLE temp_listings (data jsonb);
>
> CREATE TABLE temp_reviews (data jsonb);

![](./media/image24.jpeg)

2.  COPY 명령을 사용하여 각 임시 테이블을 공용 스토리지 계정에 있는 JSON
    파일의 데이터로 채웁니다.

+++\COPY temp_calendar (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/calendar.json'+++>

+++\COPY temp_listings (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/listings.json'+++>

+++\COPY temp_reviews (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/reviews.json'+++>

![](./media/image25.jpeg)

![](./media/image26.jpeg)

3.  다음 명령을 실행하여 이 실습에서 사용하는 셰이프에 데이터를 저장하기
    위한 테이블을 생성하세요:

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

4.  마지막으로, 다음 **INSERT INTO** 문을 실행하여 임시 테이블에서 기본
    테이블로 데이터를 로드하고 JSON 데이터 필드의 데이터를 개별 열로
    추출하세요:

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

## 연습 2: Allowlist에 Azure AI 및 Vector 확장을 추가하기

이 실습에서azure_ai 및 pgvector 확장을 사용하여PostgreSQL
데이토베이스에generative AI 기능을 추가합니다. 이 연습에서는 PostgreSQL
확장을 사용하는 방법에 설명된 대로 이러한 확장을 서버의 *Allowlist*에
추가합니다.

1.  Home 페이지에서 **Resource Groups**를 클릭하세요.

![](./media/image30.jpeg)

2.  Resource group 이름을 클릭하세요

![](./media/image14.png)

3.  Resource group에서**PostgreSQL Flexible Server** 리소스를 선택하세요

![](./media/image15.png)

4.  데이터베이스의 왼쪽 탐색 메뉴에서 **Settings** 아래의 **Server
    parameters** 선택한 후 검색 상자에
    +++**azure.extensions**+++입럭하세요. Expand the **VALUE** 드롭다운
    목록을 확장한 후 다음 각 확장을 옆에 있는 상사를 찾아 선택하세요:

    - AZURE_AI

    - POSTGIS

    - VECTOR

![](./media/image31.jpeg)

![](./media/image32.jpeg)

![](./media/image33.jpeg)

5.  도구 모음에서 **Save**를 선택허면 데이터베이스에서 배포가
    트리거됩니다.

![](./media/image34.jpeg)

## 연습 3: Azure OpenAI 리소스를 생성하기

The azure_ai 확장을 사용하려면 벡터 임베딩을 생성하기 위해 기본Azure
OpenAI 서비스가 필요합니다. 이 연습에서는 Azure 포털에서 Azure OpenAI
리소스를 프로비저닝하고 해당 서비스에 포함 모델을 배포할 것입니다.

### 작업 1: Azure OpenAI 서비스를 프로비저닝하기

이 작업에소는 새로운 Azure OpenAI 서비스를 생성할 것입니다.

1.  아래 이미지와 같이 Azure 포털 홈페이지에서Microsoft Azure 명령
    모음의 왼쪽에 있는 세 개의 가로 막대로 표시된 **Azure portal
    menu**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.jpeg)

2.  **+ Create a resource**로 이동하고 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

3.  **Create a resource** 페이지의 **Search services and
    marketplace** 검색 바에서 +++**Azure OpenAI**+++를 입력하여
    **Enter** 버튼을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

4.  아래 이미지와 같이 **Marketplace** 페이지에서 **Azure
    OpenAI** 섹션으로 이동하고 Create 버튼 드롭다운을 클릭하고 **Azure
    OpenAI**를 선택하세요. (**Azure** **OpenAI** 타일을 이미
    클릭했으면**Azure OpenAI page**에서 **Create** 버튼을 클릭하세요).

![A screenshot of a software page AI-generated content may be
incorrect.](./media/image38.png)

5.  Create Azure OpenAI **Basics** 탭에서 다음 정보를 입력하고 **Next**
    버튼을 클릭하세요.

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image39.png)

6.  **Network**  탭에서 모든 라디오 버튼을 기본 상태로 두고
    **Next** 버튼을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

7.  **Tags** 탭에서 모든 라디오 버튼을 기본 상태로 두고 **Next** 버튼을
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

8.  **Review + submit** 탭에 Validation 이 Passed되면 **Create** 버튼을
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

9.  배포가 완료될 때까지 기다리세요. 배포에는 약 2-3분이 소요됩니다.

\[!참고\] **참고:** 현재 애플리케이션 양식을 통해 고객이 Azure OpenAI
Service를 사용할 수 있다는 메시지가 표시되는 경우 선택한 구독이 서비스에
대해 활성회되지 않았으며 가격 책정 계층에 대한 할당량이 없습니다; Azure
OpenAI 서비스에 대한 액세스를 요청하려면 링크를 클릭하고 요청 양식을
작성해야 합니다.

### 작업 2: Azure OpenAI 서비스의 키 및 엔드포인트를 검색하기

1.  리소스의 **Overview** 페이지에서 **Go to resource** 버튼을
    선택하세요. 프롬프트되면 실습 자격 증명을 선택하세요:

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.jpeg)

2.  **Azure OpenAI home** 창에서 **Resource Management** 섹션으로
    이동하여 **Keys and Endpoints**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

3.  아래 이미지와 같이 **Keys and Endpoints** 페이지에서 **KEY1, KEY
    2,** 및 **Endpoint** 값을 복사하여 메모장에 붙여 넣은 다믕 메모장을
    **save**하여 작업에 정보를 사용하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.jpeg)

**참고:** KEY1 또는 KEY2를 사용할 수 있습니다. 항상 두 개의 키가 있으면
서비스 중단 없이 키를 안전하게 회전하고 다시 생성할 수 있습니다.

### 작업 3: 임베딩 모델 배포

azure_ai 확장을 사용하면 텍스트에서 벡터 임베딩을 생성할 수 있습니다.
이러한 임베딩을 생성하려면 Azure OpenAI 서비스 내에 배포된
text-embedding-ada-002 (버전 2) 모델이 필요합니다. 이 작업에서는Azure
OpenAI Studio를 사용하여 사용할 수 있는 모델 배포를 생성할 것입니다.

1.  아래 이미지와 같이 **Azure OpenAI** 페이지에서 왼쪽 탐색 메뉴에서
    **Overview**를 클릭하여 아래로 스크롤하여**Go to Azure OpenAI
    Studio** 버튼을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

2.  **Azure AI Foundry | Azure Open AI Service** 홈페이지에서
    **Components** 섹션으로 이동하여**Deployments**를 클릭하세요.

3.  **Deployments** 창에서 **+Deploy model**를 드롭다운하고 **Deploy
    base model**를 선택하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

4.  **Select a model** 대화 상자에서 **text-embedding-ada-002**를
    탐색하고 신중하게 선택한 후 **Confirm** 버튼을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

5.  **Deploy model** 대화상자에서 다음을 설장하고 모델을 배포하기 위해
    **Create**를 선택하세요.

    - **Select a model**: 목록에서 **text-embedding-ada-002**를
      선택하세요.

    - **Model version**: **2 (Default)**가 선택했는지 확인하세요.

    - **Deployment name**: +++**embeddings**+++를 입력하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

6.  **Deployments** 창에서 copy **Deployment name**을 복사하여 메모장에
    붙여넣고 (이미지에 표시된 대로) 메모장을 **save**하여 향후 작업에서
    정보를 사용하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

## 연습 4: azure_ai 확장을 설치하고 구성하기

이 연습에서는 데이터베이스에 azure_ai 확장을 설치하고 Azure OpenAI
서비스에 연결하도록 구성할 것입니다.

### 작업 1: Azure Cloud Shell에서 psql을 사용하여 데이터베이스에 연결하기

이 작업에서는 Azure Cloud Shell의 psql 명령줄 유틸리치를 사용하여
데이터베이스에 연결할 것입니다.

1.  Azure 포털 도구 모음에서 **Cloud Shell** 아이콘을 선택하여 브라우저
    창 맨 위에 새 Cloud Shell 창을 여세요.

2.  Cloud Shell에**Connection details**를 붙여 넣으세요.

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image21.jpeg)

3.  Cloud Shell 프롬프트에서 replace the **{your_password}** 토큰을
    데이터베이스를 생성할 때 **s2admin** 사용자에서 할당한 비밀번호로
    바꾸면 비밀번호는 **Seattle123Seattle123**이어야 합니다.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.jpeg)

4.  프롬프트에 다음을 입력하여 psql 명령줄 유틸리티를 사용하여
    데이터베이스에 연결하세요:

+++**psql**+++

![A black background with a black square AI-generated content may be
incorrect.](./media/image23.jpeg)

### 작업 2: azure_ai 확장을 설치하기

azure_ai 확장을 사용하면Azure OpenAI 및 Azure Cognitive Services를
데이터베이스에 통합할 수 있습니다. 데이터베이스에서 확장을 활성화하려면
아래 단계를 따르세요:

1.  psql 명령 프롬프트에서 다음을 실행하여 확장이 허용 목록에 성공적으로
    추가되었는지 확인하세요:

+++SHOW azure.extensions;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

2.  CREATE EXTENSION 명령을 사용하여 azure_ai 확장을 설치하세요.

+++CREATE EXTENSION IF NOT EXISTS azure_ai;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.jpeg)

### 작업 3: azure_ai 확장에 포함된 개체를 검토하기

azure_ai 확장 내의 개체를 검토하면 해당 기능을 더 잘 이해할 수 있습니다.
이 작업에서는 확장에 의해 데이터베이스에 추가된 다양한 스키마,
user-defined functions (UDFs) 및 복합 형식을 검사할 것입니다.

1.  **psql** 명령 프롬프트에서 \dx 메타 명령을 사용하여 확장에 포함된
    개체를 나열할 수 있습니다.

\[!참고\] **참고:** Cloud shell이 **More…**와 프롬프트할 때 아무 키를
클릭하세요

+++\dx+ azure_ai+++

![](./media/image56.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image57.jpeg)

Meta-command 출력은 azure_ai 확장이 데이터베이스에 세 개의 스키마, 여러
user-defined functions (UDFs) 및 여러 복합 형식을 생성하는 것을
보여줍니다. 아래 표에서는 확장에 의해 추가된 스키마를 나열하고 각각에
대해 설명합니다.

[TABLE]

2.  함수와 유형은 모두 스키마 중 하나와 연결됩니다. azure_ai 스키마에
    정의된 함수를 검토하려면 \df 메타 명령을 사용하여 함수를 표시해야
    하는 스키마를 지정하세요. \df 앞의 \x auto 명령을 사용하면 Azure
    Cloud Shell에서 명령의 출력을 더 쉽게 볼 수 있도록 필요할 때 확장된
    디스플레이를 자동으로 적용할 수 있습니다.

+++\x auto+++ +++\df+ azure_ai.\*+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

azure_ai.set_setting() 함수를 사용하면 Azure AI 서비스에 대한 엔드포인트
및 키 값을 설정할 수 있습니다. **Key**와 이를 할당할 **value 을**
허용합니다 . azure_ai.get_setting() 함수는 set_setting() 함수로 설정한
값을 검색하는 방법을 제공합니다. 보려는 설정의 **key**를 수락합니다. 두
방법 모두에서 키는 다음 중 하나여야 합니다:

### 작업 4: Azure OpenAI endpoint 및 key를 설정하기

azure_openai 함수를 사용하기 전에 Azure OpenAI 서비스 endpoint및 key에
대한 확장을 구성할 것입니다.

1.  아래 명령에서 **{endpoint}** 및 **{api-key}** 토큰을 Azure 포털에서
    검색한 값으로 바꾼 후 Cloud Shell 창의 psql 명령 프롬프트에서 명령을
    실행하여 구성 테이블에 값을 추가하세요.

2.  SELECT azure_ai.set_setting('azure_openai.endpoint','{endpoint}');

3.  SELECT azure_ai.set_setting('azure_openai.subscription_key',
    '{api-key}');

![A computer screen with white text AI-generated content may be
incorrect.](./media/image59.jpeg)

4.  다음 쿼리를 사용하여 구성 테이블에 기록된 설정을 확인하세요:

5.  SELECT azure_ai.get_setting('azure_openai.endpoint');

6.  SELECT azure_ai.get_setting('azure_openai.subscription_key');

이제 azure_ai 확장이Azure OpenAI 계정에 연결되있으며 벡터 임베딩을
생성할 준비가 되었습니다.

![A computer screen with white text AI-generated content may be
incorrect.](./media/image60.jpeg)

## 연습 5: Azure OpenAI로 벡터 임베딩을 생성하기

azure_ai 확장의 azure_openai 스키마를 사용하면 Azure OpenAI가 텍스트
값에 대한 벡터 포함을 생성할 수 있습니다. 이 스키마를 사용하면
데이터베이스에서 직접 Azure OpenAI로 임베딩을 생성하여 입력 텍스트의
벡터 표현을 생성할 수 있으며, 이는 벡터 유사성 검색에 사용할 수 있을
뿐만 아니라 기계 학습 모델에서도 사용할 수 있습니다.

임베딩은 기계 학습 및 natural language processing (NLP)의 개념으로,
단어, 문서 또는 엔터티와 같은 개체를 다차원 공간의 벡터로 표현하는 것과
관련이 있습니다. 임베딩을 사용하면 기계 학습 모델이 관련 정보가 얼마나
밀접하게 관련되어 있는지 평가할 수 있습니다. 이 기술은 데이터 간의
관계와 유사성을 효율적으로 식별하여 알고리즘이 패턴을 식별하고 정확한
예측을 할 수 있도록 합니다.

### 작업 1: pgvector 확장으로 벡터 지원을 활성화하기

azure_ai 확장을 사용하면 입력 텍스트에 대한 임베딩을 생성할 수 있습니다.
생성된 벡터를 데이터베이스의 나머지 데이터와 함께 저장할 수 있도록
하려면 데이터베이스 설명서의 enable vector support 에 있는 지침에 따라
pgvector 확장을 설치해야 합니다.

1.  CREATE EXTENSION 명령을 사용하여 pgvector 확장을 설치하세요.

+++CREATE EXTENSION IF NOT EXISTS vector; +++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image61.jpeg)

2.  vector supported를 데이터베이스에 추가한 상태에서 벡터 데이터 유형을
    사용하여 목록 테이블에 새 열을 추가하여 테이블 내에 임베딩을
    저장합니다. text-embedding-ada-002 모델은 1536차원의 벡터를
    생성하므로 벡터 크기로 1536을 지정해야 합니다.

3.  ALTER TABLE listings

4.  ADD COLUMN description_vector vector(1536);

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image62.jpeg)

### 작업 2: 벡터 임베딩을 생성하고 저장하기

이제 목록 테이블에서 임베딩을 저장할 준비가 되었습니다.
azure_openai.create_embeddings() 함수를 사용하여 설명 필드에 대한 벡터를
만들고 목록 테이블의 새로 생성된 description_vector 열에 삽입할
것입니다.

1.  create_embeddings() 함수를 사용하기 전에 다음 명령을 실행하여
    검사하고 필요한 인수를 검토하세요:

+++\df+ azure_openai.\* +++

![A computer screen with white text AI-generated content may be
incorrect.](./media/image63.jpeg)

\df+ azure_openai.\* 명령의 출력에 있는 Argument 데이터 types 속성은
함수가 예상하는 인수 목록을 표시합니다.

[TABLE]

2.  배포 이름을 사용하여 다음 쿼리를 실행하여 목록 테이블의 각 레코드를
    업데이트하고, azure_openai.create_embeddings() 함수를 사용하여 설명
    필드에 대해 생성된 벡터 임베딩을 description_vector 열에 삽입합니다.
    {your-deployment-name}을 Azure OpenAI Studio
    **Deployments**페이지에서 복사한 **Deployment name** 값으로 바꿉니다
    . 이 쿼리를 완료하는 데 약 5분이 걸립니다.

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

위의 쿼리는 WHILE 루프를 사용하여 description_vector 필드가 null이고
설명 필드가 빈 문자열이 아닌 목록 테이블에서 레코드를 검색하세요. 쿼리는
azure_openai.create_embeddings 함수를 사용하여 description_vector 열을
설명 열의 벡터 표현으로 업데이트하려고 시도하세요. 루프는 이 업데이트를
수행할 때 포함 함수 생성에 대한 호출이 Azure OpenAI 서비스의 호출 속도
제한을 초과하지 않도록 하는 데 사용됩니다. 통화 속도 제한을 초과하면
출력에 다음과 유사한 경고가 표시됩니다:

\[! 참고\] **참고**: 다시 시도하기 전에 1초 동안 기다리세요....

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image65.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image66.jpeg)

3.  다음 쿼리를 실행하여 모든 목록 레코드에 대해 description_vector 열이
    채워졌는지 확인할 수 있습니다:

+++SELECT COUNT(\*) FROM listings WHERE description_vector IS NULL AND
description \<\> '';+++

쿼리의 결과는 0의 개수여야 합니다.

![A black screen with white text AI-generated content may be
incorrect.](./media/image67.jpeg)

### 작업 3: 벡터 유사성 검색을 수행하기

벡터 유사성은 두 항목을 일련의 숫자인 벡터로 표현하여 유사성을 측정하는
데 사용되는 방법입니다. 벡터는 종종 LLM을 사용하여 검색을 수행하는 데
사용됩니다. 벡터 유사성은 일반적으로 유클리드 거리나 코사인 유사성과
같은 거리 메트릭을 사용하여 계산됩니다. 유클리드 거리는 n차원 공간에서
두 벡터 사이의 직선 거리를 측정하는 반면, 코사인 유사성은 두 벡터 사이의
각도의 코사인을 측정합니다. 각 임베딩은 부동 소수점 숫자의 벡터이므로
벡터 공간에서 두 임베딩 사이의 거리는 원래 형식의 두 입력 간의 의미론적
유사성과 상관 관계가 있습니다.

1.  벡터 유사성 검색을 실행하기 전에 벡터 유사성을 사용하지 않고 자연어
    쿼리를 사용하여 레코드를 검색한 결과를 관차하기 위해 ILIKE 절을
    사용하여 아래 쿼리를 실행하세요:

+++SELECT listing_id, name, description FROM listings WHERE description
ILIKE '%Properties with a private room near Discovery Park%';+++

![A black background with white text AI-generated content may be
incorrect.](./media/image68.jpeg)

쿼리는 설명 필드의 텍스트를 제공된 자연어 쿼리와 일치시키려고 시도하기
떼문에 0개의 결과를 반환합니다.

2.  목록 설명에 대해 벡터 유사성 검색을 수행하기 위해 이제 목록 테이블에
    대해 코사인 유사성 검색 쿼리를 실행하세요. 임베딩은 입력 질문에 대해
    생성된 다음 벡터 배열(::vector)로 캐스팅되어 목록 테이블에 저장된
    벡터와 비교할 수 있습니다. {your-deployment-name}을 Azure OpenAI
    Studio **Deployment**페이지에서 복사한 **Deployment name**값으로
    바꾸세요.

+++SELECT listing_id, name, description FROM listings ORDER BY
description_vector \<=\>
azure_openai.create_embeddings('{your-deployment-name}', 'Properties
with a private room near Discovery Park')::vector LIMIT 3;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image69.jpeg)

쿼리는 \<=\> [vector
operator](https://github.com/pgvector/pgvector#vector-operators)를
사용하며, 이는 다차원 공간에서 두 벡터 사이의 거리를 계산하는 데
사용되는 \cosine distance\\ 연산자를 나타냅니다.

3.  EXPLAIN ANALYZE 절을 사용하여 동일한 쿼리를 다시 실행하여 쿼리 계획
    및 실행 시간을 확인하세요. **{your-deployment-name}**을  Azure
    OpenAI Studio **Deployments** 페이지에서 복사한**Deployment
    name**값으로 바꾸세요.

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

출력에서 다음과 같이 유사한 것으로 시작하는 쿼리 계획을 확인하세요:

Limit (cost=1098.54..1098.55 rows=3 width=261) (actual
time=10.505..10.507 rows=3 loops=1) -\> Sort (cost=1098.54..1104.10
rows=2224 width=261) (actual time=10.504..10.505 rows=3 loops=1)

…

Sort Method: top-N heapsort Memory: 27kB -\> Seq Scan on listings
(cost=0.00..1069.80 rows=2224 width=261) (actual time=0.005..9.997
rows=2224 loops=1) 쿼리는 순차 스캔 정렬을 사용하여 조회를 수행합니다.
계획 및 실행 시간은 결과 끝에 나열되며 다음과 유사해야 합니다: 계획
시간: 62.020 ms 실행 시간: 10.530 ms

4.  벡터 필드를 더 효율적으로 검색할 수 있도록 하려면 코사인 거리와
    [HNSW](https://github.com/pgvector/pgvector#hnsw)(Hierarchical
    Navigable Small World의 약자)를 사용하여 목록에 대한 인덱스를
    생성하세요. 벡터 필드를 보다 효율적으로 검색할 수 있도록 하려면
    코사인 거리와
    [HNSW](https://github.com/pgvector/pgvector#hnsw)(Hierarchical
    Navigable Small World의 약자)를 사용하여 목록에 대한 인덱스를
    생성하세요. HNSW를 사용하면 pgvector가 최신 그래프 기반 알고리즘을
    활용하여 최근접 이웃 쿼리를 근사화할 수 있습니다.

+++CREATE INDEX ON listings USING hnsw (description_vector
vector_cosine_ops);+++

![](./media/image73.jpeg)

5.  hnsw 인덱스가 테이블에 미치는 영향을 관찰하려면 EXPLAIN ANALYZE 절을
    사용하여 쿼리를 다시 실행하여 쿼리 계획 및 실행 시간을 비교하세요.
    **{your-deployment-name}**을 Azure OpenAI Studio
    **Deployments**페이지에서 복사한 **Deployments name** 값으로
    바꾸세요.

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

출력에서 쿼리 계획에는 이제 더 효율적인 인덱스 스캔이 포함됩니다:

Limit (cost=116.48..119.33 rows=3 width=261) (actual time=1.112..1.130
rows=3 loops=1) -\> Index Scan using listings_description_vector_idx on
listings (cost=116.48..2228.28 rows=2224 width=261) (actual
time=1.111..1.128 rows=3 loops=1)

쿼리 실행 시간은 쿼리를 계획하고 실행하는 데 걸린 시간의 상당한 감소를
반영해야 합니다:

계획 시간: 56.802 ms

실행 시간: 1.167 ms

## 연습 6: Azure AI 서비스를 통합하기

azure_ai 확장의 azure_cognitive스키마에 포함된 Azure AI서비스 통합은
데이터베이스에서 직접 액세스할 수 있는 풍부한 AI Language 기능 집합을
제공합니다. 기능에는 감정 분석, 언어 감지, 핵심 문구 추출, 엔터티 인식
및 텍스트 요약이 포함됩니다. 이러한 기능은 Azure AI Language 서비스를
통해 사용할 수 있습니다.

확장을 통해 액세스할 수 있는 Azure AI 기능의 전체 목록을 검토하려면
Integrate Azure Database for PostgreSQL Flexible Server with Azure
Cognitive Services 설명서를 참조하세요.

### 작업 1: Azure AI Language 서비스를 프로비저닝하기

azure_ai 확장 인식 함수를 활용하려면 Azure AI Language서비스가
필요합니다. 이 연습에서는 Azure AI Language 서비스를 생성할 것입니다.

1.  Azure 포털 홈페이지에서 아래 이미지와 같이 Microsoft Azure 명령
    모음의 왼쪽에 있는 세 개의 가로 막대로 표시된 **Azure portal
    menu** 를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image77.jpeg)

2.  On the **Create a resource** 페이지의 왼쪽 메뉴에서 **AI + Machine
    Learning**를 선택하고**Language service**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image78.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image79.jpeg)

3.  **Select additional features** 대화상자에서 **Continue to create
    your resource**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image80.jpeg)

4.  Create Language **Basics** 탭에 다음을 입력하세요:

[TABLE]

5.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image81.png)

6.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image82.jpeg)

7.  기본 설정은Language 서비스 구성의 나머지 탭에 사용되므로 **Review +
    create** 버튼을 선택하세요.

8.  Language 서비스를 프로비저닝하기 위해**Review +
    create** 탭에서 **Create** 버튼을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image83.png)

9.  언어 서비스 배포가 완료되면 배포 페이지에서**Go to resource
    group**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image84.jpeg)

### 작업 2: Azure AI Language 서비스endpoint 및 key를 설정하기

azure_openai 함수와 마찬가지로 azure_ai 확장을 사용하여 Azure AI서비스를
성공적으로 호출하려면 Azure AI Language 서비스에 대한 endpoint와key 를
제공해야 합니다.

1.  Language 홈페이지에 왼쪽 탐색 메뉴에서 **Resource
    Management**의 **Keys and Endpoint** 항목을 선택하세요.

2.  아래 이미지와 같이 **Keys and Endpoints** 페이지에서 copy **KEY1,
    KEY 2,** 및 **Endpoint** 값을 복사하여 메모장에 붙여 넣고 예정된
    작업에 정보를 사용하기 위해 메모장을 **Save**하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image85.jpeg)

3.  엔드포인프 및 액세스 키 값을 복사한 후 아래 명령에서 {endpoint} 및
    {api-key} 토큰을 Azure 포털에서 검색한 값으로 바꾸세요. Cloud
    Shell의 psql 명령 프롬프트에서 명령을 실행하여 구성 테이블에 값을
    추가하세요.

\[!참고\] **참고:** 아래 명령을 실행하기 전에 psql 명령 프롬프트에
연결하세요.

SELECT azure_ai.set_setting('azure_cognitive.endpoint','{endpoint}');

SELECT azure_ai.set_setting('azure_cognitive.subscription_key',
'{api-key}');

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image86.jpeg)

### 작업 3: 리뷰의 감정을 분석하기

이 작업에서는 azure_cognitive.analyze_sentiment 함수를 사용하여 Airbnb
목록에 대한 리뷰를 평가할 것입니다.

1.  감정 분석을 수행하려면 azure_ai 확장에서 azure_cognitive스키마를
    사용하고 analyze_sentiment 함수를 사용하세요. 아래 명령을 실행하여
    해당 기능을 검토하세요:

+++\df azure_cognitive.analyze_sentiment+++

![A computer screen with white text AI-generated content may be
incorrect.](./media/image87.jpeg)

출력에는 함수의 스키마, 이름, 결과 데이터 형식 및 인수 데이터 형식이
표시됩니다. 이 정보는 함수 사용 방법을 이해하는 데 도움이 됩니다.

2.  또한 함수가 출력하는 결과 데이터 형식의 구조를 이해하여 반환 값을
    올바르게 처리할 수 있도록 하는 것도 중요합니다.
    sentiment_analysis_result 유형을 검사하기 위해 다음 명령을
    실행하세요:

+++\dT+ azure_cognitive.sentiment_analysis_result+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image88.jpeg)

3.  위 명령의 출력은 sentiment_analysis_result 유형이 튜플임을
    나타냅니다. 해당 튜플의 구조를 이해하려면 다음 명령을 실행하여
    sentiment_analysis_result composite type에 포함된 열을 확인하세요:

+++\d+ azure_cognitive.sentiment_analysis_result+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image89.jpeg)

해당 명령의 출력은 다음과 유사해야 합니다: 복합 유형
"azure_cognitive.sentiment_analysis_result"

Column | Type | Collation | Nullable | Default | Storage | Description
----------------+------------------+-----------+----------+---------+----------+-------------

sentiment | text | | | | extended |

positive_score | double precision | | | | plain |

neutral_score | double precision | | | | plain |

negative_score | double precision | | | | plain |

azure_cognitive.sentiment_analysis_result는 입력 텍스트의 감정 예측을
포함하는 복합 유형입니다. 여기에는 긍정적, 부정적, 중립적 또는 혼합될 수
있는 감정과 텍스트에서 발견되는 긍정적, 중립적 및 부정적인 측면에 대한
점수가 포함됩니다. 점수는 0과 1 사이의 실수로 표시됩니다. 예를 들어
(neutral,0.26,0.64,0.09)에서 감정은 긍정 점수 0.26, 중립 0.64, 부정 점수
0.09로 중립입니다.

## 연습 7: 모든 것을 함께 묶기 위해 최종 쿼리를 실행하기

이 연습에서는 **pgAdmin**의 데이터베이스에 연결하고 실습 3 및 4에서
azure_ai, postgis, 및 pgvector 확장과 작업을 함께 연결하는 최종 쿼리를
실행할 것입니다.

### 작업 1: pgAdmin을 설치하기

1.  웹 브라우저를
    열고 <https://www.pgadmin.org/download/pgadmin-4-windows/>로
    이동하세요

2.  최신 버전의 **pgAdmin**을 클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.jpeg)

3.  **pgadmin4-8.9-x64.exe**를 선택하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image91.jpeg)

4.  다운로드된 파일을 실행하고 설치하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image92.jpeg)

5.  Select Setup Install Mode 탭에서 **Install for me
    only(recommended)**를 선택하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image93.jpeg)

6.  **Next** 버튼을 클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image94.jpeg)

7.  **I accept the agreement**를 선택하고 **Next** 버튼을 클릭하세요

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image95.jpeg)

8.  경로를 선택하고**Next** 버튼을 클릭하세요

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image96.jpeg)

9.  **Setup-pgAdmin 4** 창에서**Next** 버튼을 클릭하세요

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image97.jpeg)

10. **Install** 버튼을 클릭하세요

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image98.jpeg)

11. **Setup-pgAdmin 4** 창에서**Finish** 버튼을 클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image99.jpeg)

### 작업 2: pgAdmin를 사용하여 데이터베이스를 연결하기

이 작업에서는pgAdmin 을 열고 데이터베이스에 연결할 것입니다.

1.  Windows 검색 상자에서 +++**pgAdmin**+++를 입력하고 **pgAdmin**에
    클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image100.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image101.jpeg)

2.  Object Explorer 에서 **Servers** 를 마우스 오른쪽 버튼으로 클릭하고
    **Register \> Server**를 선택하여 서버를 등록하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image102.jpeg)

3.  **Register - Server** 대화상자에서PostgreSQL Flexible Server용 Azure
    Database서버 이름 (연습 1\> 작업 1에서 저장된)을 **General** 탭의
    **Name** 필드에 붙여넣으세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image103.jpeg)

4.  다음, **Connection** 탭을 선택하고 서버
    이름을 **Hostname/address** 필드에 붙여넣으세요. **Username** 필드에
    +++**s2admin**+++를 입력하고 **Password** 상자에
    +++**Seattle123Seattle123**+++ 를 입력하고 필요에 따라 **Save
    password**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image104.jpeg)

5.  마지막으로, **Parameters** 탭을 선택하고 **SSL mode**를
    **require**로 설정하세요. 서버를 등록하기 위해 **Save**를
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image105.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image106.jpeg)

6.  서버에 연결되면 **Databases** 노드를 확장하고
    **airbnb** 데이터베이스를 선택하세요. **airbnb** 데이터베이스를
    마우스 오른쪽 버튼으로 클릭하고 컨텍스트 메뉴에서 **Query Tool**을
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image107.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image108.jpeg)

### 작업 3: PostGIS 확장이 데이터베이스에 설치되어 있는지 확인하기

데이터베이스에postgis 확장을 설치하려면 CREATE EXTENSION 명령을 사용할
것입니다.

1.  위에서 연 쿼리 창에서 CREATE EXTENSION 명령을 IF NOT EXISTS 절과
    함께 실행하여 데이터베이스에 postgis 확장을 설치하세요.

+++CREATE EXTENSION IF NOT EXISTS postgis;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image109.jpeg)

이제 PostGIS 확장이 로드되었으므로 데이터베이스에서 지리 공간 데이터
작업을 시작할 준비가 되었습니다. 위에서 생성하고 채운 목록 테이블에는
나열된 모든 속성의 위도와 경도가 포함되어 있습니다. 이러한 데이터를 지리
공간 분석에 사용하려면 목록 테이블을 변경하여 point 데이터 유형을
허용하는 geometry열을 추가해야 합니다. 이러한 새로운 데이터 유형은
postgis 확장에 포함되어 있습니다.

2.  점 데이터를 수용하려면 점 데이터를 허용하는 테이블에 새 geometry
    열을 추가합니다. 다음 쿼리를 복사하여 열려 있는 pgAdmin 쿼리 창에
    붙여넣으세요:

3.  ALTER TABLE listings

+++ADD COLUMN listing_location geometry(point, 4326); +++

4.  다음으로, 경도 및 위도 값을 geometry 열에 추가하여 각 목록과 연결된
    지리 공간 데이터로 테이블을 업데이트하세요.

5.  UPDATE listings

+++SET listing_location = ST_SetSRID(ST_Point(longitude, latitude),
4326);+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image110.jpeg)

### 작업 4: 쿼리를 실행하고 맵에서 결과 보기

1.  다음 쿼리를 복사하여 열려 있는 쿼리 편집기에 붙여넣은 다음, 실행하여
    **listing_location** 열에 저장된 데이터를 확인하세요.

+++SELECT listing_id, name, listing_location FROM listings LIMIT 50;+++

Data Output 패널에서 쿼리 결과의 **listing_location** 열에 표시된 **View
all geometries** in this column 버튼을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image111.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image112.jpeg)

2.  이제 다음 쿼리를 실행하여 **geospatial proximity query**를 수행하면
    2016년 1월 13일 주에 사용할 수 있고 1박당 $75.00 미만이며 시애틀의
    디스커버리 파크에서 가까운 거리에 있는 속성을 반환하세요. 쿼리는
    PostGIS 확장에서 제공하는 ST_DWithin 함수를 사용하여 공원에서 지정된
    거리 내에 있는 목록을 식별하며, 경도는 -122.410347이고 위도는
    47.655598입니다.

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

**요약**

이 실습에서는 Azure AI 서비스를 PostgreSQL과 성공적으로 통합하여 강력한
AI 지원 데이터베이스 환경을 생성했습니다. Azure 리소스를 프로비전하고
필요한 확장을 사용하여 PostgreSQL 데이터베이스를 구성하여 시작했습니다.
텍스트 데이터에 대한 벡터 임베딩을 생성하고 벡터 유사성 검색을 수행하여
의미론적으로 유사한 레코드를 찾았습니다. 지리 공간적 데이터 분석을 위해
PostGIS 확장을 활용하고 감정 분석을 위해 Azure AI Language 서비스를
활용했습니다. 마지막으로, 인덱싱을 사용하여 쿼리를 최적화하고 성능을
분석하여 고급 데이터 분석을 위한 이 통합 솔루션의 효율성과 기능을
입증했습니다.

 
