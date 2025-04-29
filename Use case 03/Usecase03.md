# Anwendungsfall 03 – Containerisieren einer Flugbuchungs-App und Bereitstellen in Azure Kubernetes Service

**Objektiv**:

Am Ende dieses Moduls werden Sie in der Lage sein:

- Containerisieren Sie eine Java-App.

- Erstellen Sie ein Containerimage für die Java-App.

- Führen Sie das Containerimage lokal aus.

- Pushen Sie das Containerimage per Push an Azure Container Registry.

Bereitstellen des Containerimages in Azure Kubernetes Service

**Verwendete Schlüsseltechnologien** -- Java 11, Docker, Maven

**Geschätzte Dauer**: 30 Minuten

**Lab-Typ:** Von einem Kursleiter geleitet

## Übung 1: Einrichten Ihrer Azure-Umgebung

In dieser Übung verwenden Sie die Azure CLI, um die Azure-Ressourcen zu
erstellen, die in späteren Einheiten benötigt werden. Führen Sie mit der
Azure CLI die folgenden Schritte aus

### Aufgabe 1: Authentifizieren mit Azure Resource Manager

1.  Öffnen Sie Gitbash auf dem Desktop, und führen Sie den folgenden
    Befehl aus: Melden Sie sich beim Azure-Portal an.

**''az login''**

**Hinweis**: Wenn siehe WARNUNG: Bei
https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize
wurde ein Webbrowser geöffnet. Bitte setzen Sie die Anmeldung im
Webbrowser fort. Wenn kein Webbrowser verfügbar ist oder der Webbrowser
nicht geöffnet werden kann, verwenden Sie den Gerätecodeflow mit az
login --use-device-code

![Ein schwarzer Hintergrund mit gelbem Text Beschreibung wird
automatisch generiert](./media/image1.jpeg)

2.  Mit diesem Befehl gelangen Sie zum Standardbrowser, um sich
    anzumelden. Melden Sie sich mit Ihrem Azure-Abonnementkonto an.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image2.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image3.jpeg)

3.  Wechseln Sie nach der Authentifizierung zurück zu Gitbash

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image4.jpeg)

4.  Jetzt aktivieren wir unser Azure-Abonnement, indem wir den folgenden
    Befehl ausführen

''az account set --subscription "\< YOUR_SUBSCRIPTION_ID \>"''

''az account list --output table''

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image5.jpeg)

5.  Lokale Variablen definieren Um die Befehle zu vereinfachen, die
    weiter unten ausgeführt werden, richten Sie die folgenden
    Umgebungsvariablen ein

Hinweis: Wir haben bereits eine Ressourcengruppe für mich in der Cloud
erstellt. Sie müssen alle Ressourcen innerhalb der vorhandenen
Ressourcengruppe bereitstellen. Sie können es in Ihrem Azurebportal
finden, oder Sie finden es auf der Registerkarte Resources auf Ihrer VM

> export AZ_CONTAINER_REGISTRY="javaaksregist"$RANDOM
>
> export AZ_KUBERNETES_CLUSTER="javaakscluster"$RANDOM
>
> export AZ_LOCATION="westus"

export AZ_KUBERNETES_CLUSTER_DNS_PREFIX="javaakscontainer"

> export AZ_RESOURCE_GROUP= Your existing resource group name

**Hinweis:** Sie sollten durch die Region Ihrer Wahl ersetzen, z. B.:
eastus Sie sollten durch einen eindeutigen Wert ersetzen, da dieser
verwendet wird, um einen eindeutigen FQDN (vollqualifizierten
Domänennamen) für Ihre Azure Container Registry zu generieren, wenn sie
erstellt wird, z. B.: someuniquevaluejavacontainerregistry.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.png)

8.  Azure Container Registry ermöglicht es Ihnen, Containerimages zu
    erstellen, zu speichern und zu verwalten, in denen letztendlich das
    Containerimage für die Java-App gespeichert wird. Erstellen Sie eine
    Azure Container Registry mit den folgenden Befehlen.

