# Caso d'uso 01 - Incorporare le applicazioni Quarkus Todo nelle app contenitore e integrarle con un database relazionale.

**Obiettivo:**

Questo caso d'uso illustra come sviluppare, configurare e distribuire
un'applicazione Quarkus To-d- sicura in app contenitore connessa a un
database di Azure per PostgreSQL. Al termine, sarà disponibile un'app
Quarkus in esecuzione nel Servizio app di Azure in Linux.

**Principali tecnologie utilizzate**: Java 17, Database di Azure per
PostgreSQL

**Durata stimata**: 45 minuti

**Tipo di laboratorio:** Guidato dall'istruttore

### Attività 0: Impostazione delle variabili ambientali

1.  Cerca Variabile ambientale dal menu Start di Windows e seleziona
    Modifica variabile ambiente di sistema.

![](./media/image1.jpeg)

2.  Fare clic sul pulsante **Environment Variable**.

![](./media/image2.jpeg)

3.  Seleziona **JAVA_HOME** in **User variable for Admin** e quindi fai
    clic su **Edit**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image3.jpeg)

4.  Immettere il valore della variabile come **C:\Program
    Files\Java\jdk-17** e quindi fare clic su **Ok**.

![](./media/image4.jpeg)

5.  Passare alla cartella **C:\Software** e fare clic con il pulsante
    destro del mouse su **apache-maven-3.9.4-bin.zip** cartella e
    selezionare **Extract All**.

![](./media/image5.jpeg)

6.  **Extract** nella stessa cartella.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.jpeg)

7.  Torna alla finestra Modifica variabile d'ambiente, seleziona
    **MAVEN_HOME** e fai clic su **Edit**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image7.jpeg)

8.  Immettere il valore della variabile come
    'C:\Software\apache-maven-3.9.4-bin\apache-maven-3.9.4' e quindi
    fare clic su **OK.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image8.jpeg)

9.  Nella finestra **Environmental Variables**, fare clic su **Ok** e di
    nuovo su **OK**.

![](./media/image9.jpeg)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image10.jpeg)

10. Aggiornare **JAVA_HOME** in base ai requisiti del lab prima di
    eseguire il lab.

## Esercizio 1 : Generare l'applicazione Quarkus utilizzando Maven

Esistono diversi modi per generare una struttura di progetto Quarkus. È
possibile utilizzare l'interfaccia web di Quarkus, un plug-in IDE o il
plug-in Quarkus Maven. Usiamo il plugin Maven per generare la struttura
del progetto.

L'applicazione viene generata con diverse dipendenze:

- Dipendenza resteasy per l'esposizione di un endpoint REST

- La dipendenza jackson per serializzare e deserializzare JSON

- La dipendenza di ibernazione per interagire con il database

- La dipendenza postgresql per connettersi al database PostgreSQL

- Dipendenza Docker per creare un'immagine Docker

Non è necessario specificare le dipendenze di Azure perché si esegue
prima l'applicazione in locale e quindi si distribuisce una versione in
contenitori in Apps Azure Container.

### Attività 1 : Generare l'applicazione Quarkus

1.  Apri **Git Bash** dal menu di avvio di Windows ed esegui il comando
    seguente

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

-Dextensions="resteasy-jackson, ibernazione-orm-briache,
jdbc-postgresql, docker"

![](./media/image11.jpeg)

2.  Questo comando crea un nuovo progetto Quarkus. Genera una struttura
    di directory Maven (src/main/java per il codice sorgente e
    src/test/java per i test). Crea alcune classi Java, alcuni test e
    alcuni Dockerfile. Genera anche un file *pom.xml* con tutte le
    dipendenze necessarie (Hibernate, RESTEasy, Jackson, PostgreSQL e
    Docker):

3.  Fare clic su Cerca e digitare ''IntelliJ IDE'', quindi selezionare
    **IntelliJ IDE**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image12.jpeg)

4.  Seleziona la casella di controllo di conferma e quindi fai clic sul
    pulsante **Continue**.

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente](./media/image13.jpeg)

5.  Chiudere la finestra Condivisione data.

![](./media/image14.jpeg)

6.  Seleziona il pulsante di opzione **Start trial** e quindi fai clic
    sul pulsante **Start trial**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image15.jpeg)

