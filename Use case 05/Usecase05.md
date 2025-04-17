# 사용 사례 05 – PostgreSQL용 Azure Database을 사용하여 데이터 기반 Python Restaurant웹 앱을 배포하기

**목표:**

이 사용 사례에서는Flask 프레임워크 및PostgreSQL용 Azure Database관계형
데이터베이스 서비스를 사용하여 Python웹앱을 배포합니다. Flask 앱은
완전히 관리되는 Azure App Service에서 호스팅됩니다. 이 앱은 로컬에서
실행된 후 Azure에 배포되도록 설계되었습니다

![A diagram of a service plan Description automatically
generated](./media/image1.jpeg)

**PostgreSQL용 Azure Database** 관계형 데이터베이스 서비스를 사용하여
데이터 기반 Python웹앱 (**Django** 또는 **Flask**) 을 **Azure App
Service**에 배포할 것입니다. Azure App Service는 Linux 서버
환경에서Python을 지원합니다.

**사용된 핵심 기술** -- Java 17, PostgreSQL용 Azure Database

**예상 소요 시간** – 45분

**실습 유형:** 강사 진행

**사전 요구 사항:**

GitHub 계정 -- 고유한 GitHub 로그인 자격 증명이 있어야 합니다. 가지고
있지 않은 경우 여기에서
생성하새요- **https://github.com/signup?user_email=&source=form-home-signupobjectives**

The **requirements.txt**에는 다음과 같은 패키지가 있으며 모두 일반적인
데이터 기반 Flask 애플리케이션에서 사용됩니다:

[TABLE]

### 작업 1: 서비스 공급자를 등록하기

1.  브라우저를 열고 <https://portal.azure.com> 로 이동하고 VM의 리소스
    탭에서 사용할 수 있는 cloud slice 계정으로 로그인하세요.

> ![](./media/image2.png)

2.  Azure 포털의 홈페이지에서 **Resource groups** 타일을 클릭하세요.

![](./media/image3.png)

3.  리소스 그룹 이름을 복사하고 메모장에 저장하여 다음 작업을 사용하여
    이 리소스 그룹에 필요한 리소스를 배포하세요.

![](./media/image4.png)

4.  상단 탐색 메뉴에서 Home을 클릭하세요.

![](./media/image5.png)

5.  **Subscriptions** 타일을 클릭하세요.

![](./media/image6.png)

6.  구독 이름을 클릭하세요.

![](./media/image7.png)

7.  왼쪽 탐색 메뉴에서Settings을 확장하세요. **Resource providers**를
    클릭하고 Microsoft.AlertsManagement를 입력하고 선택하여
    **Register**를 클릭하세요.

> ![](./media/image8.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image9.png)

### 작업 2: Azure Developer CLI 템플릿을 시작하는 Github Codespace를 생성하기

이 사용 사례에는 개발 컨테이너 구성이 있어 더 쉽게 로컬에서 앱을
개발하고 Azure에 배포하고, 모니터링할 수 있습니다. Azure 개발 CLI
템플릿을 사용하여 앱을 배포합니다

