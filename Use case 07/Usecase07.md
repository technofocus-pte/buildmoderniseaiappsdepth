# 사용 사례 07 - PostgreSQL용 Azure Database Flexible Server에서 Azure OpenAI를 사용하여 벡터 임베딩을 생성하도록 semantic 검색을 활성화하기

**목표**:

이 사용 사례에는 semantic 검색을 구현하여 임베딩을 생성하고 저장하여
PostgreSQL용 Azure Database Flexible Server에 벡터 및 azure_ai 확장을
설치한 후 확장을 적용하여 Azure OpenAI에서 생성된 임베딩 벡터를 저장할
것입니다.

**사용된 핵심 기술** -- Azure OpenAI, PostgreSQL용 Azure Database, Azure
AI 확장

**예상 소요 시간** -- 45분

**실습 유형:** 강사 진행

## 연습 1: Azure OpenAI를 사용하여 벡터 임베딩을 생성하기

Semantic 검색을 수행하려면 먼저 모델에서 임베딩 벡터를 생성하고, 이를
벡터 데이터베이스에 저장한 후, 임베딩을 쿼리해야 합니다. 데이터베이스를
생성하고, 샘플 데이터로 채우고, 해당 목록에 대해 semantic검색을 실행할
것입니다.

이 연습을 마치면 vector 및 azure_ai 확장을 사용하도록 설정된
PostgreSQL용 Azure Database Flexible Server 인스턴스를 갖게 됩니다.
Seattle Airbnb Open Data 데이터셋의 목록 테이블에 대한 임베딩을
생성합니다. 또한 쿼리의 임베딩 벡터를 생성하고 벡터 코사인 거리 검색을
수행하여 이러한 목록에 대해 semantic검색을 실행합니다.

