# 用例 01 - 将 Quarkus Todo 应用程序整合到容器应用程序中，并将它们与关系数据库集成。

**目的：**

此用例展示了如何在连接到 Azure Database for PostgreSQL
的容器应用中开发、配置和部署安全的 Quarkus To-d-
应用程序。完成后，你将在 Linux 上的 Azure 应用服务上运行一个 Quarkus
应用。

**使用的关键技术**-- Java 17, Azure Database for PostgreSQL

**预计持续时间**-- 45 分钟

**实验类型：** 讲师指导

### 任务 0：设置环境变量

1.  从 Windows 开始菜单中搜索 环境变量 ，然后选择 编辑系统环境变量。

![](./media/image1.jpeg)

2.  单击 **Environment Variable** 按钮。

![](./media/image2.jpeg)

3.  在 **Admin 的 User variable （用户变量**） 下选择 **JAVA_HOME**
    ，然后单击 **Edit （编辑**）。

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

4.  输入变量值 **C：\Program Files\Java\jdk-17**，然后单击 **Ok**。

![](./media/image4.jpeg)

5.  导航到文件夹 **C：\Software**
    并右键单击**apache-maven-3.9.4-bin.zip**文件夹并选择 **全部提取**.

![](./media/image5.jpeg)

6.  **在同一文件夹中提取**。

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

7.  切换回 Edit Environment variable 窗口，选择 **MAVEN_HOME**然后单击
    **Edit**。

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

