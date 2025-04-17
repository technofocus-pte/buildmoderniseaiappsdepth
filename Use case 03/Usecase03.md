# 用例 03 - 容器化航班預訂應用程序並部署到 Azure Kubernetes 服務

**目標**：

在本模塊結束時，您將能夠：

- 容器化 Java 應用程序。

- 為 Java 應用程序構建容器鏡像。

- 在本地運行容器鏡像。

- 將容器映像推送到 Azure 容器註冊表。

將容器映像部署到 Azure Kubernetes 服務

**使用的關鍵技術**-- Java 11, Docker ,Maven

**預計持續時間**: 30 分鐘

**實驗類型：**講師指導

## 練習 1：設置 Azure 環境

在本練習中，您將使用 Azure CLI 創建後續單元中需要的 Azure 資源。使用
Azure CLI 執行以下步驟

### 任務 1：使用 Azure 資源管理器進行身份驗證

1.  從桌面打開 Gitbash 並運行以下命令登錄到 Azure 門戶

**\`\`az login\`\`**

**注意**：如果看到警告：已在
https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize
打開了 Web 瀏覽器。請繼續在 Web 瀏覽器中登錄。如果沒有可用的 Web
瀏覽器或 Web 瀏覽器無法打開，請將設備代碼流與 az login --use-device-code
一起使用

![A black background with yellow text Description automatically
generated](./media/image1.jpeg)

1.  此命令將帶您進入默認瀏覽器進行登錄。使用 Azure 訂閱帳戶登錄。

![A screenshot of a computer Description automatically
generated](./media/image2.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

2.  通過身份驗證後，切換回 Gitbash

![A computer screen with white text Description automatically
generated](./media/image4.jpeg)

3.  現在，我們將啟用我們的 Azure 訂閱，執行以下命令

\`\`az account set --subscription "\< YOUR_SUBSCRIPTION_ID \>"\`\`

\`\`az account list --output table\`\`

![A computer screen with white text Description automatically
generated](./media/image5.jpeg)

4.  定義局部變量為了簡化將進一步執行的命令，請設置以下環境變量

注意：我們已經在雲中為我創建了資源組。您必須部署現有資源組中的所有資源。您可以在
Azurebportal 中，也可以在 VM 上的“資源”選項卡下找到它

> export AZ_CONTAINER_REGISTRY="javaaksregist"$RANDOM
>
> export AZ_KUBERNETES_CLUSTER="javaakscluster"$RANDOM
>
> export AZ_LOCATION="westus"

export AZ_KUBERNETES_CLUSTER_DNS_PREFIX="javaakscontainer"

> export AZ_RESOURCE_GROUP= Your existing resource group name

**注意：**您需要替換為您選擇的區域，例如：eastus您需要替換為唯一值，因為該值用於在創建
Azure 容器註冊表時為其生成唯一的
FQDN（完全限定域名），例如：someuniquevaluejavacontainerregistry。

![A screen shot of a computer Description automatically
generated](./media/image6.png)

5.  Azure 容器註冊表允許你生成、存儲和管理容器映像，這些映像最終將存儲
    Java 應用的容器映像。使用以下命令創建 Azure 容器註冊表。

\`\`az acr create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_CONTAINER_REGISTRY --sku Basic | jq\`\`

![A screenshot of a computer screen Description automatically
generated](./media/image7.jpeg)

6.  配置 Azure CLI 以使用此新創建的 Azure 容器註冊表

\`\`az configure --defaults acr=$AZ_CONTAINER_REGISTRY\`\`

![A black background with green and white text Description automatically
generated](./media/image8.jpeg)

7.  對新創建的 Azure 容器註冊表進行身份驗證

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`

![A computer screen with white text Description automatically
generated](./media/image9.jpeg)

8.  創建 Azure Kubernetes 群集，需要一個 Azure Kubernetes 群集才能將
    Java 應用（容器映像）部署到.

az aks create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER --attach-acr $AZ_CONTAINER_REGISTRY
--dns-name-prefix=$AZ_KUBERNETES_CLUSTER_DNS_PREFIX --generate-ssh-keys
| jq

![A computer screen with white text Description automatically
generated](./media/image10.jpeg)

**注意：**創建 Azure Kubernetes 群集最多可能需要 10
分鐘，運行上述命令後，可以選擇在該 Azure CLI
選項卡中繼續創建，然後轉到下一個單元.

### 任務 2：運行 Docker

