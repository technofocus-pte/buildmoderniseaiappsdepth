# Caso d'uso 03 - Containerizzazione di un'app per la prenotazione di voli e distribuzione nel servizio Azure Kubernetes

**Obiettivo**:

Alla fine di questo modulo, sarai in grado di:

- Containerizzare un'app Java.

- Creare un'immagine del contenitore per l'app Java.

- Eseguire l'immagine del contenitore in locale.

- Eseguire il push dell'immagine del contenitore in Registro Azure
  Container.

Distribuire l'immagine del contenitore nel servizio Azure Kubernetes

**Principali tecnologie utilizzate**: Java 11, Docker, Maven

**Durata stimata**: 30 min

**Tipo di laboratorio:** Guidato da un istruttore

## Esercizio 1 : Configurare l'ambiente Azure

In questo esercizio si userà l'interfaccia della riga di comando di
Azure per creare le risorse di Azure che saranno necessarie nelle unità
successive. Usando l'interfaccia della riga di comando di Azure,
eseguire la procedura seguente

### Attività 1: Eseguire l'autenticazione con Azure Resource Manager

1.  Aprire Gitbash dal desktop ed eseguire il comando seguente per
    accedere al portale di Azure

**''az login''**

**Nota**: Se vedere WARNING: È stato aperto un browser Web in
https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize.
Si prega di continuare il login nel browser web. Se non è disponibile
alcun Web browser o se il Web browser non si apre, usare il flusso del
codice del dispositivo con az login --use-device-code

![Uno sfondo nero con testo giallo Descrizione generata
automaticamente](./media/image1.jpeg)

2.  Questo comando ti porterà al browser predefinito per accedere.
    Accedere con l'account della sottoscrizione di Azure.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image2.jpeg)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image3.jpeg)

3.  Una volta autenticato, torna a Gitbash

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image4.jpeg)

4.  Ora abiliteremo la nostra sottoscrizione Azure eseguendo il comando
    seguente

''az Account Set --Subscription "\< YOUR_SUBSCRIPTION_ID \>"''

''az account list—output table''

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image5.jpeg)

5.  Definire le variabili locali Per semplificare i comandi che verranno
    eseguiti più avanti, impostare le seguenti variabili d'ambiente

**Nota** : Abbiamo già creato un gruppo di risorse per me nel cloud. È
necessario distribuire tutte le risorse all'interno del gruppo di
risorse esistente. È possibile farlo nel Azurebportal o nella scheda
Risorse della VM

> export AZ_CONTAINER_REGISTRY="javaaksregist"$RANDOM
>
> export AZ_KUBERNETES_CLUSTER="javaakscluster"$RANDOM
>
> export AZ_LOCATION="westus"

export AZ_KUBERNETES_CLUSTER_DNS_PREFIX="javaakscontainer"

> export AZ_RESOURCE_GROUP= Your existing resource group name

**Nota:** è consigliabile sostituire con l'area scelta, ad esempio:
eastus È consigliabile sostituire con un valore univoco in quanto viene
usato per generare un nome di dominio completo (FQDN) univoco per Azure
Container Registry al momento della creazione, ad esempio:
someuniquevaluejavacontainerregistry.

![Schermata di un computer Descrizione generata
automaticamente](./media/image6.png)

8.  Registro Azure Container consente di creare, archiviare e gestire
    immagini del contenitore, che in ultima analisi sono la posizione in
    cui verrà archiviata l'immagine del contenitore per l'app Java.
    Creare un Registro Azure Container con i comandi seguenti.

''az acr create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_CONTAINER_REGISTRY --sku Basic | jq''

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente](./media/image7.jpeg)

9.  Configurare l'interfaccia della riga di comando di Azure per l'uso
    di questo Registro Azure Container appena creato

''az configure --defaults acr=$AZ_CONTAINER_REGISTRY''

![Uno sfondo nero con testo verde e bianco Descrizione generata
automaticamente](./media/image8.jpeg)

10. Eseguire l'autenticazione al Registro Azure Container appena creato

''az acr login -n $AZ_CONTAINER_REGISTRY''

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image9.jpeg)

11. Creare un cluster Azure Kubernetes, è necessario un cluster Azure
    Kubernetes in cui distribuire l'app Java (immagine del contenitore).

