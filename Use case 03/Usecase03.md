# Cas d'usage 03 - Conteneurisation d'une application de réservation de vol et déploiement sur Azure Kubernetes Service

**Objectif** :

À la fin de ce module, vous serez en mesure de :

- Conteneuriser une application Java.

- Créez une image conteneur pour l'application Java.

- Exécutez l'image conteneur localement.

- Envoyez l'image conteneur à Azure Container Registry.

Déployer l'image conteneur sur Azure Kubernetes Service

**Principales technologies utilisées** -- Java 11, Docker, Maven

**Durée estimée** : 30 min

**Type de Lab :** Dirigé par un instructeur

## Exercice 1 : Configurer votre environnement Azure

Dans cet exercice, vous allez utiliser Azure CLI pour créer les
ressources Azure qui seront nécessaires dans les unités ultérieures. À
l'aide d'Azure CLI, procédez comme suit

### Tâche 1 : S'authentifier auprès d'Azure Resource Manager

1.  Ouvrez Gitbash à partir d'un bureau et exécutez la commande
    ci-dessous pour vous connecter au portail Azure

**''az login''**

**Remarque** : Si vous consultez AVERTISSEMENT : Un navigateur Web a été
ouvert à
https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize.
Veuillez poursuivre la connexion dans le navigateur Web. Si aucun
navigateur Web n'est disponible ou si le navigateur Web ne s'ouvre pas,
utilisez le flux de code de l'appareil avec az login --use-device-code

![Un fond noir avec du texte jaune Description générée
automatiquement](./media/image1.jpeg)

2.  Cette commande vous amènera au navigateur par défaut pour vous
    connecter. Connectez-vous avec votre compte d'abonnement Azure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image2.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image3.jpeg)

3.  Une fois authentifié, revenez au Gitbash

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image4.jpeg)

4.  Maintenant, nous allons activer notre abonnement Azure, exécuter la
    commande ci-dessous

''az account set --subscription « \< YOUR_SUBSCRIPTION_ID \>"''

''az account list --output table''

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image5.jpeg)

5.  Pour simplifier les commandes qui seront exécutées plus loin,
    configurez les variables d'environnement suivantes

Remarque : Nous avons déjà créé un groupe de ressources pour moi dans le
cloud. Vous devez déployer toutes les ressources au sein du groupe de
ressources existant. Vous pouvez le trouver dans votre portail Azureb ou
sous l'onglet Ressources de votre machine virtuelle

> export AZ_CONTAINER_REGISTRY="javaaksregist"$RANDOM
>
> export AZ_KUBERNETES_CLUSTER="javaakscluster"$RANDOM
>
> export AZ_LOCATION="westus"

export AZ_KUBERNETES_CLUSTER_DNS_PREFIX="javaakscontainer"

> export AZ_RESOURCE_GROUP= Your existing resource group name

**Remarque :** Vous devez remplacer par la région de votre choix, par
exemple : eastus Vous devez remplacer par une valeur unique, car elle
est utilisée pour générer un nom de domaine complet (FQDN) unique pour
votre Azure Container Registry lors de sa création, par exemple :
someuniquevaluejavacontainerregistry.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.png)

8.  Azure Container Registry vous permet de créer, de stocker et de
    gérer des images conteneur, qui sont finalement l'endroit où l'image
    conteneur de l'application Java sera stockée. Créez un registre de
    conteneurs Azure à l'aide des commandes suivantes.

\`\`az acr create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_CONTAINER_REGISTRY --sku Basic | jq\`\`![Une capture d'écran d'un
écran d'ordinateur Description générée
automatiquement](./media/image7.jpeg)

9.  Configurer Azure CLI pour utiliser ce registre de conteneurs Azure
    nouvellement créé

\`\`az configure --defaults acr=$AZ_CONTAINER_REGISTRY\`\`![Un fond noir
avec du texte vert et blanc Description générée
automatiquement](./media/image8.jpeg)

10. S'authentifier auprès d'Azure Container Registry nouvellement créé

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`![Un écran d'ordinateur
avec du texte blanc Description générée
automatiquement](./media/image9.jpeg)

11. Créez un cluster Azure Kubernetes, vous aurez besoin d'un cluster
    Azure Kubernetes pour déployer l'application Java (image conteneur).