1.  브라우저를 열고\`\`**https:\\github.com\`\`** 로 이동하고 Github
    계정으로 로그인하세요.

2.  아래 이미지와 같이 **Fork**을 클릭하여 이 리포지토리
    https://github.com/technofocus-pte/msdocs-flask-postgresql-sample-app 계정으로
    Fork하세요.

![A screenshot of a computer Description automatically
generated](./media/image10.jpeg)

3.  고유한 이름을 입력한 후**Create repo**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image11.jpeg)

4.  Fork의 리포지토리 루트에서 **Code** \> **Codespaces** \> **+**를
    선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image12.jpeg)

5.  작업 공간이 설정될 때까지 기다리세요.

![A screenshot of a computer Description automatically
generated](./media/image13.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

6.  Codespace 터미널에서 다음 명령을 실행하세요:

> \# Install requirements

\`\`python3 -m pip install -r requirements.txt\`\`

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

7.  환경 변수를 생성하기 위해 아래 명령을 실행하세요

> \# Create .env with environment variables

\`\`cp .env.sample.devcontainer .env\`\`

![A screenshot of a computer program Description automatically
generated](./media/image16.jpeg)

8.  데이터 마이그레이션을 위한 아래 명령을 실행하세요

> \# Run database migrations

\`\`python3 -m flask db upgrade\`\`

![A screenshot of a computer program Description automatically
generated](./media/image17.jpeg)

9.  아래 명령을 실행하여

> \# Start the development server

\`\`python3 -m flask run\`\`

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

10. Your application running on port is available.이라는 메시지가
    표시되면 **Open in Browser**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image20.jpeg)

11. **Add new restaurant** 버튼을 클릭하세요.

![A white screen with black text Description automatically
generated](./media/image21.jpeg)

12. 다음 세부 정보를 입력하고**Submit** 버튼을 클릭하세요.

Name : \`\`**Contoso Rica\`\`**

Street Adress - \`\`**3A ,8th cross, Ferns street , Singapore\`\`**

Description - \`\`**This is a medium to high priced restaurant in the
city shopping center\`\`**

![A screenshot of a restaurant Description automatically
generated](./media/image22.jpeg)

13. **Add new review** 버튼을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image23.jpeg)

14. 리뷰를 입력하고 버튼을 클릭하세요. **Save changes**

**Your name : your name**

**Rating : your rating**

\`\`This is a medium to high priced restaurant in the city shopping
center. Service was a little bit confusing as we had at least 6 waiters
coming to ask us things. Food took some time to come. We had 2 menus:
one indian and one thai. The thai is 30% cheaper so we went for some
appetizers and thai red curry. Food took some time but it was worth it.
It was delicious and very well prepared. Overall, this is a good
eat.\`\`

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

![A white card with black text Description automatically
generated](./media/image25.jpeg)

15. 더 많은 리뷰와 댓글로 새로운 레스토랑을 추가하세요.

![A screenshot of a computer Description automatically
generated](./media/image26.jpeg)

### 작업 3: Azure에서 필요한 리소스 프로비저닝하기

이 프로젝트는 Azure Developer CLI와 잘 작동하도록 설계되었으므로 더 쉽게
로컬에서 앱을 개발하고, Azure에 배포하고, 모니터링할 수 있습니다.

1.  Github codespace 탭으로 다시 전환하고 새로운 azd 환경을 초기화하기
    위해 다음 명령을 실행하세요:

\`\`azd init\`\`

![](./media/image27.jpeg)

2.  나중에 배포된 리소스의 이름에 사용될 환경 이름(예:
    **flask-app**XXXX(XXXX는 고유 번호일 수 있음))을 제공하라는 메시지가
    표시됩니다.

![](./media/image28.jpeg)

3.  필요하면 로그인\`\`**azd auth login\`\`** . 코드를 복사하고 Enter
    키를 누르세요.

![A screenshot of a computer Description automatically
generated](./media/image29.jpeg)

4.  코드를 입력한 후 Azure 자격 증명으로 로그인하세요.

![A screenshot of a computer error Description automatically
generated](./media/image30.jpeg)

![](./media/image31.jpeg)

![A screenshot of a computer error Description automatically
generated](./media/image32.jpeg)

![A screenshot of a computer program Description automatically
generated](./media/image33.jpeg)

5.  Gtihub codespace 탭으로 다시 전환하고 아래 명령을 실행하여 모든
    리소스를 프로비저닝하고 배포하세요. Azure 구독을 선택하라는 메시지가
    표시됩니다. **1**을 입력하여 구독을 선택하고 Enter 키를 누르세요.

**\`\`azd provision\`\`**

![A computer screen shot of a computer code Description automatically
generated](./media/image34.png)

6.  **WestUS/eastus**라는 위치를 선택하세요. 계정에 리소스를
    프로비저닝하고 최신 코드를 배포합니다. 배포 시 오류가 발생하는 경우
    일부 리소스에 대한 가용성 제약 조건이 있을 수 있으므로 위치를
    변경(예: "westus")하면 도움이 될 수 있습니다.

![A screenshot of a computer program Description automatically
generated](./media/image35.png)

7.  Azure Portal에서 리소스 그룹 이름(이전 작업에서 복사됨)을 입력하고
    Enter 키를 누르세요.

![](./media/image36.png)

8.  배포하는 **20 - 30 minutes**이 소요됩니다. 생성된 링크 또는 **Azure
    portal-\> Resource group-\> Deployments**에서 배포 상태를 확인할
    수도 있습니다.

![](./media/image37.png)

![A screenshot of a computer Description automatically
generated](./media/image38.png)

![A screenshot of a computer Description automatically
generated](./media/image39.png)

![A screenshot of a computer Description automatically
generated](./media/image40.png)

### 작업 4: Github에서 애플리케이션을 배포하기

1.  아래 명령을 실행하여 리소스 그룹 환경 변수를 설정하세요.

\`\`azd env set AZURE_RESOURCE_GROUP {Name of existing resource group}
\`\`

참고: {Name of existing resource group}을 VM의 리소스 섹션에서 사용할 수
있는 리소스 그룹 이름으로 바꾸세요.

![](./media/image41.png)

2.  모든 리소스를 배포하기 위해 아래 명령을 실행하여 배포가 성공적으로
    완료될 때까지 기다리세요.

\`\`azd deploy\`\`

![](./media/image42.png)

3.  생성된 Endpoint URL을 클릭하세요

![](./media/image43.png)

4.  외부 웹사이트를 열기 위해 **Open**를 클릭하세요.

![](./media/image44.png)

5.  새 탭에 앱이 열립니다.

![A screenshot of a computer Description automatically
generated](./media/image45.png)

### 작업 5: 진단 로그를 스트림

Azure App Service는 콘솔에 출력되는 모든 메시지를 캡처하여
애플리케이션의 문제를 진단하는 데 도움이 줍니다. 앱에는 아래와 같이 이
기능을 보여주는 print() 문이 포함되어 있습니다.

@app.route('/', methods=\['GET'\])

def index():

print('Request for index page received')

restaurants = Restaurant.query.all()

return render_template('index.html', restaurants=restaurants)

1.  **Azure portal- \> Resource group**로 다시 전환하고 **App
    service**를 클릭하세요.

![](./media/image46.png)

2.  App Service 페이지에서. 왼쪽 메뉴에서 **Monitoring -\>** **App
    Service logs**를 선택하세요.

![](./media/image47.png)

3.  **Application logging**에서 **File System**이 선택되어 있는지
    확인하세요. 필요하면 선택하세요. 위쪽 메뉴에서 **Save**를
    선택하세요.

![](./media/image48.png)

4.  왼쪽 메뉴에서 **Log stream**를 선택하세요. 플랫폼 로그 및 컨테이너
    내부의 로그를 포함하여 앱에 대한 로그가 표시됩니다.

![](./media/image49.png)

### 작업 6: Github에서 리소스를 정리하기.

1.  Github로 다시 전환하고 **repo -\> Code -\> Codespaces**를
    클릭하세요**.** 올바른 브렌치를 선택하세요

![](./media/image50.png)

2.  브렌치를 선택하고 **Delete**를 클릭하세요.

![](./media/image51.png)

3.  **Delete**를 클릭하세요.

![](./media/image52.png)

4.  **Azure portal -\> Resource group**로 다시 전환하세요.

![A screenshot of a computer Description automatically
generated](./media/image53.png)

5.  모든 리소스를 선택하고 **Delete**를 클릭하세요 ( DO NOT delete
    resource group)

![](./media/image54.png)

6.  \`\`**Delete**\`\`를 입력하고 **Delete**를 클릭하세요.

![](./media/image55.png)

7.  삭제를 확인하기 위해 **Delete**를 클릭하세요.

![](./media/image56.png)