az aks create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER --attach-acr $AZ_CONTAINER_REGISTRY
--dns-name-prefix=$AZ_KUBERNETES_CLUSTER_DNS_PREFIX --generate-ssh-keys
| jq

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image10.jpeg)

**Nota:** La creazione del cluster Azure Kubernetes può richiedere fino
a 10 minuti, una volta eseguito il comando precedente, è possibile
facoltativamente lasciarlo continuare nella scheda dell'interfaccia
della riga di comando di Azure e passare all'unità successiva.

### Attività 2 : Eseguire Docker

1.  Nel menu Start, fai clic su **DockerDesktop**

![Uno screenshot di un telefono Descrizione generata
automaticamente](./media/image11.jpeg)

2.  Assicurati che sia in funzione.

## Esercizio 2: Containerizzare un'app Java

In questo esercizio verrà containerizzata un'applicazione Java.

### Attività 1 : Costruire l'applicazione Java

Per prima cosa navigherai nel repository del sistema di prenotazione dei
voli per le prenotazioni aeree e il cd nella cartella del progetto
dell'applicazione web delle compagnie aeree.

Facoltativamente, se hai installato Java e Maven, puoi eseguire i
seguenti comandi nella tua CLI per avere un'idea dell'esperienza nella
creazione dell'applicazione senza Docker. Se non hai installato Java e
Maven, puoi tranquillamente passare alla sezione successiva intitolata
"Costruisci un file Docker", in quella sezione utilizzerai Docker per
scaricare Java e Maven per eseguire le build per suo conto.

1.  Eseguire il comando seguente nell'interfaccia della riga di comando
    per passare al progetto.

''cd
"C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines"''

![Uno sfondo nero con testo Descrizione generata
automaticamente](./media/image12.jpeg)

2.  Esegui il seguente comando nell'interfaccia della riga di comando

''mvn clean install''

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image13.jpeg)

**Nota:** Il comando mvn clean install è stato usato per illustrare le
sfide operative del mancato utilizzo delle build in più fasi di Docker,
che tratteremo in seguito. Anche in questo caso questo passaggio è
facoltativo, in entrambi i casi puoi muoverti in sicurezza senza
eseguire il comando Maven.

3.  Maven dovrebbe aver compilato correttamente l'artefatto
    FlightBookingSystemSample-0.0.-SNAPSHOT.war dell'archivio
    dell'applicazione Web per le prenotazioni delle compagnie aeree,
    come illustrato nell'immagine seguente

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente](./media/image14.jpeg)

## Attività 2 : Costruire un file Docker

1.  All'interno della radice del progetto,
    containerize-and-deploy-Java-app-to-Azure/Project/Airlines, creare
    un file denominato **Dockerfile.**

''vi Dockerfile''

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

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image16.jpeg)

**Nota:** Facoltativamente, il Dockerfile_Solution nella radice del
progetto contiene i contenuti necessari. Come puoi vedere, questa fase
di compilazione del file Docker ha sei istruzioni.

## Esercizio 3 : Creare ed eseguire un'immagine del contenitore per l'app Java

In questa unità, creerai ed eseguirai l'immagine del container. Come
accennato in precedenza, un'istanza in esecuzione di un'immagine è un
contenitore.

### Attività 1 : Creare un'immagine del contenitore

Dopo aver creato correttamente un Dockerfile, è possibile indicare a
Docker di creare automaticamente un'immagine del contenitore.

**Nota:** Assicurati che il runtime Docker sia configurato per creare
contenitori Linux. Questo è importante perché il Dockerfile utilizzato
fa riferimento a immagini del contenitore (JDK/JRE) per l'architettura
Linux.

1.  **Docker build** è il comando usato per creare immagini del
    contenitore. L' argomento **-t** verrà utilizzato per specificare
    un'etichetta contenitore e l'argomento **.** è il percorso in cui
    Docker trova il Dockerfile. Eseguire il comando seguente
    nell'interfaccia della riga di comando.

**IMPORTANTE**: questo laboratorio richiede jdk 11 . set java_home to
jdk 11

''docker build -t flightbookingsystemsample.''

![Schermo di un computer con testo Descrizione generata
automaticamente](./media/image17.jpeg)

2.  La build di Docker è qualcosa di simile

