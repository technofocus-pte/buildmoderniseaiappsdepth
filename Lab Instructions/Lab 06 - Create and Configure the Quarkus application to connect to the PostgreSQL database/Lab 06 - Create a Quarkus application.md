# Lab 06 - **Create a Quarkus application**

In this unit, you create a basic Quarkus application. You use Maven to
bootstrap the application and an integrated development environment
(IDE) of your choice to edit the code. Use a terminal of your choice to
run the code. You use Docker to start a local PostgreSQL database so you
can run and test the application locally.

Software -jdk 17

## **Exercise 1 : Generate the Quarkus application by using Maven**

There are several ways to generate a Quarkus project structure. You can
use the [<u>Quarkus web interface</u>](https://code.quarkus.io/), an IDE
plugin, or the Quarkus Maven plugin. Let's use the Maven plugin to
generate the project structure.

You generate your application with several dependencies:

- The resteasy dependency to expose a REST endpoint

- The jackson dependency to serialize and deserialize JSON

- The hibernate dependency to interact with the database

- The postgresql dependency to connect to the PostgreSQL database

- The docker dependency to build a Docker image

You don't need to specify Azure dependencies because you run your
application locally first and then deploy a containerized version of it
to Azure Container Apps.

1.  Open Gitbahs and run 

>```copy
>az login --use-device-code
>

<img src="./media/image1.png" style="width:6.5in;height:1.20208in" />

2.  Copy the url and paste it in the browser and enter the generated
    code.

<img src="./media/image2.png"
style="width:5.20878in;height:3.70032in" />

3.  Sign in with your Azure slice credentials.

<img src="./media/image3.png"
style="width:5.07708in;height:4.89306in" />

4.  Click on **Continue**.

<img src="./media/image4.png"
style="width:4.82708in;height:4.11319in" />

5.  Set JAVA_HOME to jdk for this instance of lab.

<img src="./media/image5.png" style="width:5.11711in;height:1.24177in"
alt="A black screen with white text and purple text Description automatically generated" />

6.  Switch back to **Gitbash** and run below command

>```copy
>mvn -U io.quarkus:quarkus-maven-plugin:3.7.3:create \
>
>-DplatformVersion=3.7.3 \
>
>-DprojectGroupId=com.example.demo \
>
>-DprojectArtifactId=todo \
>
>-DclassName="com.example.demo.TodoResource" \
>
>-Dpath="/api/todos" \
>
>-DjavaVersion=17 \
>
>-Dextensions="resteasy-jackson, hibernate-orm-panache, jdbc-postgresql, docker"
>

<img src="./media/image6.png" style="width:6.5in;height:1.84861in"
alt="A screen shot of a computer program Description automatically generated" />

<img src="./media/image7.png" style="width:6.5in;height:4.46667in"
alt="A screenshot of a computer Description automatically generated" />

7.  This command creates a new Quarkus project. It generates a Maven
    directory structure (src/main/java for source code
    and src/test/java for tests). It creates some Java classes, some
    tests, and some Dockerfiles. It also generates a *pom.xml* file with
    all the needed dependencies (Hibernate, RESTEasy, Jackson,
    PostgreSQL, and Docker):

8.  In Windows start,search for !!**IntelliJ IDE**!! and then select it.

> <img src="./media/image8.png" style="width:6.49167in;height:5.025in" />

9.  Ignore steps10-12 if you already provisioned trial version and jump
    to Step 13

10. Select the **Start trial** radio button and then click on the
    **Start trial** button.

<img src="./media/image9.png" style="width:6.5in;height:4.81667in" />

11. Click on the **Allow access** button.

<img src="./media/image10.png" style="width:4.925in;height:3.59167in" />

12. Close the browser, switch back to IntelliJ license window and click
    on the **Continue** button.

<img src="./media/image11.png" style="width:6.5in;height:4.35in" />

13. Click on **Open folder**.

> <img src="./media/image12.png" style="width:6.5in;height:5.3in" />

14. Browse to **C:\Users\Admin\todo** and select **todo** project folder
    and then click on **OK**.

<img src="./media/image13.png" style="width:6.5in;height:5.025in" />

15. Click on the **Trust project** button.

<img src="./media/image14.png"
style="width:6.49167in;height:5.16667in" />

16. Open **pom.xml** and you should see below xml format.


        <dependencies>

        <dependency>

        <groupId>io.quarkus</groupId>

        <artifactId>quarkus-hibernate-orm-panache</artifactId>

        </dependency>

        <dependency>

        <groupId>io.quarkus</groupId>

        <artifactId>quarkus-resteasy-jackson</artifactId>

        </dependency>

        <dependency>

        <groupId>io.quarkus</groupId>

        <artifactId>quarkus-jdbc-postgresql</artifactId>

        </dependency>

        <dependency>

        <groupId>io.quarkus</groupId>

        <artifactId>quarkus-container-image-docker</artifactId>

        </dependency>

        <dependency>

        <groupId>io.quarkus</groupId>

        <artifactId>quarkus-arc</artifactId>

        </dependency>

        <dependency>

        <groupId>io.quarkus</groupId>

        <artifactId>quarkus-hibernate-orm</artifactId>

        </dependency>

        <dependency>

        <groupId>io.quarkus</groupId>

        <artifactId>quarkus-resteasy</artifactId>

        </dependency>

        <dependency>

        <groupId>io.quarkus</groupId>

        <artifactId>quarkus-junit5</artifactId>

        <scope>test</scope>

        </dependency>

        <dependency>

        <groupId>io.rest-assured</groupId>

        <artifactId>rest-assured</artifactId>

        <scope>test</scope>

        </dependency>

        </dependencies>

** Note**

All the dependencies in the *pom.xml* file are defined in the Quarkus
BOM (bill of materials) io.quarkus.platform:quarkus-bom.

<img src="./media/image15.png" style="width:6.5in;height:3.44444in"
alt="A screenshot of a computer program Description automatically generated" />

### **Task 2 : Code the application**

1.  Go to **src/main/java/com.example.demo** and right-click on
    **MyEntity.Java -\> Refactor -\> Rename**.

<img src="./media/image16.png" style="width:6.5in;height:4.54167in" />

2.  Rename the generated ***MyEntity.java*** class
    to ***Todo*** (located in the same folder as
    the *TodoResource.java* file)

<img src="./media/image17.png"
style="width:6.49167in;height:4.49167in" />

3.  Replace the existing code with the following Java code. It uses Java
    Persistence API (jakarta.persistence.\* package) to store and
    retrieve data from your PostgreSQL server. It also uses **Hibernate
    ORM with Panache** (inheriting
    from io.quarkus.hibernate.orm.panache.PanacheEntity) to simplify the
    persistence layer.

4.  You use a JPA entity (@Entity) to map the Java Todo object directly
    to the PostgreSQL Todo table. The TodoResource REST endpoint then
    creates a new Todo entity class and persists it. This class is a
    domain model that's mapped on the Todo table. The table is
    automatically created by JPA.

5.  Extending PanacheEntity gets you a number of generic create, read,
    update, and delete (CRUD) methods for your type. So you can do
    things like saving and deleting Todo objects in just one line of
    Java code.

6.  Setup JDK in IntelliJ if it was not already set.

<img src="./media/image18.png" style="width:6.5in;height:2.025in"
alt="A screenshot of a computer Description automatically generated" />

7.  Replace the existing code with the following Java code to
    the **Todo** entity:

>```copy
>package com.example.demo;
>
>import io.quarkus.hibernate.orm.panache.PanacheEntity;
>
>import jakarta.persistence.Entity;
>
>import java.time.Instant;
>
>@Entity
>
>public class Todo extends PanacheEntity {
>
>public String description;
>
>public String details;
>
>public boolean done;
>
>public Instant createdAt = Instant.now();
>
>@Override
>
>public String toString() {
>
>return "Todo{" +
>
>"id=" + id + '\' +
>
>", description='" + description + '\' +
>
>", details='" + details + '\' +
>
>", done=" + done +
>
>", createdAt=" + createdAt +
>
>'}';
>
>}
>
>}
>

<img src="./media/image19.png" style="width:6.5in;height:4.50486in"
alt="A screenshot of a computer program Description automatically generated" />

8.  To manage that class, update the **TodoResource** so that it can
    publish REST interfaces to store and retrieve data by using HTTP.
    Open the **TodoResource** class and replace the code with the
    following:

>```copy
>package com.example.demo;
>
>import jakarta.inject.Inject;
>
>import jakarta.transaction.Transactional;
>
>import jakarta.ws.rs.Consumes;
>
>import jakarta.ws.rs.GET;
>
>import jakarta.ws.rs.POST;
>
>import jakarta.ws.rs.Path;
>
>import jakarta.ws.rs.Produces;
>
>import static jakarta.ws.rs.core.MediaType.APPLICATION_JSON;
>
>import jakarta.ws.rs.core.Response;
>
>import jakarta.ws.rs.core.UriBuilder;
>
>import jakarta.ws.rs.core.UriInfo;
>
>import org.jboss.logging.Logger;
>
>import java.util.List;
>
>@Path("/api/todos")
>
>@Consumes(APPLICATION_JSON)
>
>@Produces(APPLICATION_JSON)
>
>public class TodoResource {
>
>@Inject
>
>Logger logger;
>
>@Inject
>
>UriInfo uriInfo;
>
>@POST
>
>@Transactional
>
>public Response createTodo(Todo todo) {
>
>logger.info("Creating todo: " + todo);
>
>Todo.persist(todo);
>
>UriBuilder uriBuilder =
>uriInfo.getAbsolutePathBuilder().path(todo.id.toString());
>
>return Response.created(uriBuilder.build()).entity(todo).build();
>
>}
>
>@GET
>
>public List\<Todo\> getTodos() {
>
>logger.info("Getting all todos");
>
>return Todo.listAll();
>
>}
>
>}
>

<img src="./media/image20.png" style="width:6.5in;height:4.85in"
alt="A screenshot of a computer program Description automatically generated" />

### **Task 3 : Run the application**

When you run the application in development mode, Docker needs to be
running. That's because Quarkus detects that you need a PostgreSQL
database (because of the PostgreSQL
dependency quarkus-jdbc-postgresql declared in the *pom.xml* file),
downloads the PostgreSQL Docker image, and starts a container with the
database. It then automatically creates the Todo table in the database.

1.  Double-click on **Docker Desktop**.

<img src="./media/image21.png" style="width:6.5in;height:3.425in" />

2.  Go back to **Gitbash** and run the to-do application by using this
    command:

>```copy
>cd todo/
>

>```copy
>./mvnw quarkus:dev
>

>Note : ./mvnw quarkus:dev \# On Mac or Linux

>mvnw.cmd quarkus:dev \# On Windows

<img src="./media/image22.png" style="width:6.5in;height:3.72083in"
alt="A screen shot of a computer Description automatically generated" />

The Quarkus application should start and connect to your database. You
should see the following output:

<img src="./media/image23.png" style="width:6.5in;height:4.73611in" />

<img src="./media/image24.png" style="width:6.5in;height:1.93333in"
alt="A screenshot of a computer program Description automatically generated" />

<img src="./media/image25.png"
style="width:6.49375in;height:4.30347in" />

<img src="./media/image26.png" style="width:6.5in;height:4.55972in"
alt="A screenshot of a computer program Description automatically generated" />

3.  Open separate Gitbahs terminal, and create a new to-do item in the
    database with the following command. You should see the log in the
    Quarkus console:

>```copy
>curl --header "Content-Type: application/json"
>
>--request POST \\
>
>--data '{"description":"Take Quarkus MS Learn","details":"Take the MS Learn on deploying Quarkus to Azure Container Apps","done": "true"}' \
>
><http://127.0.0.1:8080/api/todos>
>

<img src="./media/image27.png" style="width:6.5in;height:1.52847in" />

This command should return the created item (with an identifier):

<img src="./media/image28.png" style="width:6.5in;height:1.56667in"
alt="A computer screen with white text Description automatically generated" />

4.  Create a second to-do by using the following cURL command:

>```copy
>curl --header "Content-Type: application/json" \
>
>--request POST \
>
>--data '{"description":"Take Azure Container Apps MS Learn","details":"Take the ACA Learn module","done": "false"}' \
>
><http://127.0.0.1:8080/api/todos>
>

<img src="./media/image29.png" style="width:6.5in;height:2.85in" />

5.  Next, retrieve the data by using a new cURL request:

>```copy
>curl http://127.0.0.1:8080/api/todos
>


This command returns the list of to-do items, including the items you
created:

<img src="./media/image30.png" style="width:6.5in;height:3.28056in" />

### **Task 4 : Test the application**

To test the application, you can use the
existing **TodoResourceTest** class. It needs to test the REST endpoint.
To test the endpoint, it
uses [<u>RESTAssured</u>](https://rest-assured.io/).

1.  Switch back to Intellij and open **TodoResourceTest**  class from
    **src/test/java/com.example.demo**. Replace code in
    the **TodoResourceTest** class with the following code:

>```copy
>package com.example.demo;
>
>import io.quarkus.test.junit.QuarkusTest;
>
>import static io.restassured.RestAssured.given;
>
>import static jakarta.ws.rs.core.HttpHeaders.CONTENT_TYPE;
>
>import static jakarta.ws.rs.core.MediaType.APPLICATION_JSON;
>
>import org.junit.jupiter.api.Test;
>
>@QuarkusTest
>
>class TodoResourceTest {
>
>@Test
>
>void shouldGetAllTodos() {
>
>given()
>
>.when().get("/api/todos")
>
>.then()
>
>.statusCode(200);
>
>}
>
>@Test
>
>void shouldCreateATodo() {
>
>Todo todo = new Todo();
>
>todo.description = "Take Quarkus MS Learn";
>
>todo.details = "Take the MS Learn on deploying Quarkus to Azure Container Apps";
>
>todo.done = true;
>
>given().body(todo)
>
>.header(CONTENT_TYPE, APPLICATION_JSON)
>
>.when().post("/api/todos")
>
>.then()
>
>.statusCode(201);
>
>}
>
>}
>

<img src="./media/image31.png" style="width:6.5in;height:3.33472in"
alt="A screenshot of a computer program Description automatically generated" />

2.  When you test the application, Docker Desktop needs to be running
    because Quarkus detects that it needs the PostgreSQL database for
    testing.

3.  Switch back to **Gitbash** and Ctrl + C. Run below commands to test
    the application by using this command:

>```copy
>./mvnw clean test
>

<img src="./media/image32.png" style="width:6.5in;height:2.90278in"
alt="A screen shot of a computer program Description automatically generated" />

>Note : mvnw.cmd clean test \# On Windows

You should see output that looks similar to this:

<img src="./media/image33.png" style="width:6.5in;height:4.725in" />

## **Exercise 2 - Set up Azure Container Apps**

In this unit, you create an Azure resource group that contains the
resources for the application. You then set up the PostgreSQL database
by using the Azure CLI. Finally, you configure the Quarkus application
to access the remote PostgreSQL database. Use a terminal of your choice
to run the commands.

### **Task 1 : Prepare the working environment**

You need to set up some environment variables. Here are some notes about
the variables you'll create:

Expand table

| **Variable**            |     | **Description**                                                                                                                                                                 |
|-------------------------|-----|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AZ_PROJECT              |     | The name of the project. To keep this value unique, we recommend that you use AZ_PROJECT\_\<your initials\>.                                                                    |
| AZ_RESOURCE_GROUP       |     | The name of the resource group that holds the resources.                                                                                                                        |
| AZ_LOCATION             |     | The Azure region. We recommend that you use a region that's close to where you live. To see the list of available regions, enter az account list-locations at a command prompt. |
| AZ_CONTAINERAPP         |     | The name of the Azure Container Apps instance that holds the containers.                                                                                                        |
| AZ_CONTAINERAPP_ENV     |     | The name of the Azure Container Apps environment.                                                                                                                               |
| AZ_POSTGRES_SERVER_NAME |     | The name of your PostgreSQL server. Nonalphanumeric characters aren't allowed: -, \_, !, \$, \#, %. The name should be unique across Azure. Be sure to use a unique identifier. |
| AZ_POSTGRES_DB_NAME     |     | The PostgreSQL database name. The default name of the PostgreSQL database is postgres.                                                                                          |
| AZ_POSTGRES_USERNAME    |     | The default admin user name for your PostgreSQL database server.                                                                                                                |
| AZ_POSTGRES_PASSWORD    |     | The default password for your PostgreSQL database server. Use a secure password.                                                                                                |

 

>**Note :** You can name your Azure resources in any way that you want,
but we recommend that you review [**<u>Abbreviation examples for Azure
resources</u>**](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations).
This article provides example abbreviations for many Azure resources
(for example, rg for resource groups and ca for container apps).

Use the following commands to set up the variables. Be sure to modify
the values as described in the preceding table. These environment
variables are used throughout the rest of this module.

>```copy
>export AZ_PROJECT_Quarkus="azure-deploy-quarkus-"$RANDOM
>
>export AZ_RESOURCE_GROUP=Your Azure cloud slice resource group
>
>export AZ_LOCATION="eastus"
>
>export AZ_CONTAINERAPP="ca${AZ_PROJECT_Quarkus}"
>
>export AZ_CONTAINERAPP_ENV="cae\${AZ_PROJECT_Quarkus}"
>
>export AZ_POSTGRES_DB_NAME="postgres\${AZ_PROJECT_Quarkus}"
>
>export AZ_POSTGRES_USERNAME="azuser123"
>
>export AZ_POSTGRES_PASSWORD="P@55w.rd12345"
>
>export AZ_POSTGRES_SERVER_NAME="psql\${AZ_PROJECT_Quarkus}"
>

<img src="./media/image34.png" style="width:6.5in;height:3.27569in" />

### **Task 2 : Create an instance of Azure Database for PostgreSQL**

1.  You'll now create a managed PostgreSQL server. Run the following
    command to create a small instance of Azure Database for PostgreSQL:

>```copy
>az postgres flexible-server create --resource-group "$AZ_RESOURCE_GROUP" --location "$AZ_LOCATION" --name "$AZ_POSTGRES_SERVER_NAME" --database-name "$AZ_POSTGRES_DB_NAME" --admin-user "$AZ_POSTGRES_USERNAME" --admin-password "$AZ_POSTGRES_PASSWORD" --public-access "All" --tier "Burstable" --sku-name "Standard_B1ms" --storage-size 32 --version "16"
>

<img src="./media/image35.png" style="width:6.5in;height:1.74653in" />

This command creates a small PostgreSQL server that uses the variables
that you set up earlier.

<img src="./media/image36.png" style="width:6.5in;height:4.79236in" />

### **Task 3 : Configure Quarkus to access the PostgreSQL database**

1.  You'll now connect the Quarkus application to the PostgreSQL
    database. To do so, you first need to obtain the connection string
    for the database:

2.  Run below command to obtain the connection string for the database.

>```copy
>export POSTGRES_CONNECTION_STRING=$(
    az postgres flexible-server show-connection-string --server-name "$AZ_POSTGRES_SERVER_NAME" --database-name "$AZ_POSTGRES_DB_NAME" --admin-user "$AZ_POSTGRES_USERNAME" --admin-password "$AZ_POSTGRES_PASSWORD" --query "connectionStrings.jdbc" --output tsv
)
>

>```copy
>export POSTGRES_CONNECTION_STRING_SSL="$POSTGRES_CONNECTION_STRING&ssl=true&sslmode=require"
>

>```copy
>echo "POSTGRES_CONNECTION_STRING_SSL=$POSTGRES_CONNECTION_STRING_SSL"
>

<img src="./media/image37.png" style="width:6.5in;height:2.95in"
alt="A computer screen with white text Description automatically generated" />

3.  Note the connection string that's returned.

<img src="./media/image38.png"
style="width:6.49375in;height:2.95208in" />

### **Task 4 :Configure the Quarkus application to connect to the PostgreSQL database**

1.  Switch back to **Intellij IDE**. Update
    the **application.properties** file in
    the **src/main/resources** folder of the project to configure the
    connection string to the PostgreSQL database.

> <img src="./media/image39.png" style="width:6.5in;height:2.92431in"
> alt="A screenshot of a computer Description automatically generated" />

2.  Set the **quarkus.datasource.jdbc.url** property to the previously
    output **$POSTGRES_CONNECTION_STRING_SSL** value.
    The **&ssl=true&sslmode=require** part of the connection string
    forces the driver to use SSL, a requirement for Azure Database for
    PostgreSQL.

>```copy
>quarkus.hibernate-orm.database.generation=update
>
>quarkus.datasource.jdbc.url=<the POSTGRES_CONNECTION_STRING_SSL value>
>

<img src="./media/image40.png" style="width:6.5in;height:1.87708in"
alt="A screenshot of a computer Description automatically generated" />

### **Task 5 : Run the Quarkus application locally to test the remote
database connection**

1.  Switch back to Gitbash and run below command to run the application
    locally:

>```copy
>./mvnw clean quarkus:dev
>

>Note : mvnw.cmd clean quarkus:dev \# On Mac or Linux

<img src="./media/image41.png" style="width:6.5in;height:3.52083in"
alt="A computer screen with text and images Description automatically generated" />

<img src="./media/image42.png" style="width:6.5in;height:4.90972in"
alt="A screenshot of a computer program Description automatically generated" />

<img src="./media/image43.png" style="width:6.5in;height:4.50417in"
alt="A screenshot of a computer program Description automatically generated" />

2.  When Quarkus is running, create a few to-dos by using the following
    cURL commands in a separate terminal window:

>```copy
>curl --header "Content-Type: application/json" \
>
>--request POST \
>
>--data '{"description":"Take Quarkus MS Learn","details":"Take the MS Learn on deploying Quarkus to Azure Container Apps","done": "true"}' \
>
><http://127.0.0.1:8080/api/todos>
>

<img src="./media/image44.png" style="width:6.5in;height:2.58194in" />

>```copy
>curl --header "Content-Type: application/json" \
>
>--request POST \
>
>--data '{"description":"Take Azure Container Apps MS Learn","details":"Take the ACA Learn module","done": "false"}' \
>
><http://127.0.0.1:8080/api/todos>
>

<img src="./media/image45.png" style="width:6.5in;height:2.79444in"
alt="A screenshot of a computer program Description automatically generated" />

3.  Next, check that the to-dos are in the database by accessing the GET
    endpoint that's defined in the to-do app:

>```copy
>curl http://127.0.0.1:8080/api/todos
>
You should see the following output:

<img src="./media/image46.png" style="width:6.5in;height:1.92708in"
alt="A screenshot of a computer program Description automatically generated" />

If you see this output, you have successfully run the Quarkus
application and connected to the remote PostgreSQL database.

## Exercise 3 : Deploy a Quarkus application to Azure Container Apps

In this excercise, you create the Azure Container Apps environment by
using the Azure CLI.

### **Task 1 : Set up the Dockerfile for the Quarkus application**

1.  Container Apps is used to deploy containerized applications. So you
    first need to containerize the Quarkus application into a Docker
    image. This process is easy because the Quarkus Maven plugin has
    already generated some Dockerfiles under **src/main/docker**.

<img src="./media/image47.png" style="width:6.5in;height:3.21667in" />

4.  Use this command to rename one of these
    **Dockerfiles, *Dockerfile.jvm*,** to ***Dockerfile*** and move it
    to the root folder:

>```copy
>mv src/main/docker/Dockerfile.jvm ./Dockerfile
>

<img src="./media/image48.png" style="width:5.83384in;height:1.20844in"
alt="A black screen with white text Description automatically generated" />

5.  Switch back to **IntelliJ** **IDEA** .Replace the content after the
    long comment in the **Dockerfile** with the following:

>```copy
>FROM registry.access.redhat.com/ubi8/openjdk-17:1.18
>
>ENV LANGUAGE='en_US:en'
>
># We make four distinct layers so if there are application changes the library layers can be re-used
>
>COPY --chown=185 target/quarkus-app/lib/ /deployments/lib/
>
>COPY --chown=185 target/quarkus-app/*.jar /deployments/
>
>COPY --chown=185 target/quarkus-app/app/ /deployments/app/
>
>COPY --chown=185 target/quarkus-app/quarkus/ /deployments/quarkus/
>
>EXPOSE 8080
>
>USER 185
>
>ENV JAVA_OPTS_APPEND="-Dquarkus.http.host=0.0.0.0 -Djava.util.logging.manager=org.jboss.logmanager.LogManager"
>
>ENV JAVA_APP_JAR="/deployments/quarkus-run.jar"
>
>ENTRYPOINT [ "/opt/jboss/container/java/run/run-java.sh" ]
>

<img src="./media/image49.png"
style="width:6.49167in;height:2.75833in" />

6.  This Dockerfile expects the Quarkus application to be packaged as
    a ***quarkus-run.jar* file**. This name is the default name for the
    Quarkus application when it's packaged as a JAR file. You need to
    make sure that the Quarkus application is packaged as a JAR file. To
    do so, run the following Maven command:

>```copy
>./mvnw package
>

<img src="./media/image50.png" style="width:6.5in;height:2.41736in"
alt="A screen shot of a computer code Description automatically generated" />

<img src="./media/image51.png" style="width:6.5in;height:4.31944in" />

>Note : mvnw.cmd package \# On Windows

7.  This command packages the Quarkus application into a JAR file and
    generates a ***quarkus-run.jar*** file in
    the ***target/quarkus-app*** folder.

<img src="./media/image52.png" style="width:6.5in;height:5.78333in" />

### Task 2 : Create the Container Apps environment and deploy the container

1.  Now that the Dockerfile is in the right location, you can create the
    Container Apps environment and deploy the container by using a
    single Azure CLI command. Switch back to Gitbahs and run the
    following command at the root of the project:

>```copy
>az containerapp up --name "$AZ_CONTAINERAPP" --environment "$AZ_CONTAINERAPP_ENV" --location "$AZ_LOCATION" --resource-group "$AZ_RESOURCE_GROUP" --ingress external --target-port 8080 --source 
>

<img src="./media/image53.png" style="width:6.5in;height:2.66875in" />

<img src="./media/image54.png" style="width:6.5in;height:4.575in" />

2.  This command does several things:

- Creates a Container Apps environment if it doesn't exist

- Creates an Azure registry if it doesn't exist

- Creates a Log Analytics workspace if it doesn't exist

- Builds the Docker image and pushes it to the Azure registry

- Deploys the Docker image to the Container Apps environment

The az containerapp up command takes some time to run. You should see
output that's similar to the following:

<img src="./media/image55.png" style="width:6.5in;height:6.35in" />

<img src="./media/image56.png" style="width:6.5in;height:4.72569in" />

### Task 3 : Validate the deployment

You can validate that the deployment has succeeded in several ways. The
easiest way is to search for your resource group on the [Azure
portal](https://portal.azure.com/). You should see resources similar to
the following:

1.  Open a browser and go to <https://portal.azure.com> and sign in with
    your Azure subscription account. Click on Resource Group tile.

<img src="./media/image57.png" style="width:6.49167in;height:3.125in" />

2.  Click on your Azure cloud slice resource group.

> <img src="./media/image58.png" style="width:6.5in;height:3.13681in" />

3.  You should see created resources.

<img src="./media/image59.png" style="width:6.5in;height:4.02986in" />

4.  You can also check the deployment by running the following command.
    It lists all the resources created by the az containerapp
    up command.


>```copy
>az resource list --location "$AZ_LOCATION" --resource-group "$AZ_RESOURCE_GROUP" --output table
>

You should see output that's similar to this:

## <img src="./media/image60.png" style="width:6.5in;height:1.32083in"
alt="A screenshot of a computer Description automatically generated" />

### Task 4 : Run the deployed Quarkus application

1.  You can now run the deployed Quarkus application. First, you need to
    get the URL of the application.

2.  Switch back to Gitbash and run below command to get the URL of the
    application.

>```copy
>export AZ_APP_URL=$(
>
>az containerapp show \
>
>--name "$AZ_CONTAINERAPP" \
>
>--resource-group "$AZ_RESOURCE_GROUP" \
>
>--query "properties.configuration.ingress.fqdn" \
>
>--output tsv \
>
>)
>
>echo "AZ_APP_URL=$AZ_APP_URL"
>

<img src="./media/image61.png" style="width:6.5in;height:3.13889in"
alt="A screenshot of a computer screen Description automatically generated" />

3.  Your application is ready
    at https://\<app-name\>.azurecontainerapps.io/. Notice
    the https protocol. That protocol is used because the application is
    deployed with a TLS certificate. To test the application, you can
    use cURL:

>```copy
>curl --header "Content-Type: application/json" \
>
>--request POST \
>
>--data '{"description":"Configuration","details":"Congratulations, you have set up your Quarkus application correctly!","done": "true"}' \
>
><https://$AZ_APP_URL/api/todos>
>

<img src="./media/image62.png" style="width:6.5in;height:1.41875in"
alt="A screenshot of a computer program Description automatically generated" />

4.  Retrieve the data by using a new cURL request. This command returns
    the list of all to-do items from the database:

>```copy
>curl <https://$AZ_APP_URL/api/todos>
>

<img src="./media/image63.png" style="width:6.5in;height:1.78958in"
alt="A screen shot of a computer Description automatically generated" />

5.  Switch back to the Azure portal and click on your container app
    name.

<img src="./media/image64.png"
style="width:6.49375in;height:3.80972in" />

6.  click on the application url link. It opens app in browser tab.

<img src="./media/image65.png" style="width:6.48819in;height:3in" />

<img src="./media/image66.png" style="width:6.5in;height:4.53125in" />

7. Run this command, you can stream the logs for your container when
you create new to-dos:

>```copy
>az containerapp logs show --name "$AZ_CONTAINERAPP" --resource-group "$AZ_RESOURCE_GROUP" --follow
>

<img src="./media/image67.png" style="width:6.5in;height:3.3875in" />

8.  Run more cURL commands. You should see the logs scrolling in the
    terminal.

curl <https://$AZ_APP_URL/api/todos>

** **<img src="./media/image68.png" style="width:6.5in;height:1.40972in"
alt="A screen shot of a computer Description automatically generated" />

## Exercise 4 : Delete resources

### Task 1 : Delete resources from Cloud slice resource group.

IMPORTANAT : DO NOT DELETE RESOURCE GROUP

1.  Switch back to the Azure portal click on your Azure slice resource
    group name.

<img src="./media/image69.png"
style="width:6.49375in;height:3.32153in" />

2.  Select all resources and click on **delete**.

> <img src="./media/image70.png"
> style="width:6.49375in;height:3.06528in" />

3.  Type **delete** in the text box and click on the **Delete**
    button.

<img src="./media/image71.png"
style="width:6.49375in;height:4.07153in" />

4.  On “**Delete Confirmation**” window ,click on **Delete** button.

<img src="./media/image72.png" style="width:6.5in;height:3.71806in" />
