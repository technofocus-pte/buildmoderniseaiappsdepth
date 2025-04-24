# Caso d'uso 09 - Creazione di un'esperienza di chat bot usando Azure Cosmos DB per MongoDB e il servizio Azure OpenAI

**Obiettivo:**

Questo caso d'uso creerà una soluzione intelligente che combina Azure
Cosmos DB basato su vCore per la ricerca vettoriale MongoDB e il
recupero di documenti con Azure OpenAI services per creare un'esperienza
di chat bot.

![Diagramma di un'applicazione software I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image1.jpeg)

**Principali tecnologie utilizzate**: servizio Azure OpenAI, Azure
Cosmos DB, modello ChatGPT

**Durata stimata**: 60 minuti

**Tipo di laboratorio** -- Guidato dall'istruttore

**Importante:** se uno qualsiasi dei comandi non viene **pasted** in
**PowerShell**, aprire un blocco note, tenere il cursore in uno spazio
vuoto del blocco note e quindi fare clic sul pulsante T del comando da
incollare. Il contenuto verrà copiato nel blocco note e quindi sarà
possibile copiare e incollare dal blocco note su PowerShell.

## Esercizio 0: Informazioni sulla macchina virtuale e sulle credenziali

In questa attività, identificheremo e comprenderemo le credenziali che
utilizzeremo in tutto il laboratorio.

1.  **Instructions tab** Tenere la guida del laboratorio con le
    istruzioni da seguire durante il laboratorio.

2.  **Resources tab** contiene le credenziali necessarie per
    l'esecuzione del lab.

    - **URL**: URL del portale di Azure

    - **Subscription**: questo è l'ID dell'abbonamento assegnato
      all'utente

    - **Username**: l'ID utente con cui è necessario accedere ai servizi
      di Azure.

    - **Password**: password per l'account di accesso di Azure.
      Chiamiamo questo nome utente e password come credenziali di
      accesso di Azure. Useremo questi crediti ogni volta che
      menzioniamo le credenziali di accesso di Azure.

    - **Resource Group**: il **Resource Group** assegnato all'utente.

\[! Vigile\] **Importante:** assicurarsi di creare tutte le risorse in
questo Resource group

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image2.png)

3.  **Help** tab contiene le informazioni di supporto. Il valore **ID**
    è l'ID **Lab Instance** che verrà utilizzato durante l'esecuzione
    del lab.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image3.png)

## Esercizio 1: Effettuare il provisioning delle risorse di Azure

### Attività 1: Creare risorse di Azure usando script

1.  Login al portale di Azure all'indirizzo
    +++\*\*[https://portal.azure.com\*\*+++]() e accedere usando le
    credenziali di accesso di Azure dalla **Resources tab**.

2.  Dal portale di Azure selezionare la sottoscrizione. Nel riquadro
    sinistro selezionare Provider di risorse in Impostazioni,
    selezionare +++**Microsoft.Alertsmanagement**+++ e fare clic su
    **Register**.

![Uno screenshot dello schermo di un computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image4.jpeg)

3.  Dalla VM, cerca +++**power shell**+++, fai clic con il pulsante
    destro del mouse su **Windows PowerShell** e seleziona **Run as
    administrator**. Fare clic su **Yes** nella finestra di dialogo di
    conferma.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image5.jpeg)

![Uno screenshot di un errore del computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image6.jpeg)

4.  Eseguire il comando seguente per installare Az in PowerShell.

+++**Install-Module** **Az**+++

Selezionare **A** (Sì a tutti) quando richiesto.

**Nota:** il completamento dell'operazione richiederà fino a 5 minuti.

![Schermo di un computer con testo bianco I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image7.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image8.jpeg)

5.  Al termine, eseguire il comando seguente per importare il modulo Az.

+++**Import-Module Az**+++

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image9.jpeg)

6.  Esegui il comando seguente per utilizzare l'accesso basato su
    browser

+++Update-AzConfig -EnableLoginByWam $false+++

![Uno screenshot dello schermo di un computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image10.jpeg)

7.  Eseguire il comando seguente e selezionare l'account di accesso ad
    Azure, se richiesto, per accedere ad Azure.

+++Connect-AzAccount+++

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image11.jpeg)

8.  Esegui i comandi seguenti per accedere alla cartella **LabFiles**.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![Un cartello rettangolare blu con testo bianco I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image12.jpeg)

9.  Esegui il comando seguente per installare **Microsoft Bicep**
    utilizzando **winget**.

+++winget install -e --id Microsoft.Bicep+++

Digitare **Y** se richiesto.

![Schermo di un computer con testo bianco I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image13.jpeg)

10. **Close** PowerShell e **open it again**.

11. Eseguire il comando seguente e selezionare l'account di accesso ad
    Azure, se richiesto, per accedere ad Azure.