8.  将 Variable （变量）
    值输入为\`C:\Software\apache-maven-3.9.4-bin\apache-maven-3.9.4\` 然后点击
    **OK**.

![A screenshot of a computer Description automatically
generated](./media/image8.jpeg)

9.  在 **Environmental Variable**s window, 点击 **Ok
    （确定**），然后再次单击 **OK（确定**）。

![](./media/image9.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image10.jpeg)

9.  在运行 lab 之前，请根据 lab 要求**更新** JAVA_HOME。

## 练习 1：使用 Maven 生成 Quarkus 应用程序

有几种方法可以生成 Quarkus 项目结构。您可以使用 Quarkus Web 界面、IDE
插件或 Quarkus Maven 插件。让我们使用 Maven 插件来生成项目结构。

您生成具有多个依赖项的应用程序：

1.  用于公开 REST 端点的 resteasy 依赖项

2.  用于序列化和反序列化 JSON 的 jackson 依赖项

3.  用于与数据库交互的 hibernate 依赖项

4.  用于连接到 PostgreSQL 数据库的 postgresql 依赖项

5.  用于构建 Docker 镜像的 docker 依赖项

无需指定 Azure
依赖项，因为先在本地运行应用程序，然后将其容器化版本部署到 Azure
容器应用。

### 任务 1 ：生成 Quarkus 应用程序

1.  从 Window 开始菜单打开 **Git Bash** 并运行以下命令

> mvn -U io.quarkus:quarkus-maven-plugin:3.7.3:create \\
>
> -DplatformVersion=3.7.3 \\
>
> -DprojectGroupId=com.example.demo \\
>
> -DprojectArtifactId=todo \\
>
> -DclassName="com.example.demo.TodoResource" \\
>
> -Dpath="/api/todos" \\
>
> -DjavaVersion=17 \\

-Dextensions="resteasy-jackson, hibernate-orm-panache, jdbc-postgresql,
docker"

![](./media/image11.jpeg)

2.  此命令将创建一个新的 Quarkus 项目。它生成一个 Maven
    目录结构（src/main/java 用于源代码，src/test/java
    用于测试）。它会创建一些 Java 类、一些测试和一些
    Dockerfile。它还会生成一个 *pom.xml*
    文件，其中包含所有需要的依赖项（Hibernate、RESTEasy、Jackson、PostgreSQL
    和 Docker）：

3.  单击搜索并输入\`\`IntelliJ IDE\`\` ，然后选择 **IntelliJ IDE**

![A screenshot of a computer Description automatically
generated](./media/image12.jpeg)

4.  选中确认复选框，然后单击 **Continue** 按钮。

![A screenshot of a computer screen Description automatically
generated](./media/image13.jpeg)

5.  关闭 Data sharing （数据共享） 窗口。

![](./media/image14.jpeg)

6.  选择 **Start trial** 单选按钮，然后单击 **Start trial** 按钮。

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

7.  Click on the **Allow access** button.

![](./media/image16.jpeg)

8.关闭浏览器，切换回 IntelliJ 许可证窗口，然后单击 **Continue** 按钮。

![A screenshot of a computer Description automatically
generated](./media/image17.jpeg)

8.  单击 **Open** folder（打开文件夹）。

![](./media/image18.jpeg)

9.  浏览到 **C：\Users\Admin\todo** 并选择 **todo** 项目文件夹，然后单击
    **OK。**

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

10. 单击 **Trust project** 按钮.

![](./media/image20.jpeg)

11. 打开 **pom.xml**，您应该会看到下面的 xml 格式。

> \<dependencies\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-hibernate-orm-panache\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-resteasy-jackson\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-jdbc-postgresql\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-container-image-docker\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-arc\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-hibernate-orm\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-resteasy\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-junit5\</artifactId\>
>
> \<scope\>test\</scope\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.rest-assured\</groupId\>
>
> \<artifactId\>rest-assured\</artifactId\>
>
> \<scope\>test\</scope\>
>
> \</dependency\>

\</dependencies\>

**注意** pom.xml 文件*中的所有依赖项* 都在 Quarkus BOM （物料清单）
io.quarkus.platform：quarkus-bom 中定义。

![](./media/image21.jpeg)

### 任务 2 ：对应用程序进行编码

1.  转到 **src/main/java/com.example.demo** 并右键单击 **MyEntity.Java
    -\> Refactor -\> Rename**。

![](./media/image22.jpeg)

2.  将生成的 ***MyEntity.java*** 类重命名为 ''Todo.java'' （与
    TodoResource.java 文件位于同一文件夹中 ）

![](./media/image23.jpeg)

1.  将现有代码替换为以下 Java 代码。它使用 Java 持久性
    API（jakarta.persistence.\* 包）来存储和检索 PostgreSQL
    服务器中的数据。它还使用 \[Hibernate ORM with Panache\]{.underline}
    （继承自 io.quarkus.hibernate.orm.panache.PanacheEntity）
    来简化持久层。

2.  使用 JPA 实体 （@Entity） 将 Java Todo 对象直接映射到 PostgreSQL
    Todo 表。然后， TodoResource REST 端点创建一个新的 Todo
    实体类并持久化它。这个类是一个映射在 Todo 表上的域模型。该表由 JPA
    自动创建。

3.  扩展 PanacheEntity 为你的类型提供了许多通用的创建、读取、更新和删除
    （CRUD） 方法。因此，您只需一行 Java 代码即可完成保存和删除 Todo
    对象等作。

4.  在 IntelliJ 中设置 JDK（如果尚未设置）。

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

5.  将现有代码替换为 Todo 实体的以下 Java 代码：

package com.example.demo;

import io.quarkus.hibernate.orm.panache.PanacheEntity;

import jakarta.persistence.Entity;

import java.time.Instant;

@Entity

public class Todo extends PanacheEntity {

public String description;

public String details;

public boolean done;

public Instant createdAt = Instant.now();

@Override

public String toString() {

return "Todo{" +

"id=" + id + '\\' +

", description='" + description + '\\' +

", details='" + details + '\\' +

", done=" + done +

", createdAt=" + createdAt +

'}';

}

}

![A screenshot of a computer Description automatically
generated](./media/image25.jpeg)

6.  要管理该类，请更新 **TodoResource**，以便它可以发布 REST 接口以使用
    HTTP 存储和检索数据。打开 **TodoResource**
    类并将代码替换为以下内容：

package com.example.demo;

import jakarta.inject.Inject;

import jakarta.transaction.Transactional;

import jakarta.ws.rs.Consumes;

import jakarta.ws.rs.GET;

import jakarta.ws.rs.POST;

import jakarta.ws.rs.Path;

import jakarta.ws.rs.Produces;

import static jakarta.ws.rs.core.MediaType.APPLICATION_JSON;

import jakarta.ws.rs.core.Response;

import jakarta.ws.rs.core.UriBuilder;

import jakarta.ws.rs.core.UriInfo;

import org.jboss.logging.Logger;

import java.util.List;

@Path("/api/todos")

@Consumes(APPLICATION_JSON)

@Produces(APPLICATION_JSON)

public class TodoResource {

@Inject

Logger logger;

@Inject

UriInfo uriInfo;

@POST

@Transactional

public Response createTodo(Todo todo) {

logger.info("Creating todo: " + todo);

Todo.persist(todo);

UriBuilder uriBuilder =
uriInfo.getAbsolutePathBuilder().path(todo.id.toString());

return Response.created(uriBuilder.build()).entity(todo).build();

}

@GET

public List\<Todo\> getTodos() {

logger.info("Getting all todos");

return Todo.listAll();

}

}

### ![A screenshot of a computer program Description automatically generated](./media/image26.jpeg)

### **任务 3 ：运行应用程序**

在开发模式下运行应用程序时，Docker Desktop 需要运行。这是因为 Quarkus
检测到您需要一个 PostgreSQL 数据库（因为 pom.xml 文件中声明的 PostgreSQL
依赖项 quarkus-jdbc-postgresql ），下载 PostgreSQL Docker Desktop
镜像，并使用该数据库启动一个容器。然后，它会自动在数据库中创建 Todo 表。

1.  双击 **Docker Desktop** 并最小化窗口。确保它正在运行。（无需登录）

![A computer screen with a white background Description automatically
generated](./media/image27.jpeg)

2.  返回 Gitbash 并使用以下命令运行 to-do 应用程序：

cd todo

./mvnw quarkus:dev

![](./media/image28.jpeg)

3.  Quarkus 应用程序应该启动并连接到您的数据库。您应该会看到以下输出:

![A screenshot of a computer program Description automatically
generated](./media/image29.jpeg)

![A computer screen with text and images Description automatically
generated](./media/image30.jpeg)

![](./media/image31.jpeg)

![A screen shot of a computer program Description automatically
generated](./media/image32.jpeg)

4.  单击 **Allow access**。

![](./media/image31.jpeg)

5.  要测试应用程序，您可以使用 cURL。

在 Gitbash
的单独新实例中，使用以下命令在数据库中创建新的待办事项。您应该会在
Quarkus 控制台中看到日志：

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description":"Take Quarkus MS Learn","details":"Take the MS
Learn on deploying Quarkus to Azure Container Apps","done": "true"}' \\

http://127.0.0.1:8080/api/todos

![A computer screen with white text Description automatically
generated](./media/image33.jpeg)

6.  此命令应返回创建的项（带有标识符）：

![A computer screen with white text Description automatically
generated](./media/image33.jpeg)

7.  使用以下 cURL 命令创建第二个 to-do：

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Take Azure Container Apps MS
> Learn","details":"Take the ACA Learn module","done": "false"}' \\

http://127.0.0.1:8080/api/todos

![A screenshot of a computer program Description automatically
generated](./media/image34.jpeg)

8.  接下来，使用新的 cURL 请求检索数据：

curl http://127.0.0.1:8080/api/todos

此命令返回待办事项列表，包括您创建的项：

![](./media/image35.jpeg)

### 任务 4 ：测试应用程序

要测试应用程序，您可以使用现有的 **TodoResourceTest** 类。它需要测试
REST 端点。为了测试终端节点，它使用 \[RESTAssured\]{.underline}.

1.  切换回 Intellij 并从 **src/test/java/com.example.demo** 中打开
    TodoResourceTest **类**。将 **TodoResourceTest**
    类中的代码替换为以下代码：

package com.example.demo;

import io.quarkus.test.junit.QuarkusTest;

import static io.restassured.RestAssured.given;

import static jakarta.ws.rs.core.HttpHeaders.CONTENT_TYPE;

import static jakarta.ws.rs.core.MediaType.APPLICATION_JSON;

import org.junit.jupiter.api.Test;

@QuarkusTest

class TodoResourceTest {

@Test

void shouldGetAllTodos() {

given()

.when().get("/api/todos")

.then()

.statusCode(200);

}

@Test

void shouldCreateATodo() {

Todo todo = new Todo();

todo.description = "Take Quarkus MS Learn";

todo.details = "Take the MS Learn on deploying Quarkus to Azure
Container Apps";

todo.done = true;

given().body(todo)

.header(CONTENT_TYPE, APPLICATION_JSON)

.when().post("/api/todos")

.then()

.statusCode(201);

}

}

![A computer screen shot of a program Description automatically
generated](./media/image36.jpeg)

2.  当您测试应用程序时，Docker Desktop 需要运行，因为 Quarkus
    检测到它需要 PostgreSQL 数据库进行测试。

3.  切换回 **Gitbash** 和 Ctrl +
    C。使用以下命令运行以下命令以测试应用程序：

./mvnw clean test

![A computer screen with text and images Description automatically
generated](./media/image37.jpeg)

您应该会看到类似于以下内容的输出：

![A computer screen with text and numbers Description automatically
generated](./media/image38.jpeg)

## 练习 2 - 设置 Azure 容器应用

在本练习中，您将创建一个包含应用程序资源的 Azure 资源组。然后，使用
Azure CLI 设置 PostgreSQL 数据库。最后，配置 Quarkus 应用程序以访问远程
PostgreSQL 数据库。使用您选择的终端运行命令。

## 任务 1 ： 准备工作环境

您需要设置一些环境变量。以下是有关您将创建的变量的一些说明：

[TABLE]

**注意：**您可以根据需要的任何方式命名 Azure 资源。本文提供了许多 Azure
资源的示例缩写（例如，rg 表示资源组，ca 表示容器应用）。

1.  使用以下命令设置变量。请务必按照上表中所述修改值。这些环境变量在本模块的其余部分中使用.

**注意：PostgreSQL 仅在** Westus **中受支持** 。首先在 westus
位置尝试，如果您有任何问题，然后在您附近的位置尝试

export AZ_PROJECT_Quarkus="azure-deploy-quarkus-"$RANDOM

export AZ_CONTAINERAPP="ca${AZ_PROJECT_Quarkus}"

export AZ_CONTAINERAPP_ENV="cae${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_DB_NAME="postgres${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_USERNAME="azuser123"

export AZ_POSTGRES_PASSWORD="P@55w.rd12345"

export AZ_POSTGRES_SERVER_NAME="psql${AZ_PROJECT_Quarkus}"

![A screen shot of a computer Description automatically
generated](./media/image39.png)

2.  切换回 Gitbash 并运行以下命令来设置资源组变量。复制资源组名称。

> export AZ_RESOURCE_GROUP="Your existing resource group"
>
> export AZ_LOCATION="Location near to you"

![A screenshot of a computer Description automatically
generated](./media/image40.png)

![A computer screen shot of text Description automatically
generated](./media/image41.png)

3.  运行 ''az login'' 它会打开默认浏览器进行登录。使用 Azure
    订阅帐户登录。

![A screenshot of a computer Description automatically
generated](./media/image42.png)

### 任务 2：创建 Azure Database for PostgreSQL 的实例

1.  现在，您将创建一个托管的 PostgreSQL 服务器。执行以下命令，创建 Azure
    Database for PostgreSQL 的小型实例。

az postgres flexible-server create --resource-group "$AZ_RESOURCE_GROUP"
--location "$AZ_LOCATION" --name "$AZ_POSTGRES_SERVER_NAME"
--database-name "$AZ_POSTGRES_DB_NAME" --admin-user
"$AZ_POSTGRES_USERNAME" --admin-password "$AZ_POSTGRES_PASSWORD"
--public-access "All" --tier "Burstable" --sku-name "Standard_B1ms"
--storage-size 32 --version "16"

![A screen shot of a computer code Description automatically
generated](./media/image43.jpeg)

2.  此命令将创建一个小型 PostgreSQL
    服务器，该服务器使用您之前设置的变量。

![A screenshot of a computer screen Description automatically
generated](./media/image44.jpeg)

### 任务 3 ： 配置 Quarkus 以访问 PostgreSQL 数据库

1.  现在，您需要将 Quarkus 应用程序连接到 PostgreSQL
    数据库。为此，您首先需要获取数据库的连接字符串：

2.  运行以下命令以获取数据库的连接字符串。

3.  出口 POSTGRES_CONNECTION_STRING=$(

> az postgres flexible-server show-connection-string --server-name
> "$AZ_POSTGRES_SERVER_NAME" --database-name "$AZ_POSTGRES_DB_NAME"
> --admin-user "$AZ_POSTGRES_USERNAME" --admin-password
> "$AZ_POSTGRES_PASSWORD" --query "connectionStrings.jdbc" --output tsv

)

export
POSTGRES_CONNECTION_STRING_SSL="$POSTGRES_CONNECTION_STRING&ssl=true&sslmode=require"

echo "POSTGRES_CONNECTION_STRING_SSL=$POSTGRES_CONNECTION_STRING_SSL"

![A computer screen with white text Description automatically
generated](./media/image45.jpeg)

1.  记下返回的连接字符串。

![A computer screen with white text Description automatically
generated](./media/image46.jpeg)

### 任务 4 ：配置 Quarkus 应用程序以连接到 PostgreSQL 数据库

1.  切换回 Intellij IDE。更新项目的 **src/main/resources 文件夹中的**
    application.properties **文件** ，以配置到 PostgreSQL
    数据库的连接字符串。

![A screenshot of a computer Description automatically
generated](./media/image47.jpeg)

2.  将 **quarkus.datasource.jdbc.url** 属性设置为之前输出的
    **\\POSTGRES_CONNECTION_STRING_SSL** 值。 连接字符串的
    **&ssl=true&sslmode=require** 部分强制驱动程序使用 SSL，这是 Azure
    Database for PostgreSQL 的要求.

quarkus.hibernate-orm.database.generation=update

quarkus.datasource.jdbc.url=\<the POSTGRES_CONNECTION_STRING_SSL value\>

![A screenshot of a computer Description automatically
generated](./media/image48.jpeg)

### 任务 5 ： 在本地运行 Quarkus 应用程序以测试远程数据库连接

1.  切换回 Gitbash 并运行以下命令以在本地运行应用程序：

./mvnw clean quarkus:dev

![A computer screen with text and images Description automatically
generated](./media/image49.jpeg)

![A screenshot of a computer program Description automatically
generated](./media/image50.jpeg)

![](./media/image51.jpeg)

2.  当 Quarkus 运行时，在单独的终端窗口中使用以下 cURL
    命令创建一些待办事项：

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Take Quarkus MS Learn","details":"Take the MS
> Learn on deploying Quarkus to Azure Container Apps","done": "true"}'
> \\

\`\`http://127.0.0.1:8080/api/todos\`\`

![A screenshot of a computer program Description automatically
generated](./media/image52.jpeg)

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description":"Take Azure Container Apps MS
Learn","details":"Take the ACA Learn module","done": "false"}' \\

\`\` http://127.0.0.1:8080/api/todos\`\`

![](./media/image53.jpeg)

3.  接下来，通过访问待办事项应用程序中定义的 GET
    端点来检查待办事项是否在数据库中：

\`\`curl http://127.0.0.1:8080/api/todos\`\`

You should see the following output:

![A computer screen with white text Description automatically
generated](./media/image54.jpeg) If 您会看到此输出，您已成功运行 Quarkus
应用程序并连接到远程 PostgreSQL 数据库。

## 练习 3：将 Quarkus 应用程序部署到 Azure 容器应用程序

在本练习中，您将使用 Azure CLI 创建 Azure 容器应用环境。

### 任务 1：为 Quarkus 应用程序设置 Dockerfile

1.  容器应用程序用于部署容器化应用程序。因此，您首先需要将 Quarkus
    应用程序容器化为 Docker 镜像。这个过程很简单，因为 Quarkus Maven
    插件已经在 **src/main/docker 下生成了一些 Dockerfile**。

![A screenshot of a computer Description automatically
generated](./media/image55.jpeg)

2.  切换回 Gitbash 并按 Ctrl+C 。运行以下命令，将其中一个 ***Dockerfile
    Dockerfile.jvm*** 重命名为 ***Dockerfile***，并将其移动到根文件夹：

\`\`mv src/main/docker/Dockerfile.jvm ./Dockerfile\`\`

![A black screen with white text Description automatically
generated](./media/image56.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image55.jpeg)

3.  将 Dockerfile **中长注释后的内容替换为** 以下内容，即第 \# 行 80 处

> FROM registry.access.redhat.com/ubi8/openjdk-17:1.18
>
> ENV LANGUAGE='en_US:en'
>
> \# We make four distinct layers so if there are application changes
> the library layers can be re-used
>
> COPY --chown=185 target/quarkus-app/lib/ /deployments/lib/
>
> COPY --chown=185 target/quarkus-app/\*.jar /deployments/
>
> COPY --chown=185 target/quarkus-app/app/ /deployments/app/
>
> COPY --chown=185 target/quarkus-app/quarkus/ /deployments/quarkus/
>
> EXPOSE 8080
>
> USER 185
>
> ENV JAVA_OPTS_APPEND="-Dquarkus.http.host=0.0.0.0
> -Djava.util.logging.manager=org.jboss.logmanager.LogManager"
>
> ENV JAVA_APP_JAR="/deployments/quarkus-run.jar"

ENTRYPOINT \[ "/opt/jboss/container/java/run/run-java.sh" \]

![A screenshot of a computer Description automatically
generated](./media/image57.jpeg)

4.  此 Dockerfile 期望将 Quarkus 应用程序打包为 ***quarkus-run.jar*
    文件**。此名称是 Quarkus 应用程序打包为 JAR
    文件时的默认名称。您需要确保 Quarkus 应用程序打包为 JAR
    文件。为此，请运行以下 Maven 命令：

\`\`./mvnw package\`\`

![A computer screen with white text Description automatically
generated](./media/image58.jpeg)

![](./media/image59.jpeg)

5.  此命令将 Quarkus 应用程序打包成一个 JAR 文件，并在
    ***target/quarkus-app*** 文件夹中生成一个 ***quarkus-run.jar***
    文件。

![A screenshot of a computer Description automatically
generated](./media/image60.jpeg)

### 任务 2：创建容器应用环境并部署容器

1.  现在，Dockerfile 位于正确的位置，可以使用单个 Azure CLI
    命令创建容器应用环境并部署容器。在项目的根目录下运行以下命令:

az containerapp up --name "$AZ_CONTAINERAPP" --environment
"$AZ_CONTAINERAPP_ENV" --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --ingress external --target-port 8080 --source .

![A screenshot of a computer program Description automatically
generated](./media/image61.jpeg)

![A screenshot of a computer program Description automatically
generated](./media/image61.jpeg)

![A screenshot of a computer program Description automatically
generated](./media/image62.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image63.jpeg)

2.  此命令执行以下几项作：

    - 如果容器应用环境不存在，则创建该环境

    - 如果 Azure 注册表不存在，则创建 Azure 注册表

    - 如果 Log Analytics 工作区不存在，则创建 Log Analytics 工作区

    - 生成 Docker 映像并将其推送到 Azure 注册表

    - 将 Docker 镜像部署到容器应用程序环境

az containerapp up
命令需要一些时间才能运行。您应该会看到类似于以下内容的输出：

![A computer screen with white text Description automatically
generated](./media/image64.jpeg)

### 任务 3：验证部署

您可以通过多种方式验证部署是否成功。最简单的方法是在 Azure
门户上搜索资源组。您应该会看到类似于以下内容的资源：

1.  打开浏览器并转到 ''https：\\portal.azure.com'' 并使用 Azure
    订阅帐户登录。单击 Resource Group 磁贴。

![A screenshot of a computer Description automatically
generated](./media/image65.jpeg)

2.  单击资源组名称。

![](./media/image66.png)

![A screenshot of a computer Description automatically
generated](./media/image67.png)

3.  您还可以通过运行以下命令来检查部署。它列出了 az containerapp up
    命令创建的所有资源。

az resource list --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --output table

您应该会看到类似于以下内容的输出：

![A screenshot of a computer program Description automatically
generated](./media/image68.png)

### 任务 4 ： 运行已部署的 Quarkus 应用程序

1.  您现在可以运行已部署的 Quarkus 应用程序。首先，您需要获取应用程序的
    URL。

2.  切换回 Gitbash 并运行以下命令以获取应用程序的 URL。

> export AZ_APP_URL=$(
>
> az containerapp show \\
>
> --name "$AZ_CONTAINERAPP" \\
>
> --resource-group "$AZ_RESOURCE_GROUP" \\
>
> --query "properties.configuration.ingress.fqdn" \\
>
> --output tsv \\ )

\`\`echo "AZ_APP_URL=$AZ_APP_URL"\`\`

![A computer screen with white text Description automatically
generated](./media/image69.jpeg)

3.  您的应用程序已在 https://\\app-name\>.azurecontainerapps.io/
    中准备就绪。请注意 https 协议。使用该协议是因为应用程序是使用 TLS
    证书部署的。要测试应用程序，您可以使用 cURL:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Configuration","details":"Congratulations, you
> have set up your Quarkus application correctly!","done": "true"}' \\

\`\` https://$AZ_APP_URL/api/todos\`\`

![A computer screen with white text Description automatically
generated](./media/image70.jpeg)

4.  使用新的 cURL 请求检索数据：

\`\`curl https://$AZ_APP_URL/api/todos\`\`

5.  此命令返回数据库中所有待办事项的列表：

![A computer screen with white text Description automatically
generated](./media/image71.jpeg)

6.  切换回 Azure 门户，然后单击容器应用名称。

![](./media/image72.png)

7.  单击应用程序 URL 链接。它会在浏览器选项卡中打开应用程序。

![](./media/image73.png)

![A screenshot of a computer Description automatically
generated](./media/image74.png)

8.  运行此命令，您可以在创建新的待办事项时流式传输容器的日志：

az containerapp logs show --name "$AZ_CONTAINERAPP" --resource-group
"$AZ_RESOURCE_GROUP" -–follow

![A screenshot of a computer screen Description automatically
generated](./media/image75.png)

9.  运行更多 cURL 命令。您应该会看到日志在终端中滚动。

\`\`curl https://$AZ_APP_URL/api/todos\`\`

![A screenshot of a computer screen Description automatically
generated](./media/image76.png)

## 练习 4 ：删除资源组中的资源

### 任务 1 ：删除资源。

1.  切换回 Azure 门户。单击 **Resource groups**.

![](./media/image77.png)

2.  单击 Resource Group name。

![](./media/image78.png)

3.  选择所有资源，然后单击 **Delete** （不要删除 – 资源组）

![](./media/image79.png)

4.  输入 ''delete'' 然后点击 **Delete**。

![](./media/image80.png)

5.  确认删除资源 。

![A screenshot of a computer Description automatically
generated](./media/image81.png)

**总结**

\[你学会了如何使用Maven来引导应用程序和使用集成开发环境（IDE）来编辑代码。您学习了如何使用
Docker 启动本地 PostgreSQL
数据库，以便在本地运行和测试应用程序。您已成功运行 Quarkus
应用程序并连接到远程 PostgreSQL 数据库。