![Schermata dello schermo di un computer Descrizione generata
automaticamente](./media/image18.jpeg)

**Nota:** come hai visto in precedenza, Docker ha eseguito le istruzioni
dalle righe che hai precedentemente scritto nell'unità precedente. Ogni
istruzione è un passaggio in ordine sequenziale. Esegui nuovamente il
comando docker build, nota le differenze nei passaggi, noterai ---\>
Utilizzo della cache per i livelli che non sono cambiati. Se non si
apportano modifiche all'app (prima di eseguire nuovamente il comando
docker build), si noterà che tutti i livelli memorizzati nella cache
sono stati memorizzati nella cache poiché i file binari non sono stati
toccati e possono essere originati dalla cache Docker. Questo è un
aspetto importante quando si ottimizzano le immagini dei container e i
costi di calcolo associati con il tempo impiegato per crearli.

3.  Docker può anche visualizzare le immagini disponibili che sono
    residenti. Questo è utile per visualizzare ciò che è disponibile per
    l'esecuzione. Esegui il seguente comando nell'interfaccia della riga
    di comando

Immagine Docker LS

Vedrai qualcosa di simile:

![](./media/image19.jpeg)

### Attività 2 : Esecuzione di un'immagine del contenitore

1.  Dopo aver creato correttamente un'immagine del contenitore, è
    possibile eseguirla.

2.  Docker run è il comando usato per eseguire un'immagine del
    contenitore. Il comando -p

:#### verrà utilizzato per inoltrare localhost HTTP (il primo

port prima dei due punti) al container in fase di esecuzione (la seconda
porta dopo i due punti). Ricorda dal Dockerfile che il server app Tomcat
è in ascolto del traffico HTTP sulla porta 8080, quindi questa è la
porta del container che deve essere esposta. Infine, il tag immagine
flightbookingsystemsample è necessario per indicare a Docker quale
immagine eseguire. Eseguire il comando seguente nell'interfaccia della
riga di comando:

''docker run -P 8080:8080 flightbookingsystemsample''

Vedrai qualcosa di simile:

![Schermata di un computer Descrizione generata
automaticamente](./media/image20.jpeg)

![Schermata dello schermo di un computer Descrizione generata
automaticamente](./media/image21.jpeg)

**Nota:** se il comando "docker run -p 8080:8080
flightbookingsystemsample" genera un errore, utilizzare la porta
indicata di seguito

''docker run -P 8081:8080 flightbookingsystemsample''

3.  Apri un browser e visita la pagina di destinazione del sistema di
    prenotazione dei voli per le prenotazioni aeree all'indirizzo
    http://localhost:8080/FlightBookingSystemSample

    - Dovrebbe essere visualizzato quanto segue:

![Un aereo che vola nel cielo Descrizione generata
automaticamente](./media/image22.jpeg)

4.  Facoltativamente, è possibile accedere con qualsiasi utente da
    tomcat-users.xml, ad esempio

Nome utente : **''someuser@azure.com''**

Parola d'ordine : ''password”

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image23.jpeg)

5.  Lascia questa istanza di git bash così com'è

## Esercizio 4: Eseguire il push dell'immagine del contenitore in Registro Azure Container

### Attività 1: Eseguire il push di un'immagine del contenitore in Registro Azure Container

1.  In questa attività verrà eseguito il push di un'immagine del
    contenitore in Registro Azure Container.Registro Azure Container
    consente di creare, archiviare e gestire immagini e artefatti del
    contenitore in un registro privato per tutti i tipi di distribuzioni
    di contenitori. Usare i registri Azure Container con le pipeline di
    sviluppo e distribuzione dei contenitori esistenti.

2.  Aprire una nuova istanza di Gitbash ed eseguire az command per
    accedere al portale di Azure.

''az login''

3.  Esegui il seguente comando nell'interfaccia della riga di comando

''C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines''

4.  Useremo lo stesso Authenticate with Azure Resource Manager che
    abbiamo creato in precedenza nell'Esercizio 1 Attività 1.set
    seguenti variabili

[TABLE]

> **Nota:** Se la sessione è inattiva, se si esegue questo passaggio in
> un altro momento e/o da un'altra CLI, potrebbe essere necessario
> reinizializzare le variabili di ambiente e autenticarsi nuovamente con
> i seguenti comandi CLI.

