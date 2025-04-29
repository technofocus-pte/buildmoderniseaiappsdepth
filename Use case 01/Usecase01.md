# Anwendungsfall 01 - Quarkus Todo Anwendungen in Container Apps einbinden und in eine relationale Datenbank integrieren.

**Objektiv:**

Dieser Anwendungsfall zeigt, wie Sie eine sichere Quarkus To-d-Anwendung
in Container Apps entwickeln, konfigurieren und bereitstellen, die mit
einer Azure Database for PostgreSQL verbunden ist. Wenn Sie fertig sind,
haben Sie eine Quarkus App, die unter Azure App Service auf Linux
ausgeführt wird.

**Verwendete Schlüsseltechnologien**: Java 17, Azure Database for
PostgreSQL

**Geschätzte Dauer**: 45 Minuten

**Lab-Typ:** Von einem Kursleiter geleitet

### Aufgabe 0: Einrichten von Umgebungsvariablen

1.  Suchen Sie im Windows-Startmenü nach **Environmental variable** und
    wählen Sie **Edit System Environment variable**.

![](./media/image1.jpeg)

2.  Klicken Sie auf die Schaltfläche **Environment Variable**.

![](./media/image2.jpeg)

3.  Wählen Sie **JAVA_HOME** unter **User variable for Admin** aus und
    klicken Sie dann auf **Edit**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image3.jpeg)

4.  Geben Sie den Variablenwert als **C:\Program Files\Java\jdk-17** ein
    und klicken Sie dann auf **Ok**.

![](./media/image4.jpeg)

5.  Navigieren Sie zum Ordner **C:\Software**, klicken Sie mit der
    rechten Maustaste auf **apache-maven-3.9.4-bin.zip** Ordner und
    wählen Sie **Extract All**.

![](./media/image5.jpeg)

6.  **Extrahieren Sie** in denselben Ordner.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.jpeg)

7.  Wechseln Sie zurück zum Fenster der “Edit Environment variable”,
    wählen Sie **MAVEN_HOME** aus und klicken Sie auf **"Edit**".

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image7.jpeg)