''az acr create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_CONTAINER_REGISTRY --sku Basic | jq''

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
generiert](./media/image7.jpeg)

9.  Konfigurieren der Azure CLI für die Verwendung dieser neu erstellten
    Azure Container Registry

''az configure --defaults acr=$AZ_CONTAINER_REGISTRY''

![Ein schwarzer Hintergrund mit grünem und weißem Text Beschreibung wird
automatisch generiert](./media/image8.jpeg)

10. Authentifizieren bei der neu erstellten Azure Container Registry

''az acr login -n $AZ_CONTAINER_REGISTRY''

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image9.jpeg)

11. Erstellen eines Azure Kubernetes-Clusters: Sie benötigen einen Azure
    Kubernetes-Cluster, in dem die Java-App (Containerimage)
    bereitgestellt werden soll.

az aks create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER --attach-acr $AZ_CONTAINER_REGISTRY
--dns-name-prefix=$AZ_KUBERNETES_CLUSTER_DNS_PREFIX --generate-ssh-keys
| JQ

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image10.jpeg)

**Hinweis:** Die Erstellung von Azure Kubernetes-Clustern kann bis zu 10
Minuten dauern. Nachdem Sie den obigen Befehl ausgeführt haben, können
Sie ihn optional auf dieser Registerkarte der Azure CLI fortsetzen
lassen und mit der nächsten Einheit fortfahren.

### Aufgabe 2: Ausführen von Docker

1.  Klicken Sie im Startmenü auf **DockerDesktop**

![Ein Screenshot eines Telefons Beschreibung wird automatisch
generiert](./media/image11.jpeg)

2.  Stellen Sie sicher, dass es läuft.

## Übung 2: Containerisieren einer Java-App

In dieser Übung erstellen Sie die Containerisierung einer
Java-Anwendung.

### Aufgabe 1: Erstellen einer Java-Anwendung

Zuerst navigieren Sie zum Repository des Flugbuchungssystems für
Flugreservierungen und navigieren cd zum Projektordner der Webanwendung
von Airlines.

Wenn Sie Java & Maven installiert haben, können Sie optional die
folgenden Befehle in Ihrer CLI ausführen, um einen Eindruck davon zu
bekommen, wie Sie die Anwendung ohne Docker erstellt haben. Wenn Sie
Java & Maven nicht installiert haben, können Sie getrost zum nächsten
Abschnitt mit dem Titel " Construct a Docker file " springen. In diesem
Abschnitt verwenden Sie Docker, um Java und Maven herunterzuziehen, um
die Builds in Ihrem Namen auszuführen.

1.  Führen Sie den folgenden Befehl in Ihrer CLI aus, um zum Projekt zu
    navigieren.

''cd
"C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines"''

![Ein schwarzer Hintergrund mit Text Beschreibung wird automatisch
generiert](./media/image12.jpeg)

2.  Führen Sie den folgenden Befehl in Ihrer CLI aus

''mvn clean install''

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image13.jpeg)

**Hinweis:** Der Befehl mvn clean install wurde verwendet, um die
betrieblichen Herausforderungen zu veranschaulichen, die sich aus der
Nichtverwendung von mehrstufigen Docker-Builds ergeben, die wir als
nächstes behandeln werden. Auch dieser Schritt ist optional, so oder so
können Sie sicher weitermachen, ohne den Maven-Befehl auszuführen.

3.  Maven sollte das Flight Booking System für das Webanwendungsarchiv
    der Flugreservierungen erfolgreich erstellt haben, wie am folgenden
    Bild zu erkennen ist:
    FlightBookingSystemSample-0.0.-SNAPSHOT.war.![Ein Screenshot eines
    Computerbildschirms Beschreibung wird automatisch
    generiert](./media/image14.jpeg)

## Aufgabe 2: Erstellen einer Docker-Datei