1.  在 Start 菜單上，單擊 **DockerDesktop**

![A screenshot of a phone Description automatically
generated](./media/image11.jpeg)

2.  確保它正在運行。

## 練習 2：容器化 Java 應用程序

在本練習中，您將容器化 Java 應用程序。

### 任務 1：構建 Java 應用程序

First you will navigate the Flight Booking System for Airline
Reservations repository and cd to the Airlines web application project
folder.

Optionally, if you have Java & Maven installed, you can run the
following command(s) in your CLI to get an sense of the experience in
building the application without Docker. If you do not have Java & Maven
installed, you can safely jump ahead to the next section titled
"Construct a Docker file", In that section you'll use Docker to pull
down Java and Maven to execute the builds on your behalf.

2.  在 CLI 中運行以下命令以導航到項目.

\`\`cd
"C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines"\`\`

![A black background with text Description automatically
generated](./media/image12.jpeg)

3.  在 CLI 中運行以下命令

\`\`mvn clean install\`\`

![A screenshot of a computer Description automatically
generated](./media/image13.jpeg)

**注意：**mvn clean install 命令用於說明不使用 Docker
多階段構建的作挑戰，我們接下來將介紹這些挑戰。同樣，此步驟是可選的，無論哪種方式，您都可以安全地繼續進行，而無需執行
Maven 命令。

4.  Maven 應該已經成功構建了航班預訂系統，用於航空公司預訂 Web
    應用程序歸檔工件
    FlightBookingSystemSample-0.0.-SNAPSHOT.war，如下圖所示

![A screenshot of a computer screen Description automatically
generated](./media/image14.jpeg)

## 任務 2：構建 Docker 文件

1.  在項目的根目錄中，containerize-and-deploy-Java-app-to-Azure/Project/Airlines
    創建一個名為 **Dockerfile 的文件.**

\`\`vi Dockerfile\`\`

![](./media/image15.jpeg)

Add the following contents to Dockerfile and then save and exit

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

**注意：**（可選）項目根目錄中的 Dockerfile_Solution
包含所需的內容。如您所見，此 Docker 文件構建階段有 6 個說明。

## 練習 3：為 Java 應用程序構建並運行容器映像

在本單元中，您將構建並運行容器鏡像。如前所述，映像的運行實例是容器。

### 任務 1：構建容器鏡像

現在，您已經成功構建了 Dockerfile，您可以指示 Docker 為您構建容器鏡像。

**注意：**確保您的 Docker 運行時配置為構建 Linux
容器。這一點很重要，因為正在使用的 Dockerfile 引用了 Linux
架構的容器映像 （JDK/JRE）。

1.  **Docker build** 是用於構建容器鏡像的命令。-**t**
    參數將用於指定容器標簽，而 **.** 是 Docker 查找 Dockerfile
    的位置。在 CLI 中運行以下命令。

**重要** : 此實驗室需要 jdk 11 . set java_home to jdk 11

\`\`docker build -t flightbookingsystemsample .\`\`

![A computer screen with text Description automatically
generated](./media/image17.jpeg)

2.  Docker 構建與此類似

![A screen shot of a computer screen Description automatically
generated](./media/image18.jpeg)

**注意：**如前所述，Docker
已執行您之前在上一個單元中編寫的行中的指令。每條指令都是按順序排列的一個步驟。再次重新運行
docker build 命令，注意步驟中的差異，您會注意到 ---\> Using cache for
layers that hasn-changed
（對未更改的圖層使用緩存）。如果您未更改應用程序（在重新運行 docker
build
命令之前），那麼您會注意到所有緩存的層，因為二進制文件保持不變，並且可以從
Docker
緩存中獲取。在優化容器鏡像和相關的計算成本以及構建容器鏡像所花費的時間時，這是一個重要的收穫。

1.  Docker 還可以顯示駐留的可用映像。這對於查看可運行的內容很有幫助。在
    CLI 中運行以下命令

docker image ls

You will see something similar:

![](./media/image19.jpeg)

### 任務 2：運行容器映像

1.  現在，您已經成功構建了容器鏡像，可以運行它。

2.  Docker run 是用於運行容器鏡像的命令. The -p

:#### 參數將用於轉發 localhost HTTP（第一個

port before the colon）的 port
在運行時（冒號後的第二個端口）的流量。請記住，在 Dockerfile 中，Tomcat
應用程序服務器正在端口 8080 上偵聽 HTTP
流量，因此這是需要公開的容器端口。最後，需要 image 標簽
flightbookingsystemsample 來指示 Docker 要運行什麼鏡像。在 CLI
中運行以下命令：

\`\`docker run -p 8080:8080 flightbookingsystemsample\`\`

您將看到類似的內容：

![A screen shot of a computer Description automatically
generated](./media/image20.jpeg)

![A screen shot of a computer screen Description automatically
generated](./media/image21.jpeg)

**注意：**如果命令 “docker run -p 8080：8080 flightbookingsystemsample”
出現錯誤，請使用下面提到的端口

\`\`docker run -p 8081:8080 flightbookingsystemsample\`\`

3.  打開瀏覽器並訪問航空公司預訂航班預訂系統登錄頁面，網址為
    http://localhost:8080/FlightBookingSystemSample

    1.  您應該會看到以下內容：

![A plane flying in the sky Description automatically
generated](./media/image22.jpeg)

4.  例如，您可以選擇使用 tomcat-users.xml 中的任何用戶登錄

用戶名 ： **\`\`someuser@azure.com\`\`**

密碼 : \`\`password\`\`

![A screenshot of a computer Description automatically
generated](./media/image23.jpeg)

5.  保持此 git bash 實例原樣

## 練習 4：將容器映像推送到 Azure 容器註冊表

### 任務 1：將容器映像推送到 Azure 容器註冊表

1.  在此任務中，你將容器映像推送到 Azure 容器註冊表。Azure
    容器註冊表允許你在專用註冊表中為所有類型的容器部署構建、存儲和管理容器映像和項目。將
    Azure 容器註冊表與現有容器開發和部署管道配合使用。

2.  打開 Gitbash 的新實例並運行 az command 以登錄到 Azure 門戶。

\`\`az login\`\`

3.  在 CLI 中運行以下命令

\`\`C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines\`\`

1.  我們將使用之前在練習 1 任務 1 中創建的相同的 使用 Azure Resource
    Manager 進行身份驗證。

[TABLE]

> **注意：**如果您的會話已空閒，則您在另一個時間點和/或從另一個 CLI
> 執行此步驟時，可能需要重新初始化環境變量並使用以下 CLI
> 命令重新進行身份驗證。

![A screen shot of a computer program Description automatically
generated](./media/image24.png)

### 任務 2：推送容器鏡像

在此任務中，您可以將新構建的容器映像推送到 Azure
容器註冊表。這樣，容器映像將位於靠近所有 Azure 資源（例如 Azure
Kubernetes 群集）的網絡。最終，你將配置 AKS 以從 Azure 容器註冊表中提取
flightbookingsystemsample 映像。

1.  若要將容器映像推送到 Azure 容器註冊表，請在 CLI 中運行以下三個命令

2.  登錄 **Azure Container Registry** 執行以下命令

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`

