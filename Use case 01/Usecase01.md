# 用例 01 - 將 Quarkus Todo 應用程序整合到容器應用程序中，並將它們與關系數據庫集成。

**目的：**

此用例展示了如何在連接到 Azure Database for PostgreSQL
的容器應用中開發、配置和部署安全的 Quarkus To-d-
應用程序。完成後，你將在 Linux 上的 Azure 應用服務上運行一個 Quarkus
應用。

**使用的關鍵技術**-- Java 17, Azure Database for PostgreSQL

**預計持續時間**-- 45 分鐘

**實驗類型：** 講師指導

### 任務 0：設置環境變量

1.  從 Windows 開始菜單中搜索 環境變量 ，然後選擇 編輯系統環境變量。

![](./media/image1.jpeg)

2.  單擊 **Environment Variable** 按鈕。

![](./media/image2.jpeg)

3.  在 **Admin 的 User variable （用戶變量**） 下選擇 **JAVA_HOME**
    ，然後單擊 **Edit （編輯**）。

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

4.  輸入變量值 **C：\Program Files\Java\jdk-17**，然後單擊 **Ok**。

![](./media/image4.jpeg)

5.  導航到文件夾 **C：\Software**
    並右鍵單擊**apache-maven-3.9.4-bin.zip**文件夾並選擇 **全部提取**.

![](./media/image5.jpeg)

6.  **在同一文件夾中提取**。

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

7.  切換回 Edit Environment variable 窗口，選擇 **MAVEN_HOME**然後單擊
    **Edit**。

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

