# 用例 03 - 容器化航班预订应用程序并部署到 Azure Kubernetes 服务

**目标**：

在本模块结束时，您将能够：

- 容器化 Java 应用程序。

- 为 Java 应用程序构建容器镜像。

- 在本地运行容器镜像。

- 将容器映像推送到 Azure 容器注册表。

将容器映像部署到 Azure Kubernetes 服务

**使用的关键技术**-- Java 11, Docker ,Maven

**预计持续时间**: 30 分钟

**实验类型：**讲师指导

## 练习 1：设置 Azure 环境

在本练习中，您将使用 Azure CLI 创建后续单元中需要的 Azure 资源。使用
Azure CLI 执行以下步骤

### 任务 1：使用 Azure 资源管理器进行身份验证

1.  从桌面打开 Gitbash 并运行以下命令登录到 Azure 门户

**\`\`az login\`\`**

**注意**：如果看到警告：已在
https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize
打开了 Web 浏览器。请继续在 Web 浏览器中登录。如果没有可用的 Web
浏览器或 Web 浏览器无法打开，请将设备代码流与 az login --use-device-code
一起使用

![A black background with yellow text Description automatically
generated](./media/image1.jpeg)

1.  此命令将带您进入默认浏览器进行登录。使用 Azure 订阅帐户登录。

![A screenshot of a computer Description automatically
generated](./media/image2.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

2.  通过身份验证后，切换回 Gitbash

![A computer screen with white text Description automatically
generated](./media/image4.jpeg)

3.  现在，我们将启用我们的 Azure 订阅，执行以下命令

\`\`az account set --subscription "\< YOUR_SUBSCRIPTION_ID \>"\`\`

\`\`az account list --output table\`\`

![A computer screen with white text Description automatically
generated](./media/image5.jpeg)

4.  定义局部变量为了简化将进一步执行的命令，请设置以下环境变量

注意：我们已经在云中为我创建了资源组。您必须部署现有资源组中的所有资源。您可以在
Azurebportal 中，也可以在 VM 上的“资源”选项卡下找到它

> export AZ_CONTAINER_REGISTRY="javaaksregist"$RANDOM
>
> export AZ_KUBERNETES_CLUSTER="javaakscluster"$RANDOM
>
> export AZ_LOCATION="westus"

export AZ_KUBERNETES_CLUSTER_DNS_PREFIX="javaakscontainer"

> export AZ_RESOURCE_GROUP= Your existing resource group name

**注意：**您需要替换为您选择的区域，例如：eastus您需要替换为唯一值，因为该值用于在创建
Azure 容器注册表时为其生成唯一的
FQDN（完全限定域名），例如：someuniquevaluejavacontainerregistry。

![A screen shot of a computer Description automatically
generated](./media/image6.png)

5.  Azure 容器注册表允许你生成、存储和管理容器映像，这些映像最终将存储
    Java 应用的容器映像。使用以下命令创建 Azure 容器注册表。

\`\`az acr create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_CONTAINER_REGISTRY --sku Basic | jq\`\`

![A screenshot of a computer screen Description automatically
generated](./media/image7.jpeg)

6.  配置 Azure CLI 以使用此新创建的 Azure 容器注册表

\`\`az configure --defaults acr=$AZ_CONTAINER_REGISTRY\`\`

![A black background with green and white text Description automatically
generated](./media/image8.jpeg)

7.  对新创建的 Azure 容器注册表进行身份验证

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`

![A computer screen with white text Description automatically
generated](./media/image9.jpeg)

8.  创建 Azure Kubernetes 群集，需要一个 Azure Kubernetes 群集才能将
    Java 应用（容器映像）部署到.

az aks create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER --attach-acr $AZ_CONTAINER_REGISTRY
--dns-name-prefix=$AZ_KUBERNETES_CLUSTER_DNS_PREFIX --generate-ssh-keys
| jq

![A computer screen with white text Description automatically
generated](./media/image10.jpeg)

**注意：**创建 Azure Kubernetes 群集最多可能需要 10
分钟，运行上述命令后，可以选择在该 Azure CLI
选项卡中继续创建，然后转到下一个单元.

### 任务 2：运行 Docker

1.  在 Start 菜单上，单击 **DockerDesktop**

![A screenshot of a phone Description automatically
generated](./media/image11.jpeg)