1.  Erstellen Sie im Stammverzeichnis Ihres Projekts,
    containerize-and-deploy-Java-app-to-Azure/Project/Airlines, eine
    Datei mit dem Namen **Dockerfile.**

''vi Dockerfile''

![](./media/image15.jpeg)

Fügen Sie Dockerfile die folgenden Inhalte hinzu, speichern und beenden
Sie es

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

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image16.jpeg)

**Hinweis:** Optional enthält die Dockerfile_Solution im
Stammverzeichnis Ihres Projekts den erforderlichen Inhalt. Wie Sie sehen
können, enthält diese Dockerfile-Build-Phase sechs Anweisungen.

## Übung 3: Erstellen und Ausführen eines Containerimages für die Java-App

In dieser Einheit erstellen Sie das Containerimage und führen es aus.
Wie bereits erwähnt, handelt es sich bei einer ausgeführten Instanz
eines Images um einen Container.

### Aufgabe 1: Erstellen eines Containerimages

Nachdem Sie nun erfolgreich eine Dockerfile erstellt haben, können Sie
Docker anweisen, ein Containerimage für Sie zu erstellen.

**Hinweis:** Stellen Sie sicher, dass Ihre Docker-Runtime für die
Erstellung von Linux-Containern konfiguriert ist. Dies ist wichtig, da
die verwendete Dockerfile auf Containerimages (JDK/JRE) für die
Linux-Architektur verweist.

1.  **Docker build** ist der Befehl, der zum Erstellen von
    Containerimages verwendet wird. Das Argument **-t** wird verwendet,
    um eine Containerbeschriftung anzugeben, und die **.** ist der
    Speicherort für Docker, um die Dockerfile zu finden. Führen Sie den
    folgenden Befehl in Ihrer CLI aus.

**WICHTIG**: Für dieses Lab ist jdk 11 erforderlich. Legen Sie java_home
auf jdk 11 fest.

''docker build -t flightbookingsystemsample .''

![Ein Computerbildschirm mit Text Beschreibung wird automatisch
generiert](./media/image17.jpeg)

2.  Docker-Build ist etwas Ähnliches

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
generiert](./media/image18.jpeg)

**Hinweis:** Wie Sie zuvor gesehen haben, hat Docker die Anweisungen aus
den Zeilen ausgeführt, die Sie zuvor in der vorherigen Einheit
geschrieben haben. Jede Anweisung ist ein Schritt in sequenzieller
Reihenfolge. Führen Sie den Befehl docker build erneut aus, beachten Sie
die Unterschiede in den Schritten, Sie werden bemerken, dass ---\> Cache
für Layer verwenden, die sich nicht geändert haben. Wenn Sie keine
App-Änderungen vornehmen (bevor Sie den Befehl docker build erneut
ausführen), werden alle Schichten aus dem Cache geladen, da die
Binärdateien unverändert geblieben sind und aus dem Docker-Cache bezogen
werden können. Dies ist ein wichtiger Aspekt bei der Optimierung von
Container-Images und den damit verbundenen Rechenkosten sowie der für
den Build-Prozess benötigten Zeit.

3.  Docker kann auch die verfügbaren Images anzeigen, die lokal
    vorhanden sind. Dies ist hilfreich, um zu sehen, welche Images zum
    Ausführen bereitstehen. Führen Sie dazu den folgenden Befehl in
    Ihrer Kommandozeile (CLI) aus:

> docker image ls

Sie werden etwas Ähnliches sehen:

![](./media/image19.jpeg)

### Aufgabe 2: Ausführen eines Containerimages

1.  Nachdem Sie erfolgreich ein Containerimage erstellt haben, können
    Sie es ausführen.

2.  Docker run ist der Befehl, der zum Ausführen eines Containerimages
    verwendet wird. Das -p

:#### Argument wird verwendet, um localhost HTTP weiterzuleiten (das
erste