az aks create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER --attach-acr $AZ_CONTAINER_REGISTRY
--dns-name-prefix=$AZ_KUBERNETES_CLUSTER_DNS_PREFIX --generate-ssh-keys
| jq

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image10.jpeg)

**Remarque : La** création d'un cluster Azure Kubernetes peut prendre
jusqu'à 10 minutes. Une fois que vous avez exécuté la commande
ci-dessus, vous pouvez éventuellement la laisser continuer dans cet
onglet Azure CLI et passer à l'unité suivante.

### Tâche 2 : Exécuter Docker

1.  Dans le menu Démarrer, cliquez sur **DockerDesktop**

![Une capture d'écran d'un téléphone Description générée
automatiquement](./media/image11.jpeg)

2.  Assurez-vous qu'il fonctionne.

## Exercice 2 : Conteneuriser une application Java

Dans cet exercice, vous allez conteneuriser une application Java.

### Tâche 1 : Créer l'application Java

Tout d'abord, vous allez naviguer dans le référentiel du système de
réservation de vols pour les réservations de compagnies aériennes et sur
le cd dans le dossier du projet de l'application Web des compagnies
aériennes.

Si Java et Maven sont installés, vous pouvez exécuter la ou les
commandes suivantes dans votre CLI pour avoir une idée de l'expérience
de création de l'application sans Docker. Si Java et Maven ne sont pas
installés, vous pouvez passer en toute sécurité à la section suivante
intitulée « Construire un fichier Docker », dans cette section, vous
utiliserez Docker pour extraire Java et Maven pour exécuter les
constructions en votre nom.

1.  Exécutez la commande suivante dans votre interface de ligne de
    commande pour accéder au projet.