![Schermata di un programma per computer Descrizione generata
automaticamente](./media/image24.png)

### Attività 2: Eseguire il push di un'immagine del contenitore

In questa attività è possibile eseguire il push dell'immagine del
contenitore appena compilata nel Registro Azure Container. In questo
modo, l'immagine del contenitore sarà vicina alla rete di tutte le
risorse di Azure, ad esempio il cluster Azure Kubernetes. Alla fine si
configurerà il servizio Azure Kubernetes per eseguire il pull
dell'immagine flightbookingsystemsample da Registro Azure Container.

1.  Per eseguire il push dell'immagine del contenitore in Registro Azure
    Container, eseguire i tre comandi seguenti nell'interfaccia della
    riga di comando

2.  Accedere ad **Azure Container Registry** eseguire il comando
    seguente

''az acr login -n $AZ_CONTAINER_REGISTRY''

![](./media/image25.jpeg)

3.  Prima di tutto contrassegnare l'immagine del contenitore compilata
    in precedenza con il Registro Azure Container:

''docker tag flightbookingsystemsample
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample''

![](./media/image26.jpeg)

4.  In secondo luogo, eseguire il push dell'immagine del contenitore in
    Registro Azure Container

''docker push
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample''

![Schermata di un computer Descrizione generata
automaticamente](./media/image27.jpeg)

![Schermata di un computer Descrizione generata
automaticamente](./media/image28.jpeg)

5.  Visualizzare ora i metadata dell'immagine di Registro Azure
    Container dell'immagine di cui è stato appena eseguito il push.
    Esegui il seguente comando nell'interfaccia della riga di comando

''az acr repository show -n $AZ_CONTAINER_REGISTRY --image
flightbookingsystemsample:latest''

Vedrai qualcosa di simile:

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image29.jpeg)

6.  L'immagine del contenitore è ora residente all'interno di Registro
    Azure Container ed è pronta per le distribuzioni da parte dei
    servizi di Azure, ad esempio il servizio Azure Kubernetes.

## Esercizio 5: Distribuire l'immagine del contenitore nel servizio Azure Kubernetes

In questo esercizio si distribuirà un'immagine del contenitore nel
servizio Azure Kubernetes.

### Attività 1: Distribuire un'immagine del contenitore

1.  L'immagine del contenitore **flightbookingsystemsample** verrà
    distribuita nel cluster Azure Kubernetes.

2.  All'interno della radice del suo progetto,
    **Flight-Booking-System-JavaServlets_App/Project/Airlines**, crea un
    file chiamato deployment.yml. Eseguire il comando seguente
    nell'interfaccia della riga di comando:

''VI deployment.yml''

![](./media/image30.jpeg)

3.  Aggiungere i seguenti contenuti a deployment.yml, quindi salvare ed
    uscire:

**Nota:** Ti consigliamo di aggiornare con il suo AZ_CONTAINER_REGISTRY
valore della variabile d'ambiente che è stato impostato in precedenza,
Esercizio 1 Task1( AZ_CONTAINER_REGISTRY= javaaksregist )

apiVersion: apps/v1

kind: Deployment

Metadata:

name: flightbookingsystemsample

Spec:

replicas: 1

selector:

matchLabels:

app: flightbookingsystemsample

template:

Metadata:

labels:

app: flightbookingsystemsample

Spec:

containers:

\- name: flightbookingsystemsample

Image:
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

Metadata:

name: flightbookingsystemsample

Spec:

type: LoadBalancer

ports:

\- port: 8080

targetPort: 8080

selector:

app: flightbookingsystemsample

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image31.jpeg)

4.  Premere Esc e: e poi digitare
    ''**[wq](urn:gd:lg%F0%9F%85%B0%EF%B8%8Fsend-vm-keys)''** e premere
    invio per salvare il file.

**Note:** Facoltativamente, il deployment_solution.yml nella radice del
suo progetto contiene i contenuti necessari, potresti trovare più facile
rinominare/aggiornare il contenuto di quel file.