Port vor dem Doppelpunkt) Datenverkehr zum Container zur Laufzeit (der
zweite Port nach dem Doppelpunkt). Denken Sie daran, dass der
Tomcat-App-Server in der Dockerfile auf HTTP-Datenverkehr an Port 8080
lauscht, daher ist dies der Containerport, der verfügbar gemacht werden
muss. Schließlich wird das Image-Tag flightbookingsystemsample benötigt,
um Docker anzuweisen, welches Image ausgeführt werden soll. Führen Sie
den folgenden Befehl in Ihrer CLI aus:

''docker run -p 8080:8080 flightbookingsystemsample''

Sie werden etwas Ähnliches sehen:

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image20.jpeg)

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
generiert](./media/image21.jpeg)

**Hinweis:** Wenn der Befehl "docker run -p 8080:8080
flightbookingsystemsample" einen Fehler ausgibt, verwenden Sie den unten
genannten Port

''docker run -p 8081:8080 flightbookingsystemsample''

3.  Öffnen Sie einen Browser und rufen Sie die Startseite des Flight
    Booking Systems für Flugreservierungen unter der folgenden Adresse
    auf: http://localhost:8080/FlightBookingSystemSample

    - Folgendes sollte angezeigt werden:

![Ein Flugzeug, das am Himmel fliegt Beschreibung wird automatisch
generiert](./media/image22.jpeg)

4.  Sie können sich optional mit einem beliebigen Benutzer aus
    tomcat-users.xml anmelden, z. B.

Benutzername : **''someuser@azure.com''**

Passwort : ''password''

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image23.jpeg)

5.  Lassen Sie diese Git-Bash-Instanz so, wie sie ist

## Übung 4: Pushen des Containerimages in Azure Container Registry

### Aufgabe 1: Pushen eines Containerimages in Azure Container Registry

1.  In dieser Aufgabe werden Sie ein Container-Image in die Azure
    Container Registry hochladen. Die Azure Container Registry
    ermöglicht es Ihnen, Container-Images und Artefakte in einem
    privaten Registry zu erstellen, zu speichern und zu verwalten – für
    alle Arten von Container-Bereitstellungen. Verwenden Sie
    Azure-Container-Registries zusammen mit Ihren bestehenden
    Entwicklungs- und Bereitstellungspipelines für Container.

2.  Öffnen Sie eine neue Instanz von Gitbash, und führen Sie den Befehl
    az aus, um sich beim Azure-Portal anzumelden.

''az login''

3.  Führen Sie den folgenden Befehl in Ihrer CLI aus

''C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines''

4.  Wir werden die gleiche Authentifizierung mit Azure Resource Manager
    verwenden, die wir zuvor in Übung 1 Aufgabe 1 erstellt haben. Legen
    Sie die folgenden Variablen fest

[TABLE]

> **Hinweis:** Wenn Ihre Sitzung im Leerlauf ausgeführt wurde, müssen
> Sie diesen Schritt zu einem anderen Zeitpunkt und/oder über eine
> andere CLI ausführen. Möglicherweise müssen Sie Ihre
> Umgebungsvariablen neu initialisieren und sich mit den folgenden
> CLI-Befehlen erneut authentifizieren.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image24.png)

### Aufgabe 2: Pushen eines Containerimages

In dieser Aufgabe können Sie Ihr neu erstelltes Container-Image in die
Azure Container Registry hochladen. Dadurch befindet sich Ihr
Container-Image in Netzwerknähe zu all Ihren Azure-Ressourcen, wie
beispielsweise Ihrem Azure Kubernetes Cluster (AKS). Letztendlich werden
Sie AKS so konfigurieren, dass es das Image flightbookingsystemsample
aus der Azure Container Registry abruft.

1.  Um das Containerimage per Push an Azure Container Registry zu
    übertragen, führen Sie die folgenden drei Befehle in Ihrer CLI aus

2.  Melden Sie sich bei **Azure Container Registry** an, und führen Sie
    den folgenden Befehl aus.

''az acr login -n $AZ_CONTAINER_REGISTRY''