7.  Fare clic sul pulsante **Allow access.**

![](./media/image16.jpeg)

8.  Chiudi il browser, torna alla finestra della licenza IntelliJ e fai
    clic sul pulsante **Continue**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image17.jpeg)

9.  Fare clic su **Open** cartella.

![](./media/image18.jpeg)

10. Passare a **C:\Users\Admin\todo** e selezionare la cartella del
    progetto **todo**, quindi fare clic su **OK.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image19.jpeg)

11. Fare clic sul pulsante **Trust project**.

![](./media/image20.jpeg)

12. Apri **pom.xml** e dovresti vedere sotto il formato xml.

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

**Note** Tutte le dipendenze nel file *pom.xml* sono definite nella
distinta base di Quarkus io.quarkus.platform:quarkus-bom.

![](./media/image21.jpeg)

### Attività 2 : Codifica dell'applicazione

1.  Vai su **src/main/java/com.example.demo** e fai clic con il pulsante
    destro del mouse su **MyEntity.Java -\> Refactor -\> Rename**.

![](./media/image22.jpeg)

2.  Rinomina la classe ***MyEntity.java*** generata in **''Todo.java''**
    (che si trova nella stessa cartella del *file TodoResource.java*)

![](./media/image23.jpeg)

3.  Sostituire il codice esistente con il seguente codice Java. Utilizza
    la API Java Persistence (jakarta.persistence.\* package) per
    archiviare e recuperare i data dal server PostgreSQL. Utilizza anche
    \[Hibernate ORM with Panache\]{.underline} (ereditando da
    io.quarkus.hibernate.orm.panache.PanacheEntity) per semplificare il
    livello di persistenza.

4.  Si utilizza un'entità JPA (@Entity) per mappare l'oggetto Java Todo
    direttamente alla tabella Todo di PostgreSQL. La endpoint REST
    TodoResource crea quindi una nuova classe di entità Todo e la rende
    permanente. Questa classe è un modello di dominio mappato nella
    tabella Todo. La tabella viene creata automaticamente da JPA.

5.  L'estensione di PanacheEntity consente di ottenere una serie di
    metodi generici di creazione, lettura, aggiornamento ed eliminazione
    (CRUD) per il tipo. Quindi puoi fare cose come salvare ed eliminare
    gli oggetti Todo in una sola riga di codice Java.

6.  Configurare JDK in IntelliJ se non è già stato impostato.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image24.jpeg)

7.  Sostituire il codice esistente con il seguente codice Java
    nell'entità Todo:

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

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image25.jpeg)

8.  Per gestire tale classe, aggiornare **TodoResources** in modo che
    possa pubblicare interfacce REST per archiviare e recuperare i data
    tramite HTTP. Aprire la class **TodoResource** e sostituire il
    codice con il seguente:

package com.example.demo;

import jakarta.inject.Inject;

import jakarta.transaction.Transactional;

import jakarta.ws.rs.Consumes;

import jakarta.ws.rs.GET;

import jakarta.ws.rs.POST;

import jakarta.ws.rs.Path;

import jakarta.ws.rs.Produces;

import jakarta.ws.rs.core.MediaType.APPLICATION_JSON statici;

import jakarta.ws.rs.core.Response;

import jakarta.ws.rs.core.UriBuilder;

import jakarta.ws.rs.core.UriInfo;

import org.jboss.logging.Logger;

import java.util.List;

@Path("/api/todos")

@Consumes(APPLICATION_JSON)

@Produces(APPLICATION_JSON)

