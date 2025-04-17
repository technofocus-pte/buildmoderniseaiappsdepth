# 사용 사례 03 - 항공편 예약 앱을 컨테이너화하고 Azure Kubernetes Service에 배포하기

**목표**:

이 모듈을 마치면 다음을 수행할 수 있습니다:

- Java 앱을 컨테이너화합니다.

- Java 앱용 컨테이너 이미지를 빌드합니다.

- 컨테이너 이미지를 로컬로 실행합니다.

- Azure Container Registry에 컨테이너 이미지 푸시합니다.

Azure Kubernetes Service에 컨테이너 이미지 배포하세요

**사용된 핵심 기술** -- Java 11, Docker ,Maven

**예상 소요 시간**: 30분

**실습 유형:** 강사 진행

## 연습 1: Azure 환경을 설정하기

이 연습에서는 Azure CLI를 사용하여 이후 단원에서 필요한 Azure 리소스를
생성할 것입니다. Azure CLI를 사용하여 다음 단계를 수행하세요

### 작업 1: Azure Resource Manager를 사용하여 인증하기

1.  데스크톱에서 Gitbash를 열고 아래 명령을 실행하여 Azure portal에
    로그인하세요

**\`\`az login\`\`**

**참고**: 경고를 참조하세요: 웹 브라우저가
https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize 에
열렸습니다. 웹 브라우저에서 로그인을 계속하세요. 웹 브라우저를 사용할 수
없거나 웹 브라우저가 열리지 않는 경우 az login --use-device-code와 함께
디바이스 코드 흐름을 사용하세요

![A black background with yellow text Description automatically
generated](./media/image1.jpeg)

2.  이 명령은 로그인할 기본 브라우저로 이동하세요. Azure 구독 계정으로
    로그인하세요.

![A screenshot of a computer Description automatically
generated](./media/image2.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

3.  인증되면 Gitbash로 다시 전환하세요

![A computer screen with white text Description automatically
generated](./media/image4.jpeg)

4.  이제 Azure 구독을 활성화하고 아래 명령을 실행합니다.

\`\`az account set --subscription "\< YOUR_SUBSCRIPTION_ID \>"\`\`

\`\`az account list --output table\`\`

![A computer screen with white text Description automatically
generated](./media/image5.jpeg)

5.  지역 변수 정의: 나중에 실행될 명령을 단순화하려면 다음 환경 변수를
    설정하세요

참고: 우리는 이미 클라우드에서 나를 위해 리소스 그룹을 생성했습니다.
기존 리소스 그룹 내에 모든 리소스를 배포해야 합니다. Azurebportal에서
찾거나 VM의 리소스 탭에서 찾을 수 있습니다.

> export AZ_CONTAINER_REGISTRY="javaaksregist"$RANDOM
>
> export AZ_KUBERNETES_CLUSTER="javaakscluster"$RANDOM
>
> export AZ_LOCATION="westus"

export AZ_KUBERNETES_CLUSTER_DNS_PREFIX="javaakscontainer"

> export AZ_RESOURCE_GROUP= Your existing resource group name

**참고:** 선택한 지역(예: eastus)으로 바꾸려는 경우 Azure Container
Registry를 생성할 때 고유한 FQDN (fully qualified domain name)을
생성하는 데 사용되므로 고유한 값으로 바꾸고 싶을 것입니다:
someuniquevaluejavacontainerregistry.

![A screen shot of a computer Description automatically
generated](./media/image6.png)

8.  Azure Container Registry를 사용하면 컨테이너 이미지를 빌드, 저장 및
    관리할 수 있으며, 컨테이너 이미지는 궁극적으로 Java 앱의 컨테이너
    이미지가 저장되는 위치입니다. 다음 명령을 사용하여 Azure Container
    Registry를 생성하세요.

\`\`az acr create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_CONTAINER_REGISTRY --sku Basic | jq\`\`

![A screenshot of a computer screen Description automatically
generated](./media/image7.jpeg)

9.  새로 생성된 Azure Container Registry를 사용하도록 Azure CLI
    구성하세요

\`\`az configure --defaults acr=$AZ_CONTAINER_REGISTRY\`\`

![A black background with green and white text Description automatically
generated](./media/image8.jpeg)

10. 새로 만든 Azure Container Registry에 인증하세요

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`

