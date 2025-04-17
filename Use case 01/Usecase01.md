# 사용 사례 01 - Quarkus Todo 애플리케이션을 Container Apps에 통합하고 관계형 데이터베이스와 통합하기

**목표:**

이 사용 사례에서는PostgreSQL용 Azure Database에 연결된 Container
Apps에서 안전한 Quarkus To-d- 애플리케이션을 개발, 구성 및 배포하는
방법을 보여 줍니다. 완료되면 Linux의 Azure App Service에서 Quarkus 앱이
실행됩니다.

**사용된 핵심 기술** -- Java 17, PostgreSQL용 Azure Database

**예상 소요 시간** -- 45 분

**실습 유형:** 강사 진행

### 작업 0: Environmental variables 설정하기

1.  Windows 시작 메뉴에서 Environmental variable 를 검색하고 Edit System
    Environment 변수 편집을 선택하세요.

![](./media/image1.jpeg)

2.  **Environment Variable** 버튼을 클릭하세요.

![](./media/image2.jpeg)

3.  **User variable for Admin**에서 **JAVA_HOME**를 선택하고**Edit**를
    클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

4.  변수 값을 **C:\Program Files\Java\jdk-17**로 입력하고**Ok**를
    클릭하세요.

![](./media/image4.jpeg)

5.  **C:\Software** 폴더로 이동하여**apache-maven-3.9.4-bin.zip** 폴더를
    마우스 오른쪽 버튼으로 클릭하고**Extract All**를 선택하세요.

![](./media/image5.jpeg)

6.  같은 폴더에서 **Extract**하세요.

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

7.  Edit Environment 변수 창으로 다시 전환하거 **MAVEN_HOME**를
    선택하고**Edit**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