2.  确保它正在运行。

## 练习 2：容器化 Java 应用程序

在本练习中，您将容器化 Java 应用程序。

### 任务 1：构建 Java 应用程序

First you will navigate the Flight Booking System for Airline
Reservations repository and cd to the Airlines web application project
folder.

Optionally, if you have Java & Maven installed, you can run the
following command(s) in your CLI to get an sense of the experience in
building the application without Docker. If you do not have Java & Maven
installed, you can safely jump ahead to the next section titled
"Construct a Docker file", In that section you'll use Docker to pull
down Java and Maven to execute the builds on your behalf.

2.  在 CLI 中运行以下命令以导航到项目.

\`\`cd
"C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines"\`\`

![A black background with text Description automatically
generated](./media/image12.jpeg)

3.  在 CLI 中运行以下命令

\`\`mvn clean install\`\`

![A screenshot of a computer Description automatically
generated](./media/image13.jpeg)

**注意：**mvn clean install 命令用于说明不使用 Docker
多阶段构建的作挑战，我们接下来将介绍这些挑战。同样，此步骤是可选的，无论哪种方式，您都可以安全地继续进行，而无需执行
Maven 命令。

4.  Maven 应该已经成功构建了航班预订系统，用于航空公司预订 Web
    应用程序归档工件
    FlightBookingSystemSample-0.0.-SNAPSHOT.war，如下图所示

![A screenshot of a computer screen Description automatically
generated](./media/image14.jpeg)

## 任务 2：构建 Docker 文件

1.  在项目的根目录中，containerize-and-deploy-Java-app-to-Azure/Project/Airlines
    创建一个名为 **Dockerfile 的文件.**

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

**注意：**（可选）项目根目录中的 Dockerfile_Solution
包含所需的内容。如您所见，此 Docker 文件构建阶段有 6 个说明。

## 练习 3：为 Java 应用程序构建并运行容器映像

在本单元中，您将构建并运行容器镜像。如前所述，映像的运行实例是容器。

### 任务 1：构建容器镜像

现在，您已经成功构建了 Dockerfile，您可以指示 Docker 为您构建容器镜像。

**注意：**确保您的 Docker 运行时配置为构建 Linux
容器。这一点很重要，因为正在使用的 Dockerfile 引用了 Linux
架构的容器映像 （JDK/JRE）。

1.  **Docker build** 是用于构建容器镜像的命令。-**t**
    参数将用于指定容器标签，而 **.** 是 Docker 查找 Dockerfile
    的位置。在 CLI 中运行以下命令。

**重要** : 此实验室需要 jdk 11 . set java_home to jdk 11

\`\`docker build -t flightbookingsystemsample .\`\`

![A computer screen with text Description automatically
generated](./media/image17.jpeg)

2.  Docker 构建与此类似

![A screen shot of a computer screen Description automatically
generated](./media/image18.jpeg)

**注意：**如前所述，Docker
已执行您之前在上一个单元中编写的行中的指令。每条指令都是按顺序排列的一个步骤。再次重新运行
docker build 命令，注意步骤中的差异，您会注意到 ---\> Using cache for
layers that hasn-changed
（对未更改的图层使用缓存）。如果您未更改应用程序（在重新运行 docker
build
命令之前），那么您会注意到所有缓存的层，因为二进制文件保持不变，并且可以从
Docker
缓存中获取。在优化容器镜像和相关的计算成本以及构建容器镜像所花费的时间时，这是一个重要的收获。

1.  Docker 还可以显示驻留的可用映像。这对于查看可运行的内容很有帮助。在
    CLI 中运行以下命令

docker image ls

You will see something similar:

![](./media/image19.jpeg)

### 任务 2：运行容器映像

1.  现在，您已经成功构建了容器镜像，可以运行它。

2.  Docker run 是用于运行容器镜像的命令. The -p

:#### 参数将用于转发 localhost HTTP（第一个

port before the colon）的 port
在运行时（冒号后的第二个端口）的流量。请记住，在 Dockerfile 中，Tomcat
应用程序服务器正在端口 8080 上侦听 HTTP
流量，因此这是需要公开的容器端口。最后，需要 image 标签
flightbookingsystemsample 来指示 Docker 要运行什么镜像。在 CLI
中运行以下命令：

\`\`docker run -p 8080:8080 flightbookingsystemsample\`\`

您将看到类似的内容：

![A screen shot of a computer Description automatically
generated](./media/image20.jpeg)

![A screen shot of a computer screen Description automatically
generated](./media/image21.jpeg)

**注意：**如果命令 “docker run -p 8080：8080 flightbookingsystemsample”
出现错误，请使用下面提到的端口

\`\`docker run -p 8081:8080 flightbookingsystemsample\`\`

3.  打开浏览器并访问航空公司预订航班预订系统登录页面，网址为
    http://localhost:8080/FlightBookingSystemSample

    1.  您应该会看到以下内容：

![A plane flying in the sky Description automatically
generated](./media/image22.jpeg)

4.  例如，您可以选择使用 tomcat-users.xml 中的任何用户登录

用户名 ： **\`\`someuser@azure.com\`\`**

密码 : \`\`password\`\`

![A screenshot of a computer Description automatically
generated](./media/image23.jpeg)

5.  保持此 git bash 实例原样

## 练习 4：将容器映像推送到 Azure 容器注册表

### 任务 1：将容器映像推送到 Azure 容器注册表

1.  在此任务中，你将容器映像推送到 Azure 容器注册表。Azure
    容器注册表允许你在专用注册表中为所有类型的容器部署构建、存储和管理容器映像和项目。将
    Azure 容器注册表与现有容器开发和部署管道配合使用。

2.  打开 Gitbash 的新实例并运行 az command 以登录到 Azure 门户。

\`\`az login\`\`

3.  在 CLI 中运行以下命令

\`\`C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines\`\`

1.  我们将使用之前在练习 1 任务 1 中创建的相同的 使用 Azure Resource
    Manager 进行身份验证。

[TABLE]

> **注意：**如果您的会话已空闲，则您在另一个时间点和/或从另一个 CLI
> 执行此步骤时，可能需要重新初始化环境变量并使用以下 CLI
> 命令重新进行身份验证。

![A screen shot of a computer program Description automatically
generated](./media/image24.png)

### 任务 2：推送容器镜像

在此任务中，您可以将新构建的容器映像推送到 Azure
容器注册表。这样，容器映像将位于靠近所有 Azure 资源（例如 Azure
Kubernetes 群集）的网络。最终，你将配置 AKS 以从 Azure 容器注册表中提取
flightbookingsystemsample 映像。

1.  若要将容器映像推送到 Azure 容器注册表，请在 CLI 中运行以下三个命令

2.  登录 **Azure Container Registry** 执行以下命令

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`