![A computer screen with white text Description automatically
generated](./media/image9.jpeg)

11. Azure Kubernetes 클러스터 생성, Java 앱(컨테이너 이미지)을
    배포하려면 Azure Kubernetes 클러스터가 필요합니다.

az aks create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER --attach-acr $AZ_CONTAINER_REGISTRY
--dns-name-prefix=$AZ_KUBERNETES_CLUSTER_DNS_PREFIX --generate-ssh-keys
| jq

![A computer screen with white text Description automatically
generated](./media/image10.jpeg)

**참고:** Azure Kubernetes 클러스터 생성은 최대 10분이 걸릴 수 있으며,
위의 명령을 실행한 후 필요에 따라 해당 Azure CLI 탭에서 계속하고 다음
단위로 이동하도록 할 수 있습니다.

### 작업 2: Docker를 실행하기

1.  Start 메뉴에서 **DockerDesktop**를 클릭하세요

![A screenshot of a phone Description automatically
generated](./media/image11.jpeg)

2.  실행 중인지 확인하세요.

## 연습 2: Java 앱 컨테이너화하기

이 연습에서는 Java 애플리케이션을 컨테이너화합니다.

### 작업 1: Java 애플리케이션을 빌드하기

먼저 항공사 예약을 위한 항공편 예약 시스템 저장소를 탐색하고 항공사 웹
애플리케이션 프로젝트 폴더로 이동할 것입니다.

선택적으로 Java 및 Maven이 설치된 경우 CLI에서 다음 명령을 실행하여
Docker 없이 애플리케이션을 빌드하는 경험을 파악할 수 있습니다. Java 및
Maven이 설치되어 있지 않은 경우 "Docker 파일 구성"이라는 제목의 다음
섹션으로 안전하게 이동할 수 있습니다.이 섹션에서는 Docker를 사용하여
Java 및 Maven을 풀다운하여 사용자를 대신하여 빌드를 실행합니다.

1.  CLI에서 다음 명령을 실행하여 프로젝트로 이동하세요.