8.  將 Variable （變量）
    值輸入為\`C:\Software\apache-maven-3.9.4-bin\apache-maven-3.9.4\` 然後點擊
    **OK**.

![A screenshot of a computer Description automatically
generated](./media/image8.jpeg)

9.  在 **Environmental Variable**s window, 點擊 **Ok
    （確定**），然後再次單擊 **OK（確定**）。

![](./media/image9.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image10.jpeg)

9.  在運行 lab 之前，請根據 lab 要求**更新** JAVA_HOME。

## 練習 1：使用 Maven 生成 Quarkus 應用程序

有幾種方法可以生成 Quarkus 項目結構。您可以使用 Quarkus Web 界面、IDE
插件或 Quarkus Maven 插件。讓我們使用 Maven 插件來生成項目結構。

您生成具有多個依賴項的應用程序：

1.  用於公開 REST 端點的 resteasy 依賴項

2.  用於序列化和反序列化 JSON 的 jackson 依賴項

3.  用於與數據庫交互的 hibernate 依賴項

4.  用於連接到 PostgreSQL 數據庫的 postgresql 依賴項

5.  用於構建 Docker 鏡像的 docker 依賴項

無需指定 Azure
依賴項，因為先在本地運行應用程序，然後將其容器化版本部署到 Azure
容器應用。

### 任務 1 ：生成 Quarkus 應用程序

1.  從 Window 開始菜單打開 **Git Bash** 並運行以下命令

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

2.  此命令將創建一個新的 Quarkus 項目。它生成一個 Maven
    目錄結構（src/main/java 用於源代碼，src/test/java
    用於測試）。它會創建一些 Java 類、一些測試和一些
    Dockerfile。它還會生成一個 *pom.xml*
    文件，其中包含所有需要的依賴項（Hibernate、RESTEasy、Jackson、PostgreSQL
    和 Docker）：

3.  單擊搜索並輸入\`\`IntelliJ IDE\`\` ，然後選擇 **IntelliJ IDE**

![A screenshot of a computer Description automatically
generated](./media/image12.jpeg)

4.  選中確認複選框，然後單擊 **Continue** 按鈕。

![A screenshot of a computer screen Description automatically
generated](./media/image13.jpeg)

5.  關閉 Data sharing （數據共享） 窗口。

![](./media/image14.jpeg)

6.  選擇 **Start trial** 單選按鈕，然後單擊 **Start trial** 按鈕。

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

7.  Click on the **Allow access** button.

![](./media/image16.jpeg)

8.關閉瀏覽器，切換回 IntelliJ 許可證窗口，然後單擊 **Continue** 按鈕。

![A screenshot of a computer Description automatically
generated](./media/image17.jpeg)

8.  單擊 **Open** folder（打開文件夾）。

![](./media/image18.jpeg)

9.  瀏覽到 **C：\Users\Admin\todo** 並選擇 **todo** 項目文件夾，然後單擊
    **OK。**

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

10. 單擊 **Trust project** 按鈕.

![](./media/image20.jpeg)

11. 打開 **pom.xml**，您應該會看到下面的 xml 格式。

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

**注意** pom.xml 文件*中的所有依賴項* 都在 Quarkus BOM （物料清單）
io.quarkus.platform：quarkus-bom 中定義。

![](./media/image21.jpeg)

### 任務 2 ：對應用程序進行編碼

1.  轉到 **src/main/java/com.example.demo** 並右鍵單擊 **MyEntity.Java
    -\> Refactor -\> Rename**。

![](./media/image22.jpeg)

2.  將生成的 ***MyEntity.java*** 類重命名為 ''Todo.java'' （與
    TodoResource.java 文件位於同一文件夾中 ）

![](./media/image23.jpeg)

1.  將現有代碼替換為以下 Java 代碼。它使用 Java 持久性
    API（jakarta.persistence.\* 包）來存儲和檢索 PostgreSQL
    服務器中的數據。它還使用 \[Hibernate ORM with Panache\]{.underline}
    （繼承自 io.quarkus.hibernate.orm.panache.PanacheEntity）
    來簡化持久層。

2.  使用 JPA 實體 （@Entity） 將 Java Todo 對象直接映射到 PostgreSQL
    Todo 表。然後， TodoResource REST 端點創建一個新的 Todo
    實體類並持久化它。這個類是一個映射在 Todo 表上的域模型。該表由 JPA
    自動創建。

3.  擴展 PanacheEntity 為你的類型提供了許多通用的創建、讀取、更新和刪除
    （CRUD） 方法。因此，您只需一行 Java 代碼即可完成保存和刪除 Todo
    對象等作。

4.  在 IntelliJ 中設置 JDK（如果尚未設置）。

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

5.  將現有代碼替換為 Todo 實體的以下 Java 代碼：

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

6.  要管理該類，請更新 **TodoResource**，以便它可以發佈 REST 接口以使用
    HTTP 存儲和檢索數據。打開 **TodoResource**
    類並將代碼替換為以下內容：

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

### **任務 3 ：運行應用程序**

在開發模式下運行應用程序時，Docker Desktop 需要運行。這是因為 Quarkus
檢測到您需要一個 PostgreSQL 數據庫（因為 pom.xml 文件中聲明的 PostgreSQL
依賴項 quarkus-jdbc-postgresql ），下載 PostgreSQL Docker Desktop
鏡像，並使用該數據庫啟動一個容器。然後，它會自動在數據庫中創建 Todo 表。

1.  雙擊 **Docker Desktop** 並最小化窗口。確保它正在運行。（無需登錄）

![A computer screen with a white background Description automatically
generated](./media/image27.jpeg)

2.  返回 Gitbash 並使用以下命令運行 to-do 應用程序：

cd todo

./mvnw quarkus:dev

![](./media/image28.jpeg)

3.  Quarkus 應用程序應該啟動並連接到您的數據庫。您應該會看到以下輸出:

![A screenshot of a computer program Description automatically
generated](./media/image29.jpeg)

![A computer screen with text and images Description automatically
generated](./media/image30.jpeg)

![](./media/image31.jpeg)

![A screen shot of a computer program Description automatically
generated](./media/image32.jpeg)

4.  單擊 **Allow access**。

![](./media/image31.jpeg)

5.  要測試應用程序，您可以使用 cURL。

在 Gitbash
的單獨新實例中，使用以下命令在數據庫中創建新的待辦事項。您應該會在
Quarkus 控制台中看到日誌：

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description":"Take Quarkus MS Learn","details":"Take the MS
Learn on deploying Quarkus to Azure Container Apps","done": "true"}' \\

http://127.0.0.1:8080/api/todos

![A computer screen with white text Description automatically
generated](./media/image33.jpeg)

6.  此命令應返回創建的項（帶有標識符）：

![A computer screen with white text Description automatically
generated](./media/image33.jpeg)

7.  使用以下 cURL 命令創建第二個 to-do：

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Take Azure Container Apps MS
> Learn","details":"Take the ACA Learn module","done": "false"}' \\

http://127.0.0.1:8080/api/todos

![A screenshot of a computer program Description automatically
generated](./media/image34.jpeg)

8.  接下來，使用新的 cURL 請求檢索數據：

curl http://127.0.0.1:8080/api/todos

此命令返回待辦事項列表，包括您創建的項：

![](./media/image35.jpeg)

### 任務 4 ：測試應用程序

要測試應用程序，您可以使用現有的 **TodoResourceTest** 類。它需要測試
REST 端點。為了測試終端節點，它使用 \[RESTAssured\]{.underline}.

1.  切換回 Intellij 並從 **src/test/java/com.example.demo** 中打開
    TodoResourceTest **類**。將 **TodoResourceTest**
    類中的代碼替換為以下代碼：

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

2.  當您測試應用程序時，Docker Desktop 需要運行，因為 Quarkus
    檢測到它需要 PostgreSQL 數據庫進行測試。

3.  切換回 **Gitbash** 和 Ctrl +
    C。使用以下命令運行以下命令以測試應用程序：

./mvnw clean test

![A computer screen with text and images Description automatically
generated](./media/image37.jpeg)

您應該會看到類似於以下內容的輸出：

![A computer screen with text and numbers Description automatically
generated](./media/image38.jpeg)

## 練習 2 - 設置 Azure 容器應用

在本練習中，您將創建一個包含應用程序資源的 Azure 資源組。然後，使用
Azure CLI 設置 PostgreSQL 數據庫。最後，配置 Quarkus 應用程序以訪問遠程
PostgreSQL 數據庫。使用您選擇的終端運行命令。

## 任務 1 ： 準備工作環境

您需要設置一些環境變量。以下是有關您將創建的變量的一些說明：

[TABLE]

**注意：**您可以根據需要的任何方式命名 Azure 資源。本文提供了許多 Azure
資源的示例縮寫（例如，rg 表示資源組，ca 表示容器應用）。

1.  使用以下命令設置變量。請務必按照上表中所述修改值。這些環境變量在本模塊的其餘部分中使用.

**注意：PostgreSQL 僅在** Westus **中受支持** 。首先在 westus
位置嘗試，如果您有任何問題，然後在您附近的位置嘗試

export AZ_PROJECT_Quarkus="azure-deploy-quarkus-"$RANDOM

export AZ_CONTAINERAPP="ca${AZ_PROJECT_Quarkus}"

export AZ_CONTAINERAPP_ENV="cae${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_DB_NAME="postgres${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_USERNAME="azuser123"

export AZ_POSTGRES_PASSWORD="P@55w.rd12345"

export AZ_POSTGRES_SERVER_NAME="psql${AZ_PROJECT_Quarkus}"

![A screen shot of a computer Description automatically
generated](./media/image39.png)

2.  切換回 Gitbash 並運行以下命令來設置資源組變量。複製資源組名稱。

> export AZ_RESOURCE_GROUP="Your existing resource group"
>
> export AZ_LOCATION="Location near to you"

![A screenshot of a computer Description automatically
generated](./media/image40.png)

![A computer screen shot of text Description automatically
generated](./media/image41.png)

3.  運行 ''az login'' 它會打開默認瀏覽器進行登錄。使用 Azure
    訂閱帳戶登錄。

![A screenshot of a computer Description automatically
generated](./media/image42.png)

### 任務 2：創建 Azure Database for PostgreSQL 的實例

1.  現在，您將創建一個託管的 PostgreSQL 服務器。執行以下命令，創建 Azure
    Database for PostgreSQL 的小型實例。

az postgres flexible-server create --resource-group "$AZ_RESOURCE_GROUP"
--location "$AZ_LOCATION" --name "$AZ_POSTGRES_SERVER_NAME"
--database-name "$AZ_POSTGRES_DB_NAME" --admin-user
"$AZ_POSTGRES_USERNAME" --admin-password "$AZ_POSTGRES_PASSWORD"
--public-access "All" --tier "Burstable" --sku-name "Standard_B1ms"
--storage-size 32 --version "16"

![A screen shot of a computer code Description automatically
generated](./media/image43.jpeg)

2.  此命令將創建一個小型 PostgreSQL
    服務器，該服務器使用您之前設置的變量。

![A screenshot of a computer screen Description automatically
generated](./media/image44.jpeg)

### 任務 3 ： 配置 Quarkus 以訪問 PostgreSQL 數據庫

1.  現在，您需要將 Quarkus 應用程序連接到 PostgreSQL
    數據庫。為此，您首先需要獲取數據庫的連接字符串：

2.  運行以下命令以獲取數據庫的連接字符串。

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

1.  記下返回的連接字符串。

![A computer screen with white text Description automatically
generated](./media/image46.jpeg)

### 任務 4 ：配置 Quarkus 應用程序以連接到 PostgreSQL 數據庫

1.  切換回 Intellij IDE。更新項目的 **src/main/resources 文件夾中的**
    application.properties **文件** ，以配置到 PostgreSQL
    數據庫的連接字符串。

![A screenshot of a computer Description automatically
generated](./media/image47.jpeg)

2.  將 **quarkus.datasource.jdbc.url** 屬性設置為之前輸出的
    **\\POSTGRES_CONNECTION_STRING_SSL** 值。 連接字符串的
    **&ssl=true&sslmode=require** 部分強制驅動程序使用 SSL，這是 Azure
    Database for PostgreSQL 的要求.

quarkus.hibernate-orm.database.generation=update

quarkus.datasource.jdbc.url=\<the POSTGRES_CONNECTION_STRING_SSL value\>

![A screenshot of a computer Description automatically
generated](./media/image48.jpeg)

### 任務 5 ： 在本地運行 Quarkus 應用程序以測試遠程數據庫連接

1.  切換回 Gitbash 並運行以下命令以在本地運行應用程序：

./mvnw clean quarkus:dev

![A computer screen with text and images Description automatically
generated](./media/image49.jpeg)

![A screenshot of a computer program Description automatically
generated](./media/image50.jpeg)

![](./media/image51.jpeg)

2.  當 Quarkus 運行時，在單獨的終端窗口中使用以下 cURL
    命令創建一些待辦事項：

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

3.  接下來，通過訪問待辦事項應用程序中定義的 GET
    端點來檢查待辦事項是否在數據庫中：

\`\`curl http://127.0.0.1:8080/api/todos\`\`

You should see the following output:

![A computer screen with white text Description automatically
generated](./media/image54.jpeg) If 您會看到此輸出，您已成功運行 Quarkus
應用程序並連接到遠程 PostgreSQL 數據庫。

## 練習 3：將 Quarkus 應用程序部署到 Azure 容器應用程序

在本練習中，您將使用 Azure CLI 創建 Azure 容器應用環境。

### 任務 1：為 Quarkus 應用程序設置 Dockerfile

1.  容器應用程序用於部署容器化應用程序。因此，您首先需要將 Quarkus
    應用程序容器化為 Docker 鏡像。這個過程很簡單，因為 Quarkus Maven
    插件已經在 **src/main/docker 下生成了一些 Dockerfile**。

![A screenshot of a computer Description automatically
generated](./media/image55.jpeg)

2.  切換回 Gitbash 並按 Ctrl+C 。運行以下命令，將其中一個 ***Dockerfile
    Dockerfile.jvm*** 重命名為 ***Dockerfile***，並將其移動到根文件夾：

\`\`mv src/main/docker/Dockerfile.jvm ./Dockerfile\`\`

![A black screen with white text Description automatically
generated](./media/image56.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image55.jpeg)

3.  將 Dockerfile **中長注釋後的內容替換為** 以下內容，即第 \# 行 80 處

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

4.  此 Dockerfile 期望將 Quarkus 應用程序打包為 ***quarkus-run.jar*
    文件**。此名稱是 Quarkus 應用程序打包為 JAR
    文件時的默認名稱。您需要確保 Quarkus 應用程序打包為 JAR
    文件。為此，請運行以下 Maven 命令：

\`\`./mvnw package\`\`

![A computer screen with white text Description automatically
generated](./media/image58.jpeg)

![](./media/image59.jpeg)

5.  此命令將 Quarkus 應用程序打包成一個 JAR 文件，並在
    ***target/quarkus-app*** 文件夾中生成一個 ***quarkus-run.jar***
    文件。

![A screenshot of a computer Description automatically
generated](./media/image60.jpeg)

### 任務 2：創建容器應用環境並部署容器

1.  現在，Dockerfile 位於正確的位置，可以使用單個 Azure CLI
    命令創建容器應用環境並部署容器。在項目的根目錄下運行以下命令:

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

2.  此命令執行以下幾項作：

    - 如果容器應用環境不存在，則創建該環境

    - 如果 Azure 註冊表不存在，則創建 Azure 註冊表

    - 如果 Log Analytics 工作區不存在，則創建 Log Analytics 工作區

    - 生成 Docker 映像並將其推送到 Azure 註冊表

    - 將 Docker 鏡像部署到容器應用程序環境

az containerapp up
命令需要一些時間才能運行。您應該會看到類似於以下內容的輸出：

![A computer screen with white text Description automatically
generated](./media/image64.jpeg)

### 任務 3：驗證部署

您可以通過多種方式驗證部署是否成功。最簡單的方法是在 Azure
門戶上搜索資源組。您應該會看到類似於以下內容的資源：

1.  打開瀏覽器並轉到 ''https：\\portal.azure.com'' 並使用 Azure
    訂閱帳戶登錄。單擊 Resource Group 磁貼。

![A screenshot of a computer Description automatically
generated](./media/image65.jpeg)

2.  單擊資源組名稱。

![](./media/image66.png)

![A screenshot of a computer Description automatically
generated](./media/image67.png)

3.  您還可以通過運行以下命令來檢查部署。它列出了 az containerapp up
    命令創建的所有資源。

az resource list --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --output table

您應該會看到類似於以下內容的輸出：

![A screenshot of a computer program Description automatically
generated](./media/image68.png)

### 任務 4 ： 運行已部署的 Quarkus 應用程序

1.  您現在可以運行已部署的 Quarkus 應用程序。首先，您需要獲取應用程序的
    URL。

2.  切換回 Gitbash 並運行以下命令以獲取應用程序的 URL。

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

3.  您的應用程序已在 https://\\app-name\>.azurecontainerapps.io/
    中準備就緒。請注意 https 協議。使用該協議是因為應用程序是使用 TLS
    證書部署的。要測試應用程序，您可以使用 cURL:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Configuration","details":"Congratulations, you
> have set up your Quarkus application correctly!","done": "true"}' \\

\`\` https://$AZ_APP_URL/api/todos\`\`

![A computer screen with white text Description automatically
generated](./media/image70.jpeg)

4.  使用新的 cURL 請求檢索數據：

\`\`curl https://$AZ_APP_URL/api/todos\`\`

5.  此命令返回數據庫中所有待辦事項的列表：

![A computer screen with white text Description automatically
generated](./media/image71.jpeg)

6.  切換回 Azure 門戶，然後單擊容器應用名稱。

![](./media/image72.png)

7.  單擊應用程序 URL 鏈接。它會在瀏覽器選項卡中打開應用程序。

![](./media/image73.png)

![A screenshot of a computer Description automatically
generated](./media/image74.png)

8.  運行此命令，您可以在創建新的待辦事項時流式傳輸容器的日誌：

az containerapp logs show --name "$AZ_CONTAINERAPP" --resource-group
"$AZ_RESOURCE_GROUP" -–follow

![A screenshot of a computer screen Description automatically
generated](./media/image75.png)

9.  運行更多 cURL 命令。您應該會看到日誌在終端中滾動。

\`\`curl https://$AZ_APP_URL/api/todos\`\`

![A screenshot of a computer screen Description automatically
generated](./media/image76.png)

## 練習 4 ：刪除資源組中的資源

### 任務 1 ：刪除資源。

1.  切換回 Azure 門戶。單擊 **Resource groups**.

![](./media/image77.png)

2.  單擊 Resource Group name。

![](./media/image78.png)

3.  選擇所有資源，然後單擊 **Delete** （不要刪除 – 資源組）

![](./media/image79.png)

4.  輸入 ''delete'' 然後點擊 **Delete**。

![](./media/image80.png)

5.  確認刪除資源 。

![A screenshot of a computer Description automatically
generated](./media/image81.png)

**總結**

\[你學會了如何使用Maven來引導應用程序和使用集成開發環境（IDE）來編輯代碼。您學習了如何使用
Docker 啟動本地 PostgreSQL
數據庫，以便在本地運行和測試應用程序。您已成功運行 Quarkus
應用程序並連接到遠程 PostgreSQL 數據庫。