\`\`cd
"C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines"\`\`

![Un fond noir avec du texte Description générée
automatiquement](./media/image12.jpeg)

2.  Exécutez la commande suivante dans votre CLI

\`\`mvn clean install\`\`

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image13.jpeg)

**Remarque :** La commande mvn clean install a été utilisée pour
illustrer les défis opérationnels liés à l'absence d'utilisation des
versions en plusieurs étapes de Docker, que nous aborderons ensuite.
Encore une fois, cette étape est facultative, dans tous les cas, vous
pouvez avancer en toute sécurité sans exécuter la commande Maven.

3.  Maven devrait avoir réussi à créer le système de réservation de vols
    pour les réservations de compagnies aériennes Artéfact d'archive Web
    FlightBookingSystemSample-0.0.-SNAPSHOT.war, comme le montre l'image
    suivante

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement](./media/image14.jpeg)

## Tâche 2 : Construire un fichier Docker

1.  À la racine de votre projet,
    containerize-and-deploy-Java-app-to-Azure/Project/Airlines, créez un
    fichier appelé **Dockerfile.**

\`\`vi Dockerfile\`\`![](./media/image15.jpeg)

Ajoutez le contenu suivant à Dockerfile, puis enregistrez-le et
quittez-le

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

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image16.jpeg)

**Remarque :** Si vous le souhaitez, le Dockerfile_Solution à la racine
de votre projet contient le contenu nécessaire. Comme vous pouvez le
voir, cette étape de génération de fichier Docker comporte six
instructions.

## Exercice 3 : Générer et exécuter une image conteneur pour l'application Java

Dans cette unité, vous allez créer et exécuter l'image de conteneur.
Comme mentionné précédemment, une instance en cours d'exécution d'une
image est un conteneur.

### Tâche 1 : Construire une image de conteneur

Maintenant que vous avez réussi à créer un fichier Dockerfile, vous
pouvez demander à Docker de créer une image de conteneur pour vous.

**Remarque :** Assurez-vous que votre runtime Docker est configuré pour
générer des conteneurs Linux. Ceci est important car le Dockerfile
utilisé fait référence à des images de conteneur (JDK/JRE) pour
l'architecture Linux.

1.  **Docker build** est la commande utilisée pour créer des images de
    conteneur. L**'argument -t** (**-t** argument) sera utilisé pour
    spécifier une étiquette de conteneur et le **.** est l'emplacement
    où Docker trouve le Dockerfile. Exécutez la commande suivante dans
    votre interface de ligne de commande.

**IMPORTANT** : Cet atelier nécessite jdk 11. Réglez java_home sur jdk
11

\`\`docker build -t flightbookingsystemsample .\`\`

![Un écran d'ordinateur avec du texte Description générée
automatiquement](./media/image17.jpeg)

2.  La version Docker est quelque chose de similaire

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement](./media/image18.jpeg)

**Remarque :** Comme vous l'avez vu précédemment, Docker a exécuté les
instructions à partir des lignes que vous avez précédemment écrites dans
l'unité précédente. Chaque instruction est une étape dans un ordre
séquentiel. Réexécutez la commande docker build, remarquez les
différences dans les étapes, vous remarquerez ---\> Utilisation du cache
pour les couches qui n'ont pas changé. Si vous n'apportez pas de
modifications à l'application (avant de réexécuter la commande docker
build), vous remarquerez que toutes les couches mises en cache sont
intactes et peuvent provenir du cache Docker. Il s'agit d'un point
important à retenir lors de l'optimisation de vos images de conteneur et
des coûts de calcul associés au temps passé à les créer.

3.  Docker peut également afficher les images disponibles qui sont
    résidentes. Ceci est utile pour afficher ce qui est disponible pour
    l'exécution. Exécutez la commande suivante dans votre CLI

docker image ls

Vous verrez quelque chose de similaire :

![](./media/image19.jpeg)

### Tâche 2 : Exécuter une image conteneur

1.  Maintenant que vous avez réussi à créer une image conteneur, vous
    pouvez l'exécuter.

2.  Docker run est la commande utilisée pour exécuter une image de
    conteneur. L'option -p

:#### sera utilisé pour rediriger le HTTP localhost (le premier

port avant le deux-points) trafic vers le conteneur au moment de
l'exécution (le deuxième port après le deux-points). N'oubliez pas, à
partir du fichier Dockerfile, que le serveur d'applications Tomcat est à
l'écoute du trafic HTTP sur le port 8080, c'est donc le port du
conteneur qui doit être exposé. Enfin, la balise d'image
flightbookingsystemsample est nécessaire pour indiquer à Docker quelle
image exécuter. Exécutez la commande suivante dans votre CLI :

\`\`docker run -p 8080:8080 flightbookingsystemsample\`\`

Vous verrez quelque chose de similaire :

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image20.jpeg)

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement](./media/image21.jpeg)

**Remarque :** si la commande "docker run -p 8080:8080
flightbookingsystemsample" génère une erreur, utilisez le port mentionné
ci-dessous

\`\`docker run -p 8081:8080 flightbookingsystemsample\`\`

3.  Ouvrez un navigateur et visitez la page d'accueil du système de
    réservation de vols pour les réservations de compagnies aériennes à
    http://localhost:8080/FlightBookingSystemSample

    - Vous devriez voir ce qui suit :

![Un avion qui vole dans le ciel Description générée
automatiquement](./media/image22.jpeg)

4.  Vous pouvez éventuellement vous connecter avec n'importe quel
    utilisateur de tomcat-users.xml par exemple

Nom d'utilisateur : **\`\`someuser@azure.com\`\`**

Mot de passe : \`\`password\`\`

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image23.jpeg)

5.  Laissez cette instance git bash telle quelle

## Exercice 4 : Envoyer l'image conteneur à Azure Container Registry

### Tâche 1 : envoyer une image conteneur à Azure Container Registry

1.  Dans cette tâche, vous allez envoyer une image conteneur à Azure
    Container Registry.Azure Container Registry vous permet de créer,
    stocker et gérer des images de conteneur et des artefacts dans un
    registre privé pour tous les types de déploiements de conteneurs.
    Utilisez les registres de conteneurs Azure avec vos pipelines de
    développement et de déploiement de conteneurs existants.

2.  Ouvrez une nouvelle instance de Gitbash et exécutez az command pour
    vous connecter au portail Azure.

\`\`az login\`\`

3.  Exécutez la commande suivante dans votre CLI

\`\`C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines\`\`

4.  Nous utiliserons le même code S'authentifier auprès d'Azure Resource
    Manager que celui que nous avons créé précédemment dans l'exercice 1
    Tâche 1.set ci-dessous

[TABLE]

> **Remarque :** Si votre session a été interrompue, que vous effectuez
> cette étape à un autre moment dans le temps et/ou à partir d'une autre
> interface de ligne de commande, vous devrez peut-être réinitialiser
> vos variables d'environnement et vous authentifier à nouveau avec les
> commandes CLI suivantes.

![Capture d'écran d'un programme informatique Description générée
automatiquement](./media/image24.png)

### Tâche 2 : Envoyer une image de conteneur

Dans cette tâche, vous pouvez envoyer votre image conteneur nouvellement
créée à Azure Container Registry. Ce faisant, votre image de conteneur
sera proche du réseau de toutes vos ressources Azure, telles que votre
cluster Azure Kubernetes. En fin de compte, vous allez configurer AKS
pour extraire l'image flightbookingsystemsample d'Azure Container
Registry.

1.  Pour envoyer l'image conteneur à Azure Container Registry, exécutez
    les trois commandes suivantes dans votre interface de ligne de
    commande

2.  Connectez-vous à **Azure Container Registry** et exécutez la
    commande ci-dessous

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`