8.  Geben Sie den Variablenwert als
    \`C:\Software\apache-maven-3.9.4-bin\apache-maven-3.9.4\` ein und
    klicken Sie dann auf **OK**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image8.jpeg)

9.  Klicken Sie im Fenster **Environmental Variables** auf **OK** und
    erneut **auf OK.**

![](./media/image9.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image10.jpeg)

10. Aktualisieren Sie **JAVA_HOME** gemäß den Lab-Anforderungen, bevor
    Sie Lab ausführen.

## Übung 1 : Generieren der Quarkus-Anwendung mit Maven

Es gibt mehrere Möglichkeiten, eine Quarkus Projektstruktur zu
generieren. Sie können die Quarkus Weboberfläche, ein IDE-Plugin oder
das Quarkus Maven Plugin verwenden. Verwenden wir das Maven-Plugin, um
die Projektstruktur zu generieren.

Sie generieren Ihre Anwendung mit mehreren Abhängigkeiten:

- Die resteasy-Abhängigkeit zum Verfügbarmachen eines REST-Endpunkts

- Die jackson-Abhängigkeit zum Serialisieren und Deserialisieren von
  JSON

- Die hibernate-Abhängigkeit für die Interaktion mit der Datenbank

- Die postgreSQL-Abhängigkeit zum Herstellen einer Verbindung mit der
  PostgreSQL-Datenbank

- Die docker-Abhängigkeit zum Erstellen eines Docker-Images

Sie müssen keine Azure-Abhängigkeiten angeben, da Sie Ihre Anwendung
zuerst lokal ausführen und dann eine containerisierte Version davon in
Azure Container Apps bereitstellen.

### Aufgabe 1 : Generieren der Quarkus-Anwendung

1.  Öffnen Sie **Git Bash** über das Windows-Startmenü und führen Sie
    den folgenden Befehl aus

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

2.  Mit diesem Befehl wird ein neues Quarkus Projekt erstellt. Es
    generiert eine Maven-Verzeichnisstruktur (src/main/java für
    Quellcode und src/test/java für Tests). Es werden einige
    Java-Klassen, einige Tests und einige Dockerfiles erstellt. Außerdem
    wird eine *pom.xml* Datei mit allen erforderlichen Abhängigkeiten
    (Hibernate, RESTEasy, Jackson, PostgreSQL und Docker) generiert:

3.  Klicken Sie auf Search und geben Sie \`\`IntelliJ IDE\`\` ein und
    wählen Sie dann **IntelliJ IDE**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image12.jpeg)

4.  Aktivieren Sie das Kontrollkästchen Bestätigung und klicken Sie dann
    auf die Schaltfläche **Continue**.

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
generiert](./media/image13.jpeg)

5.  Schließen Sie das Fenster Datenfreigabe(Data Sharing).

![](./media/image14.jpeg)

6.  Wählen Sie das Optionsfeld **Start trial** und klicken Sie dann auf
    die Schaltfläche **Start trial**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image15.jpeg)

7.  Klicken Sie auf die Schaltfläche **Allow access**.

![](./media/image16.jpeg)

8.  Schließen Sie den Browser, wechseln Sie zurück zum
    IntelliJ-Lizenzfenster und klicken Sie auf die Schaltfläche
    **Continue**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image17.jpeg)

9.  Klicken Sie auf Ordner **Open**.

![](./media/image18.jpeg)

10. Navigieren Sie zu **C:\Users\Admin\todo,** wählen Sie den
    **Todo**-Projektordner aus und klicken Sie dann auf **OK**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image19.jpeg)

11. Klicken Sie auf die Schaltfläche **Trust project**.

![](./media/image20.jpeg)

12. Öffnen Sie **pom.xml** und Sie sollten das folgende XML-Format
    sehen.

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

**Hinweis**: Alle Abhängigkeiten in der *pom.xml*-Datei sind in der
Quarkus BOM (Stückliste) io.quarkus.platform:quarkus-bom definiert.

![](./media/image21.jpeg)

### Aufgabe 2: Codieren der Anwendung

1.  Gehen Sie zu **src/main/java/com.example.demo** und klicken Sie mit
    der rechten Maustaste auf **MyEntity.Java -\> Refactor -\> Rename**.

![](./media/image22.jpeg)

2.  Benennen Sie die generierte ***MyEntity.java*** Klasse in
    ''**Todo.java**'' um (befindet sich im selben Ordner wie die
    *TodoResource.java* Datei)

![](./media/image23.jpeg)

3.  Ersetzen Sie den vorhandenen Code durch den folgenden Java-Code. Es
    verwendet die Java Persistence API (jakarta.persistence.\*-Paket),
    um Daten von Ihrem PostgreSQL-Server zu speichern und abzurufen. Es
    verwendet auch \[Hibernate ORM with Panache\]{.underline} (erbt von
    io.quarkus.hibernate.orm.panache.PanacheEntity), um die
    Persistenzschicht zu vereinfachen.

4.  Sie verwenden eine JPA-Entität (@Entity), um das Java Todo-Objekt
    direkt der PostgreSQL-Todo-Tabelle zuzuordnen. Der
    TodoResource-REST-Endpunkt erstellt dann eine neue
    Todo-Entitätsklasse und behält sie bei. Bei dieser Klasse handelt es
    sich um ein Domänenmodell, das der Todo-Tabelle zugeordnet ist. Die
    Tabelle wird automatisch von JPA erstellt.

5.  Durch das Erweitern von PanacheEntity erhalten Sie eine Reihe von
    generischen CRUD-Methoden (Create, Read, Update und Delete) für
    Ihren Typ. So können Sie Dinge wie das Speichern und Löschen von
    Todo-Objekten in nur einer Zeile Java-Code erledigen.

6.  Richten Sie das JDK in IntelliJ ein, falls es noch nicht festgelegt
    wurde.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image24.jpeg)

7.  Ersetzen Sie den vorhandenen Code durch den folgenden Java-Code für
    die Todo-Entität:

> package com.example.demo;
>
> import io.quarkus.hibernate.orm.panache.PanacheEntity;
>
> import jakarta.persistence.Entity;
>
> import java.time.Instant;
>
> @Entity
>
> public class Todo extends PanacheEntity {
>
> public String description;
>
> public String details;
>
> public boolean done;
>
> public Instant createdAt = Instant.now();
>
> @Override
>
> public String toString() {
>
> return "Todo{" +
>
> "id=" + id + '\\' +
>
> ", description='" + description + '\\' +
>
> ", details='" + details + '\\' +
>
> ", done=" + done +
>
> ", createdAt=" + createdAt +
>
> '}';
>
> }
>
> }

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image25.jpeg)

8.  Um diese Klasse zu verwalten, aktualisieren Sie die **TodoResource**
    so, dass sie REST-Schnittstellen zum Speichern und Abrufen von Daten
    mithilfe von HTTP veröffentlichen kann. Öffnen Sie die
    **TodoResource**-Klasse, und ersetzen Sie den Code durch Folgendes:

> package com.example.demo;
>
> import jakarta.inject.Inject;
>
> import jakarta.transaction.Transactional;
>
> import jakarta.ws.rs.Consumes;
>
> import jakarta.ws.rs.GET;
>
> import jakarta.ws.rs.POST;
>
> import jakarta.ws.rs.Path;
>
> import jakarta.ws.rs.Produces;
>
> import static jakarta.ws.rs.core.MediaType.APPLICATION_JSON;
>
> import jakarta.ws.rs.core.Response;
>
> import jakarta.ws.rs.core.UriBuilder;
>
> import jakarta.ws.rs.core.UriInfo;
>
> import org.jboss.logging.Logger;
>
> import java.util.List;
>
> @Path("/api/todos")
>
> @Consumes(APPLICATION_JSON)
>
> @Produces(APPLICATION_JSON)
>
> public class TodoResource {
>
> @Inject
>
> Logger logger;
>
> @Inject
>
> UriInfo uriInfo;
>
> @POST
>
> @Transactional
>
> public Response createTodo(Todo todo) {
>
> logger.info("Creating todo: " + todo);
>
> Todo.persist(todo);
>
> UriBuilder uriBuilder =
> uriInfo.getAbsolutePathBuilder().path(todo.id.toString());
>
> return Response.created(uriBuilder.build()).entity(todo).build();
>
> }
>
> @GET
>
> public List\<Todo\> getTodos() {
>
> logger.info("Getting all todos");
>
> return Todo.listAll();
>
> }
>
> }

### ![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch generiert](./media/image26.jpeg)

### **Aufgabe 3: Ausführen der Anwendung**

Wenn Sie die Anwendung im Entwicklungsmodus ausführen, muss Docker
Desktop ausgeführt werden. Das liegt daran, dass Quarkus erkennt, dass
Sie eine PostgreSQL-Datenbank benötigen (aufgrund der
PostgreSQL-Abhängigkeit quarkus-jdbc-postgresql, die in der *pom.xml*
Datei deklariert ist ), das PostgreSQL Docker Desktop-Image herunterlädt
und einen Container mit der Datenbank startet. Anschließend wird die
Todo-Tabelle automatisch in der Datenbank erstellt.

1.  Doppelklicken Sie auf **Docker Desktop** und minimieren Sie das
    Fenster. Stellen Sie sicher, dass es ausgeführt wird. (keine
    Anmeldung erforderlich)

![Ein Computerbildschirm mit weißem Hintergrund Beschreibung wird
automatisch generiert](./media/image27.jpeg)

2.  Kehren Sie zu Gitbash zurück, und führen Sie die to-do-Anwendung mit
    dem folgenden Befehl aus:

cd todo

./mvnw quarkus:dev

![](./media/image28.jpeg)

3.  Die Quarkus-Anwendung sollte gestartet werden und eine Verbindung zu
    Ihrer Datenbank herstellen. Die folgende Ausgabe sollte angezeigt
    werden:

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image29.jpeg)

![Ein Computerbildschirm mit Text und Bildern Beschreibung wird
automatisch generiert](./media/image30.jpeg)

![](./media/image31.jpeg)

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image32.jpeg)

4.  Klicken Sie auf **Allow access**.

![](./media/image31.jpeg)

5.  Um die Anwendung zu testen, können Sie cURL verwenden.

Erstellen Sie in einer separaten neuen Instanz von Gitbash mit dem
folgenden Befehl ein neues to-do-Item in der Datenbank. Sie sollten das
Logl in der Quarkus Konsole sehen:

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description":"Take Quarkus MS Learn","details":"Take the MS
Learn on deploying Quarkus to Azure Container Apps","done": "true"}' \\

http://127.0.0.1:8080/api/todos

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image33.jpeg)

6.  Dieser Befehl sollte das erstellte Element (mit einem Bezeichner)
    zurückgeben:

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image33.jpeg)

7.  Erstellen Sie eine zweite Aufgabe(to-do), indem Sie den folgenden
    cURL-Befehl verwenden:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Take Azure Container Apps MS
> Learn","details":"Take the ACA Learn module","done": "false"}' \\

http://127.0.0.1:8080/api/todos

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image34.jpeg)

8.  Rufen Sie als Nächstes die Daten mit einer neuen cURL-Anfrage ab:

curl http://127.0.0.1:8080/api/todos

Dieser Befehl gibt die Liste der to-do-Items zurück, einschließlich der
Elemente, die Sie erstellt haben:

![](./media/image35.jpeg)

### Aufgabe 4: Testen der Anwendung

Zum Testen der Anwendung können Sie die vorhandene
**TodoResourceTest-**Klasse verwenden . Der REST-Endpunkt muss getestet
werden. Um den Endpunkt zu testen, wird \[RESTAssured\]{.underline}
verwendet.

1.  Wechseln Sie zurück zu Intellij und öffnen Sie die
    **TodoResourceTest**-Klasse aus **src/test/java/com.example.demo**.
    Ersetzen Sie den Code in der **TodoResourceTest**-Klasse durch den
    folgenden Code:

> package com.example.demo;
>
> import io.quarkus.test.junit.QuarkusTest;
>
> import static io.restassured.RestAssured.given;
>
> import static jakarta.ws.rs.core.HttpHeaders.CONTENT_TYPE;
>
> import static jakarta.ws.rs.core.MediaType.APPLICATION_JSON;
>
> import org.junit.jupiter.api.Test;
>
> @QuarkusTest
>
> class TodoResourceTest {
>
> @Test
>
> void shouldGetAllTodos() {
>
> given()
>
> .when().get("/api/todos")
>
> .then()
>
> .statusCode(200);
>
> }
>
> @Test
>
> void shouldCreateATodo() {
>
> Todo todo = new Todo();
>
> todo.description = "Take Quarkus MS Learn";
>
> todo.details = "Take the MS Learn on deploying Quarkus to Azure
> Container Apps";
>
> todo.done = true;
>
> given().body(todo)
>
> .header(CONTENT_TYPE, APPLICATION_JSON)
>
> .when().post("/api/todos")
>
> .then()
>
> .statusCode(201);
>
> }
>
> }

![Ein Computer-Screenshot eines Programms Beschreibung wird automatisch
generiert](./media/image36.jpeg)

2.  Wenn Sie die Anwendung testen, muss Docker Desktop ausgeführt
    werden, da Quarkus erkennt, dass die PostgreSQL-Datenbank zum Testen
    benötigt wird.

3.  Wechseln Sie zurück zu **Gitbash** und Ctrl + C. Führen Sie die
    folgenden Befehle aus, um die Anwendung mit diesem Befehl zu testen:

./mvnw clean test

![Ein Computerbildschirm mit Text und Bildern Beschreibung wird
automatisch generiert](./media/image37.jpeg)

Es sollte eine Ausgabe angezeigt werden, die in etwa wie folgt aussieht:

![Ein Computerbildschirm mit Text und Zahlen Beschreibung wird
automatisch generiert](./media/image38.jpeg)

## Übung 2: Einrichten von Azure Container Apps

In dieser Übung erstellen Sie eine Azure-Ressourcengruppe, die die
Ressourcen für die Anwendung enthält. Anschließend richten Sie die
PostgreSQL-Datenbank mithilfe der Azure CLI ein. Schließlich
konfigurieren Sie die Quarkus-Anwendung für den Zugriff auf die
entfernte PostgreSQL-Datenbank. Verwenden Sie ein Terminal Ihrer Wahl,
um die Befehle auszuführen.

## Aufgabe 1 : Bereiten Sie die Arbeitsumgebung vor

Sie müssen einige Umgebungsvariablen einrichten. Im Folgenden finden Sie
einige Hinweise zu den Variablen, die Sie erstellen:

[TABLE]

**Hinweis:** Sie können Ihre Azure-Ressourcen beliebig benennen. Dieser
Artikel enthält Beispielabkürzungen für viele Azure-Ressourcen (z. B. rg
für Ressourcengruppen und ca für Container-Apps).

1.  Verwenden Sie die folgenden Befehle, um die Variablen einzurichten.
    Achten Sie darauf, die Werte wie in der vorherigen Tabelle
    beschrieben zu ändern. Diese Umgebungsvariablen werden im Rest
    dieses Moduls verwendet.

**Hinweis:** PostgreSQL wird nur in **Westus** unterstützt . Versuchen
Sie es zuerst in Westus und wenn Sie Probleme haben, versuchen Sie es
dann in Ihrer Nähe

export AZ_PROJECT_Quarkus="azure-deploy-quarkus-"$RANDOM

export AZ_CONTAINERAPP="ca${AZ_PROJECT_Quarkus}"

export AZ_CONTAINERAPP_ENV="cae${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_DB_NAME="postgres${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_USERNAME="azuser123"

export AZ_POSTGRES_PASSWORD="P@55w.rd12345"

export AZ_POSTGRES_SERVER_NAME="psql${AZ_PROJECT_Quarkus}"

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image39.png)

2.  Wechseln Sie zurück zu Gitbash, und führen Sie den folgenden Befehl
    aus, um die Ressourcengruppenvariable festzulegen. Kopieren Sie den
    Namen der Ressourcengruppe.

> export AZ_RESOURCE_GROUP="Your existing resource group"
>
> export AZ_LOCATION="Location near to you"

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image40.png)

![Ein Computer-Screenshot von Text Beschreibung wird automatisch
generiert](./media/image41.png)

3.  Führen Sie ''az login'' aus, um sich anzumelden. Melden Sie sich mit
    Ihrem Azure-Abonnementkonto an.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image42.png)

### Aufgabe 2: Erstellen einer Instanz von Azure Database for PostgreSQL

1.  Sie erstellen nun einen verwalteten PostgreSQL-Server. Führen Sie
    den folgenden Befehl aus, um eine kleine Instanz von Azure Database
    for PostgreSQL zu erstellen:

> az postgres flexible-server create --resource-group
> "$AZ_RESOURCE_GROUP" --location "$AZ_LOCATION" --name
> "$AZ_POSTGRES_SERVER_NAME" --database-name "$AZ_POSTGRES_DB_NAME"
> --admin-user "$AZ_POSTGRES_USERNAME" --admin-password
> "$AZ_POSTGRES_PASSWORD" --public-access "All" --tier "Burstable"
> --sku-name "Standard_B1ms" --storage-size 32 --version "16"

![Ein Screenshot eines Computercodes Beschreibung wird automatisch
generiert](./media/image43.jpeg)

2.  Mit diesem Befehl wird ein kleiner PostgreSQL-Server erstellt, der
    die Variablen verwendet, die Sie zuvor eingerichtet haben.

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
generiert](./media/image44.jpeg)

### Aufgabe 3 : Quarkus für den Zugriff auf die PostgreSQL-Datenbank konfigurieren

1.  Sie verbinden nun die Quarkus-Anwendung mit der
    PostgreSQL-Datenbank. Dazu müssen Sie zunächst die
    Verbindungszeichenfolge für die Datenbank abrufen:

2.  Führen Sie den folgenden Befehl aus, um die Verbindungszeichenfolge
    für die Datenbank abzurufen.

3.  export POSTGRES_CONNECTION_STRING=$(

> az postgres flexible-server show-connection-string --server-name
> "$AZ_POSTGRES_SERVER_NAME" --database-name "$AZ_POSTGRES_DB_NAME"
> --admin-user "$AZ_POSTGRES_USERNAME" --admin-password
> "$AZ_POSTGRES_PASSWORD" --query "connectionStrings.jdbc" --output tsv

)

export
POSTGRES_CONNECTION_STRING_SSL="$POSTGRES_CONNECTION_STRING&ssl=true&sslmode=require"

echo "POSTGRES_CONNECTION_STRING_SSL=$POSTGRES_CONNECTION_STRING_SSL"

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image45.jpeg)

4.  Notieren Sie sich die zurückgegebene Verbindungszeichenfolge.

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image46.jpeg)

### Aufgabe 4 :Konfigurieren der Quarkus-Anwendung für die Verbindung mit der PostgreSQL-Datenbank

1.  Wechseln Sie zurück zur Intellij IDE. Aktualisieren Sie die Datei
    **application.properties** im Ordner **src/main/resources** des
    Projekts, um die Verbindungszeichenfolge zur PostgreSQL-Datenbank zu
    konfigurieren.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image47.jpeg)

2.  Legen Sie die Eigenschaft **quarkus.datasource.jdbc.url** auf den
    zuvor ausgegebenen Wert **\\POSTGRES_CONNECTION_STRING_SSL** fest .
    Der **&ssl=true&sslmode=require**-Teil der Verbindungszeichenfolge
    zwingt den Treiber, SSL zu verwenden, eine Anforderung für Azure
    Database for PostgreSQL.

> quarkus.hibernate-orm.database.generation=update
>
> quarkus.datasource.jdbc.url=\<the POSTGRES_CONNECTION_STRING_SSL
> value\>

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image48.jpeg)

### Aufgabe 5 : Lokales Ausführen der Quarkus-Anwendung zum Testen der Remote-Datenbankverbindung

1.  Wechseln Sie zurück zu Gitbash und führen Sie den folgenden Befehl
    aus, um die Anwendung lokal auszuführen:

./mvnw clean quarkus:dev

![Ein Computerbildschirm mit Text und Bildern Beschreibung wird
automatisch generiert](./media/image49.jpeg)

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image50.jpeg)

![](./media/image51.jpeg)

2.  Wenn Quarkus ausgeführt wird, erstellen Sie einige Aufgaben, indem
    Sie die folgenden cURL-Befehle in einem separaten Terminalfenster
    verwenden:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Take Quarkus MS Learn","details":"Take the MS
> Learn on deploying Quarkus to Azure Container Apps","done": "true"}'
> \\

''http://127.0.0.1:8080/api/todos''

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image52.jpeg)

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description":"Take Azure Container Apps MS
Learn","details":"Take the ACA Learn module","done": "false"}' \\

'' http://127.0.0.1:8080/api/todos''

![](./media/image53.jpeg)

3.  Überprüfen Sie als Nächstes, ob sich die Aufgaben in der Datenbank
    befinden, indem Sie auf den GET-Endpunkt zugreifen, der in der
    to-do-App definiert ist:

> \`\`curl http://127.0.0.1:8080/api/todos\`\`

Die folgende Ausgabe sollte angezeigt werden:

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image54.jpeg)Wenn Sie diese Ausgabe sehen, haben Sie
die Quarkus-Anwendung erfolgreich ausgeführt und eine Verbindung zur
entfernten PostgreSQL-Datenbank hergestellt.

## Übung 3: Bereitstellen einer Quarkus Anwendung in Azure Container Apps

In dieser Übung erstellen Sie die Azure Container Apps-Umgebung mithilfe
der Azure CLI.

### Aufgabe 1 : Einrichten des Dockerfiles für die Quarkus-Anwendung

1.  Container Apps wird zum Bereitstellen von containerisierten
    Anwendungen verwendet. Sie müssen also zunächst die
    Quarkus-Anwendung in ein Docker-Image containerisieren. Dieser
    Vorgang ist einfach, da das Quarkus Maven Plugin bereits einige
    Dockerfiles unter **src/main/docker** generiert hat.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image55.jpeg)

2.  Wechseln Sie zurück zu Gitbash und drücken Sie Ctrl+C . Führen Sie
    den folgenden Befehl aus, um eine dieser **Dockerfiles,
    *Dockerfile.jvm*,** in ***Dockerfile*** umzubenennen und in den
    Stammordner zu verschieben:

''mv src/main/docker/Dockerfile.jvm ./Dockerfile''

![Ein schwarzer Bildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image56.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image55.jpeg)

3.  Ersetzen Sie den Inhalt nach dem langen Kommentar in der
    **Dockerfile** durch Folgendes, z. B. in Zeile \# 80

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

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image57.jpeg)

4.  Dieses Dockerfile erwartet, dass die Quarkus Anwendung als
    ***quarkus-run.jar* Datei** gepackt wird. Dieser Name ist der
    Standardname für die Quarkus Anwendung, wenn sie als JAR-Datei
    gepackt wird. Sie müssen sicherstellen, dass die Quarkus-Anwendung
    als JAR-Datei verpackt ist. Führen Sie dazu den folgenden
    Maven-Befehl aus:

> \`\`./mvnw package\`\`

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image58.jpeg)

![](./media/image59.jpeg)

5.  Dieser Befehl packt die Quarkus Anwendung in eine JAR-Datei und
    erzeugt eine ***quarkus-run.jar*** Datei im Ordner
    ***target/quarkus-app***.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image60.jpeg)

### Aufgabe 2: Erstellen der Container Apps-Umgebung und Bereitstellen des Containers

1.  Nachdem sich die Dockerfile nun am richtigen Speicherort befindet,
    können Sie die Container Apps-Umgebung erstellen und den Container
    mit einem einzigen Azure CLI-Befehl bereitstellen. Führen Sie den
    folgenden Befehl im Stammverzeichnis des Projekts aus:

az containerapp up --name "$AZ_CONTAINERAPP" --environment
"$AZ_CONTAINERAPP_ENV" --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --ingress external --target-port 8080 --source .

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image61.jpeg)

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image61.jpeg)

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image62.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image63.jpeg)

2.  Dieser Befehl führt mehrere Funktionen aus:

    - Erstellt eine Container Apps-Umgebung, wenn sie nicht vorhanden
      ist

    - Erstellt eine Azure-Registry, wenn sie nicht vorhanden ist

    - Erstellt einen Log Analytics-Arbeitsbereich, wenn er nicht
      vorhanden ist

    - Erstellt das Docker-Image und pusht es in die Azure-Registry

    - Stellt das Docker-Image in der Container Apps-Umgebung bereit

Die Ausführung des Befehls az containerapp up nimmt einige Zeit in
Anspruch. Es sollte eine Ausgabe angezeigt werden, die der folgenden
ähnelt:

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image64.jpeg)

### Aufgabe 3: Überprüfen der Bereitstellung

Sie können auf verschiedene Arten überprüfen, ob die Bereitstellung
erfolgreich war. Am einfachsten ist es, im Azure-Portal nach Ihrer
Ressourcengruppe zu suchen. Es sollten Ressourcen angezeigt werden, die
den folgenden ähneln:

1.  Öffnen Sie einen Browser, wechseln Sie zu
    ''https:\\portal.azure.com'', und melden Sie sich mit Ihrem
    Azure-Abonnementkonto an. Klicken Sie auf die Kachel Resource
    Groups.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image65.jpeg)

2.  Klicken Sie auf den Namen der Ressourcengruppe.

![](./media/image66.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image67.png)

3.  Sie können die Bereitstellung auch überprüfen, indem Sie den
    folgenden Befehl ausführen. Es werden alle Ressourcen aufgelistet,
    die mit dem Befehl az containerapp up erstellt wurden.

az resource list --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --output table

Es sollte eine Ausgabe angezeigt werden, die ähnlich wie die folgende
aussieht:

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image68.png)

### Aufgabe 4 : Ausführen der bereitgestellten Quarkus Anwendung

1.  Sie können nun die bereitgestellte Quarkus Anwendung ausführen.
    Zuerst müssen Sie die URL der Anwendung abrufen.

2.  Wechseln Sie zurück zu Gitbash und führen Sie den folgenden Befehl
    aus, um die URL der Anwendung abzurufen.

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

''echo "AZ_APP_URL=$AZ_APP_URL"''

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image69.jpeg)

3.  Ihre Anwendung ist unter https://\\app-name\>.azurecontainerapps.io/
    bereit. Beachten Sie das https-Protokoll. Dieses Protokoll wird
    verwendet, da die Anwendung mit einem TLS-Zertifikat bereitgestellt
    wird. Um die Anwendung zu testen, können Sie cURL verwenden:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Configuration","details":"Congratulations, you
> have set up your Quarkus application correctly!","done": "true"}' \\

'' https://$AZ_APP_URL/api/todos''

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image70.jpeg)

4.  Rufen Sie die Daten mithilfe einer neuen cURL-Anforderung ab:

''curl https://$AZ_APP_URL/api/todos''

5.  Dieser Befehl gibt die Liste aller To-Do-Items aus der Datenbank
    zurück:

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image71.jpeg)

6.  Wechseln Sie zurück zum Azure-Portal, und klicken Sie auf den Namen
    Ihrer Container-App.

![](./media/image72.png)

7.  Klicken Sie auf den Link zur Anwendungs-URL. Es öffnet die App im
    Browser-Tab.

![](./media/image73.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image74.png)

8.  Führen Sie diesen Befehl aus, um die Logs für Ihren Container zu
    streamen, wenn Sie neue Aufgaben(to-dos) erstellen:

az containerapp logs show --name "$AZ_CONTAINERAPP" --resource-group
"$AZ_RESOURCE_GROUP" -–follow

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
generiert](./media/image75.png)

9.  Führen Sie weitere cURL-Befehle aus. Sie sollten sehen, dass die
    Logs im Terminal scrollen.

''curl https://$AZ_APP_URL/api/todos''

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
generiert](./media/image76.png)

## Übung 4: Löschen von Ressourcen in der Ressourcengruppe

### Aufgabe 1 : Löschen von Ressourcen.

1.  Wechseln Sie zurück zum Azure-Portal. Klicken Sie auf **Resource
    groups**.

![](./media/image77.png)

2.  Klicken Sie auf Name der Ressourcengruppe.

![](./media/image78.png)

3.  Wählen Sie alle Ressourcen aus und klicken Sie dann auf **Delete**
    (NICHT LÖSCHEN – Ressourcengruppe)

![](./media/image79.png)

4.  Geben Sie ''delete'' ein und klicken Sie dann auf **Delete**.

![](./media/image80.png)

5.  Bestätigen Sie das Löschen von Ressourcen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image81.png)

**Zusammenfassung**

\[Sie haben gelernt, wie Sie Maven verwenden, um die Anwendung zu
bootstrappen, und eine integrierte Entwicklungsumgebung (IDE), um den
Code zu bearbeiten. Sie haben gelernt, wie Sie Docker verwenden, um eine
lokale PostgreSQL-Datenbank zu starten, damit Sie die Anwendung lokal
ausführen und testen können. Sie haben die Quarkus-Anwendung erfolgreich
ausgeführt und sich mit der entfernten PostgreSQL-Datenbank verbunden.