pubblic class TodoResource {

@Inject

Logger logger;

@Inject

UriInfo, uriInfo;

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

### ![Uno screenshot di un programma per computer Descrizione generata automaticamente](./media/image26.jpeg)

### **Attività 3 : Eseguire l'applicazione**

Quando si esegue l'applicazione in modalità di sviluppo, Docker Desktop
deve essere in esecuzione. Questo perché Quarkus rileva che è necessario
un database PostgreSQL (a causa della dipendenza PostgreSQL
quarkus-jdbc-postgresql dichiarata nel *file pom.xml*), scarica
l'immagine PostgreSQL Docker Desktop e avvia un contenitore con il
database. Quindi crea automaticamente la tabella Todo nel database.

1.  Fare doppio clic su **Docker Desktop** e ridurre a icona la
    finestra. Assicurati che sia in esecuzione. (non è necessario
    effettuare l'accesso)

![Schermo di un computer con sfondo bianco Descrizione generata
automaticamente](./media/image27.jpeg)

2.  Torna a Gitbash ed esegui l'applicazione to-do utilizzando questo
    comando:

cd todo

./mvnw quarkus:dev

![](./media/image28.jpeg)

3.  L'applicazione Quarkus dovrebbe avviarsi e connettersi al database.
    Dovrebbe essere visualizzato l'output seguente:

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image29.jpeg)

![Lo schermo di un computer con testo e immagini Descrizione generata
automaticamente](./media/image30.jpeg)

![](./media/image31.jpeg)

![Schermata di un programma per computer Descrizione generata
automaticamente](./media/image32.jpeg)

4.  Fare clic su **Allow access**.

![](./media/image31.jpeg)

5.  Per testare l'applicazione, è possibile utilizzare cURL.

In una nuova istanza separata di Gitbash ,crea un nuovo elemento da fare
nel database con il seguente comando. Dovresti vedere il log nella
console di Quarkus:

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description":"Take Quarkus MS Learn", "details":"Take the MS
Learn on deploying Quarkus in Azure Container Apps","done": "true"}' \\

http://127.0.0.1:8080/api/todos

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image33.jpeg)

6.  Questo comando dovrebbe restituire l'elemento creato (con un
    identificatore):

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image33.jpeg)

7.  Creare una seconda attività utilizzando il comando cURL seguente:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Take Azure Container Apps MS Learn",
> "details": "Take the ACA Learn module", "done": "false"}' \\

http://127.0.0.1:8080/api/todos

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image34.jpeg)

8.  Quindi, recupera i data utilizzando una nuova richiesta cURL:

curl http://127.0.0.1:8080/api/todos

Questo comando restituisce l'elenco delle attività da eseguire, inclusi
gli elementi creati:

![](./media/image35.jpeg)

### Attività 4 : Testare l'applicazione

Per testare l'applicazione, è possibile utilizzare la classe
**TodoResourceTest** esistente . Deve testare l'endpoint REST. Per
testare l'endpoint, utilizza \[RESTAssured\]{.underline}.

1.  Torna a Intellij e apri la classe **TodoResourceTest** da
    **src/test/java/com.example.demo**. Sostituire il codice nella
    classe **TodoResourceTest** con il codice seguente:

package com.example.demo;

import io.quarkus.test.junit.QuarkusTest;

import static io.restassured.RestAssured.given;

import static jakarta.ws.rs.core.HttpHeaders.CONTENT_TYPE;

import static jakarta.ws.rs.core.MediaType.APPLICATION_JSON;

import org.junit.jupiter.api.Test;

@QuarkusTest