1.  웹 브라우저를 열고 \`\`https:\\portal.azure.com/\`\`로 이동하여
    Azure 자격 증명으로 로그인하세요.

2.  브라우저 창 아래쪽에 새 Cloud Shell 창을 열기 위해  Azure portal
    toolbar에서 **Cloud Shell** 아이콘을 선택하세요. **Bash**를
    선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image1.jpeg)

3.  **No storage account required** 라디오 버튼을 선택하고 구독을
    선택하고 **Apply**를 클릭하세요.

![](./media/image2.png)

4.  Cloud Shell 프롬프트에서 프로젝트를 복제하기 위해 다음 명령을
    실행하세요

\`\`git clone https://github.com/technofocus-pte/postgresql-case\`\`

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

5.  프로젝트 폴더로 이동하세요.

**\`\`cd postgresql-case\`\`**

![A screenshot of a computer Description automatically
generated](./media/image4.jpeg)

6.  다음으로, Azure CLI 명령을 사용하여 Azure 리소스를 생성할 때 중복
    입력을 줄이기 위해 변수를 정의하는 세 가지 명령을 실행합니다. 변수는
    리소스 그룹에 할당할 이름(RG_NAME), 리소스가 배포될 Azure
    지역(REGION) 및 PostgreSQL 관리자 로그인(ADMIN_PASSWORD)을 위해
    임의로 생성된 암호를 나타냅니다.

7.  첫 번째 명령에서 해당 변수에 할당된 영역은 eastus 또는
    westus입니다, **\[but you can replace it with a location of your
    preference.\]{.mark}** 그러나 기본값을 바꾸는 경우 다른 \[추상적
    요약을 지원하는 Azure 지역\]{.underline}을 선택하여 이 학습 경로의
    모듈에 있는 모든 작업을 완료할 수 있도록 해야 합니다.

\`\`REGION=westus\`\`

8.  다음 명령은 이 연습에서 사용되는 모든 리소스를 저장할 리소스 그룹에
    사용할 기존 리소스 그룹 이름을 할당합니다.

> \`\`RG_NAME=Your existing resource group name\`\`

![](./media/image5.png)

9.  마지막 명령은 PostgreSQL 관리자 로그인을 위한 암호를 임의로
    생성하세요. 나중에 PostgreSQL flexible server에 연결하는 데 사용할
    수 있도록 안전한 장소에 **복사해야 합니다**.

> a=()
>
> for i in {a..z} {A..Z} {0..9};
>
> do
>
> a\[$RANDOM\]=$i
>
> done
>
> ADMIN_PASSWORD=$(IFS=; echo "${a\[\*\]::18}")
>
> echo "Your randomly generated PostgreSQL admin user's password is:"

echo $ADMIN_PASSWORD

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

### 작업 1: Cognitive Services Contributor를 할당하기

1.  새 탭을 열고 \`\`**https://portal.azure.com\`\`**로 이동하세요.
    Azure 자격 증명으로 로그인한 후 **Subscription** 타일을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

2.  Subscription 이름을 클릭하세요.

![](./media/image8.png)

3.  왼쪽 탐색 메뉴에서 Access control (IAM)을 클릭하세요. **Add**를
    클릭하고 **Add role assignment**를 선택하세요**.**

![](./media/image9.png)

4.  \`\`Cognitive Services Contributor\`\`를 검색하고
    선택하고 **Next** 버튼을 클릭하세요.

![A screenshot of a service assignment Description automatically
generated](./media/image10.jpeg)

5.  **User, group or service principal**를 선택하고 **select
    member** 링크를 클리하세요. Azure 구독 계정을 검색하여 선택하세요.
    마지막으로 **Select** 버튼을 클릭하세요.

![](./media/image11.png)

6.  **Review + assign** 버튼을 클릭하세요.

![](./media/image12.png)

7.  **Review + assign** 버튼을 다시 클릭하세요.

> ![](./media/image13.png)

### 작업 2: Azure 리소스를 프로비저닝하기 위해 Bicep 배포 스크립트를 실행하기

1.  Bicep 배포 스크립트를 실행하여 리소스 그룹에서 Azure 리소스를
    프로비전하기 위해 Azure CLI를 사용하여 Azure Portal의 첫 번째 탭으로
    다시 전환하세요. 배포에는 3 - 5분이 걸립니다.

\`\`cd\`\`

\`\`az deployment group create --resource-group $RG_NAME --template-file
"postgresql-case/Allfiles/Labs/Shared/deploy.bicep" --parameters
restore=false adminLogin=pgAdmin adminLoginPassword=$ADMIN_PASSWORD\`\`

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

![A computer screen shot of a black screen Description automatically
generated](./media/image15.jpeg)

2.  Bicep 배포 스크립트는 이 연습을 완료하는 데 필요한 Azure 서비스를
    리소스 그룹에 프로비저닝합니다. 배포된 리소스에는PostgreSQL용 Azure
    Database - Flexible Server가 포함됩니다. 리소스 그룹에서 리소스를
    확안할 수 있습니다.

- **Azure OpenAI,**

- **Azure AI Language service.**

![](./media/image16.png)

3.  Open AI 리소스를 클릭하세요

![](./media/image17.png)

4.  왼쪽 탐색 메뉴의 **Resource Management**에서**Keys and Endpoint**를
    클릭하세요. Key 1 및 endpoint를 기록해 두어 작업5에 사용하세요

![](./media/image18.png)

5.  또한 Bicep 스크립트는 PostgreSQL 서버의 *allowlist*에 azure_ai 및
    벡터 확장을 추가하고 (azure.extensions 서버 매개 변수를 통해),
    서버에서 rentals라는 데이터베이스를 생성하고,
    **text-embedding-ada-002** 모델을 사용하여 embedding이라는 배포를
    Azure OpenAI 서비스에 추가하는 등의 몇 가지 구성 단계를 수행합니다.

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

6.  배포를 완료하는 데 몇 분 정도 걸립니다. Cloud Shell에서
    모니터링하거나 생성된 리소스 그룹의**Deployments** 페이지로 이동하여
    배포 진행 상황을 관찰할 수 있습니다.

7.  리소스 배포가 완료되면 Cloud Shell 창을 닫으세요.

### 작업 3: Azure Cloud Shell에서 psql를 사용하여 데이터베이스와 연결하기

이 작업에서는 Azure Cloud Shell의 psql 명령줄 유틸리티를 사용하여
PostgreSQL용 Azure Database 서버의 rentals 데이터베이스에 연결할
것입니다.

1.  Azure 포털 (https://portal.azure.com/)에서 새로 생성한PostgreSQL용
    Azure Database - Flexible Server로 이동하세요.

![](./media/image20.png)

1.  사이드바에서, **Server Parameters**를 선택하세요.
    **azure.extensions** 파라미터를 검색하세요.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

2.  이미 선택되지 않은 경우 확장**Vector** 및 **AZURE_AI**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  리소스 메뉴에서**Settings**에서 **Databases**를 선택하고 rentals
    데이터베이스에 대해 **Connect** 를 선택하세요.

![](./media/image23.png)

3.  Cloud Shell의 "Password for user pgAdmin" 프롬프트에서
    **pgAdmin** 로그인을 위해 임의로 생성된 비밀번호를 입력하세요.

로그인하면 rentals 데이터베이스에 대한 psql 프롬프트가 표시됩니다.

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

4.  이 연습의 나머지 부분에서는 Cloud Shell에서 계속 작업하므로 창의
    오른쪽 위에 있는 **Maximize** 버튼을 선택하여 브라우저 창 내에서
    창을 확장하는 것이 도움이 될 수 있습니다 .

![A screenshot of a computer Description automatically
generated](./media/image25.jpeg)

### 작업 4: 확장을 구성하기

벡터를 저장 및 쿼리하고 임베딩을 생성하려면 PostgreSQL용 Azure Database
flexible server에 대해 두 개의 확장을 허용 목록에 추가하고 사용하도록
설정해야 합니다: vector and azure_ai.

1.  벡터 확장을 활성화하기 위해 Azure CLI를 사용하여 Azure Portal 탭을
    다시 전환하고 다음 SQL 명령을 실행하세요. 자세한 지침은 다음과
    같습니다

\`\`CREATE EXTENSION vector;\`\`

![A screenshot of a computer Description automatically
generated](./media/image26.jpeg)

3.  azure_ai 확장을 활성화하려면 다음 SQL 명령을 **update and
    run**하세요. Azure OpenAI 리소스에 대한 엔드포인트 및 API 키가
    필요합니다.

> \`\`CREATE EXTENSION azure_ai;\`\`
>
> \`\`SELECT azure_ai.set_setting('azure_openai.endpoint',
> 'https://\<endpoint\>.openai.azure.com');\`\`

\`\`SELECT azure_ai.set_setting('azure_openai.subscription_key', '\<API
Key\>');\`\`

![A screenshot of a computer program Description automatically
generated](./media/image27.jpeg)

### 작업 5: 데이터베이스를 샘플 데이터로 채우기

azure_ai 확장을 탐색하기 전에 임대 데이터베이스에 몇 개의 테이블을
추가하고 샘플 데이터로 채워 확장 프로그램의 기능을 검토할 때 작업할
정보를 확보합니다.

1.  임대 부동산 목록 및 고객 리뷰 데이터를 저장하기 위한 목록 및 리뷰
    테이블을 생성하기 위해 다음 명령을 실행하세요:

DROP TABLE IF EXISTS listings;

CREATE TABLE listings (

id int,

name varchar(100),

description text,

property_type varchar(25),

room_type varchar(30),

price numeric,

weekly_price numeric

);

![A screenshot of a computer Description automatically
generated](./media/image28.jpeg)

DROP TABLE IF EXISTS reviews;

CREATE TABLE reviews (

id int,

listing_id int,

date date,

comments text

);

![A screenshot of a computer Description automatically
generated](./media/image29.jpeg)

2.  CSV 파일의 데이터를 위에서 만든 각 테이블로 로드하기 위해 COPY
    명령을 사용하세요. 목록 테이블을 채우기 위해 먼저 다음 명령을
    실행하세요:

\`\`\COPY listings FROM
'postgresql-case/Allfiles/Labs/Shared/listings.csv' CSV HEADER\`\`

명령 출력은 CSV 파일에서 테이블로 50개의 행이 기록되었음을 나타내는 COPY
50이어야 합니다.

![A screenshot of a computer program Description automatically
generated](./media/image30.jpeg)

3.  마지막으로 고객 리뷰를 리뷰 테이블에 로드하기 위해 아래 명령을
    실행하세요:

\`\`\COPY reviews FROM
'postgresql-case/Allfiles/Labs/Shared/reviews.csv' CSV HEADER\`\`

명령 출력은 COPY 354여야 하며, 이는 354개의 행이 CSV 파일에서 테이블로
기록되었음을 나타냅니다.

![](./media/image31.jpeg)

4.  샘플 데이터를 재설정하려면 DROP TABLE 목록을 실행하고 이러한 단계를
    반복할 수 있습니다.

### 작업 6: 임베딩 벡터를 생성하고 저장하기

이제 몇 가지 샘플 데이터가 있으므로 임베딩 벡터를 생성하고 저장할
차례입니다. azure_ai 확장을 사용하면 Azure OpenAI 포함 API를 쉽게 호출할
수 있습니다.

1.  임베딩 벡터 열을 추가하세요.

text-embedding-ada-002 모델은 1,536차원을 반환하도록 구성되므로 벡터 열
크기에 사용하세요.

\`\`ALTER TABLE listings ADD COLUMN listing_vector vector(1536);\`\`

![A computer screen shot of a black screen Description automatically
generated](./media/image32.jpeg)

2.  azure_ai 확장에 의해 구현되는 create_embeddings 사용자 정의 함수를
    통해 Azure OpenAI를 호출하여 각 목록에 대한 설명에 대한 포함 벡터를
    생성하세요:

> UPDATE listings SET listing_vector =
> azure_openai.create_embeddings('embedding', description, max_attempts
> =\> 5, retry_delay_ms =\> 500) WHERE listing_vector IS NULL;

사용 가능한 할당량에 따라 몇 분 정도 걸릴 수 있습니다.

![A screenshot of a computer screen Description automatically
generated](./media/image33.png)

### 작업 7: Semantic 검색 쿼리를 수행하기

이제 임베딩 벡터로 보강된 목록 데이터를 얻었으므로 semantic검색 쿼리를
실행할 차례입니다. 이렇게 하려면 쿼리 문자열 임베딩 벡터를 가져온 후
코사인 검색을 수행하여 설명이 쿼리와 의미상 가장 유사한 목록을 찾습니다.

1.  코사인 검색에 임베딩을 사용하여(\\=\>은 코사인 거리 연산을 나타냄)
    쿼리와 가장 유사한 상위 10개 목록을 가져오세요.

SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;

다음과 유사한 결과를 얻을 수 있습니다. 임베딩 벡터가 결정론적이라고
보장되지 않기 때문에 결과는 다를 수 있습니다:

![A screenshot of a computer Description automatically
generated](./media/image34.jpeg)

2.  또한 설명이 의미상 유사한 일치하는 행의 텍스트를 읽을 수 있도록 설명
    열을 프로젝션할 수도 있습니다. 예를 들어 이 쿼리는 가장 일치하는
    항목을 반환합니다:

SELECT id, description FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 1;

이렇게 인쇄합니다.:

![A screenshot of a computer Description automatically
generated](./media/image35.jpeg)

Semantic검색을 직관적으로 이해하려면 설명에 실제로 "bright" 또는
"natural"이라는 용어가 포함되어 있지 않은지 확인합니다. 그러나
"summer"과 "sunlight," "windows," "ceiling window"을 강조합니다.

### 작업 8: 작업을 확인하기

위의 단계를 수행한 후 목록 테이블에는 Kaggle의 Seattle Airbnb Open
Data의 샘플 데이터가 포함됩니다. 목록은 semantic 검색을 실행하기 위해
임베딩 벡터로 보강되었습니다.

1.  목록 테이블에 id, name, description 및 listing_vector의 4개의 열이
    있는지 확인하세요.

\`\`\d listings\`\`

다음과 같은 것을 인쇄해야합니다:

![A screenshot of a computer Description automatically
generated](./media/image36.jpeg)

2.  하나 이상의 행에 채워진 listing_vector 열이 있는지 확인하세요.

\`\`SELECT COUNT(\*) \> 0 FROM listings WHERE listing_vector IS NOT
NULL;\`\`

결과에는 true를 의미하는 t가 표시되어야 합니다. 해당 설명 열의 임베딩이
있는 행이 하나 이상 있음을 나타냅니다:

![A screen shot of a computer Description automatically
generated](./media/image37.jpeg)

3.  임베딩 벡터의 차원이 1536개인지 확인하세요:

\`\`SELECT vector_dims(listing_vector) FROM listings WHERE
listing_vector IS NOT NULL LIMIT 1;\`\`

Yielding:

![A screen shot of a computer Description automatically
generated](./media/image38.jpeg)

4.  시맨틱 검색이 결과를 반환하는지 확인하세요.

코사인 검색에 포함을 사용하여 쿼리와 가장 유사한 상위 10개 목록을
가져오세요.

\`\`SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;\`\`

![A screenshot of a computer program Description automatically
generated](./media/image39.jpeg)

5.  다음 작업을 진행하기 위해 동일한 페이지에 계세요.

## 연습 2 – 추천 시스템에 대한 검색 기능을 생성하기

벡터 임베딩 로직 및 API 호출을 함수로 래핑해 보겠습니다. 이 연습에서는
PostgreSQL용 Azure Database flexible server에 벡터 및 azure_ai 확장을
[Azure
OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/overview)를
데이터베이스에 통합하기 위한 확장의 기능을 살펴봅니다.

### 작업 1: 추천 시스템에 대한 검색 기능을 생성하기

Semantic 검색을 활용한 추천 시스템을 구축해 보겠습니다. 시스템은 제공된
샘플 리스팅을 기반으로 여러 리스팅을 추천합니다. 샘플은 사용자가 보고
있는 목록 또는 기본 설정에서 가져온 것일 수 있습니다. azure_openai
확장을 활용하여 시스템을 PostgreSQL 함수로 구현합니다.

이 연습을 마치면 제공된 sampleListingId와 가장 유사한 최대 numResults
목록을 제공하는 함수 recommend_listing 정의하게 됩니다. 이 데이터를
사용하여 할인된 리스팅에 대해 추천 리스팅에 가입하는 것과 같은 새로운
기회를 창출할 수 있습니다.

Azure 구독에 리소스 배포하기

이 단계에서는 Azure Cloud Shell의 Azure CLI 명령을 사용하여 리소스
그룹을 생성하고 Bicep 스크립트를 실행하여 이 연습을 완료하는 데 필요한
Azure 서비스를 Azure 구독에 배포하는 방법을 안내합니다.

**참고:** 이 학습 경로에서 여러 모듈을 수행하는 경우 모듈 간에 Azure
환경을 공유할 수 있습니다. 이 경우 이 리소스 배포 단계를 한 번만
완료하면 됩니다.

### 작업 2: 추천 함수를 생성하기

1.  추천 함수는 sampleListingId를 사용하여 다른 목록과 가장 유사한
    numResults를 반환합니다. 이를 위해 샘플 목록의 이름과 설명의
    임베딩을 생성하고 목록 임베딩에 대해 해당 쿼리 벡터의 semantic
    검색을 실행합니다.

> CREATE FUNCTION
>
> recommend_listing(sampleListingId int, numResults int)
>
> RETURNS TABLE(
>
> out_listingName text,
>
> out_listingDescription text,
>
> out_score real)
>
> AS $$
>
> DECLARE
>
> queryEmbedding vector(1536);
>
> sampleListingText text;
>
> BEGIN
>
> sampleListingText := (
>
> SELECT
>
> name || ' ' || description
>
> FROM
>
> listings WHERE id = sampleListingId
>
> );
>
> queryEmbedding := (
>
> azure_openai.create_embeddings('embedding', sampleListingText,
> max_attempts =\> 5, retry_delay_ms =\> 500)
>
> );
>
> RETURN QUERY
>
> SELECT
>
> name::text,
>
> description,
>
> -- cosine distance:
>
> (listings.listing_vector \<=\> queryEmbedding)::real AS score
>
> FROM
>
> listings
>
> ORDER BY score ASC LIMIT numResults;
>
> END $$
>
> LANGUAGE plpgsql;

![A screenshot of a computer Description automatically
generated](./media/image40.jpeg)

### 작업 3: 추천 함수를 쿼리하기

1.  추천 함수를 쿼리하려면 목록 ID와 수행해야 하는 권장 사항의 수를
    전달하세요.

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommendations
closest to a listing

결과는 다음과 같습니다:

![A screenshot of a computer Description automatically
generated](./media/image41.jpeg)

2.  함수 런타임을 보려면 Azure Portal의 **Server Parameters**섹션에서
    **track_functions** 사용하도록 설정되어 있는지 확인하세요 (PL 또는
    ALL을 사용할 수 있음).:

![](./media/image42.png)

![A screenshot of a computer Description automatically
generated](./media/image43.png)

### 작업 4: 작업을 환인하기

1.  함수가 올바른 서명으로 존재하는지 확인하세요:

\`\`\df recommend_listing\`\`

다음과 같은 내용이 표시됩니다:

![A screenshot of a computer Description automatically
generated](./media/image44.jpeg)

2.  다음 쿼리를 사용하여 쿼리할 수 있는지 확인하세요:

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommendations
closest to a listing

![A screenshot of a computer Description automatically
generated](./media/image45.jpeg)

### 작업 5: 정리하기

이 연습을 완료했으면 만든 Azure 리소스를 삭제합니다. 데이터베이스가
얼마나 사용되는지가 아니라 구성된 용량에 대한 요금이 청구됩니다. 다음
지침에 따라 리소스 그룹과 이 랩에 대해 만든 모든 리소스를 삭제합니다.

1.  홈페이지에서 **Azure Open AI** 를 검색하여 선택하세요.

![](./media/image46.png)

2.  Open AI 리소스를 선택한 후 **Delete**를 클릭하세요.

![](./media/image47.png)

3.  텍스트 상자에 **delete **를 입력하고 **\*\*Delete**를
    선택하세요**.** 삭제되는지 확인하세요.

![](./media/image48.png)

![A screenshot of a computer Description automatically
generated](./media/image49.png)

4.  **Manage deleted resources**를 클릭하고 리소스를 선택한 후 아래
    이미지와 같이**Purge** 버튼을 클릭하세요.

![](./media/image50.png)

5.  **Yes**를 클릭하여 purge를 확인하세요 .

![](./media/image51.png)

6.  홈페이지의Azure services에서 **Resource groups**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image52.jpeg)

7.  Resource group 이름을 클릭하세요.

![](./media/image53.png)

8.  리소스 그룹의 **Overview** 페이지에서**all the resource** 를
    선택하고**Delete**를 클릭하세요**. DO NOT delete RESOURCE Group.**

> ![](./media/image54.png)

9.  **Delete**를 입력하고 Delete를 클릭하세요. **Delete** 버튼을
    클릭하여 리소스가 삭제되는지 확인하세요.

![](./media/image55.png)

**요약**:

PostgreSQL용 Azure Database flexible server에서 semantic 검색을 사용하여
Azure OpenAI에서 생성된 포함을 사용하여 쿼리하는 방법에 대해 배웠습니다.
다음을 통해 이 검색을 수행했습니다:

- vector 및 azure_ai 확장을 활성화.

- 임베딩을 저장하기 위해 벡터 열을 생성.

- 임베딩을 생성하고 저장.

- 쿼리 벡터를 사용하여 데이터베이스를 쿼리.