\`\`cd
"C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines"\`\`

![A black background with text Description automatically
generated](./media/image12.jpeg)

2.  CLI에서 다음 명령을 실행하세요

\`\`mvn clean install\`\`

![A screenshot of a computer Description automatically
generated](./media/image13.jpeg)

**참고:** mvn clean install 명령은 다음에 다룰 Docker 다단계 빌드를
사용하지 않는 경우의 운영 문제를 설명하는 데 사용되었습니다. 다시
말하지만이 단계는 선택 사항이며, 어느 쪽이든 Maven 명령을 실행하지 않고
안전하게 이동할 수 있습니다.

3.  Maven은 다음 이미지와 같이 항공사 예약 웹 애플리케이션 아카이브
    아티팩트 FlightBookingSystemSample-0.0.-SNAPSHOT.war에 대한 항공편
    예약 시스템을 성공적으로 빌드했어야 합니다.

![A screenshot of a computer screen Description automatically
generated](./media/image14.jpeg)

## 작업 2: Docker 파일을 구성하기

1.  프로젝트의 루트 내에서
    containerize-and-deploy-Java-app-to-Azure/Project/Airlines,
    **Dockerfile**이라는 파일을 생성하세요**.**

\`\`vi Dockerfile\`\`

![](./media/image15.jpeg)

Dockerfile에 다음 내용을 추가한 후 저장하고 종료합니다

\#

\# Build stage

\#

FROM maven:3.6.0-jdk-11-slim AS build

WORKDIR /build

COPY pom.xml .

COPY src ./src

COPY web ./web

RUN mvn clean package

\#

\# Package stage

\#

FROM tomcat:8.5.72-jre11-openjdk-slim

COPY tomcat-users.xml /usr/local/tomcat/conf

COPY --from=build /build/target/\*.war
/usr/local/tomcat/webapps/FlightBookingSystemSample.war

EXPOSE 8080

CMD \["catalina.sh", "run"\]

![A screenshot of a computer Description automatically
generated](./media/image16.jpeg)

**참고:** 필요에 따라 프로젝트 루트의 Dockerfile_Solution에는 필요한
콘텐츠가 포함되어 있습니다. 보시다시피 이 Docker 파일 빌드 단계에는
6개의 지침이 있습니다.

## 연습 3: Java 앱에서 컨테이터 이미지를 빌드하고 실행하기

이 유닛에서는 컨테이너 이미지를 빌드하고 실행합니다. 앞서 언급했듯이
실행 중인 이미지 인스턴스는 컨테이너입니다.

### 작업 1: 컨테이너 이미지를 빌드하기

이제 Dockerfile을 성공적으로 구성했으므로 Docker에 컨테이너 이미지를
빌드하도록 지시할 수 있습니다.

**참고:** Docker 런타임이 Linux 컨테이너를 빌드하도록 구성되어 있는지
확인합니다. 이는 사용 중인 Dockerfile이 Linux 아키텍처에 대한 컨테이너
이미지(JDK/JRE)를 참조하기 때문에 중요합니다.

1.  **Docker build**는 컨테이너 이미지를 빌드하는 데 사용되는
    명령입니다. **-t** 인수는 컨테이너 레이블과 를 지정하는 데
    사용됩니다**.** 는 Docker가 Dockerfile을 찾을 수 있는 위치입니다.
    CL에서 다음 명령을 실행합니다.

**중요**: 이 실습에는 jdk 11이 필요합니다. java_home JDK 11로
설정합니다.

\`\`docker build -t flightbookingsystemsample .\`\`

![A computer screen with text Description automatically
generated](./media/image17.jpeg)

2.  Docker 빌드는 비슷한 것입니다

![A screen shot of a computer screen Description automatically
generated](./media/image18.jpeg)

**참고:** 앞에서 보았듯이 Docker는 이전 유닛에서 이전에 작성한 줄의
지침을 실행했습니다. 각 명령은 순차적인 단계입니다. docker build 명령을
다시 실행하고, 단계의 차이점을 확인하면 변경되지 않은 레이어에 캐시 사용
---\> 확인할 수 있습니다. 앱을 변경하지 않는 경우(docker build 명령을
다시 실행하기 전에) 이진 파일이 변경되지 않고 Docker 캐시에서 가져올 수
있으므로 캐시된 모든 계층을 확인할 수 있습니다. 이는 컨테이너 이미지 및
컨테이너 이미지를 구축하는 데 소요되는 시간과 관련된 컴퓨팅 비용을
최적화할 때 중요한 시사점입니다.

3.  Docker는 상주하는 사용 가능한 이미지를 표시할 수도 있습니다. 이렇게
    하면 실행할 수 있는 항목을 보는 데 유용합니다. CLI에서 다음 명령을
    실행하세요

docker image ls

비슷한 것을 볼 수 있습니다:

![](./media/image19.jpeg)

### 작업 2: 컨테이너 이미지를 실행하기

1.  이제 컨테이너 이미지를 성공적으로 빌드했으므로 실행할 수 있습니다.

&nbsp;

1.  Docker run은 컨테이너 이미지를 실행하는 데 사용되는 명령입니다. -p

:#### 인수는 localhost HTTP를 전달하는 데 사용됩니다. (첫 번째

콜론 앞의 포트) 런타임에 컨테이너에 대한 트래픽(콜론 뒤의 두 번째 포트).
Dockerfile에서 Tomcat 앱 서버는 포트 8080에서 HTTP 트래픽을 수신
대기하고 있으므로 노출해야 하는 컨테이너 포트임을 기억하세요. 마지막으로
flightbookingsystemsample 이미지 태그는 Docker에 실행할 이미지를
지시하는 데 필요합니다. CLI에서 다음 명령을 실행하세요:

\`\`docker run -p 8080:8080 flightbookingsystemsample\`\`

비슷한 내용이 표시됩니다:

![A screen shot of a computer Description automatically
generated](./media/image20.jpeg)

![A screen shot of a computer screen Description automatically
generated](./media/image21.jpeg)

**참고:** "docker run -p 8080:8080 flightBookingSystemSample" 명령에
오류가 발생하면 아래에 언급된 포트를 사용하세요.

\`\`docker run -p 8081:8080 flightbookingsystemsample\`\`

2.  브라우저를 열고 http://localhost:8080/FlightBookingSystemSample 의
    항공사 예약을 위한 항공편 예약 시스템 랜딩 페이지를 방문하세요

    - 다음과 같은 내용이 표시됩니다:

![A plane flying in the sky Description automatically
generated](./media/image22.jpeg)

3.  예를 들어 tomcat-users.xml에서 원하는 사용자로 선택적으로 로그인할
    수 있습니다

Username :  **\`\`someuser@azure.com\`\`**

Password : \`\`password\`\`

![A screenshot of a computer Description automatically
generated](./media/image23.jpeg)

4.  이 git bash 인스턴스를 그대로 두세요

## 연습 4: Azure Container Registry에 컨테이너 이미지를 푸시하기

### 작업 1: Azure Container Registry에 컨테이너 이미지를 푸시하기

1.  이 작업에서는 컨테이너 이미지를 Azure Container Registry에 푸시할
    것입니다. Azure Container Registry를 사용하면 모든 유형의 컨테이너
    배포에 대한 프라이빗 레지스트리에서 컨테이너 이미지 및 아티팩트를
    빌드, 저장 및 관리할 수 있습니다. 기존 컨테이너 개발 및 배포
    파이프라인과 함께 Azure 컨테이너 레지스트리 사용하세요.

2.  Gitbash의 새 인스턴스를 열고 az 명령을 실행하여 Azure Portal에
    로그인하세요.

\`\`az login\`\`

3.  CLI에서 다음 명령을 실행하세요

\`\`C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines\`\`

4.  연습 1 작업 1에서 이전에 생성한 것과 동일한 Azure Resource Manager를
    사용하여 인증을 사용합니다

[TABLE]

> **참고:** 세션이 유휴 상태이거나 다른 시점에서 이 단계를 수행하거나
> 다른 CLI에서 환경 변수를 다시 초기화하고 다음 CLI 명령을 사용하여 다시
> 인증해야 할 수 있습니다.

![A screen shot of a computer program Description automatically
generated](./media/image24.png)

### 작업2: 컨테이너 이미지를 푸시하기

이 작업에서는 새로 빌드된 컨테이너 이미지를 Azure Container Registry에
푸시할 수 있습니다. 이렇게 하면 컨테이너 이미지가 Azure Kubernetes
클러스터와 같은 모든 Azure 리소스에 가까운 네트워크가 됩니다. 궁극적으로
Azure Container Registry에서 flightbookingsystemsample 이미지를
끌어오도록 AKS를 구성할 것입니다.

1.  컨테이너 이미지를 Azure Container Registry에 푸시하려면 CLI에서 다음
    세 가지 명령을 실행하세요

2.  **Azure Container Registry**에 로그인하고 아래 명령을 실행하세요

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`