![](./media/image25.jpeg)

3.  Markieren Sie zunächst das zuvor erstellte Containerimage mit Ihrer
    Azure Container Registry:

''docker tag flightbookingsystemsample
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample''

![](./media/image26.jpeg)

4.  Zweitens: Pushen Sie das Container-Image in die Azure Container
    Registry

''docker push
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample''

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image27.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image28.jpeg)

5.  Zeigen Sie nun die Metadaten des Azure Container Registry-Images des
    neu gepushten Images an. Führen Sie den folgenden Befehl in Ihrer
    CLI aus

''az acr repository show -n $AZ_CONTAINER_REGISTRY --image
flightbookingsystemsample:latest''

Sie werden etwas Ähnliches sehen:

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image29.jpeg)

6.  Das Container-Image befindet sich jetzt in Azure Container Registry
    und kann von Azure-Diensten wie Azure Kubernetes Service
    bereitgestellt werden.

## Übung 5: Bereitstellen des Containerimages in Azure Kubernetes Service

In dieser Übung stellen Sie ein Containerimage in Azure Kubernetes
Service bereit.

### Aufgabe 1: Bereitstellen eines Containerimages

1.  Sie stellen dieses **flightbookingsystemsample**-Containerimage in
    Ihrem Azure Kubernetes-Cluster bereit.

2.  Erstellen Sie im Stammverzeichnis Ihres Projekts,
    **Flight-Booking-System-JavaServlets_App/Project/Airlines**, eine
    Datei mit dem Namen deployment.yml. Führen Sie den folgenden Befehl
    in Ihrer CLI aus:

''vi deployment.yml''

![](./media/image30.jpeg)

3.  Fügen Sie deployment.yml die folgenden Inhalte hinzu, speichern Sie
    sie und beenden Sie sie:

**Hinweis:** Sie sollten mit Ihrem AZ_CONTAINER_REGISTRY
Umgebungsvariablenwert aktualisieren, der zuvor festgelegt wurde, Übung
1 Aufgabe1( AZ_CONTAINER_REGISTRY= javaaksregist )

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

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image31.jpeg)

4.  Drücken Sie Esc und: und geben Sie dann
    ''**[wq](urn:gd:lg%F0%9F%85%B0%EF%B8%8Fsend-vm-keys)''** ein und
    drücken Sie die Eingabetaste, um die Datei zu speichern.

**Hinweis:** Optional enthält die deployment_solution.yml im
Stammverzeichnis Ihres Projekts den benötigten Inhalt, sodass Sie den
Inhalt dieser Datei möglicherweise leichter umbenennen/aktualisieren
können.

5.  Im obigen deployment.yml werden Sie feststellen, dass diese
    deployment.yml eine Bereitstellung und einen Dienst enthält. Die
    Bereitstellung wird verwendet, um eine Gruppe von Pods zu verwalten,
    während der Dienst verwendet wird, um den Netzwerkzugriff auf die
    Pods zu ermöglichen. Sie werden feststellen, dass die Pods so
    konfiguriert sind, dass sie ein einzelnes Image, das \<
    AZ_CONTAINER_REGISTRY
    \>.azurecr.io/flightbookingsystemsample:latest, aus Azure Container
    Registry abrufen. Sie werden auch feststellen, dass der Dienst so
    konfiguriert ist, dass er eingehenden HTTP-Pod-Datenverkehr an Port
    8080 zulässt, ähnlich wie Sie das Containerimage lokal mit dem
    Portargument -p ausgeführt haben.

6.  Inzwischen sollte die Erstellung Ihres Azure Kubernetes-Clusters
    erfolgreich abgeschlossen sein.

7.  Konfigurieren Sie nun Ihre Azure CLI für den Zugriff auf Ihren Azure
    Kubernetes-Cluster über den kubectl-Befehl. Installieren Sie kubectl
    lokal mit dem Befehl az aks install-cli. Führen Sie den folgenden
    Befehl in Ihrer CLI aus