classe TodoResourceTest {

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

![Schermata di un programma per computer Descrizione generata
automaticamente](./media/image36.jpeg)

2.  Quando si esegue il test dell'applicazione, Docker Desktop deve
    essere in esecuzione perché Quarkus rileva che è necessario il
    database PostgreSQL per il test.

3.  Torna a **Gitbash** e Ctrl + C. Esegui i comandi seguenti per
    testare l'applicazione utilizzando questo comando:

./mvnw clean test

![Lo schermo di un computer con testo e immagini Descrizione generata
automaticamente](./media/image37.jpeg)

Dovresti vedere un output simile al seguente:

![Schermo di un computer con testo e numeri Descrizione generata
automaticamente](./media/image38.jpeg)

## Esercizio 2 - Configurare le app Azure Container

In questo esercizio, viene creato un gruppo di risorse di Azure che
contiene le risorse per l'applicazione. È quindi possibile configurare
il database PostgreSQL usando l'interfaccia della riga di comando di
Azure. Infine, si configura l'applicazione Quarkus per accedere al
database PostgreSQL remoto. Usa un terminale a tua scelta per eseguire i
comandi.

## Compito 1 : Preparare l'ambiente di lavoro

È necessario impostare alcune variabili d'ambiente. Di seguito sono
riportate alcune note sulle variabili che verranno create:

[TABLE]

**Nota:** è possibile denominare le risorse di Azure nel modo
desiderato. Questo articolo fornisce abbreviazioni di esempio per molte
risorse di Azure (ad esempio, rg per i gruppi di risorse e ca per le app
contenitore).

1.  Utilizzare i comandi seguenti per impostare le variabili.
    Assicurarsi di modificare i valori come descritto nella tabella
    precedente. Queste variabili d'ambiente vengono utilizzate nel resto
    di questo modulo.

**Nota:** PostgreSQL è supportato solo in **Westus** . Prova prima nella
posizione westus e hai problemi, quindi prova nella posizione vicino a
te

export AZ_PROJECT_Quarkus="azure-deploy-quarkus-"$RANDOM

export AZ_CONTAINERAPP="ca${AZ_PROJECT_Quarkus}"

export AZ_CONTAINERAPP_ENV="cae${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_DB_NAME="postgres${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_USERNAME="azuser123"

export AZ_POSTGRES_PASSWORD="P@55w.rd12345"

export AZ_POSTGRES_SERVER_NAME="psql${AZ_PROJECT_Quarkus}"

![Schermata di un computer Descrizione generata
automaticamente](./media/image39.png)

2.  Torna a Gitbash ed esegui il comando seguente per impostare la
    variabile del gruppo di risorse. Copiare il nome del gruppo di
    risorse.

> export AZ_RESOURCE_GROUP="Your existing resource group"
>
> export AZ_LOCATION="Location near to you"

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image40.png)

![Schermata di testo del computer Descrizione generata
automaticamente](./media/image41.png)

3.  Esegui ''az login'' Apre il browser predefinito per accedere.
    Accedere con l'account della sottoscrizione di Azure.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image42.png)

### Attività 2: Creare un'istanza di Database di Azure per PostgreSQL

1.  A questo punto verrà creato un server PostgreSQL gestito. Eseguire
    il comando seguente per creare una piccola istanza di Database di
    Azure per PostgreSQL:

az postgres flexible-server create --resource-group "$AZ_RESOURCE_GROUP"
--location "$AZ_LOCATION" --name "$AZ_POSTGRES_SERVER_NAME"
--database-name "$AZ_POSTGRES_DB_NAME" --admin-user
"$AZ_POSTGRES_USERNAME" --admin-password "$AZ_POSTGRES_PASSWORD"
--public-access "All" --tier "Burstable" --sku-name "Standard_B1ms"
--storage-size 32 --version "16"

![Schermata di un codice informatico Descrizione generata
automaticamente](./media/image43.jpeg)

2.  Questo comando crea un piccolo server PostgreSQL che utilizza le
    variabili configurate in precedenza.

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente](./media/image44.jpeg)

### Attività 3 : Configurare Quarkus per accedere al database PostgreSQL

1.  A questo punto si collegherà l'applicazione Quarkus al database
    PostgreSQL. Per fare ciò, è necessario prima ottenere la stringa di
    connessione per il database:

2.  Eseguire il comando seguente per ottenere la stringa di connessione
    per il database.

3.  export POSTGRES_CONNECTION_STRING=$(

> az postgres flexible-server show-connection-string --server-name
> "$AZ_POSTGRES_SERVER_NAME" --database-name "$AZ_POSTGRES_DB_NAME"
> --admin-user "$AZ_POSTGRES_USERNAME" --admin-password
> "$AZ_POSTGRES_PASSWORD" --query "connectionStrings.jdbc" --output tsv

)

export
POSTGRES_CONNECTION_STRING_SSL="$POSTGRES_CONNECTION_STRING&ssl=true&sslmode=require"

echo "POSTGRES_CONNECTION_STRING_SSL=$POSTGRES_STRINGA_CONNESSIONE_SSL"

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image45.jpeg)

4.  Prendere nota della stringa di connessione restituita.

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image46.jpeg)

### Attività 4 : Configurare l'applicazione Quarkus per la connessione al database PostgreSQL

1.  Torna all'IDE Intellij. Aggiornare il file
    **application.properties** nella cartella **src/main/resources** del
    progetto per configurare la stringa di connessione al database
    PostgreSQL.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image47.jpeg)

2.  Imposta la proprietà **quarkus.datasource.jdbc.url** sul valore
    **\\POSTGRES_CONNECTION_STRING_SSL** di output precedente.