+++Connect-AzAccount+++

12. Esegui i comandi seguenti per accedere alla cartella **LabFiles**.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![Un cartello rettangolare blu con testo bianco I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image12.jpeg)

13. Eseguire il comando seguente per impostare l'ID sottoscrizione.

+++Set-AzContext -SubscriptionId @lab.CloudSubscription.Id+++

![Schermata del computer di una schermata blu I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image14.jpeg)

14. Aprire il file **azuredeploy.bicep** nel percorso
    **C:\LabFiles\Build a Chat bot\Labs\deploy** e sostituire le lettere
    **dgxxxxxxx** alla riga 35 con <+++dg@lab.LabInstance.Id>+++. Alla
    riga **74**, aggiornare la versione come +++**0125**+++.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image15.png)

![](./media/image16.png)

15. Eseguire il comando seguente per distribuire le risorse come l'area
    di lavoro Azure Cosmos DB, Azure OpenAI in Azure.

New-AzResourceGroupDeployment -ResourceGroupName @lab.
CloudResourceGroup(ResourceGroup1). Name -TemplateFile
.\azuredeploy.bicep -TemplateParameterFile .\azuredeploy.parameters.json
-c '''

\>\[! Note\] \*\*Note:\*\* La distribuzione richiederà dai 10 ai 15
minuti circa.

Se si verifica un problema con la distribuzione e non riesce, provare ad
aggiornare il nome nel passaggio 14 con un nome diverso e riprovare.

\>\[! Note\] \*\*Note:\*\* Digitare Y quando richiesto.

![Schermata del computer di una schermata blu I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image17.jpeg)

![](./media/image18.jpeg)

\>\[! Note\]\*\*Note:\*\* se non è disponibile alcun aggiornamento in
PowerShell dopo 15-20 minuti, controllare in Resource Group -\>
Distribuzioni nel portale di Azure o premere \*\*Enter\*\* nella
\*\*PowerShell\*\* window.

### Attività 2: Controllare le risorse create in Azure

1.  Accedere al **Azure Portal** all'indirizzo
    +++<https://portal.azure.com/+++> usando le **Azure Login
    credentials**. Selezionare **Resource Groups**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image19.jpeg)

2.  Nell'elenco Resource groups, selezionare **Assigned Resource
    Group**.

![Uno screenshot di una pagina web I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image20.png)

3.  Si noti che viene creato un set di risorse, tra cui **Azure OpenAI
    resources**, **App Service**, **Azure Cosmos DB for MongoDB**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image21.png)

4.  Fare clic sulla risorsa **Azure OpenAI**.

![](./media/image22.png)

5.  Selezionare **Keys and Endpoint** in **Resource Management**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image23.jpeg)

6.  Copiare e salvare la **Key 1** e **Endpoint** in un blocco note per
    riferimento futuro.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image24.jpeg)

7.  Tornare alla pagina del gruppo di risorse e selezionare la risorsa
    **Azure Cosmos DB for Mongo DB**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image25.png)

8.  Fare clic su **Connection Strings** in **Settings**. Copia il valore
    di Self (sempre questo cluster.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image26.jpeg)

9.  Copiare la stringa di connessione e incollarla in un notepad.
    Sostituisci il della \<**password\>** con +++**myMongoDB98**+++
    nella stringa di connessione copiata e salvalo nel notepad.

## Esercizio 2: Esplorare e usare i modelli Azure OpenAI dal codice

### Attività 1: Configurare l'ambiente

1.  Dalla barra di ricerca di Windows della macchina virtuale lab
    cercare +++Visual Studio code+++ e aprire **Visual Studio Code**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image27.jpeg)

2.  Fare clic su **Open Folder**. (Se non viene visualizzato,
    selezionare **File -\> Open Folder**)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image28.jpeg)

3.  Vai su **C:\Labfiles**, fai clic su **Build a** **Chat bot** e
    seleziona **Select Folder.**

![Uno screenshot di un chat bot I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image29.jpeg)

4.  Clicca su **Yes, I trust the Authors** nel pop-up.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image30.jpeg)

5.  Da Visual Studio Code aprire **lab_0_explore_and_use_models.ipynb**
    dalla **Labs** Folder**.**

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image31.jpeg)

6.  Fare clic su **Select Kernel.**

7.  Seleziona **Install** nel popup **Do you want to install the
    recommended extensions for Python.**

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image32.jpeg)

8.  Fare clic su **Allow access** se richiesto.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image33.jpeg)

9.  Fare clic su **Select Kernel**. Selezionare **Python Environments**
    e quindi **Python 3.12.3** o versione successiva che viene elencato
    come **Suggested** o **Recommended**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image34.jpeg)

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image35.jpeg)

10. Apri il .**env** file

11. Sostituire **DB_CONNECTION STRING,** **AOAI_KEY** e
    **AOAI_Endpoint** salvati nel **notepad** in precedenza
    nell'attività 2 dell'esercizio 1.

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image36.jpeg)

A questo punto, le variabili di ambiente sono impostate in modo che
puntino alle Azure resources già create.

### Attività 2: Eseguire il codice

1.  Torna nel file **Lab 0 ipynb**, **execute** la **first cell**
    facendo clic sul pulsante Riproduci per installare l'ultima libreria
    client OpenAI.

![Uno schermo nero con uno sfondo nero I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image37.jpeg)

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image38.jpeg)

2.  **Execute** la cella successiva per installare **Python-dotenv**

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image39.jpeg)

3.  Premi **Ctrl+Shift+P**, digita +++Reload Window+++ e seleziona
    l'opzione Developer:Reload Window che viene elencata.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image40.jpeg)

4.  Eseguire nuovamente dalla **first cell**.

5.  **Execute** la cella successiva per importare la libreria OpenAI
    richiesta, os per accedere alle variabili d'ambiente e dotenv per
    caricare le variabili d'ambiente dal .env file.

![Schermata di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image41.jpeg)

6.  **Execute** la cella successiva per creare il **Azure OpenAI
    client** per chiamare l'API di completamento di Azure OpenAI Chat:

![Schermo di un computer con testo I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image42.jpeg)

7.  **Execute** la cella successiva per chiamare il metodo
    **.chat.completions.create()** sul client per eseguire il di una
    **chat Completion**. Dovresti ricevere una risposta in chat.

![Schermo di un computer con del testo I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image43.jpeg)

## Esercizio 3: Prima applicazione API Cosmos DB per MongoDB

Questo esercizio illustra come creare il primo progetto Cosmos DB.
Utilizzeremo un taccuino per illustrare le operazioni CRUD di base.

1.  Apri il file **lab_1_first_application.ipynb** dalla cartella
    **Labs.**

2.  Fare clic su **Select kernel** e scegliere la **Python version**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image44.jpeg)

3.  **Execute** la prima cella per eseguire l'installazione di
    **pymongo**.

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image45.jpeg)

4.  **Execute** la cella successiva per eseguire le **imports**
    richieste

![Schermata di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image46.jpeg)

5.  Eseguire la cella successiva per **Create a Database.**

\[! Nota\] **Nota:** Questo utilizzerà la stringa di connessione che
abbiamo aggiornato nel .env file

![Schermata di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image47.jpeg)

6.  **Execute** la cella successiva per creare una **collection**.

![Una schermata nera con testo bianco I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image48.jpeg)

7.  **Execute** la cella successiva per creare un **document**. Un
    metodo per creare un documento consiste nell'utilizzare il metodo
    insert_one. Questo metodo prende un singolo documento e lo inserisce
    nel database.

![Schermata di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image49.jpeg)

8.  **Execute** la cella successiva per **retrieve a single document**
    dal database. A tale scopo viene utilizzato il metodo **find_one**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image50.jpeg)

9.  **Execute** la cella successiva in cui viene utilizzato
    **find_one_and_update** metodo per aggiornare un singolo documento
    nel database.

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image51.jpeg)

10. **Execute** la cella successiva in cui viene utilizzato
    **delete_one** metodo per eliminare un singolo documento dal
    database.

![](./media/image52.jpeg)

11. Il metodo **find** viene utilizzato per eseguire una query per più
    documenti nel database. **Execute** le **next 3 cells** una per una
    per vederlo in azione.

![Schermata di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image53.jpeg)

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image54.jpeg)

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image55.jpeg)

![Schermata di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image56.jpeg)

12. Nella cella seguente verranno **delete** il database e la raccolta
    creati in questo esercizio. Questa operazione viene eseguita
    utilizzando il metodo **drop_database** sull'oggetto database

![Schermo di un computer con testo I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image57.jpeg)

## Esercizio 4: Caricare i data in Cosmos DB usando l'API MongoDB

Nell'esercizio precedente è stato illustrato come aggiungere data a una
raccolta singolarmente. In questo esercizio viene illustrato come
caricare i data utilizzando operazioni di massa in più raccolte. Questi
data verranno usati nei laboratori successivi per illustrare
ulteriormente le funzionalità dell'API Azure Cosmos DB per MongoDB
sull'intelligenza artificiale.

Questo notebook illustra come caricare i data in Cosmos DB dai file JSON
di Cosmic Works nel database usando l'API MongoDB.

1.  Apri il file **lab_2_load_data.ipynb** dalla cartella **Labs**. Fare
    clic su **Select Kernel** e selezionare la **Python version**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image58.jpeg)

2.  Eseguire la prima cella per installare le **requests**.

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image59.jpeg)

3.  **Execute** la cella successiva per eseguire le **imports**
    richieste.

![Schermo di un computer con testo verde I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image60.jpeg)

4.  **Execute** la cella successiva che stabilisce una **connection**
    con il **database**.

![Schermo di un computer con testo I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image61.jpeg)

![Schermata di testo del computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image62.jpeg)

5.  **Execute** la cella successiva per **load** i **products**.

![Schermata dello schermo di un computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image63.jpeg)

6.  **Execute** le celle successive per **load** i **customers e sales
    raw data**. In questo repository, i data dei clienti e delle vendite
    sono memorizzati nello stesso file. Il campo type viene utilizzato
    per distinguere tra i due tipi di documenti.

![Schermata di un codice informatico I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image64.jpeg)

![](./media/image65.jpeg)

![Schermata di un codice informatico I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image66.jpeg)

7.  **Execute** la cella successiva da **clean up**.

![Schermata di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image67.jpeg)

## Esercizio 5: Ricerca vettoriale con Azure Cosmos DB basato su vCore per MongoDB

1.  Apri il file **lab_3_mongodb_vector_search.ipynb** dalla cartella
    **Labs.**

2.  Fare clic su **Select Kernel** e selezionare la **Python version**.

![](./media/image68.jpeg)

3.  **Execute** la prima cella per installare la **tenacity**.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image69.jpeg)

4.  **Execute** la cella successiva per eseguire le **imports**
    richieste.

![Schermo di un computer con testo I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image70.jpeg)

5.  **Execute** la cella successiva per **load** le **settings** dal
    .env file.

![Schermo di un computer con testo I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image71.jpeg)

6.  **Execute** la cella successiva per stabilire **connectivity** al
    **database**.

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image72.jpeg)

7.  **Execute** la cella successiva per stabilire la **Azure OpenAI
    connectivity**.

![Schermata di un codice informatico I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image73.jpeg)

8.  Il processo di creazione di un campo di incorporamento vettoriale su
    ciascun documento deve essere eseguito una sola volta. Tuttavia, se
    un documento viene modificato, il campo di incorporamento vettoriale
    dovrà essere aggiornato con un vettore aggiornato. Viene fatto nelle
    due celle successive. **Execute** le due celle successive e osserva
    gli **embeddings** ottenuti come output nella seconda.

![Schermata di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image74.jpeg)

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image75.jpeg)

9.  **Execute** la cella successiva per**Vectorize and update all
    documents in the Cosmic Works database.**

![Schermata del computer di un codice di programma I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image76.jpeg)

10. **Execute** le **3** celle successive per aggiungere **vector
    fields** a **products, customer and sales documents.**

> **Nota:** la prima cella impiegherà circa 5 minuti, la seconda circa 3
> minuti e la terza circa 20 minuti per completare l'esecuzione.

! \[\](./media/image77.jpeg)

11. **Execute** la cella successiva per creare l **products vector
    index.**

![Schermata dello schermo di un computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image77.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image78.jpeg)

12. Ora che a ogni documento è associato l'incorporamento vettoriale e
    gli indici vettoriali sono stati creati in ogni raccolta, è ora
    possibile usare le funzionalità di ricerca vettoriale di Azure
    Cosmos DB basato su vCore per MongoDB. **Execute** le prossime **3**
    celle.

![](./media/image79.jpeg)

![](./media/image80.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image81.jpeg)

13. **Execute** le celle successive per osservare l'utilizzo dei
    **vector search results** in un modello RAG con Chat GPT-3.5

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image82.jpeg)

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image83.jpeg)

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image84.jpeg)

14. Osservare l'output delle seguenti celle.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image85.jpeg)

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image86.jpeg)

## 

## Esercizio 6: Eliminare le risorse distribuite

1.  Nel portale di Azure
    (+++[https://portal.azure.com+++](https://portal.azure.com+++/))
    selezionare il gruppo di risorse assegnato.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image87.png)

2.  Seleziona tutte le risorse sottostanti, fai clic sui **three dots**
    nel menu e seleziona **Delete** per eliminare tutte le risorse.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image88.png)

3.  Digita +++**delete**+++ nella casella di testo e fai clic sul
    pulsante Delete.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image89.png)

4.  Dopo aver eliminato le risorse, nella home page del portale di Azure
    cercare **+++Azure AI Services+++** e selezionarlo.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image90.jpeg)

5.  Selezionare **Azure OpenAI** nel riquadro sinistro e quindi
    selezionare **Manage Deleted resources**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image91.jpeg)

6.  Seleziona la risorsa che viene elencata lì e quindi fai clic su
    **Purge**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image92.jpeg)

7.  Fare clic su **yes**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image93.jpeg)

**Sommario:**

È stata creata una soluzione con Azure Cosmos DB per la ricerca
vettoriale MongoDB e il recupero di documenti con i servizi Azure
OpenAI.