''az aks install-cli''

![](./media/image32.jpeg)

8.  Konfigurieren Sie kubectl so, dass mit dem Befehl az aks
    get-credentials eine Verbindung mit Ihrem Kubernetes-Cluster
    hergestellt wird. Führen Sie den folgenden Befehl in Ihrer CLI aus

''az aks get-credentials --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER''

- Sie werden etwas Ähnliches sehen:

![](./media/image33.jpeg)

9.  Weisen Sie nun Azure Kubernetes Service an,
    deployment.yml-Änderungen auf Ihren Cluster anzuwenden. Führen Sie
    den folgenden Befehl in Ihrer CLI aus

''kubectl apply -f deployment.yml''

- Sie werden etwas Ähnliches sehen:

![](./media/image34.jpeg)

10. Verwenden Sie nun **kubectl**, um den Status der Bereitstellung zu
    überwachen. Führen Sie den folgenden Befehl in Ihrer CLI aus

''kubectl get all''

- Sie werden etwas Ähnliches sehen:

![Ein Computerbildschirm mit Text und Zahlen Beschreibung wird
automatisch generiert](./media/image35.jpeg)

**Hinweis:** Sie sollten die IP-Adresse im Folgenden, 20.81.13.151,
durch die Ihrer EXTERNAL-IP ersetzen und sich den POD-Namen notieren,
den wir in den nächsten Schritten verwenden werden.

11. Wenn Ihr **POD**-Status **"Running"** lautet, sollte die App
    zugänglich sein.

12. Sie können die App-Logs auch in jedem Pod anzeigen. Führen Sie den
    folgenden Befehl in Ihrer CLI aus

> \`\`kubectl logs pod/flightbookingsystemsample-\`\`

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image36.jpeg)

![Ein Computerbildschirm mit weißem Text Beschreibung wird automatisch
generiert](./media/image37.jpeg)

13. Verwenden Sie nun die **EXTERNAL-IP** aus Ihrer kubectl get services
    flightbookingsystemsample-Ausgabe, um auf die ausgeführte App in
    Azure Kubernetes Service zuzugreifen.

**Hinweis:** Sie sollten die IP-Adresse im Folgenden 20.81.13.151 durch
die Adresse Ihrer EXTERNAL-IP aus dem zuvor ausgeführten Befehl
ersetzen.

14. Öffnen Sie einen Browser und rufen Sie die Startseite des Flight
    Booking System Sample unter der folgenden Adresse auf: **http://YOUR
    IPCON:8080/FlightBookingSystemSample** (aktualisieren Sie mit Ihrer
    externen IP-Adresse).

    - Sie werden etwas Ähnliches sehen:

![Ein Flugzeug, das am Himmel fliegt Beschreibung wird automatisch
generiert](./media/image38.jpeg)

**Hinweis:** Sie können sich optional mit einem beliebigen Benutzer aus
tomcat-users.xml anmelden, z. B. someuser@azure.com: password

### Aufgabe 2: Bereinigen von Ressourcen

1.  Wechseln Sie zurück zum Azure-Portal. Klicken Sie auf **Resource
    groups**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image39.png)

2.  Klicken Sie auf Name der Ressourcengruppe.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image40.png)

3.  Wählen Sie alle Ressourcen aus und klicken Sie dann auf **Delete**
    (NICHT LÖSCHEN – Ressourcengruppe)

4.  Geben Sie ''delete'' ein und klicken Sie dann auf **Delete**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image41.png)

5.  Bestätigen Sie das Löschen von Ressourcen .

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image42.png)

**Zusammenfassung:** Herzlichen Glückwunsch! Wir haben eine Java-App
containerisiert und in Azure Kubernetes Service bereitgestellt. Im
Rahmen des Labs haben wir eine Java-App containerisiert, das
Containerimage per Push an Azure Container Registry übertragen und dann
in Azure Kubernetes Service bereitgestellt.