> La parte **&ssl=true&sslmode=require** della stringa di connessione
> forza il driver a utilizzare SSL, un requisito per Database Azure per
> PostgreSQL.

quarkus.hibernate-orm.database.generation=update

quarkus.datasource.jdbc.url=\<the POSTGRES_CONNECTION_STRING_SSL\>

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image48.jpeg)

### Attività 5 : Eseguire l'applicazione Quarkus in locale per testare la connessione al database remoto

1.  Torna a Gitbash ed esegui il comando seguente per eseguire
    l'applicazione localmente:

./mvnw clean quarkus :dev

![Lo schermo di un computer con testo e immagini Descrizione generata
automaticamente](./media/image49.jpeg)

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image50.jpeg)

![](./media/image51.jpeg)

2.  Quando Quarkus è in esecuzione, crea alcune cose da fare utilizzando
    i seguenti comandi cURL in una finestra di terminale separata:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description": "Take Quarkus MS Learn", "details": "Take the
> MS Learn on deploying Quarkus to Azure Container Apps" ,"done":
> "true"}' \\

''http://127.0.0.1:8080/api/todos''

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image52.jpeg)

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description": "Take Azure Container Apps MS Learn", "details":
"Take the ACA Learn module", "done": "false"}' \\

'' http://127.0.0.1:8080/api/todos''

![](./media/image53.jpeg)

3.  Verificare quindi che le attività siano presenti nel database
    accedendo all'endpoint GET definito nell'app attività da eseguire:

''curl http://127.0.0.1:8080/api/todos''

Dovrebbe essere visualizzato l'output seguente:

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image54.jpeg)Se viene visualizzato questo
output, l'applicazione Quarkus è stata eseguita correttamente e si è
connessi al database PostgreSQL remoto.

## Esercizio 3 : Distribuire un'applicazione Quarkus in Azure Container Apps

In questo esercizio viene creato l'ambiente App Azure Container usando
l'interfaccia della riga di comando di Azure.

### Attività 1 : Configurare il Dockerfile per l'applicazione Quarkus

1.  Container Apps viene utilizzato per distribuire applicazioni
    containerizzate. È quindi necessario prima di tutto containerizzare
    l'applicazione Quarkus in un'immagine Docker. Questo processo è
    semplice perché il plug-in Quarkus Maven ha già generato alcuni
    Dockerfile in **src/main/docker**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image55.jpeg)

2.  Torna a Gitbash e premi Ctrl+C . Eseguire il comando seguente per
    rinominare uno di questi **Dockerfiles, *Dockerfile.jvm*,** in
    ***Dockerfile*** e spostarlo nella cartella radice:

''mv src/main/docker/Dockerfile.jvm ./Dockerfile''

![Una schermata nera con testo bianco Descrizione generata
automaticamente](./media/image56.jpeg)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image55.jpeg)

3.  Sostituisci il contenuto dopo il commento lungo nel **Dockerfile**
    con il seguente, ad esempio alla riga \# 80

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

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image57.jpeg)

4.  Questo Dockerfile prevede che l'applicazione Quarkus venga inserita
    in un pacchetto come ***quarkus-run.jarfile***. Questo nome è il
    nome predefinito per l'applicazione Quarkus quando viene inserita in
    un pacchetto come file JAR. È necessario assicurarsi che
    l'applicazione Quarkus sia impacchettata come file JAR. A tale
    scopo, eseguire il seguente comando Maven:

''./mvnw package''

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image58.jpeg)

![](./media/image59.jpeg)

5.  Questo comando crea un pacchetto dell'applicazione Quarkus in un
    file JAR e genera un *file **quarkus-run.jar*** nella cartella
    ***target/quarkus-app***.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image60.jpeg)

### Attività 2: Creare l'ambiente App contenitore e distribuire il contenitore

1.  Ora che il Dockerfile si trova nella posizione corretta, è possibile
    creare l'ambiente App contenitore e distribuire il contenitore
    usando un singolo comando dell'interfaccia della riga di comando di
    Azure. Eseguire il comando seguente nella radice del progetto:

az containerapp up --name "$AZ_CONTAINERAPP" --environment
"$AZ_CONTAINERAPP_ENV" --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --ingress external --target-port 8080 --source .

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image61.jpeg)

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image61.jpeg)

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image62.jpeg)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image63.jpeg)

