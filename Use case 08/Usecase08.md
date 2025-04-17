# 사용 사례 08 - 고객을 지원하기 위해 Contoso Real Estate 채팅 앱 빌드 및 배포하기

**목표**

이 사용 사례는 Retrieval Augmented Generation 패턴을 사용하여 자체
데이터에 대해 ChatGPT와 같은 경험을 만드는 몇 가지 접근 방식을
보여줍니다. Azure OpenAI 서비스를 사용하여 ChatGPT 모델 (gpt-35-turbo)에
액세스하고 Azure AI Search를 사용하여 데이터 인덱싱 및 검색을
수행합니다.

![A diagram of a software process Description automatically
generated](./media/image1.jpeg)

사용 사례에는 샘플 데이터가 포함되어 있으므로 처음부터 끝까지 시도할
준비가 되어 있습니다. 이 샘플 애플리케이션에서는 Contoso Real Estate라는
가상의 회사를 사용하며, 이 환경을 통해 고객은 제품 사용에 대한 지원
질문을 할 수 있습니다. 샘플 데이터에는 서비스 약관, 개인 정보 취급 방침
및 지원 가이드를 설명하는 문서 집합이 포함되어 있습니다.

애플리케이션은 다음을 포함한 여러 구성 요소로 생성됩니다:

- **Search service**: 검색 및 검색 기능을 제공하는 백 엔드 서비스입니다.

- **Indexer service**: 데이터를 인덱싱하고 검색 인덱스를 생성하는
  서비스입니다.

- **Web app**: 사용자 인터페이스를 제공하고 사용자와 백엔드 서비스 간의
  상호 작용을 오케스트레이션하는 프런트엔드 웹 애플리케이션.

![A diagram of a software system Description automatically
generated](./media/image2.jpeg)

- 채팅 및 Q&A 인터페이스

- 사용자가 인용, 출처 콘텐츠 추적 등을 통해 응답의 신뢰성을 평가하는 데
  도움이 되는 다양한 옵션을 살펴봅니다.

- 데이터 준비, 프롬프트 생성 및 모델 (ChatGPT)과 검색기(Azure AI Search)
  간의 상호 작용 오케스트레이션에 대한 가능한 접근 방식을 보여 줍니다.

- UX에서 직접 설정하여 동작을 조정하고 옵션을 실험할 수 있습니다.

- Application Insights를 사용한 선택적 성능 추적 및 모니터링

**사용된 핵심 기술** -- Azure OpenAI Service, ChatGPT model
(gpt-35-turbo), 및Azure AI Search

**예상 소요 시간 --** 40분

## 연습 1: 애플리케이션을 배포하고 브라우저에서 테스트하기 

### 작업 1: 개발 환경을 열기

1.  브라우저를 열고 주소 바로 이동하고 다음 URL을 입력하거나
    복사하세요: \`\`https://github.com/technofocus-pte/azure-search-openai-javascript\`\`
    Github 계정으로 로그인하세요.

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

2.  **Fork**를 클릭하세요.

![A screenshot of a web page Description automatically
generated](./media/image4.jpeg)

3.  리포지토리 이름을 입력한 다음 **Create fork**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image5.jpeg)

4.  **Code -\> Codespaces -\> +**를 클릭하세요

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

5.  환경이 설정될 때까지 기다리세요. 5-10분 정도 걸립니다.

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

### 작업 2: 채팅 앱을 빌드하고 Azure에 배포하는 데 필요한 서비스 프로비저닝하기

1.  터미널에서 다음 명령을 실행하세요. 코드를 복사하고 Enter 키를
    누르세요

\`\`azd auth login\`\`

![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  코드를 입력하기 위해 기본 브라우저가 열립니다. 복사된 코드를
    입력하고 **Next**을 클릭하세요.

![](./media/image9.png)

3.  Azure 자격 증명으로 로그인하세요.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

![A screenshot of a computer error Description automatically
generated](./media/image11.png)

6.  Github Codespace 탭으로 다시 전환하세요. 아래 명령을 실행하여 현재
    디렉토리에서 프로젝트 환경을 초기화하세요. 환경 이름을 \`\`**ragpgpy
    \`\`**로 입력하고 Enter를 키를 누르세요.

Note : env name should be unique

\`\` azd env new\`\`

![](./media/image12.png)

7.  Azure에 서비스를 프로비전하고 컨테이너를 빌드하기 위해 아래 명령을
    실행하세요.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

8.  아래 값을 선택하세요.

> \`\`azd provision\`\`

- **Select an Azure Subscription to use** : 구독을 선택하세요

- **Select an Azure location to use** : **East us2/west us2** (경우에
  따라 미국 동부를 사용하지 못할 수 있으며 아래에 언급된 목록에서 위치를
  선택하하세요.)

- Select existing resource group : 기존 리소스 그룹 (eg
  :**ResourceGroup1 )**

![](./media/image14.png)

9.  리소스가 완전히 프로비저닝될 때까지 기다리세요. 이 프로세스는 필요한
    모든 리소스를 생성하는 데 5-10분이 걸립니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)

### 작업 3: 채팅 앱을 배포하고 탐색하기

10. 앱을 배포하기 위해 아래 명령을 실행하세요.

\`\`azd deploy\`\`

![](./media/image16.png)

11. 배포를 기다리세요. 소요 시간은 \<5분소요됩니다.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

12. 생성된 엔드포인트 URL을 클릭하세요.

![](./media/image18.png)

13. **Open**를 클릭하세요.

![](./media/image19.png)

14. 새 탭에서 앱이 열립니다.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

15. **How to search and book rental?** 컨테이너를 선택하고 텍스트 상자
    옆에 있는 Enter 버튼을 클릭하세요

![](./media/image21.png)

### 작업 4: 리소스를 정리하기

1.  **Azure portal -\> Resource group- \> Resource group name**로 다시
    전환하세요**.**

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  모든 리소스를 선택한 다음 아래 이미지와 같이 Delete를 클릭하세요.
    (**DO NOT DELETE** resource group)

![](./media/image23.png)

3.  텍스트 상자에 \`\`**delete**\`\`를 입력하고**Delete**를 클릭하세요.

![](./media/image24.png)

4.  Delete를 클릭하여 삭제를 확인하세요.

![](./media/image25.png)

5.  Github 포털 탭으로 다시 전환하고 페이지를 새로 고치세요.

![A screenshot of a computer Description automatically
generated](./media/image26.png)

6.  Code를 클릭하고, 이 랩에 대해 생성된 분기를 선택하고, **Delete**를
    클릭하세요.

![](./media/image27.png)

7.  **Delete** 버튼을 클릭하여 브랜치 삭제를 확인하세요.

![](./media/image28.png)

### 요약:

이 사용 사례에서는 Azure에서 실행되는 Retrieval Augmented Generation
패턴에 대한 채팅 애플리케이션을 배포하고, 검색을 위해 Azure AI Search를
사용하고 Azure OpenAI 및 LangChain large language model (LLM)을 사용하여
ChatGPT 스타일 및 Q&A 경험을 강화한다고 배웠습니다