5.  Nella deployment.yml precedente si noterà che questo deployment.yml
    contiene una distribuzione e un servizio. La distribuzione viene
    utilizzata per amministrare un set di pod mentre il servizio viene
    utilizzato per consentire l'accesso di rete ai pod. Si noterà che i
    pod sono configurati per eseguire il pull di una singola immagine,
    la \< AZ_CONTAINER_REGISTRY
    \>,azurecr.io/flightbookingsystemsample:latest da Registro Azure
    Container. Si noterà anche che il servizio è configurato per
    consentire il traffico del pod HTTP in ingresso alla porta 8080, in
    modo simile al modo in cui è stata eseguita l'immagine del
    contenitore in locale con l'argomento -p port.

6.  A questo punto la creazione del cluster Azure Kubernetes dovrebbe
    essere stata completata correttamente.

7.  Configurare ora l'interfaccia della riga di comando di Azure per
    accedere al cluster Azure Kubernetes tramite il comando kubectl.
    Installare kubectl in locale usando il comando az aks install-cli .
    Esegui il seguente comando nell'interfaccia della riga di comando

''az aks install-cli''

![](./media/image32.jpeg)

8.  Configurare kubectl per connettersi al cluster Kubernetes usando il
    comando az aks get-credentials. Esegui il seguente comando
    nell'interfaccia della riga di comando

''az aks get-credentials --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER''

- Vedrai qualcosa di simile:

![](./media/image33.jpeg)

9.  A questo punto, indicare al servizio Azure Kubernetes di applicare
    deployment.yml modifiche al cluster. Esegui il seguente comando
    nell'interfaccia della riga di comando

''kubectl apply -f deployment.yml''

- Vedrai qualcosa di simile:

![](./media/image34.jpeg)

10. Usare ora **kubectl** per monitorare lo stato della distribuzione.
    Esegui il seguente comando nell'interfaccia della riga di comando

''kubectl get all''

- Vedrai qualcosa di simile:

![Schermo di un computer con testo e numeri Descrizione generata
automaticamente](./media/image35.jpeg)

**Nota:** Ti consigliamo di sostituire l'indirizzo IP nel seguente,
20.81.13.151, con quello del suo EXTERNAL-IP e annotare il nome del POD,
lo useremo nei prossimi passaggi.

11. Se lo stato del suo **POD** è In **Running** , l'app dovrebbe essere
    accessibile.

12. È possibile visualizzare anche i log delle app all'interno di
    ciascun pod. Esegui il seguente comando nell'interfaccia della riga
    di comando

''kubectl logs pod/flightbookingsystemsample-''

![Schermata di un computer Descrizione generata
automaticamente](./media/image36.jpeg)

![Schermo di un computer con testo bianco Descrizione generata
automaticamente](./media/image37.jpeg)

13. Usare ora la **External-IP** dall'output kubectl get services
    flightbookingsystemsample per accedere all'app in esecuzione
    all'interno del servizio Azure Kubernetes.

**Nota:** ti consigliamo di sostituire l'indirizzo IP nel seguente,
20.81.13.151, con quello del suo EXTERNAL-IP dal comando che hai
eseguito in precedenza.

14. Apri un browser e visita la pagina di destinazione del Flight
    Booking System Sample all'indirizzo **http://YOUR
    IPCON:8080/FlightBookingSystemSample** (aggiorna con il suo
    indirizzo IP esterno)

    - Vedrai qualcosa di simile:

![Un aereo che vola nel cielo Descrizione generata
automaticamente](./media/image38.jpeg)

**Nota:** Facoltativamente, puoi accedere con qualsiasi utente da
tomcat-users.xml, ad esempio someuser@azure.com: password

### Compito 2 : Pulizia delle risorse

1.  Tornare al portale di Azure. Fare clic su **Resource groups**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image39.png)

2.  Fare clic su Nome **resource group.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image40.png)

3.  Seleziona tutte le risorse e poi clicca su **Delete** (NON ELIMINARE
    – Gruppo di risorse)

4.  Immettere ''delete'' e quindi fare clic su **Delete**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image41.png)

5.  Confermare l'eliminazione delle risorse.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image42.png)

**Sommario:** Congratulazioni! Abbiamo containerizzato e distribuito
un'app Java nel servizio Azure Kubernetes. Come parte del lab è stata
inserita un'app Java in contenitori, è stato eseguito il push
dell'immagine del contenitore in Registro Azure Container, e quindi è
stata eseguita la distribuzione nel servizio Azure Kubernetes.
