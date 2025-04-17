# 사용사례 02 - Linux 및 PostgreSQL에서 Azure App Service를 사용하여 Fruits List Quarkus 웹앱 빌드하기

**예상 소요 시간:** 40분

**실습 유형:** 강사 진행

**목표:**

이 사용 사례에서는 PostgreSQL 데이터베이스(Azure Database for PostgreSQL
사용)에 연결된 Azure App Service에서 보안 Quarkus 애플리케이션을 빌드,
구성 및 배포하는 방법을 보여 줍니다. Azure App Service는 Windows 또는
Linux에서 앱을 쉽게 배포할 수 있는 확장성이 뛰어난 자체 패치 웹 호스팅
서비스입니다. 완료되면 Linux의 Azure App Service에서 Quarkus 앱이
실행됩니다.

**사전 요구 사항:**

**GitHub 계정** -- 고유한 GitHub 로그인 자격 증명이 있어야 합니다.
가지고 있지 않은 경우 여기에서 생성하세요
- +++<https://github.com/signup?user_email=&source=form-home-signup+++>

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
incorrect.](./media/image2.png)![A screenshot of a computer AI-generated
content may be incorrect.](./media/image2.png)

## 연습 1: 샘플을 실행하기

먼저 샘플 데이터 기반 앱을 시작점으로 설정합니다. 여기에서 사용하는 샘플
리포지토리에는 개발 컨테이너 구성이 포함되어 있습니다. 개발 컨테이너에는
데이터베이스, 캐시 및 샘플 애플리케이션에 필요한 모든 환경 변수를
포함하여 애플리케이션을 개발하는 데 필요한 모든 것이 있습니다. 개발
컨테이너는 GitHub codespace에서 실행할 수 있으므로 웹 브라우저가 있는
모든 컴퓨터에서 샘플을 실행할 수 있습니다.

1.  브라우저에서 GitHub 계정으로 로그인하세요
    +++\*\*<https://github.com/login**+++>.

2.  다음url을 새 탭에서
    여세요, +++\*\*<https://github.com/technofocus-pte/msdocs-quarkus-postgresql-sample-app**+++>.

3.  **Fork -\> Create a new fork**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.jpeg)

4.  Create a new fork 페이지에서 **Create fork**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

5.  repo의 분기된 페이지에서 **Code** \> **Create codespace on main**를
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

**참고:** 기본 옵션에서 Codespace 만들기가 표시되지 않으면 Codespaces
옆에 있는 + 기호를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

**참고:** codespace 만들기는 설정하는 데 약 10분이 걸립니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

6.  터미널에서 +++mvn quarkus:dev+++를 실행하세요. 팝업에서 **Allow**를
    클릭하세요.

![A screenshot of a browser AI-generated content may be
incorrect.](./media/image8.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

7.  알림이 표시되면 **Your application running on port 8080**을 사용할
    수 있습니다. **Open in Browser**를 선택하세요. 새 브라우저 탭에서
    샘플 애플리케이션을 볼 수 있습니다.

포트 **5005**에 대한 **notification** 이 표시되면 **skip**하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

8.  Quarkus 개발 서버를 중단하기 위해 Codespace 터미널에서 **Ctrl+C** 를
    입력하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

## 연습 2: Create App Service and PostgreSQL

먼저 Azure 리소스를 생성합니다. 이 실습에 사용된 단계에서는 App Service
및 PostgreSQL용 Azure Database을 포함하는 기본 보안 리소스 집합을
생성합니다.

1.  Open the Azure portal at +++<https://portal.azure.com/+++> 에서
    Azure 포털을 열고VM의  **Resources** 탭에 있는 Azure 로그인 자격
    증명으로**login**하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

2.  Welcome 페이지에 있는 **Cancel** 또는 close 버튼을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.jpeg)

3.  Azure 포털의 상단에 검색 바에서 +++**web app database**+++ 를
    입력하세요. **Marketplace** 제목 아래에서 **Web App +
    Database** 라는 레이블이 지정된 항목을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.jpeg)

4.  **Create Web App + Database**에서 다음 세부 정보를 입력하고
    **Review + create**를 선택하세요

[TABLE]

5.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image16.png)

6.  ![A screenshot of a web application AI-generated content may be
    incorrect.](./media/image17.jpeg)