![](./media/image25.jpeg)

3.  首先使用 Azure 容器註冊表標記以前生成的容器映像：

\`\`docker tag flightbookingsystemsample
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`

![](./media/image26.jpeg)

4.  其次，將容器映像推送到 Azure 容器註冊表

\`\`docker push
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`

![A screen shot of a computer Description automatically
generated](./media/image27.jpeg)

![A screen shot of a computer Description automatically
generated](./media/image28.jpeg)

5.  現在查看新推送的映像的 Azure Container Registry 映像元數據。在 CLI
    中運行以下命令

\`\`az acr repository show -n $AZ_CONTAINER_REGISTRY --image
flightbookingsystemsample:latest\`\`

您將看到類似的內容:

![A computer screen with white text Description automatically
generated](./media/image29.jpeg)

6.  容器映像現在駐留在 Azure 容器註冊表中，並可供 Azure 服務（如 Azure
    Kubernetes 服務）進行部署。

## 練習 5：將容器映像部署到 Azure Kubernetes 服務

在本練習中，你將容器映像部署到 Azure Kubernetes 服務。

### 任務 1：部署容器鏡像

1.  您需要將此 **flightbookingsystemsample** 容器映像部署到 Azure
    Kubernetes 集群。

2.  在項目的根目錄
    **Flight-Booking-System-JavaServlets_App/Project/Airlines**
    中，創建一個名為 deployment.yml 的文件。在 CLI 中運行以下命令：

\`\`vi deployment.yml\`\`

![](./media/image30.jpeg)

3.  將以下內容添加到 deployment.yml 中，然後保存並退出：

**注意：**您需要使用之前設置的 AZ_CONTAINER_REGISTRY
環境變量值進行更新，即練習 1 Task1（ AZ_CONTAINER_REGISTRY=
javaaksregist ）

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

4.  按 Esc 和 ：，然後鍵入
    ''**[wq](urn:gd:lg%F0%9F%85%B0%EF%B8%8Fsend-vm-keys)''** 並按 Enter
    保存文件。

**注意：**（可選）項目根目錄中的 deployment_solution.yml
包含所需的內容，您可能會發現重命名/更新該文件的內容更容易。

1.  在上面的deployment.yml中，你會注意到這個 deployment.yml 包含一個
    Deployment 和一個 Service。Deployment 用於管理一組
    Pod，而服務用於允許對 Pod 進行網絡訪問。你會注意到，Pod 配置為從
    Azure Container Registry 拉取單個映像，即 \< AZ_CONTAINER_REGISTRY
    \>.azurecr.io/flightbookingsystemsample:latest。您還會注意到，該服務配置為允許傳入的
    HTTP Pod 流量到端口 8080，類似於使用 -p port
    參數在本地運行容器映像的方式。

2.  到目前為止，您的 Azure Kubernetes 集群創建應該已成功完成。

3.  現在，將 Azure CLI 配置為通過 kubectl 命令訪問 Azure Kubernetes
    群集。使用 az aks install-cli 命令在本地安裝 kubectl。在 CLI
    中運行以下命令

\`\`az aks install-cli\`\`

![](./media/image32.jpeg)

4.  將 kubectl 配置為使用 az aks get-credentials 命令連接到 Kubernetes
    群集。在 CLI 中運行以下命令

\`\`az aks get-credentials --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER\`\`

> 您將看到類似的內容：

![](./media/image33.jpeg)

5.  現在指示 Azure Kubernetes 服務將deployment.yml更改應用於群集。在 CLI
    中運行以下命令

\`\`kubectl apply -f deployment.yml\`\`

- You will see something similar:

![](./media/image34.jpeg)

6.  現在使用 **kubectl** 監控部署的狀態。在 CLI 中運行以下命令

\`\`kubectl get all\`\`

- 您將看到類似的內容:

![A computer screen with text and numbers Description automatically
generated](./media/image35.jpeg)

**注意：**您需要將下面的 IP 地址 20.81.13.151 替換為您的 EXTERNAL-IP 的
IP 地址，並記下 POD 名稱，我們將在後續步驟中使用它。

7.  如果您的 **POD** 狀態為 **Running
    （正在運行**），則應用程序應該可訪問。

8.  您還可以查看每個 Pod 中的應用程序日誌。在 CLI 中運行以下命令

\`\`kubectl logs pod/flightbookingsystemsample-\`\`

![A screen shot of a computer Description automatically
generated](./media/image36.jpeg)

![A computer screen with white text Description automatically
generated](./media/image37.jpeg)

9.  現在，使用 **kubectl get services flightbookingsystemsample 輸出中的
    EXTERNAL-IP 訪問 Azure Kubernetes 服務中正在運行的應用。**

**注意：**您需要將以下 20.81.13.151 中的 IP
地址替換為您之前執行的命令中的 EXTERNAL-IP 地址。

10. 打開瀏覽器並訪問航班預訂系統示例登錄頁面，網址為 **http://YOUR
    IPCON：8080/FlightBookingSystemSample**（使用您的外部 IP 地址更新）

    - 您將看到類似的內容：

![A plane flying in the sky Description automatically
generated](./media/image38.jpeg)

**注意：**您可以選擇使用 tomcat-users.xml 中的任何用戶登錄，例如
someuser@azure.com：password

### 任務 2：清理資源

1.  切換回 Azure 門戶。單擊 **Resource groups**。

![A screenshot of a computer Description automatically
generated](./media/image39.png)

2.  單擊 Resource Group name（資源組名稱）。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

3.  選擇所有資源，然後單擊 **Delete** （Do NOT DELETE – Resource group）

4.  輸入 ''delete'' 然後點擊 **Delete**。

![A screenshot of a computer Description automatically
generated](./media/image41.png)

5.  確認刪除資源 。

![A screenshot of a computer Description automatically
generated](./media/image42.png)

**摘要：**恭喜！我們已將 Java 應用容器化並部署到 Azure Kubernetes
服務。在實驗室中，我們將 Java 應用容器化，將容器映像推送到 Azure
容器註冊表，然後部署到 Azure Kubernetes 服務。
