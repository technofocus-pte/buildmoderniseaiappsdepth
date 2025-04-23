# Cas d'usage 01 - Incorporer des applications Quarkus Todo dans des applications de conteneur et les intégrer à une base de données relationnelle.

**Objectif :**

Ce cas d'usage montre comment développer, configurer et déployer une
application Quarkus To-d sécurisée dans Container Apps connectée à une
base de données Azure Database for PostgreSQL. Lorsque vous avez
terminé, une application Quarkus s'exécute sur Azure App Service sur
Linux.

**Principales technologies utilisées** : Java 17, Azure Database for
PostgreSQL

**Durée estimée** -- 45 minutes

**Type de Lab :** Dirigé par un instructeur

### Tâche 0: Configurer les variables d'environnement

1.  Recherchez la variable d'environnement dans le menu Démarrer de
    Windows et sélectionnez Modifier la variable d'environnement
    système.

![](./media/image1.jpeg)

2.  Cliquez sur le bouton **Environment Variable**.

![](./media/image2.jpeg)

3.  Sélectionnez **JAVA_HOME** sous **User variable for Admin**, puis
    cliquez sur **Edit**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image3.jpeg)

4.  Entrez la valeur de la variable **C:\Program Files\Java\jdk-17**
    puis cliquez sur **Ok**.

![](./media/image4.jpeg)

5.  Naviguez jusqu'au dossier **C:\Software** et faites un clic droit
    sur **apache-maven-3.9.4-bin.zip** dossier et sélectionnez **Extract
    All**.

![](./media/image5.jpeg)

6.  **Extraire (Extract)** dans le même dossier.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.jpeg)

7.  Revenez à la fenêtre Modifier la variable d'environnement
    (Environment variable), sélectionnez**-MAVEN_HOME** et cliquez sur
    **Edit**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image7.jpeg)