![](./media/image25.jpeg)

3.  먼저 Azure Container Registry를 사용하여 이전에 빌드된 컨테이너
    이미지에 태그를 지정하세요:

\`\`docker tag flightbookingsystemsample
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`

![](./media/image26.jpeg)

4.  둘째, 컨테이너 이미지를Azure Container Registry에 푸시하세요

\`\`docker push
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`

![A screen shot of a computer Description automatically
generated](./media/image27.jpeg)

![A screen shot of a computer Description automatically
generated](./media/image28.jpeg)

5.  이제 새로 푸시된 이미지의 Azure Container Registry 이미지
    메타데이터를 보세요. CLI에서 다음 명령을 실행하세요

\`\`az acr repository show -n $AZ_CONTAINER_REGISTRY --image
flightbookingsystemsample:latest\`\`

비슷한 내용이 표시됩니다:

![A computer screen with white text Description automatically
generated](./media/image29.jpeg)

6.  컨테이너 이미지는 이제 Azure Container Registry 내에 상주하며Azure
    Kubernetes Service와 같은 Azure Services 에 배포할 준비가
    되었습니다.

## 연습 5: Azure Kubernetes Service에 컨테이너 이미지를 배포하기

이 연습에서는 Azure Kubernetes Service에 컨테이너 이미지를 배포할
것입니다.

### 작업 1: 컨테이너 이미지를 배포하기

1.  이 **flightbookingsystemsample** 컨테이너 이미지를 Azure Kubernetes
    Cluster에 배포할 것입니다.

2.  프로젝트
    **Flight-Booking-System-JavaServlets_App/Project/Airlines**의 루트
    내에서 deployment.yml라는 파일을 생성하세요. CLI에서 다음 명령을
    실행하세요:

\`\`vi deployment.yml\`\`

![](./media/image30.jpeg)

3.  다음 내용을 deployment.yml에 추가하고 저장하고 종료하세요:

**참고:** 이전에 설정한 AZ_CONTAINER_REGISTRY 환경 변수 값인 Exercise 1
Task1( AZ_CONTAINER_REGISTRY= javaaksregist )로 업데이트할 수 있습니다.

apiVersion: apps/v1

kind: Deployment

metadata:

name: flightbookingsystemsample

spec:

replicas: 1

selector:

matchLabels:

app: flightbookingsystemsample

template:

metadata:

labels:

app: flightbookingsystemsample

spec:

containers:

\- name: flightbookingsystemsample

image:
\<AZ_CONTAINER_REGISTRY\>.azurecr.io/flightbookingsystemsample:latest

resources:

requests:

cpu: "1"

memory: "1Gi"

limits:

cpu: "2"

memory: "2Gi"

ports:

\- containerPort: 8080

---

apiVersion: v1

kind: Service

metadata:

name: flightbookingsystemsample

spec:

type: LoadBalancer

ports:

\- port: 8080

targetPort: 8080

selector:

app: flightbookingsystemsample

![A screenshot of a computer program Description automatically
generated](./media/image31.jpeg)

4.  Press Esc 키를 누르고
    \`\`**[wq](urn:gd:lg%F0%9F%85%B0%EF%B8%8Fsend-vm-keys)\`\`** 를
    입력하고enter를 누르고 파일을 저장하세요.

**참고:** 선택적으로 프로젝트 루트의 deployment_solution.yml에는 필요한
내용이 포함되어 있으므로 해당 파일의 내용을 바꾸거나 업데이트하는 것이
더 쉬울 수 있습니다.

5.  위의 deployment.yml 보면 이 deployment.yml Deployment 및 Service가
    포함되어 있음을 알 수 있습니다. 배포는 Pod 집합을 관리하는 데
    사용되며 서비스는 Pod에 대한 네트워크 액세스를 허용하는 데
    사용됩니다. Pod는 Azure Container Registry에서 단일 이미지인 \<
    AZ_CONTAINER_REGISTRY \>.azurecr.io/flightbookingsystemsample:latest
    를 끌어오도록 구성되어 있음을 알 수 있습니다. 또한 -p port 인수를
    사용하여 컨테이너 이미지를 로컬로 실행한 방식과 유사하게 들어오는
    HTTP Pod 트래픽을 포트 8080으로 허용하도록 서비스가 구성되어 있음을
    알 수 있습니다.

6.  지금쯤이면 Azure Kubernetes Cluster 생성은 성공적으로 완료되었을
    것입니다.

7.  이제 kubectl 명령을 통해 Azure Kubernetes 클러스터에 액세스하도록
    Azure CLI를 구성하세요. az aks install-cli 명령을 사용하여 kubectl을
    로컬로 설치하세요. CLI에서 다음 명령을 실행하세요

\`\`az aks install-cli\`\`

![](./media/image32.jpeg)

8.  az aks get-credentials 명령을 사용하여 Kubernetes 클러스터에
    연결하도록 kubectl을 구성하세요. CLI에서 다음 명령을 실행하세요

\`\`az aks get-credentials --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER\`\`

- 비슷한 것을 볼 수 있습니다:

![](./media/image33.jpeg)

9.  이제 Azure Kubernetes Service에 클러스터에 deployment.yml 변경
    내용을 적용하도록 지시하세요. CLI에서 다음 명령을 실행하세요

\`\`kubectl apply -f deployment.yml\`\`

- 비슷한 것을 볼 수 있습니다:

![](./media/image34.jpeg)

10. 이제 **kubectl**을 사용하여 배포 상태를 모니터링하세요. CLI에서 다음
    명령을 실행하세요

\`\`kubectl get all\`\`

- 비슷한 것을 볼 수 있습니다:

![A computer screen with text and numbers Description automatically
generated](./media/image35.jpeg)

**참고:** 20.81.13.151의 IP 주소를 EXTERNAL-IP의 IP 주소로 대체하고 POD
이름을 기록해 두면 다음 단계에서 사용할 것입니다.

11. **POD** 상태가 **Running**이면 앱에 액세스할 수 있어야 합니다.

12. 각 pod 내에서 앱 로그도 볼 수 있습니다. CLI에서 다음 명령을
    실행하세요.

\`\`kubectl logs pod/flightbookingsystemsample-\`\`

![A screen shot of a computer Description automatically
generated](./media/image36.jpeg)

![A computer screen with white text Description automatically
generated](./media/image37.jpeg)

13. Azure Kubernetes Service 내에서 실행 중인 앱에 액세스하기 위해 이제
    **kubectl get services flightbookingsystemsample** 출력의
    EXTERNAL-IP를 사용하세요.

**참고:** 20.81.13.151의 IP 주소를 이전에 실행한 명령의 EXTERNAL-IP
주소로 대체하는 것이 좋습니다.

14. 브라우저를 열고 **IPCON:8080/FlightBookingSystemSample http://YOUR**
    의 항공편 예약 시스템 샘플 랜딩 페이지를 방문하세요 (외부 IP 주소로
    업데이트)

    - 비슷한 것을 볼 수 있습니다:

![A plane flying in the sky Description automatically
generated](./media/image38.jpeg)

**참고:** 선택적으로 tomcat-users.xml에서 사용자로 로그인할 수 있습니다.
예를 들어: someuser@azure.com: password

### 작업 2: 리소스를 정리하기

1.  Azure portal로 다시 이동하세요. **Resource groups**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

2.  Resource group 이름을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

3.  모든 리소스를 선택하고**Delete**를 클릭하세요 (Do NOT DELETE –
    Resource group)

4.  \`\`delete\`\` 를 입력하고 **Delete**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image41.png)

5.  리소스가 삭제되는지 확인하세요.

![A screenshot of a computer Description automatically
generated](./media/image42.png)

**요약:** 축하합니다! Java 앱을 컨테이너화하여 Azure Kubernetes
Service에 배포했습니다. 랩의 일부로 Java 앱을 컨테이너화하고, 컨테이너
이미지를 Azure Container Registry에 푸시한 후 Azure Kubernetes Service에
배포했습니다.