![](./media/image25.jpeg)

3.  Tout d'abord, balisez l'image conteneur précédemment créée avec
    votre Azure Container Registry :

\`\`docker tag flightbookingsystemsample
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`

![](./media/image26.jpeg)

4.  Deuxièmement, envoyez l'image conteneur à Azure Container Registry

\`\`docker push
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`![Une
capture d'écran d'un ordinateur Description générée
automatiquement](./media/image27.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image28.jpeg)

5.  Affichez maintenant les métadonnées de l'image Azure Container
    Registry de l'image nouvellement envoyée. Exécutez la commande
    suivante dans votre Interface de ligne de commande

\`\`az acr repository show -n $AZ_CONTAINER_REGISTRY --image
flightbookingsystemsample:latest\`\`

Vous verrez quelque chose de similaire :

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image29.jpeg)

6.  L'image conteneur est désormais résidente dans Azure Container
    Registry et prête pour les déploiements par des services Azure tels
    qu'Azure Kubernetes Service.

## Exercice 5 : Déployer l'image conteneur sur Azure Kubernetes Service

Dans cet exercice, vous allez déployer une image conteneur sur Azure
Kubernetes Service.

### Tâche 1 : Déployer une image conteneur

1.  Vous allez déployer l'image conteneur **flightbookingsystemsample**
    sur votre cluster Azure Kubernetes.

2.  À la racine de votre projet,
    **Flight-Booking-System-JavaServlets_App/Project/Airlines**, créez
    un fichier appelé deployment.yml. Exécutez la commande suivante dans
    votre CLI :

\`\`vi deployment.yml\`\`

![](./media/image30.jpeg)

3.  Ajoutez le contenu suivant à deployment.yml, puis enregistrez et
    quittez :

**Remarque :** Vous devez mettre à jour avec la valeur de votre variable
d'environnement AZ_CONTAINER_REGISTRY définie précédemment, Exercice 1
Tâche1( AZ_CONTAINER_REGISTRY= javaaksregist )

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

spec:

type: LoadBalancer

ports:

\- port: 8080

targetPort: 8080

selector:

app: flightbookingsystemsample

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image31.jpeg)

4.  Appuyez sur Échap et, puis tapez
    \`\`**[wq](urn:gd:lg%F0%9F%85%B0%EF%B8%8Fsend-vm-keys)\`\`** et
    appuyez sur Entrée pour enregistrer le fichier.

**Remarque :** Si vous le souhaitez, le deployment_solution.yml à la
racine de votre projet contient le contenu nécessaire, vous trouverez
peut-être plus facile de renommer/mettre à jour le contenu de ce
fichier.

5.  Dans la deployment.yml ci-dessus, vous remarquerez que ce
    deployment.yml contient un déploiement et un service. Le déploiement
    est utilisé pour administrer un ensemble d'espaces, tandis que le
    service est utilisé pour autoriser l'accès réseau aux espaces. Vous
    remarquerez que les pods sont configurés pour extraire une seule
    image, le fichier \< AZ_CONTAINER_REGISTRY
    \>.azurecr.io/flightbookingsystemsample:latest d'Azure Container
    Registry. Vous remarquerez également que le service est configuré
    pour autoriser le trafic entrant des pods HTTP vers le port 8080, de
    la même manière que vous avez exécuté l'image de conteneur
    localement avec l'argument -p port.

6.  À présent, la création de votre cluster Azure Kubernetes doit s'être
    terminée avec succès.

7.  Configurez maintenant votre Azure CLI pour accéder à votre cluster
    Azure Kubernetes via la commande kubectl. Installez kubectl
    localement à l'aide de la commande az aks install-cli. Exécutez la
    commande suivante dans votre CLI

\`\`az aks install-cli\`\`

![](./media/image32.jpeg)

8.  Configurez kubectl pour qu'il se connecte à votre cluster Kubernetes
    à l'aide de la commande az aks get-credentials. Exécutez la commande
    suivante dans votre CLI

\`\`az aks get-credentials --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER\`\`

- Vous verrez quelque chose de similaire :

![](./media/image33.jpeg)

9.  Demandez maintenant à Azure Kubernetes Service d'appliquer
    deployment.yml modifications à votre cluster. Exécutez la commande
    suivante dans votre CLI

\`\`kubectl apply -f deployment.yml\`\`

- Vous verrez quelque chose de similaire :

![](./media/image34.jpeg)

10. Utilisez maintenant **kubectl** pour surveiller l'état du
    déploiement. Exécutez la commande suivante dans votre CLI

\`\`kubectl get all\`\`

- Vous verrez quelque chose de similaire :

![Un écran d'ordinateur avec du texte et des chiffres Description
générée automatiquement](./media/image35.jpeg)

**Remarque :** Vous voudrez remplacer l'adresse IP dans ce qui suit,
20.81.13.151, par celle de votre EXTERNAL-IP et noter le nom du POD,
nous l'utiliserons dans les prochaines étapes.

11. Si l'état de votre **POD** est **En cours d**'exécution,
    l'application doit être accessible.

12. Vous pouvez également afficher les journaux de l'application au sein
    de chaque espace. Exécutez la commande suivante dans votre CLI

\`\`kubectl logs pod/flightbookingsystemsample-\`\`![Une capture d'écran
d'un ordinateur Description générée
automatiquement](./media/image36.jpeg)

![Un écran d'ordinateur avec du texte blanc Description générée
automatiquement](./media/image37.jpeg)

13. Utilisez maintenant l'adresse **EXTERNAL-IP** de votre sortie
    kubectl get services flightbookingsystemsample pour accéder à
    l'application en cours d'exécution dans Azure Kubernetes Service.

**Remarque :** Vous voudrez remplacer l'adresse IP dans ce qui suit,
20.81.13.151, par celle de votre EXTERNAL-IP de la commande que vous
avez exécutée précédemment.

14. Ouvrez un navigateur et visitez la page d'accueil de l'exemple de
    système de réservation de vols à http://YOUR l'adresse **http://YOUR
    IPCON:8080/FlightBookingSystemSample** (mise à jour avec votre
    adresse IP externe)

    - Vous verrez quelque chose de similaire :

![Un avion qui vole dans le ciel Description générée
automatiquement](./media/image38.jpeg)

**Remarque :** Vous pouvez éventuellement vous connecter avec n'importe
quel utilisateur de tomcat-users.xml par exemple someuser@azure.com :
password

### Tâche 2 : Nettoyer les ressources

1.  Revenez au portail Azure. Cliquez sur **Resource groups**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image39.png)

2.  Cliquez sur le nom du groupe de ressources.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image40.png)

3.  Sélectionnez toutes les ressources, puis cliquez sur **Delete** (Ne
    PAS SUPPRIMER – Groupe de ressources)

4.  Entrez ''supprimer'' puis cliquez sur **Delete**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image41.png)

5.  Confirmez la suppression des ressources.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image42.png)

**Résumé :** Félicitations ! Nous avons conteneurisé et déployé une
application Java sur Azure Kubernetes Service. Dans le cadre de
l'atelier, nous avons conteneurisé une application Java, envoyé l'image
conteneur à Azure Container Registry, puis déployé sur Azure Kubernetes
Service.