8.  변수 값을
    \`C:\Software\apache-maven-3.9.4-bin\apache-maven-3.9.4\`로
    입력하고**OK**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image8.jpeg)

9.  **Environmental Variable** 창에서 **Ok**를 클릭하고 다시**OK**를
    클릭하세요.

![](./media/image9.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image10.jpeg)

10. 실습을 실행하기 전에 실습 요구 사항에 따라**JAVA_HOME**를
    업데이트하세요.

## 연습1: Maven을 사용하여 Quarkus 애플리케이션을 생성하기

Quarkus 프로젝트 구조를 생성하는 방법에는 여러 가지가 있습니다. Quarkus
웹 인터페이스, IDE 플러그인 또는 Quarkus Maven 플러그인을 사용할 수
있습니다. Maven 플러그인을 사용하여 프로젝트 구조를 생성해 보겠습니다.

여러 종속성이 있는 애플리케이션을 생성합니다:

- REST 엔드포인트를 노출하기 위한 resteasy 종속성

- JSON을 직렬화 및 역직렬화하기 위한 jackson 종속성

- 데이터베이스와 상호 작용하기 위한 hibernate 종속성

- PostgreSQL 데이터베이스에 연결하기 위한 postgresql 종속성

- Docker 이미지를 빌드하기 위한 docker 종속성

먼저 로컬에서 애플리케이션을 실행한 후 컨테이너화된 버전을 Azure
Container Apps에 배포하기 때문에 Azure 종속성을 지정할 필요가 없습니다.

### 작업 1: Quarkus 애플리케이션을 생성하기

1.  Window 시작 메뉴에서 **Git Bash**를 열고 아래 명령을 실행하세요.

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

2.  이 명령은 새 Quarkus 프로젝트를 만듭니다. Maven 디렉토리 구조 (소스
    코드의 경우 src/main/java, 테스트의 경우 src/test/java)를
    생성합니다. 일부 Java 클래스, 일부 테스트 및 일부 Dockerfile을
    생성합니다. 또한 필요한 모든 종속성(Hibernate, RESTEasy, Jackson,
    PostgreSQL 및 Docker)이 포함된 pom.xml 파일을 생성합니다:

3.  Search를 클릭하고 \`\`IntelliJ IDE\`\`를 입력하고 **IntelliJ IDE**를
    선택하세요

![A screenshot of a computer Description automatically
generated](./media/image12.jpeg)

4.  확인란을 선택하고 **Continue** 버튼을 클릭하세요.

![A screenshot of a computer screen Description automatically
generated](./media/image13.jpeg)

5.  Data sharing 창을 닫으세요.

![](./media/image14.jpeg)

6.  **Start trial** 라디오 버튼을 선택하고**Start trial** 버튼을
    클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

7.  **Allow access** 버튼을 클릭하세요.

![](./media/image16.jpeg)

8.  브라우저를 닫고 IntelliJ 라이선스 창으로 다시
    전환하고 **Continue** 버튼을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image17.jpeg)

9.  **Open** 폴더를 클릭하세요.

![](./media/image18.jpeg)

10. **C:\Users\Admin\todo**로 이동하고**todo** 프로젝트 폴더를 선택하고
    **OK**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

11. **Trust project** 버튼을 클릭하세요.

![](./media/image20.jpeg)

12. **pom.xml** 를 열면 다음xml 형식이 보입니다.

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

**참고** *pom.xml 파일*의 모든 종속성은 Quarkus BOM(Bill of Materials)에
정의되어 있습니다 io.quarkus.platform:quarkus-bom.

![](./media/image21.jpeg)

### 작업 2: 애플리케이션을 코드하기

1.  **src/main/java/com.example.demo**로 이동하고 **MyEntity.Java -\>
    Refactor -\> Rename**을 마우스 오른쪽 버튼으로 클릭하세요.

![](./media/image22.jpeg)

2.  생성된 ***MyEntity.java*** 클래스의 이름을
    \`\`**Todo.java\`\`**(TodoResource.java 파일과 같은 폴더에 있음)으로
    바꾸세요.

![](./media/image23.jpeg)

3.  기존 코드를 다음 Java 코드로 바꾸세요. Java Persistence
    API(jakarta.persistence.\* 패키지)를 사용하여 PostgreSQL 서버에서
    데이터를 저장하고 검색합니다. 또한 \[Hibernate ORM with
    Panache\]{.underline}
    (io.quarkus.hibernate.orm.panache.PanacheEntity 에서 상속)을
    사용하여 지속성 계층을 단순화합니다.

4.  JPA 엔터티(@Entity)를 사용하여 Java Todo 객체를 PostgreSQL Todo
    테이블에 직접 매핑합니다. TodoResource REST 엔드포인트는 새 Todo
    엔터티 클래스를 만들고 유지합니다. 이 클래스는 Todo 테이블에 매핑된
    도메인 모델입니다. 테이블은 JPA에 의해 자동으로 생성됩니다.

5.  PanacheEntity를 확장하면 유형에 대한 여러 가지 일반적인생성, 읽기,
    업데이트 및 삭제 (CRUD) 메서드를 사용할 수 있습니다. 따라서 Java
    코드 한 줄로 Todo 객체를 저장하고 삭제하는 것과 같은 작업을 수행할
    수 있습니다.

6.  JDK가 아직 설정되지 않은 경우 IntelliJ에서 JDK를 설정하세요.

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

7.  기존 코드를 Todo 엔터티에 대한 다음 Java 코드로 바꾸세요:

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

8.  해당 클래스를 관리하려면 HTTP 를 사용하여 데이터를 저장하고 검색하기
    위해 REST 인터페이스를 게시할 수 있도록 **TodoResource**를
    업데이트하세요 . **TodoResource** 클래스를 열고 코드를 다음으로
    바꾸세요:

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

### **작업 3: 애플리케이션을 실행하기**

개발 모드에서 애플리케이션을 실행하는 경우 Docker Desktop이 실행
중이어야 합니다. Quarkus가 PostgreSQL 데이터베이스가 필요하다는 것을
감지하고*(pom.xml* 파일에 선언된 PostgreSQL 종속성
quarkus-jdbc-postgresql로 인해 ) PostgreSQL Docker Desktop 이미지를
다운로드하고, 데이터베이스로 컨테이너를 시작하기 때문입니다.
데이터베이스에 Todo 테이블을 자동으로 생성합니다.

1.  **Docker Desktop**을 두 번 클릭하고 창을 최소화하세요. 실행 중인지
    확인하세요. (로그인 필요 없음)

![A computer screen with a white background Description automatically
generated](./media/image27.jpeg)

2.  Gitbash로 돌아가서 다음 명령을 사용하여 할 일 애플리케이션을
    실행하세요:

cd todo

./mvnw quarkus:dev

![](./media/image28.jpeg)

3.  Quarkus 애플리케이션이 시작되고 데이터베이스에 연결되어야 합니다.
    다음 출력이 표시되어야 합니다:

![A screenshot of a computer program Description automatically
generated](./media/image29.jpeg)

![A computer screen with text and images Description automatically
generated](./media/image30.jpeg)

![](./media/image31.jpeg)

![A screen shot of a computer program Description automatically
generated](./media/image32.jpeg)

4.  **Allow access**를 클릭하세요.

![](./media/image31.jpeg)

5.  애플리케이션을 테스트하기 위해cURL을 사용할 수 있습니다.

Gitbash 의 별도 새 인스턴스에서 다음 명령을 사용하여 데이터베이스에 새
할 일 항목을 만듭니다. Quarkus 콘솔에 로그가 표시되어야 합니다:

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description":"Take Quarkus MS Learn","details":"Take the MS
Learn on deploying Quarkus to Azure Container Apps","done": "true"}' \\

http://127.0.0.1:8080/api/todos

![A computer screen with white text Description automatically
generated](./media/image33.jpeg)

6.  이 명령은 생성된 항목(식별자 포함)을 반환해야 합니다.:

![A computer screen with white text Description automatically
generated](./media/image33.jpeg)

7.  다음 cURL 명령을 사용하여 두 번째 할 일을 생성하세요:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Take Azure Container Apps MS
> Learn","details":"Take the ACA Learn module","done": "false"}' \\

http://127.0.0.1:8080/api/todos

![A screenshot of a computer program Description automatically
generated](./media/image34.jpeg)

8.  새 cURL 요청을 사용하여 데이터를 검색하세요:

curl http://127.0.0.1:8080/api/todos

이 명령은 사용자가 생성한 항목을 포함하여 할 일 항목 목록을 반환합니다:

![](./media/image35.jpeg)

### 작업 4: 애플리케이션을 테스트하기

응용 프로그램을 테스트하려면 기존 **TodoResourceTest** 클래스를 사용할
수 있습니다. REST 엔드포인트를 테스트해야 합니다. 엔드포인트를
테스트하기 위해 \[RESTAssured\]{.underline}을 사용합니다.

1.  Intellij로 다시 전환하고 **src/test/java/com.example.demo**에서
    **TodoResourceTest** 클래스를 여세요. **TodoResourceTest** 클래스의
    코드를 다음 코드로 바꾸세요:

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

2.  애플리케이션을 테스트할 때 Quarkus가 테스트를 위해 PostgreSQL
    데이터베이스가 필요하다는 것을 감지하기 때문에 Docker Desktop이 실행
    중이어야 합니다.

3.  **Gitbash** 및 Ctrl + C로 다시 전환하세요 . 다음 명령을 사용하여
    애플리케이션을 테스트하려면 아래 명령을 실행하세요:

./mvnw clean test

![A computer screen with text and images Description automatically
generated](./media/image37.jpeg)

다음과 유사한 출력이 표시되어야 합니다:

![A computer screen with text and numbers Description automatically
generated](./media/image38.jpeg)

## 연습 2 - Azure Container App을 설정하기

이 연습에서는 애플리케이션에 대한 리소스를 포함하는 Azure 리소스 그룹을
생성합니다. Azure CLI를 사용하여 PostgreSQL 데이터베이스를 설정합니다.
마지막으로 원격 PostgreSQL 데이터베이스에 액세스하도록 Quarkus
애플리케이션을 구성합니다. 원하는 터미널을 사용하여 명령을 실행할 수
있습니다.

## 작업 1: 작업 환경을 준비하기

몇 가지 환경 변수를 설정해야 합니다. 다음은 만들 변수에 대한 몇 가지
참고 사항입니다:

[TABLE]

**참고:** 원하는 방식으로 Azure 리소스의 이름을 지정할 수 있습니다. 이
문서에서는 많은 Azure 리소스에 대한 약어 예제를 제공합니다(예: 리소스
그룹의 경우 rg, 컨테이너 앱의 경우 ca).

1.  다음 명령을 사용하여 변수를 설정합니다. 앞의 표에 설명된 대로 값을
    수정해야 합니다. 이러한 환경 변수는 이 모듈의 나머지 부분에서
    사용됩니다.

**참고:** PostgreSQL은 **Westus**에서만 지원됩니다 . 먼저 westus
위치에서 시도하고 문제가 있으면 가까운 위치에서 시도하세요.

export AZ_PROJECT_Quarkus="azure-deploy-quarkus-"$RANDOM

export AZ_CONTAINERAPP="ca${AZ_PROJECT_Quarkus}"

export AZ_CONTAINERAPP_ENV="cae${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_DB_NAME="postgres${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_USERNAME="azuser123"

export AZ_POSTGRES_PASSWORD="P@55w.rd12345"

export AZ_POSTGRES_SERVER_NAME="psql${AZ_PROJECT_Quarkus}"

![A screen shot of a computer Description automatically
generated](./media/image39.png)

2.  리소스 그룹 변수를 설정하기 위해 Gitbash로 다시 전환하고 아래 명령을
    실행하세요. 리소스 그룹 이름을 복사하세요.

> export AZ_RESOURCE_GROUP="Your existing resource group"
>
> export AZ_LOCATION="Location near to you"

![A screenshot of a computer Description automatically
generated](./media/image40.png)

![A computer screen shot of text Description automatically
generated](./media/image41.png)

3.  \`\`az login\`\` 을 실행하세요. 로그인할 수 있는 기본 브라우저가
    열립니다. Azure 구독 계정으로 로그인하세요.

![A screenshot of a computer Description automatically
generated](./media/image42.png)

### 작업 2: PostgreSQL용 Azure Database의 인스턴스를 생성하기

1.  이제 관리형 PostgreSQL 서버를 생성하세요. PostgreSQL용 Azure
    Database의 작은 인스턴스를 생성하기 위해 다음 명령을 실행하세요:

az postgres flexible-server create --resource-group "$AZ_RESOURCE_GROUP"
--location "$AZ_LOCATION" --name "$AZ_POSTGRES_SERVER_NAME"
--database-name "$AZ_POSTGRES_DB_NAME" --admin-user
"$AZ_POSTGRES_USERNAME" --admin-password "$AZ_POSTGRES_PASSWORD"
--public-access "All" --tier "Burstable" --sku-name "Standard_B1ms"
--storage-size 32 --version "16"

![A screen shot of a computer code Description automatically
generated](./media/image43.jpeg)

2.  이 명령은 이전에 설정한 변수를 사용하는 작은 PostgreSQL 서버를
    생성합니다.

![A screenshot of a computer screen Description automatically
generated](./media/image44.jpeg)

### 작업 3: PostgreSQL 데이터베이스를 액세스하기 위해Quarkus 를 구성하기

1.  이제 Quarkus 애플리케이션을 PostgreSQL 데이터베이스에 연결합니다.
    이렇게 하려면 먼저 데이터베이스에 대한 연결 문자열을 가져와야
    합니다:

2.  데이터베이스에 대한 연결 문자열을 가져오기 위해 아래 명령을
    실행하세요.

3.  export POSTGRES_CONNECTION_STRING=$(

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

4.  반환되는 연결 문자열을 기록해 둡니다.

![A computer screen with white text Description automatically
generated](./media/image46.jpeg)

### 작업 4: PostgreSQL 데이터베이스에 연결하도록 Quarkus애플리케이션을 구성하기

1.  Intellij IDE로 다시 전환하세요. 프로젝트의 **src/main/resources**
    폴더에서 **application.properties** 파일을 업데이트 하여 PostgreSQL
    데이터베이스에 대한 연결 문자열을 구성하세요.

![A screenshot of a computer Description automatically
generated](./media/image47.jpeg)

2.  **quarkus.datasource.jdbc.url** 속성을 이전에 출력된
    **\\POSTGRES_CONNECTION_STRING_SSL** 값으로 설정하세요. 연결
    문자열의 **&ssl=true&sslmode=require** 부분은 드라이버가
    PostgreSQL용 Azure Database에 대한 요구 사항인 SSL을 사용하도록
    합니다.

quarkus.hibernate-orm.database.generation=update

quarkus.datasource.jdbc.url=\<the POSTGRES_CONNECTION_STRING_SSL value\>

![A screenshot of a computer Description automatically
generated](./media/image48.jpeg)

### 작업 5: Quarkus 애플리케이션을 로컬에서 실행하여 원격 데이터베이스 연결을 테스트하기

1.  Gitbash로 다시 전환하고 아래 명령을 실행하여 애플리케이션을 로컬로
    실행하하세요:

./mvnw clean quarkus:dev

![A computer screen with text and images Description automatically
generated](./media/image49.jpeg)

![A screenshot of a computer program Description automatically
generated](./media/image50.jpeg)

![](./media/image51.jpeg)

2.  Quarkus가 실행 중일 때 별도의 터미널 창에서 다음 cURL 명령을
    사용하여 몇 가지 할 일을 생성하세요:

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

3.  다음으로, 할 일 앱에 정의된 GET 엔드포인트에 액세스하여 할 일이
    데이터베이스에 있는지 확인하세요:

\`\`curl http://127.0.0.1:8080/api/todos\`\`

다음 출력이 표시되어야 합니다:

![A computer screen with white text Description automatically
generated](./media/image54.jpeg) If 이 출력이 표시되면 Quarkus
애플리케이션을 성공적으로 실행하고 원격 PostgreSQL 데이터베이스에 연결한
것입니다.

## 연습 3: Quarkus 애플리케이션을 Azure Container App로 배포하기

이 연습에서는 Azure CLI를 사용하여 Azure Container Apps 환경을 생성할
것입니다.

### 작업 1: Quarkus 애플리케이션을 위해 Dockerfile을 설정하기 

1.  Container Apps는 컨테이너화된 애플리케이션을 배포하는 데 사용됩니다.
    따라서 먼저 Quarkus 애플리케이션을 Docker 이미지로 컨테이너화해야
    합니다. Quarkus Maven 플러그인이 이미 **src/main/docker** 아래에
    일부 Dockerfile을 생성했기 때문에 이 프로세스는 쉽습니다.

![A screenshot of a computer Description automatically
generated](./media/image55.jpeg)

2.  Gitbash로 다시 전환하고 Ctrl+C를 누르세요. 아래 명령을 실행하여
    이러한 **Dockerfile** 중 하나인 ***Dockerfile.jvm***의 이름을
    ***Dockerfile***로 바꾸고 루트 폴더로 이동하세요:

\`\`mv src/main/docker/Dockerfile.jvm ./Dockerfile\`\`

![A black screen with white text Description automatically
generated](./media/image56.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image55.jpeg)

3.  **Dockerfile**의 긴 주석 뒤의 내용을 다음 (예: Line \# 80)으로
    바꾸세요.

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

4.  이 Dockerfile은 Quarkus 애플리케이션이 **quarkus-run.jar** 파일로
    패키징될 것으로 예상합니다. 이 이름은 JAR 파일로 패키징될 때 Quarkus
    애플리케이션의 기본 이름입니다. Quarkus 애플리케이션이 JAR 파일로
    패키징되어 있는지 확인해야 합니다. 이렇게 하려면 다음 Maven 명령을
    실행하세요:

\`\`./mvnw package\`\`

![A computer screen with white text Description automatically
generated](./media/image58.jpeg)

![](./media/image59.jpeg)

5.  이 명령은 Quarkus 애플리케이션을 JAR 파일로 패키징하고
    ***target/quarkus-app*** 폴더에 ***quarkus-run.jar*** 파일을
    생성합니다.

![A screenshot of a computer Description automatically
generated](./media/image60.jpeg)

### 작업 2: Container Apps 환경을 생성하고 컨테이너를 배포하기

1.  이제 Dockerfile이 올바른 위치에 있으므로 단일 Azure CLI 명령을
    사용하여 Container Apps 환경을 생성하고 컨테이너를 배포할 수
    있습니다. 프로젝트의 루트에서 다음 명령을 실행하세요:

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

2.  이 명령은 다음과 같은 작업을 수행하세요:

    1.  Container Apps 환경이 없는 경우 생성합니다.

    2.  Azure 레지스트리가 없는 경우 생성합니다.

    3.  Log Analytics 작업 영역이 없는 경우 생성합니다.

    4.  Docker 이미지를 빌드하고 Azure 레지스트리에 푸시합니다.

    - Docker 이미지를 Container Apps 환경에 배포합니다.

az containerapp up 명령을 실행하는 데 다소 시간이 걸립니다. 다음과
유사한 출력이 표시되어야 합니다:

![A computer screen with white text Description automatically
generated](./media/image64.jpeg)

### 작어 3: 배포의 유효성을 검사하기

여러 가지 방법으로 배포가 성공했는지 확인할 수 있습니다. 가장 쉬운
방법은 Azure Portal에서 리소스 그룹을 검색하는 것입니다. 다음과 유사한
리소스가 표시되어야 합니다:

1.  브라우저를 열고 \`\`https:\\portal.azure.com\`\` 로 이동하고 Azure
    구독 계정으로 로그인하세요. Resource Group 타일을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image65.jpeg)

2.  Resource group 이름을 클릭하세요.

![](./media/image66.png)

![A screenshot of a computer Description automatically
generated](./media/image67.png)

3.  다음 명령을 실행하여 배포를 확인할 수도 있습니다. az containerapp up
    명령으로 만든 모든 리소스를 나열합니다.

az resource list --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --output table

이와 유사한 출력이 표시되어야 합니다:

![A screenshot of a computer program Description automatically
generated](./media/image68.png)

### 작업 4: 배포된 Quarkus 애플리케이션을 실행하기

1.  이제 배포된 Quarkus 애플리케이션을 실행할 수 있습니다. 먼저
    애플리케이션의 URL을 가져와야 합니다.

2.  Gitbash로 다시 전환하고 아래 명령을 실행하여 애플리케이션의 URL을
    가져오세요.

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

3.  애플리케이션이 https://\\app-name\>.azurecontainerapps.io/ 에서
    준비되었습니다. https 프로토콜에 주목하세요. 이 프로토콜은
    애플리케이션이 TLS 인증서와 함께 배포되기 때문에 사용됩니다.
    애플리케이션을 테스트하려면 cURL을 사용할 수 있습니다:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Configuration","details":"Congratulations, you
> have set up your Quarkus application correctly!","done": "true"}' \\

\`\` https://$AZ_APP_URL/api/todos\`\`

![A computer screen with white text Description automatically
generated](./media/image70.jpeg)

4.  새 cURL 요청을 사용하여 데이터 검색하세요:

\`\`curl https://$AZ_APP_URL/api/todos\`\`

5.  이 명령은 데이터베이스에서 모든 할 일 항목의 목록을 반환합니다:

![A computer screen with white text Description automatically
generated](./media/image71.jpeg)

6.  Azure Portal로 다시 전환하고 컨테이너 앱 이름을 클릭하세요.

![](./media/image72.png)

7.  애플리케이션 URL 링크를 클릭하세요. 브라우저 탭에서 앱을 여세요.

![](./media/image73.png)

![A screenshot of a computer Description automatically
generated](./media/image74.png)

8.  이 명령을 실행하면 새 할 일을 생성할 때 컨테이너에 대한 로그를
    스트리밍할 수 있습니다:

az containerapp logs show --name "$AZ_CONTAINERAPP" --resource-group
"$AZ_RESOURCE_GROUP" -–follow

![A screenshot of a computer screen Description automatically
generated](./media/image75.png)

9.  더 많은 cURL 명령을 실행하세요. 터미널에서 로그가 스크롤되는 것을 볼
    수 있습니다.

\`\`curl https://$AZ_APP_URL/api/todos\`\`

![A screenshot of a computer screen Description automatically
generated](./media/image76.png)

## 연습 4: 리소스 그룹의 리소스를 삭제하기

### 작업 1: 리소스를 삭제하기

1.  Azure 포털로 다시 이동하세요. **Resource groups**를 클릭하세요.

![](./media/image77.png)

2.  Resource group 이름을 클릭하세요.

![](./media/image78.png)

3.  모든 리소스를 선택하고 **Delete**를 클릭하세요 (Do NOT DELETE –
    리소스 그룹)

![](./media/image79.png)

4.  \`\`delete\`\` 를 입력하고 **Delete**를 클릭하세요.

![](./media/image80.png)

5.  리소스 삭제 확인하세요.

![A screenshot of a computer Description automatically
generated](./media/image81.png)

**요약**

\[Maven을 사용하여 애플리케이션을 부트스트랩하고 integrated development
environment (IDE)를 사용하여 코드를 편집하는 방법을 배웠습니다. 로컬에서
애플리케이션을 실행하고 테스트할 수 있도록 Docker를 사용하여 로컬
PostgreSQL 데이터베이스를 시작하는 방법을 배웠습니다. Quarkus
애플리케이션을 성공적으로 실행하고 원격 PostgreSQL 데이터베이스에
연결했습니다.