7.  유효성 검사가 통과되면 **Create**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

**참고:** 앱을 생성하는 데 약 15분이 걸립니다.

8.  배포가 완료되면 **Go to resource**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

9.  **App Service page**로 바로 이동합니다**.** 왼쪽 상단에서**Home**를
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.jpeg)

10. 포털 메뉴를 클릭하고**Resource Groups**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.jpeg)

11. 할당된 리소스 그룹을 선택하고 방금 수행한 배포에서 다음 리소스가
    생성했는지 확인하세요.

> \- App Service plan
>
> \- App Service
>
> \- Virtual network
>
> \- Azure Database for PostgreSQL flexible server
>
> \- Private DNS zone

![A screenshot of a group AI-generated content may be
incorrect.](./media/image22.png)

## 연습 3: 연결 설정을 확인하세요

Creation wizard는 이미 앱 설정으로 연결 변수를 생성했습니다. 이
단계에서는 앱 설정을 찾을 수 있는 위치와 앱 설정을 직접 만드는 방법을
알아볼 것입니다.

1.  Resource group의 리소스 목록에서 **App Service** 를 클릭하세요.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image23.jpeg)

2.  App Service 페이지에 왼쪽 메뉴에서**Settings**의 **Environment
    variables**를 선택하세요.

3.  **Environment variables** 페이지에서 **App settings** 탭에서
    **AZURE_POSTGRESQL_CONNECTIONSTRING**가 있는지 확인하세요. 런타임에
    환경 변수로 주입됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

4.  **+ Add**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.jpeg)

5.  설정 이름을 +++**PORT**+++로 지정하고 해당 값을 Quarkus
    애플리케이션의 기본 포트인 +++**8080**+++로 설정하세요. **Apply**를
    선택하세요.

![A screenshot of a login AI-generated content may be
incorrect.](./media/image26.jpeg)

6.  **Apply**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

7.  **Confirm**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

8.  앱 설정이 업데이트되었다는 알림을 받게 됩니다..

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image29.jpeg)

## 연습 4: 샘플 코드를 배포하기

이 단계에서는 GitHub Actions를 사용하여 GitHub 배포를 구성합니다. App
Service에 배포하는 여러 방법 중 하나일 뿐만 아니라 배포 프로세스에서
지속적인 통합을 수행하는 좋은 방법이기도 합니다. 기본적으로 GitHub
리포지토리에 대한 모든 git push는 빌드 및 배포 작업을 시작합니다.

1.  App Service 페이지의 왼쪽 메뉴에서 **Deployment**에 **Deployment
    Center**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

2.  Source에서 **GitHub**를 선택하세요. 기본적으로 GitHub Actions는 빌드
    공급자로 선택됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

3.  **Authorize**를 클릭하고 GitHub 계정에 로그인한 후 프롬프트에 따라
    Azure에 권한을 부여하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image32.jpeg)

4.  아래와 같이 세부 정보를 입력하고 나머지는 기본값으로 두고 **Save**을
    클릭하세요.

[TABLE]

5.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image33.jpeg)

6.  **Save**을 클릭하면 App Service는 .github/workflows 디렉터리의
    선택한 GitHub 리포지토리에 워크플로 파일을 커밋합니다.

7.  샘플 포크의 GitHub codespace로 돌아가서 +++git pull origin main+++를
    실행합니다. 이렇게 하면 새로 커밋된 워크플로 파일을 codespace로
    끌어옵니다.

\[!참고\] **참고:** 터미널에서 테스트 사례가 여전히 실행 중인 경우
Ctrl+C를 누른 후 위의 명령을 실행할 수 있습니다.

![A screenshot of a computer code AI-generated content may be
incorrect.](./media/image34.jpeg)

7.  Explorer에서**src/main/resources/application.properties**를 여세요.
    Quarkus는 이 파일을 사용하여 Java 속성을 로드합니다.

8.  코드(10-11행)를 찾습니다. 이 코드는 프로덕션 변수
    **%prod.quarkus.datasource.jdbc.url**을 create wizard의 앱 설정으로
    설정합니다. **quarkus.package.type**은 App Service에서 실행해야 하는
    Uber-Jar를 빌드하도록 설정됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.jpeg)

