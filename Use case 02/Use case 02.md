# Caso d'uso 02 - Creare un'app Web Fruits List Quarkus con il servizio app di Azure in Linux e PostgreSQL

**Durata stimata:** 40 minuti

**Tipo di laboratorio:** Guidato dall'istruttore

**Obiettivo:**

Questo caso d'uso illustra come compilare, configurare e distribuire
un'applicazione Quarkus sicura nel servizio app di Azure connessa a un
database PostgreSQL (usando Database di Azure per PostgreSQL). Il
servizio app di Azure è un servizio di hosting Web altamente scalabile,
con applicazione automatica di patch, in grado di distribuire facilmente
app in Windows o Linux. Al termine, sarà disponibile un'app Quarkus in
esecuzione nel Servizio app di Azure in Linux.

**Prerequisiti:**

**Account GitHub**: è necessario che tu abbia le tue credenziali di
accesso a GitHub. Se non lo hai, creane uno da qui -
+++<https://github.com/signup?user_email=&source=form-home-signup+++>

## Esercizio 0: Informazioni sulla VM e sulle credenziali

In questa attività, identificheremo e comprenderemo le credenziali che
utilizzeremo in tutto il laboratorio.

1.  **Disposizioni** Tenere la guida del laboratorio con le istruzioni
    da seguire durante il laboratorio.

2.  **Risorse** contiene le credenziali necessarie per l'esecuzione del
    lab.

    - **URL**: URL del portale di Azure

    - **Subscription**: questo è l'ID dell'abbonamento assegnato
      all'utente

    - **Username**: l'ID utente con cui è necessario accedere ai servizi
      di Azure.

    - **Password**: password per l'account di accesso di Azure.

Chiamiamo questo nome utente e password come credenziali di accesso di
Azure. Useremo questi crediti ogni volta che menzioniamo le credenziali
di accesso di Azure.

- **Resource group**: il **Resource Group**  assegnato all'utente.

\[! Vigile\] **Importante:** assicurarsi di creare tutte le risorse in
questo **Resource group**

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image1.png)

3.  **Help** contiene le informazioni di supporto. Il valore **ID** è il
     **Lab instance ID** che verrà utilizzato durante l'esecuzione del
    lab.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image2.png)

## Esercizio 1: Run the sample

Innanzitutto, imposti un'app basata sui data di esempio come punto di
partenza. Il repository di esempio che stiamo utilizzando qui include
una configurazione del contenitore dev. Il contenitore dev include tutto
il necessario per sviluppare un'applicazione, inclusi il database, la
cache e tutte le variabili di ambiente necessarie per l'applicazione di
esempio. Il contenitore dev può essere eseguito in uno codespace GitHub,
il che significa che è possibile eseguire l'esempio in qualsiasi
computer con un Web browser.

1.  Da un browser, accedi al tuo account GitHub
    +++\*\*https://github.com/login[\*\*+++](https://github.com/login**+++).

2.  Apri questo URL da una nuova scheda,
    +++\*\*https://github.com/technofocus-pte/msdocs-quarkus-postgresql-sample-app[\*\*+++](https://github.com/technofocus-pte/msdocs-quarkus-postgresql-sample-app**+++).

3.  Seleziona **Fork -\> Create a new fork. **

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image3.jpeg)

4.  Fare clic su **Create fork** nella pagina **Create a new fork**. 

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image4.jpeg)

5.  Nella pagina biforcata del repository selezionare **Code** \>
    **Create codespaces on main**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image5.jpeg)

**Nota:** Se l'opzione Crea **Codespaces** su Main non viene
visualizzata, click on the + symbol next to **Codespaces**. 

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image6.jpeg)

**Nota:** la creazione dello **Codespaces** richiede circa 10 minuti per
la configurazione.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image7.jpeg)

6.  Esegui +++mvn quarkus:dev+++ nel terminale. Fare clic su **Allow**
    nel pop-up.

![Uno screenshot di un browser I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image8.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image9.jpeg)

7.  Quando viene visualizzata la notifica, **Your application running on
    port 8080**  è disponibile., selezionare **Open in Browser**.
    L'applicazione di esempio dovrebbe essere visualizzata in una nuova
    scheda del browser.

Se viene visualizzata una **notification** con la porta **5005**, **skip
it**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image10.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image11.jpeg)

8.  Per arrestare il server di sviluppo Quarkus, digitare **CTRL+C** nel
    terminale Codespace.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image12.jpeg)

## Esercizio 2: Creare il servizio app e PostgreSQL

Prima di tutto, si creano le risorse di Azure. I passaggi usati in
questo lab creano un set di risorse sicure per impostazione predefinita
che includono il servizio app e Database di Azure per PostgreSQL.

1.  Aprire il portale di Azure al numero
    +++<https://portal.azure.com/+++> e **login** con le credenziali di
    accesso di Azure dalla **Resources** tab della macchina virtuale.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image13.png)

2.  Selezionare **Cancel** o il pulsante Chiudi nella pagina di Welcome.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image14.jpeg)