2.  Questo comando esegue diverse operazioni:

    - Crea un ambiente di app contenitore se non esiste

    - Crea un registro di Azure se non esiste

    - Crea un'area di lavoro Log Analytics se non esiste

    - Compila l'immagine Docker e ne esegue il push nel registro di
      Azure

    - Distribuisce l'immagine Docker nell'ambiente Container Apps

L'esecuzione del comando az containerapp up richiede del tempo. Dovrebbe
essere visualizzato un output simile al seguente:

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image64.jpeg)

### Attività 3 : Convalidare la distribuzione

È possibile verificare che la distribuzione sia riuscita in diversi
modi. Il modo più semplice consiste nel cercare il gruppo di risorse nel
portale di Azure. Verranno visualizzate risorse simili alle seguenti:

1.  Aprire un browser e passare a ''https:\\portal.azure.com'' e
    accedere con l'account della sottoscrizione di Azure. Fare clic sul
    riquadro **Resource Group.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image65.jpeg)

2.  Fare clic sul nome del resource group.

![](./media/image66.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image67.png)

3.  È inoltre possibile controllare la distribuzione eseguendo il
    comando seguente. Elenca tutte le risorse create dal comando az
    containerapp up.

az resource list --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --output table

Dovresti vedere un output simile al seguente:

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image68.png)

### Attività 4 : Eseguire l'applicazione Quarkus distribuita

1.  A questo punto è possibile eseguire l'applicazione Quarkus
    distribuita. Innanzitutto, è necessario ottenere l'URL
    dell'applicazione.

2.  Torna a Gitbash ed esegui il comando seguente per ottenere l'URL
    dell'applicazione.

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

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image69.jpeg)

3.  L'applicazione è pronta all'indirizzo
    https://\\app-name\>.azurecontainerapps.io/. Si noti il protocollo
    https. Tale protocollo viene utilizzato perché l'applicazione viene
    distribuita con un certificato TLS. Per testare l'applicazione, è
    possibile utilizzare cURL:

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Configuration","details":"Congratulations, you
> have set up your Quarkus application correctly!", "done": "true"}' \\

'' https://$AZ_APP_URL/api/todos''

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image70.jpeg)

4.  Recuperare i data utilizzando una nuova richiesta cURL:

''curl https://$AZ_APP_URL/api/todos''

5.  Questo comando restituisce l'elenco di tutte le attività dal
    database:

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image71.jpeg)

6.  Tornare al portale di Azure e fare clic sul nome dell'app
    contenitore.

![](./media/image72.png)

7.  Fare clic sul collegamento all'URL dell'applicazione. Apre l'app
    nella scheda del browser.

![](./media/image73.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image74.png)

8.  Eseguire questo comando, è possibile trasmettere i log per il
    contenitore quando si creano nuove cose da fare:

az containerapp logs show --name "$AZ_CONTAINERAPP" --resource-group
"$AZ_RESOURCE_GROUP" -–follow

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente](./media/image75.png)

9.  Esegui più comandi cURL. Dovresti vedere i log scorrere nel
    terminale.

''curl https://$AZ_APP_URL/api/todos''

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente](./media/image76.png)

## Esercizio 4 : Eliminare le risorse nel gruppo di risorse

### Attività 1 : Eliminare le risorse.

1.  Tornare al portale di Azure. Fare clic su **Resource groups**.

![](./media/image77.png)

2.  Fare clic su Nome resource group.

![](./media/image78.png)

3.  Seleziona tutte le risorse e poi clicca su **Delete** (NON ELIMINARE
    – Resource group)

![](./media/image79.png)

4.  Immettere ''elimina'' e quindi fare clic su **Delete**.

![](./media/image80.png)

5.  Confermare l'eliminazione delle risorse.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image81.png)

**Sommario**

\[Hai imparato a utilizzare Maven per avviare l'applicazione e un
ambiente di sviluppo integrato (IDE) per modificare il codice. Si è
appreso come utilizzare Docker per avviare un database PostgreSQL locale
in modo da poter eseguire e testare l'applicazione in locale.
L'applicazione Quarkus è stata eseguita correttamente e si è connessa al
database PostgreSQL remoto.