![](./media/image25.jpeg)

3.  首先使用 Azure 容器注册表标记以前生成的容器映像：

\`\`docker tag flightbookingsystemsample
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`

![](./media/image26.jpeg)

4.  其次，将容器映像推送到 Azure 容器注册表

\`\`docker push
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`

![A screen shot of a computer Description automatically
generated](./media/image27.jpeg)

![A screen shot of a computer Description automatically
generated](./media/image28.jpeg)

5.  现在查看新推送的映像的 Azure Container Registry 映像元数据。在 CLI
    中运行以下命令

\`\`az acr repository show -n $AZ_CONTAINER_REGISTRY --image
flightbookingsystemsample:latest\`\`

您将看到类似的内容:

![A computer screen with white text Description automatically
generated](./media/image29.jpeg)

6.  容器映像现在驻留在 Azure 容器注册表中，并可供 Azure 服务（如 Azure
    Kubernetes 服务）进行部署。

## 练习 5：将容器映像部署到 Azure Kubernetes 服务

在本练习中，你将容器映像部署到 Azure Kubernetes 服务。

### 任务 1：部署容器镜像

1.  您需要将此 **flightbookingsystemsample** 容器映像部署到 Azure
    Kubernetes 集群。

2.  在项目的根目录
    **Flight-Booking-System-JavaServlets_App/Project/Airlines**
    中，创建一个名为 deployment.yml 的文件。在 CLI 中运行以下命令：

\`\`vi deployment.yml\`\`

![](./media/image30.jpeg)

3.  将以下内容添加到 deployment.yml 中，然后保存并退出：

**注意：**您需要使用之前设置的 AZ_CONTAINER_REGISTRY
环境变量值进行更新，即练习 1 Task1（ AZ_CONTAINER_REGISTRY=
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

4.  按 Esc 和 ：，然后键入
    ''**[wq](urn:gd:lg%F0%9F%85%B0%EF%B8%8Fsend-vm-keys)''** 并按 Enter
    保存文件。

**注意：**（可选）项目根目录中的 deployment_solution.yml
包含所需的内容，您可能会发现重命名/更新该文件的内容更容易。

1.  在上面的deployment.yml中，你会注意到这个 deployment.yml 包含一个
    Deployment 和一个 Service。Deployment 用于管理一组
    Pod，而服务用于允许对 Pod 进行网络访问。你会注意到，Pod 配置为从
    Azure Container Registry 拉取单个映像，即 \< AZ_CONTAINER_REGISTRY
    \>.azurecr.io/flightbookingsystemsample:latest。您还会注意到，该服务配置为允许传入的
    HTTP Pod 流量到端口 8080，类似于使用 -p port
    参数在本地运行容器映像的方式。

2.  到目前为止，您的 Azure Kubernetes 集群创建应该已成功完成。

3.  现在，将 Azure CLI 配置为通过 kubectl 命令访问 Azure Kubernetes
    群集。使用 az aks install-cli 命令在本地安装 kubectl。在 CLI
    中运行以下命令

\`\`az aks install-cli\`\`

![](./media/image32.jpeg)

4.  将 kubectl 配置为使用 az aks get-credentials 命令连接到 Kubernetes
    群集。在 CLI 中运行以下命令

\`\`az aks get-credentials --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER\`\`

> 您将看到类似的内容：

![](./media/image33.jpeg)

5.  现在指示 Azure Kubernetes 服务将deployment.yml更改应用于群集。在 CLI
    中运行以下命令

\`\`kubectl apply -f deployment.yml\`\`

- You will see something similar:

![](./media/image34.jpeg)

6.  现在使用 **kubectl** 监控部署的状态。在 CLI 中运行以下命令

\`\`kubectl get all\`\`

- 您将看到类似的内容:

![A computer screen with text and numbers Description automatically
generated](./media/image35.jpeg)

**注意：**您需要将下面的 IP 地址 20.81.13.151 替换为您的 EXTERNAL-IP 的
IP 地址，并记下 POD 名称，我们将在后续步骤中使用它。

7.  如果您的 **POD** 状态为 **Running
    （正在运行**），则应用程序应该可访问。

8.  您还可以查看每个 Pod 中的应用程序日志。在 CLI 中运行以下命令

\`\`kubectl logs pod/flightbookingsystemsample-\`\`

![A screen shot of a computer Description automatically
generated](./media/image36.jpeg)

![A computer screen with white text Description automatically
generated](./media/image37.jpeg)

9.  现在，使用 **kubectl get services flightbookingsystemsample 输出中的
    EXTERNAL-IP 访问 Azure Kubernetes 服务中正在运行的应用。**

**注意：**您需要将以下 20.81.13.151 中的 IP
地址替换为您之前执行的命令中的 EXTERNAL-IP 地址。

10. 打开浏览器并访问航班预订系统示例登录页面，网址为 **http://YOUR
    IPCON：8080/FlightBookingSystemSample**（使用您的外部 IP 地址更新）

    - 您将看到类似的内容：

![A plane flying in the sky Description automatically
generated](./media/image38.jpeg)

**注意：**您可以选择使用 tomcat-users.xml 中的任何用户登录，例如
someuser@azure.com：password

### 任务 2：清理资源

1.  切换回 Azure 门户。单击 **Resource groups**。

![A screenshot of a computer Description automatically
generated](./media/image39.png)

2.  单击 Resource Group name（资源组名称）。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

3.  选择所有资源，然后单击 **Delete** （Do NOT DELETE – Resource group）

4.  输入 ''delete'' 然后点击 **Delete**。

![A screenshot of a computer Description automatically
generated](./media/image41.png)

5.  确认删除资源 。

![A screenshot of a computer Description automatically
generated](./media/image42.png)

**摘要：**恭喜！我们已将 Java 应用容器化并部署到 Azure Kubernetes
服务。在实验室中，我们将 Java 应用容器化，将容器映像推送到 Azure
容器注册表，然后部署到 Azure Kubernetes 服务。