3.  Immettere +++**Web app database**+++ nella barra di ricerca nella
    parte superiore del portale di Azure. Selezionare l'elemento con
    l'etichetta **Web App+ Database** sotto l' intestazione
    **Marketplace**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image15.jpeg)

4.  In **Create Web App + Database** immettere i dettagli seguenti e
    selezionare **Review + create**

[TABLE]

5.  ![Uno screenshot di un computer I contenuti generati
    dall'intelligenza artificiale potrebbero non essere
    corretti.](./media/image16.png)

6.  ![Uno screenshot di un'applicazione web I contenuti generati
    dall'intelligenza artificiale potrebbero non essere
    corretti.](./media/image17.jpeg)

7.  Una volta superata la convalida, fare clic su **Create**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image18.png)

**Nota:** la creazione dell'app richiede circa 15 minuti.

8.  Una volta completata la distribuzione, fare clic su **Go to
    resource**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image19.jpeg)

9.  Viene visualizzata direttamente la **App Service page.** Fare clic
    su **Home** nell'angolo in alto a sinistra.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image20.jpeg)

10. Fare clic sul menu Portale e selezionare **Resource Groups** da
    esso.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image21.jpeg)

11. Selezionare il **Resource Group** assegnato all'utente e verificare
    che le risorse seguenti vengano create dalla deployment appena
    eseguita.

> \- App Service Plan
>
> \- App Service
>
> \- Virtual network
>
> \- Azure Database for PostgreSQL flexible server
>
> \- Private DNS zone

![Uno screenshot di un gruppo I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image22.png)

## Esercizio 3: Verificare le impostazioni di connessione

La procedura guidata di creazione ha generato le variabili di
connettività già per te come impostazioni dell'app. In questo passaggio
si apprenderà dove trovare le impostazioni dell'app e come crearne di
proprie.

1.  Fare clic sul **App Service** nell'elenco delle risorse nel Resource
    Group.

![Uno screenshot di un telefono I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image23.jpeg)

2.  Nella pagina App Service, dal menu a sinistra, selezionare
    **Environment variables** in **Settings**.

3.  Nella scheda **App** **settings** della pagina **Environment
    variables** verificare che **AZURE_POSTGRESQL_CONNECTIONSTRING** sia
    presente. Viene iniettato in fase di esecuzione come variabile di
    ambiente.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image24.jpeg)

4.  Seleziona **+ Add**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image25.jpeg)

5.  Assegnare all'impostazione il nome +++**PORT**+++ e impostarne il
    valore su +++**8080**+++, che è la porta predefinita
    dell'applicazione Quarkus. Seleziona **Apply**.

![Uno screenshot di un accesso I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image26.jpeg)

6.  Seleziona **Apply**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image27.jpeg)

7.  Seleziona **Confirm**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image28.jpeg)

8.  Riceverai una **Notifications** che indica che le impostazioni
    dell'app sono state aggiornate.

![Uno screenshot di un telefono I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image29.jpeg)

## Esercizio 4: Distribuire il codice di esempio

In questo passaggio si configurerà la distribuzione di GitHub usando
GitHub Actions. È solo uno dei molti modi per eseguire la distribuzione
nel App Service, ma anche un ottimo modo per avere un'integrazione
continua nel processo di distribuzione. Per impostazione predefinita,
ogni push git nel repository GitHub avvierà l'azione di compilazione e
distribuzione.

1.  Nella pagina App Service, dal menu a sinistra, selezionare
    **Deployment Center** in **Deployment**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image30.jpeg)

2.  In Origine selezionare **GitHub**. Per impostazione predefinita,
    GitHub Actions è selezionato come provider di compilazione.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image31.jpeg)

3.  Fai clic su **Authorize** e accedi al tuo account GitHub e segui le
    istruzioni per autorizzare Azure.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image32.jpeg)

4.  Compila i dettagli come di seguito, lascia il resto al valore
    predefinito e fai clic su **Save**.

[TABLE]

5.  ![Uno screenshot di un computer I contenuti generati
    dall'intelligenza artificiale potrebbero non essere
    corretti.](./media/image33.jpeg)

6.  Dopo aver fatto clic su **Save**, il App Sevice esegue il commit di
    un file del flusso di lavoro nel repository GitHub scelto, nella
    directory .github/workflows.

7.  Torna nello spazio di codice GitHub del tuo fork di esempio, esegui
    +++**git pull origin main+++.** In questo modo il file del flusso di
    lavoro di cui è stato eseguito il commit viene inserito nello
    codespaces.

\[! Nota\] **Nota:** Se trovi casi di test ancora in esecuzione nel
terminale, puoi premere Ctrl+C e quindi eseguire il comando precedente.

![Uno screenshot di un codice informatico I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image34.jpeg)

7.  Aprire **src/main/resources/application.properties** in Esplora
    risorse. Quarkus utilizza questo file per caricare le proprietà
    Java.