9.  Explorer에서 **.github/workflows/main_quarkuwebapp\[lab instance
    id\].yml** 를 여세요. 이 파일은 App Service create wizard에서
    생성했습니다.

10. Build with Maven 단계에서 Maven 명령을 +++**mvn clean install
    -DskipTests**+++로 변경하세요.

11. 

**-DskipTests**는 GitHub 워크플로가 조기에 실패하는 것을 방지하기 위해
Quarkus 프로젝트의 테스트를 건너뜁니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

12. **Source Control** 확장을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

13. 텍스트 상자에 +++ **Configure DB and deployment workflow**+++와 같은
    커밋 메시지를 입력하세요. **Commit**를 선택하고 **Yes**로
    확인하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.jpeg)

14. **Sync changes 1**를 선택하고**OK**로 확인하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

15. Azure Portal의 배포 센터 페이지로 돌아가서 **Logs**를 선택하세요.
    커밋된 변경 사항에서 새 배포 실행이 이미 시작되었을 것입니다.

16. 배포 실행에 대한 로그 항목에서 최신 타임스탬프가 있는 **Build/Deploy
    Logs** 항목을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.jpeg)

17. GitHub 리포지토리로 이동하여 GitHub 작업이 실행 중인지 확인합니다.
    워크플로 파일은 빌드와 배포라는 두 개의 별도 단계를 정의합니다.
    GitHub 실행이 완료 상태를 표시할 때까지 기다립니다. 소요 시간은 약
    5분입니다.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image42.jpeg)

## 연습 5: 앱으로 이동하기

1.  Azure 포털
    (+++[https://portal.azure.com+++](https://portal.azure.com+++/))에서Resource
    group **ResourceGroup1**를 열고**App Service** 리소스를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

2.  왼쪽 메뉴에서 **Overview**를 선택하고 **Default domain**아래에
    앱의URL을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

3.  앱을 열기 위해 복사한 URL을 새 브라우저에 붙여넣으세요.

![A screenshot of a fruit list AI-generated content may be
incorrect.](./media/image45.jpeg)

4.  목록에 몇 가지 과일을 추가하세요. 이제PostgreSQL용 Azure Database에
    대한 보안 연결을 사용하여 Azure App Service에서 웹앱을 실행하고
    있습니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.jpeg)

## 연습 6: 진단 로그 스트림하기 

Azure App Service는 콘솔에 출력되는 모든 메시지를 캡처하여
애플리케이션의 문제를 진단하는 데 도움을 줍니다. 샘플 애플리케이션에는
아래와 같이 이 기능을 보여주는 표준 JBoss 로깅 문이 포함되어 있습니다.

1.  Azure portal App Service 패이지의 왼쪽 메뉴에서**Monitoring**의**App
    Service logs**를 선택하세요.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image48.jpeg)

2.  **Application logging**에서**File System**를 선택하세요. 위
    메뉴에서**Save**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.jpeg)

3.  왼쪽 메뉴에서 **Log stream** 을 선택하세요. 플랫폼 로그 및 컨테이너
    내부의 로그를 포함하여 앱에 대한 로그가 표시됩니다.

![A computer screen shot of a computer screen AI-generated content may
be incorrect.](./media/image50.jpeg)

## 연습 7: 리소스를 정리하기

1.  Azure 포털 Home page에서 Resource groups를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

2.  **NetworkWatcherRG**를 선택하고**Delete resource group**를
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

3.  텍스트 상자에서 +++NetworkWatcherRG+++를 입력하고**Delete**를
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image54.png)

4.  다음으로, Resource group 페이지에서 할당된Resource group을
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

5.  모든 **resources**를 선택하고**Delete**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

6.  텍스트 상자에 +++**delete**+++를 입력하고**Delete**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image58.png)

7.  삭제된 리소스에 대한 성공 알림은 삭제를 확인합니다.

8.  GitHub 작업 영역으로 돌아가서 Code옆에 있는 드롭다운을 클릭하고,
    코드스페이스 이름 옆에 있는 세 개의 점을 선택하고, **Delete**를
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

**요약:**

Azure App Service에 보안 Quarkus 애플리케이션을 배포하고 PostgreSQL
데이터베이스에 연결하여 앱의 UI에서 Fruit names이 추가하는 방법을
배웠습니다.