8.  Entrez la valeur de la variable sous la forme
    C:\Software\apache-maven-3.9.4-bin\apache-maven-3.9.4\`puis cliquez
    sur **OK.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image8.jpeg)

9.  Dans la fenêtre **Environmental Variables**, cliquez sur **OK** puis
    à nouveau sur **OK.**

![](./media/image9.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image10.jpeg)

10. Mettez à jour **JAVA_HOME** conformément aux exigences du Lab avant
    d'exécuter lab.

## Exercice 1 : Générer l'application Quarkus à l'aide de Maven

Il existe plusieurs façons de générer une structure de projet Quarkus.
Vous pouvez utiliser l'interface Web Quarkus, un plug-in IDE ou le
plug-in Quarkus Maven. Utilisons le plugin Maven pour générer la
structure du projet.

Vous générez votre application avec plusieurs dépendances :

- La dépendance reposante pour exposer un point de terminaison REST

- Dépendance jackson pour la sérialisation et la désérialisation de JSON

- La dépendance de mise en veille prolongée pour interagir avec la base
  de données

- La dépendance postgresql pour se connecter à la base de données
  PostgreSQL

- La dépendance docker pour créer une image Docker

Vous n'avez pas besoin de spécifier de dépendances Azure, car vous
exécutez d'abord votre application localement, puis déployez une version
conteneurisée de celle-ci sur Azure Container Apps.

### Tâche 1 : Générer l'application Quarkus

1.  Ouvrez **Git Bash** à partir du menu Démarrer de Windows et exécutez
    la commande ci-dessous

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

2.  Cette commande crée un nouveau projet Quarkus. Il génère une
    structure de répertoire Maven (src/main/java pour le code source et
    src/test/java pour les tests). Il crée des classes Java, des tests
    et des Dockerfiles. Il génère également un *fichier pom.xml* avec
    toutes les dépendances nécessaires (Hibernate, RESTEasy, Jackson,
    PostgreSQL et Docker) :

3.  Cliquez sur Rechercher et tapez \`\`IntelliJ IDE\`\`puis
    sélectionnez **IntelliJ IDE**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image12.jpeg)

4.  Cochez la case de confirmation, puis cliquez sur le bouton
    **Continue**.

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement](./media/image13.jpeg)

5.  Fermez la fenêtre Partage de données.

![](./media/image14.jpeg)

6.  Sélectionnez la case d'option **Start trial**, puis cliquez sur le
    bouton Démarrer **Start trial**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image15.jpeg)

7.  Cliquez sur le bouton **Allow access**.

![](./media/image16.jpeg)

8.  Fermez le navigateur, revenez à la fenêtre de licence IntelliJ et
    cliquez sur le bouton **Continue**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image17.jpeg)

9.  Cliquez sur **Open** le dossier.

![](./media/image18.jpeg)

10. Naviguez jusqu'à **C :\Users\Admin\todo** et sélectionnez le dossier
    du projet **todo**, puis cliquez sur **OK.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image19.jpeg)

11. Cliquez sur le bouton **Trust project**.

![](./media/image20.jpeg)

12. Ouvrez **pom.xml** et vous devriez voir ci-dessous le format xml.

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

\</dependency\>

**Remarque** : Toutes les dépendances dans le fichier *pom.xml* sont
définies dans le *BOM (bill of materials)* de Quarkus :
*io.quarkus.platform:quarkus-bom*. ![](./media/image21.jpeg)

### Tâche 2 : Coder l'application

1.  Allez dans **src/main/java/com.example.demo** et faites un clic
    droit sur **MyEntity.Java -\> Refactor -\> Rename**.

![](./media/image22.jpeg)

2.  Renommez la classe ***MyEntity.java*** générée en
    \`\`**Todo.java\`\`** (située dans le même dossier que le *fichier
    TodoResource.java*)

![](./media/image23.jpeg)

3.  Remplacez le code existant par le code Java suivant. Il utilise
    l'API Java Persistence (package jakarta.persistence.\*) pour stocker
    et récupérer les données de votre serveur PostgreSQL. Il utilise
    également \[Hibernate ORM with Panache\]{.underline} (héritier de
    io.quarkus.hibernate.orm.panache.PanacheEntity) pour simplifier la
    couche de persistance.

4.  Vous utilisez une entité JPA (@Entity) pour mapper l'objet Java Todo
    directement à la table Todo PostgreSQL. Le point de terminaison REST
    TodoResource crée ensuite une nouvelle classe d'entité Todo et la
    conserve. Cette classe est un modèle de domaine qui est mappé sur la
    table Todo. La table est automatiquement créée par JPA.

5.  L'extension de PanacheEntity vous permet d'obtenir un certain nombre
    de méthodes génériques de création, de lecture, de mise à jour et de
    suppression (CRUD) pour votre type. Vous pouvez donc faire des
    choses comme enregistrer et supprimer des objets Todo dans une seule
    ligne de code Java.

6.  Configurez JDK dans IntelliJ s'il n'est pas déjà défini.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image24.jpeg)

7.  Remplacez le code existant par le code Java suivant dans l'entité
    Todo :

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

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image25.jpeg)

8.  Pour gérer cette classe, mettez à jour **TodoResource** afin qu'il
    puisse publier des interfaces REST pour stocker et récupérer des
    données à l'aide de HTTP. Ouvrez la classe **TodoResource** et
    remplacez le code par ce qui suit :

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

### ![Une capture d'écran d'un programme informatique Description générée automatiquement](./media/image26.jpeg)

### **Tâche 3 : Exécuter l'application**

Lorsque vous exécutez l'application en mode développement, Docker
Desktop doit être en cours d'exécution. En effet, Quarkus détecte que
vous avez besoin d'une base de données PostgreSQL (en raison de la
dépendance PostgreSQL quarkus-jdbc-postgresql déclarée dans le fichier
*pom.xml*), télécharge l'image PostgreSQL Docker Desktop et démarre un
conteneur avec la base de données. Il crée ensuite automatiquement la
table Todo dans la base de données.

1.  Double-cliquez sur **Docker Desktop** et réduisez la fenêtre.
    Assurez-vous qu'il est en marche. (pas besoin de se connecter)

![Un écran d'ordinateur avec un fond blanc Description générée
automatiquement](./media/image27.jpeg)

2.  Retournez à Gitbash et exécutez l'application to-do à l'aide de
    cette commande :

cd todo

./mvnw quarkus:dev

![](./media/image28.jpeg)

3.  L'application Quarkus doit démarrer et se connecter à votre base de
    données. Vous devriez voir le résultat suivant :

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image29.jpeg)

![Un écran d'ordinateur avec du texte et des images Description générée
automatiquement](./media/image30.jpeg)

![](./media/image31.jpeg)

![Capture d'écran d'un programme informatique Description générée
automatiquement](./media/image32.jpeg)

4.  Cliquez sur **Allow access**.

![](./media/image31.jpeg)

5.  Pour tester l'application, vous pouvez utiliser cURL.

Dans une nouvelle instance distincte de Gitbash , créez une nouvelle
tâche dans la base de données à l'aide de la commande suivante. Le
journal doit s'afficher dans la console Quarkus :

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description":"Take Quarkus MS Learn","details":"Take the MS
Learn on deploying Quarkus to Azure Container Apps","done": "true"}' \\

http://127.0.0.1:8080/api/todos

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image33.jpeg)

6.  Cette commande doit renvoyer l'élément créé (avec un identifiant) :

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image33.jpeg)

7.  Créez une deuxième tâche à l'aide de la commande cURL suivante :

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Take Azure Container Apps MS
> Learn","details":"Take the ACA Learn module","done": "false"}' \\

http://127.0.0.1:8080/api/todos

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image34.jpeg)

8.  Ensuite, récupérez les données à l'aide d'une nouvelle requête cURL
    :

curl http://127.0.0.1:8080/api/todos

Cette commande renvoie la liste des tâches, y compris celles que vous
avez créées :

![](./media/image35.jpeg)

### Tâche 4 : Tester l'application

Pour tester l'application, vous pouvez utiliser la classe
**TodoResourceTest** existante. Il doit tester le point de terminaison
REST. Pour tester le point de terminaison, il utilise
\[RESTAssured\]{.underline}.

1.  Revenez à Intellij et ouvrez la classe **TodoResourceTest** à partir
    de **src/test/java/com.example.demo**. Remplacez le code de la
    classe **TodoResourceTest** par le code suivant :

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

}![Une capture d'écran d'ordinateur d'un programme Description générée
automatiquement](./media/image36.jpeg)

2.  Lorsque vous testez l'application, Docker Desktop doit être en cours
    d'exécution, car Quarkus détecte qu'il a besoin de la base de
    données PostgreSQL pour les tests.

3.  Revenez à **Gitbash** et Ctrl + C. Exécutez les commandes ci-dessous
    pour tester l'application à l'aide de cette commande :

./mvnw clean test

![Un écran d'ordinateur avec du texte et des images Description générée
automatiquement](./media/image37.jpeg)

Vous devriez voir un résultat qui ressemble à ceci :

![Un écran d'ordinateur avec du texte et des chiffres Description
générée automatiquement](./media/image38.jpeg)

## Exercice 2 : Configurer Azure Container Apps

Dans cet exercice, vous allez créer un groupe de ressources Azure qui
contient les ressources de l'application. Vous configurez ensuite la
base de données PostgreSQL à l'aide d'Azure CLI. Enfin, vous configurez
l'application Quarkus pour qu'elle accède à la base de données
PostgreSQL distante. Utilisez le terminal de votre choix pour exécuter
les commandes.

## Tâche 1: Préparer l'environnement de travail

Vous devez configurer certaines variables d'environnement. Voici
quelques notes sur les variables que vous allez créer :

[TABLE]

**Remarque :** Vous pouvez nommer vos ressources Azure de la manière que
vous souhaitez. Cet article fournit des exemples d'abréviations pour de
nombreuses ressources Azure (par exemple, rg pour les groupes de
ressources et ca pour les applications de conteneur).

1.  Utilisez les commandes suivantes pour configurer les variables.
    Veillez à modifier les valeurs comme décrit dans le tableau
    précédent. Ces variables d'environnement sont utilisées dans le
    reste de ce module.

**Remarque :** PostgreSQL n'est pris en charge que dans **Westus**.
Essayez d'abord dans l'emplacement westus et si vous rencontrez des
problèmes, essayez dans l'emplacement près de chez vous

export AZ_PROJECT_Quarkus="azure-deploy-quarkus-"$RANDOM

export AZ_CONTAINERAPP="ca${AZ_PROJECT_Quarkus}"

export AZ_CONTAINERAPP_ENV="cae${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_DB_NAME="postgres${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_USERNAME="azuser123"

export AZ_POSTGRES_PASSWORD="P@55w.rd12345"

export AZ_POSTGRES_SERVER_NAME="psql${AZ_PROJECT_Quarkus}"

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image39.png)

2.  Revenez à Gitbash et exécutez la commande ci-dessous pour définir la
    variable du groupe de ressources. Copiez le nom du groupe de
    ressources.

> export AZ_RESOURCE_GROUP="Your existing resource group"
>
> export AZ_LOCATION="Location near to you"

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image40.png)

![Une capture d'écran d'ordinateur de texte Description générée
automatiquement](./media/image41.png)

3.  Exécutez ''az login'' Il ouvre le navigateur par défaut pour se
    connecter. Connectez-vous avec votre compte d'abonnement Azure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image42.png)

### Tâche 2 : Créer une instance d'Azure Database for PostgreSQL

1.  Vous allez maintenant créer un serveur PostgreSQL géré. Exécutez la
    commande suivante pour créer une petite instance d'Azure Database
    for PostgreSQL :

az postgres flexible-server create --resource-group "$AZ_RESOURCE_GROUP"
--location "$AZ_LOCATION" --name "$AZ_POSTGRES_SERVER_NAME"
--database-name "$AZ_POSTGRES_DB_NAME" --admin-user
"$AZ_POSTGRES_USERNAME" --admin-password "$AZ_POSTGRES_PASSWORD"
--public-access "All" --tier "Burstable" --sku-name "Standard_B1ms"
--storage-size 32 --version "16"

![Capture d'écran d'un code informatique Description générée
automatiquement](./media/image43.jpeg)

2.  Cette commande crée un petit serveur PostgreSQL qui utilise les
    variables que vous avez configurées précédemment.

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement](./media/image44.jpeg)

### Tâche 3 : Configurer Quarkus pour accéder à la base de données PostgreSQL

1.  Vous allez maintenant connecter l'application Quarkus à la base de
    données PostgreSQL. Pour ce faire, vous devez d'abord obtenir la
    chaîne de connexion de la base de données :

2.  Exécutez la commande below pour obtenir la chaîne de connexion de la
    base de données.

3.  exportation POSTGRES_CONNECTION_STRING=$(

> az postgres flexible-server show-connection-string --server-name
> "$AZ_POSTGRES_SERVER_NAME" --database-name "$AZ_POSTGRES_DB_NAME"
> --admin-user "$AZ_POSTGRES_USERNAME" --admin-password
> "$AZ_POSTGRES_PASSWORD" --query "connectionStrings.jdbc" --output tsv

)

export
POSTGRES_CONNECTION_STRING_SSL="$POSTGRES_CONNECTION_STRING&ssl=true&sslmode=require"

echo "POSTGRES_CONNECTION_STRING_SSL=$POSTGRES_CONNECTION_STRING_SSL"

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image45.jpeg)

4.  Notez la chaîne de connexion renvoyée.

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image46.jpeg)

### Tâche 4 : Configurer l'application Quarkus pour se connecter à la base de données PostgreSQL

1.  Revenez à l'IDE Intellij. Mettez à jour le fichier
    **application.properties** dans le dossier **src/main/resources** du
    projet pour configurer la chaîne de connexion à la base de données
    PostgreSQL.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image47.jpeg)

2.  Attribuez à la propriété **quarkus.datasource.jdbc.url** la valeur
    **\\POSTGRES\_\_CONNECTION_STRING_SSL** précédemment affichée. La
    partie **&ssl=true&sslmode=require** de la chaîne de connexion force
    le pilote à utiliser SSL, une exigence pour Azure Database for
    PostgreSQL.

quarkus.hibernate-orm.database.generation=update

quarkus.datasource.jdbc.url=\<the POSTGRES_CONNECTION_STRING_SSL value\>

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image48.jpeg)

### Tâche 5 : Exécuter l'application Quarkus localement pour tester la connexion à la base de données distante

1.  Revenez à Gitbash et exécutez la commande ci-dessous pour exécuter
    l'application localement :

./mvnw clean quarkus:dev

![Un écran d'ordinateur avec du texte et des images Description générée
automatiquement](./media/image49.jpeg)

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image50.jpeg)

![](./media/image51.jpeg)

2.  Lorsque Quarkus est en cours d'exécution, créez quelques tâches à
    l'aide des commandes cURL suivantes dans une fenêtre de terminal
    distincte :

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Take Quarkus MS Learn","details":"Take the MS
> Learn on deploying Quarkus to Azure Container Apps","done": "true"}'
> \\

''http://127.0.0.1:8080/api/todos''

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image52.jpeg)

curl --header "Content-Type: application/json" \\

--request POST \\

--data '{"description":"Take Azure Container Apps MS
Learn","details":"Take the ACA Learn module","done": "false"}' \\

'' http://127.0.0.1:8080/api/todos''

![](./media/image53.jpeg)

3.  Ensuite, vérifiez que les tâches se trouvent dans la base de données
    en accédant au point de terminaison GET défini dans l'application de
    tâches :

\`\`curl http://127.0.0.1:8080/api/todos\`\`

Vous devriez voir le résultat suivant :

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image54.jpeg)Si vous voyez ce résultat, cela
signifie que vous avez exécuté l'application Quarkus et que vous vous
êtes connecté à la base de données PostgreSQL distante.

## Exercice 3 : Déployer une application Quarkus sur Azure Container Apps

Dans cet exercice, vous allez créer l'environnement Azure Container Apps
à l'aide d'Azure CLI.

### Tâche 1 : Configurer le Dockerfile pour l'application Quarkus

1.  Container Apps est utilisé pour déployer des applications
    conteneurisées. Vous devez donc d'abord conteneuriser l'application
    Quarkus dans une image Docker. Ce processus est facile car le plugin
    Quarkus Maven a déjà généré des fichiers Dockerfile sous
    **src/main/docker**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image55.jpeg)

2.  Revenez à Gitbash et appuyez sur Ctrl+C . Exécutez la commande
    ci-dessous pour renommer l'un de ces **Dockerfiles,
    *Dockerfile.jvm*,** en ***Dockerfile*** et le déplacer vers le
    dossier racine :

\`\`mv src/main/docker/Dockerfile.jvm ./Dockerfile\`\`

![Un écran noir avec du texte blanc Description générée
automatiquement](./media/image56.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image55.jpeg)

3.  Remplacez le contenu après le long commentaire dans le
    **Dockerfile** par ce qui suit, c'est-à-dire à la ligne \# 80

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

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image57.jpeg)

4.  Ce fichier Dockerfile s'attend à ce que l'application Quarkus soit
    empaquetée sous la forme d’un ***quarkus-run.jar* file**. Ce nom est
    le nom par défaut de l'application Quarkus lorsqu'elle est
    empaquetée sous forme de fichier JAR. Vous devez vous assurer que
    l'application Quarkus est empaquetée sous forme de fichier JAR. Pour
    ce faire, exécutez la commande Maven suivante :

\`\`./mvnw package\`\`

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image58.jpeg)

![](./media/image59.jpeg)

5.  Cette commande empaquette l'application Quarkus dans un fichier JAR
    et génère un fichier ***quarkus-run.jar*** dans le dossier
    ***target/quarkus-app***.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image60.jpeg)

### Tâche 2 : Créer l'environnement Container Apps et déployer le conteneur

1.  Maintenant que le fichier Dockerfile se trouve au bon emplacement,
    vous pouvez créer l'environnement Container Apps et déployer le
    conteneur à l'aide d'une seule commande Azure CLI. Exécutez la
    commande suivante à la racine du projet :

az containerapp up --name "$AZ_CONTAINERAPP" --environment
"$AZ_CONTAINERAPP_ENV" --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --ingress external --target-port 8080 --source .

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image61.jpeg)

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image61.jpeg)

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image62.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image63.jpeg)

2.  Cette commande fait plusieurs choses :

    - Crée un environnement Container Apps s'il n'existe pas

    - Crée un registre Azure s'il n'existe pas

    - Crée un espace de travail Log Analytics s'il n'existe pas

    - Génère l'image Docker et l'envoie au registre Azure

    - Déploie l'image Docker dans l'environnement Container Apps

L'exécution de la commande az containerapp up prend un certain temps.
Vous devriez voir une sortie similaire à ce qui suit :

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image64.jpeg)

### Tâche 3 : Valider le déploiement

Vous pouvez vérifier que le déploiement a réussi de plusieurs façons. Le
moyen le plus simple consiste à rechercher votre groupe de ressources
sur le portail Azure. Vous devriez voir des ressources similaires à ce
qui suit :

1.  Ouvrez un navigateur et accédez à \`\`https:\\portal.azure.com\`\`
    et connectez-vous avec votre compte d'abonnement Azure. Cliquez sur
    la vignette Groupe de ressources (Resource Group).

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image65.jpeg)

2.  Cliquez sur le nom du groupe de ressources.

![](./media/image66.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image67.png)

3.  Vous pouvez également vérifier le déploiement en exécutant la
    commande suivante. Il répertorie toutes les ressources créées par la
    commande az containerapp up.

az resource list --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --output table

Vous devriez voir une sortie similaire à celle-ci :

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image68.png)

### Tâche 4 : Exécuter l'application Quarkus déployée

1.  Vous pouvez maintenant exécuter l'application Quarkus déployée. Tout
    d'abord, vous devez obtenir l'URL de l'application.

2.  Revenez à Gitbash et exécutez la commande ci-dessous pour obtenir
    l'URL de l'application.

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

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image69.jpeg)

3.  Votre application est prête à l'adresse
    https://\\nom-application\>.azurecontainerapps.io/. Notez le
    protocole https. Ce protocole est utilisé parce que l'application
    est déployée avec un certificat TLS. Pour tester l'application, vous
    pouvez utiliser cURL :

> curl --header "Content-Type: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Configuration","details":"Congratulations, you
> have set up your Quarkus application correctly!","done": "true"}' \\

\`\` https://$AZ_APP_URL/api/todos\`\`

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image70.jpeg)

4.  Récupérez les données à l'aide d'une nouvelle requête cURL :

''curl https ://$AZ_APP_URL/api/todos''

5.  Cette commande renvoie la liste de toutes les tâches de la base de
    données :

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image71.jpeg)

6.  Revenez au portail Azure et cliquez sur le nom de votre application
    conteneur.

![](./media/image72.png)

7.  Cliquez sur le lien de l'URL de l'application. Il ouvre
    l'application dans l'onglet du navigateur.

![](./media/image73.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image74.png)

8.  Exécutez cette commande, vous pouvez diffuser les journaux de votre
    conteneur lorsque vous créez de nouvelles tâches :

az containerapp logs show --name "$AZ_CONTAINERAPP" --resource-group
"$AZ_RESOURCE_GROUP" -–follow

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement](./media/image75.png)

9.  Exécutez d'autres commandes cURL. Vous devriez voir les journaux
    défiler dans le terminal.

\`\`curl https://$AZ_APP_URL/api/todos\`\`![Une capture d'écran d'un
écran d'ordinateur Description générée
automatiquement](./media/image76.png)

## Exercice 4 : Supprimer des ressources dans le groupe de ressources

### Tâche 1 : Supprimer des ressources.

1.  Revenez au portail Azure. Cliquez sur **Resource groups**.

![](./media/image77.png)

2.  Cliquez sur le nom du groupe de ressources.

![](./media/image78.png)

3.  Sélectionnez toutes les ressources, puis cliquez sur **Delete** (Ne
    PAS SUPPRIMER – Groupe de ressources)

![](./media/image79.png)

4.  Entrez \`\`delete\`\` puis cliquez sur **Delete**.

![](./media/image80.png)

5.  Confirmez la suppression des ressources.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image81.png)

**Résumé**

\[Vous avez appris à utiliser Maven pour démarrer l'application et un
environnement de développement intégré (IDE) pour modifier le code. Vous
avez appris à utiliser Docker pour démarrer une base de données
PostgreSQL locale afin de pouvoir exécuter et tester l'application
localement. Vous avez exécuté l'application Quarkus et vous vous êtes
connecté à la base de données PostgreSQL distante.