8.  Trova il codice (lines 10-11). Questo codice imposta la variabile di
    produzione **%prod.quarkus.datasource.jdbc.url** sull'impostazione
    dell'app che la procedura guidata di creazione ti aspetta. Il
    **quarkus.package.type** è impostato per creare un Uber-Jar, che è
    necessario eseguire nel App Service.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image35.jpeg)

9.  Apri **.github/workflows/main_quarkuwebapp\[lab instance id\].yml**
    in explorer. Questo file è stato creato dalla procedura guidata di
    creazione del App Service.

10. Nel passaggio Compila con Maven modificare il comando Maven in
    +++**mvn clean install -DskipTests**+++.

**-DskipTests** salta i test nel tuo progetto Quarkus, per evitare che
il flusso di lavoro GitHub fallisca prematuramente.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image36.jpeg)

11. Selezionare l' estensione **Source Control**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image37.jpeg)

12. Nella casella di testo digitare un messaggio di commit come
    +++**Configure DB and Deployment Workflow**+++. Seleziona
    **Commit**, quindi conferma con **Yes**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image38.jpeg)

13. Selezionare **Sync changes 1**, quindi confermare con **OK**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image39.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image40.jpeg)

14. Tornare alla pagina Deployment Center nel portale di Azure e
    selezionare **Logs**. Una nuova esecuzione di deployment sarebbe già
    iniziata dalle modifiche di cui è stato eseguito il commit.

15. Nell'elemento del log per l'esecuzione della distribuzione
    selezionare la **Build/Deploy Logs** voce con il timestamp più
    recente.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image41.jpeg)

16. Viene visualizzato il repository GitHub e si verifica che l'azione
    GitHub è in esecuzione. Il file del flusso di lavoro definisce due
    fasi separate, la compilazione e la distribuzione. Attendi che
    l'esecuzione di GitHub mostri lo stato Completato. Ci vogliono circa
    5 minuti.

![Uno screenshot di una pagina web I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image42.jpeg)

## Esercizio 5: Accedere all'app

1.  Dal portale di Azure
    (+++[https://portal.azure.com+++](https://portal.azure.com+++/))
    aprire resource group **ResourceGroup1** e selezionare la risorsa
    del **App Service**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image43.png)

2.  Dal menu a sinistra, seleziona **Overview** e seleziona l'URL della
    tua app in **Default domain**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image44.jpeg)

3.  Incolla l'URL copiato in un nuovo browser per aprire l'app.

![Uno screenshot di una lista di frutta I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image45.jpeg)

4.  Aggiungi alcuni frutti alla lista. A questo punto, si esegue un'app
    Web nel servizio app di Azure, con connettività sicura a Database di
    Azure per PostgreSQL.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image46.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image47.jpeg)

## Esercizio 6: Trasmettere i log di diagnostica

Il servizio app di Azure acquisisce l'output di tutti i messaggi nella
console per facilitare la diagnosi dei problemi con l'applicazione.
L'applicazione di esempio include istruzioni di registrazione JBoss
standard per illustrare questa funzionalità, come illustrato di seguito.

1.  Nella pagina del servizio app del portale di Azure, dal menu a
    sinistra, selezionare **App Service Logs** in **Monitoring**.

![Uno screenshot di un telefono I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image48.jpeg)

2.  In **Application Logging** selezionare **File system**. Nel menu in
    alto, seleziona **Save**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image49.jpeg)

3.  Dal menu a sinistra, seleziona **Log Stream**. Vengono visualizzati
    i log per l'app, inclusi i log della piattaforma e i log
    dall'interno del container.

![Schermata di un computer che ritrae lo schermo di un computer I
contenuti generati dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image50.jpeg)

## 

## 

## Esercizio 7: Pulire le risorse

1.  Nella home page del portale di Azure selezionare Resources Groups.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image51.jpeg)

2.  Selezionare **NetworkWatcherRG** e fare clic su **Delete Resource
    Group**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image52.png)

3.  Digita +++**NetworkWatcherRG**+++ nella casella di testo e fai clic
    su **Delete**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image53.png)

![Uno screenshot di un errore del computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image54.png)

4.  Successivamente, nella pagina Resource Group, selezionare il
    **Resource groups** assegnato.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image55.png)

5.  Selezionare tutte le **resources**, quindi selezionare **Delete**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image56.png)

6.  Digita +++**delete**+++ nella casella di testo e fai clic su
    **Delete**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image57.png)

![Uno screenshot di un errore del computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image58.png)

7.  Una notifica di esito positivo per le risorse eliminate conferma
    l'eliminazione.

8.  Torna nell'area di lavoro GitHub, fai clic sul menu a discesa
    accanto a **Code**, seleziona i tre punti accanto al nome dello
    Codespaces e fai clic su **Delete**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image59.jpeg)

**Sommario:**

Abbiamo imparato a distribuire un'applicazione Quarkus sicura nel Azure
App Service, a connetterla al database PostgreSQL per aggiungere i nomi
Fruit dall'interfaccia utente dell'app.
