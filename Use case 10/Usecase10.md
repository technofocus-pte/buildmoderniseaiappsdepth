# 

# Caso d'uso 10 : Distribuzione di un'applicazione di chat per rispondere alle domande dell'utente e tenere traccia della cronologia chat nelle conversazioni

**Obiettivo:**

Questo caso d'uso illustra i passaggi per connettere un'applicazione
Blazor esistente a un account Azure Cosmos DB per NoSQL e a un account
Azure OpenAI. L'applicazione invia prompt al modello in Azure OpenAI e
analizza le risposte. L'applicazione archivia anche varie sessioni di
conversazione e i messaggi corrispondenti come elementi collocati in un
singolo contenitore all'interno di Azure Cosmos DB per NoSQL.

In sintesi, l'applicazione potrà:

- **Connect** al modello di Azure OpenAI usando .NET SDK

- **Send** prompt al modello e analizza la risposta di completamento

- **Connect** ad Azure Cosmos DB per NoSQL usando .NET SDK

- **Manage** gli articoli con singole operazioni, query e batch
  transazionali

Questa applicazione di chat di esempio risponde alle domande dell'utente
e tiene traccia della cronologia chat nelle conversazioni.

![](./media/image1.jpeg)

**Principali tecnologie utilizzate** --, Csharp, nosql, asp-net, blazor,
azure-cosmos-db,

**Durata stimata**: 45 minuti

**Tipo di laboratorio:** Guidato dall'istruttore

**Prerequisiti:**

Account GitHub: è necessario che tu abbia le tue credenziali di accesso
a GitHub. Se non ne hai, per favore creane uno da qui -''
**https://github.com/signup?user_email=&source=form-home-signupobjectives''**

### Attività 1 : Eseguire la finestra mobile

1.  Nella casella di ricerca di Windows, digita **Docker** , quindi fai
    clic su **Docker Desktop**.

![](./media/image2.jpeg)

### Attività 2 : Registrare il fornitore di servizi

1.  Aprire un browser e passare a <https://portal.azure.com> e accedere
    con le credenziali di Azure disponibili nella scheda **Resource**
    della VM.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image3.png)

2.  Nella home page del portale di Azure fare clic sul riquadro di
    **Resource Groups**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image4.png)

3.  Copiare il nome del di resource group e salvarlo nel notepad per
    usare l'attività successiva per deploy le risorse necessarie in
    questo di resource group.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image5.png)

4.  Torna alla Home page, fai clic sul riquadro **Subscription**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.png)

5.  Fare clic sul nome dell'abbonamento.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image7.png)

6.  Fare clic su **Settings - \>** di **Resource provider** dal menu di
    spostamento a sinistra.

![](./media/image8.png)

7.  Digita ''**Microsoft.AlertsManagement**'' e premi invio. Selezionalo
    e poi clicca su **Register**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image9.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image10.png)

### Attività 3: Effettuare il provisioning di servizi e applicazioni in Azure

1.  Apri un browser e vai su ''https:\\github.com'' e accedi con il tuo
    account Github. Cerca il repository sottostante

![](./media/image11.jpeg)

2.  Cerca il repository sottostante e fai clic su **Fork**.

> ''https://github.com/technofocus-pte/chat-csharp-cosmos-db-nosql-openai''

![](./media/image12.jpeg)

3.  Inserisci il nome del repository e fai clic su **Create
    repository**.

![](./media/image13.jpeg)

4.  Fare clic su **Code -\> Code space -\> Open Code space.**

![](./media/image14.jpeg)

5.  Attendi la configurazione del contenitore Dev. Ci vogliono 3-5
    minuti

![](./media/image15.jpeg)

6.  Eseguire il comando seguente per accedere ad AZD. Copia il codice
    generato e premi Invio.

> ''**azd auth login''**

![](./media/image16.jpeg)

7.  Incolla il codice generato e accedi con le tue Azure credentials.

![](./media/image17.jpeg)

![](./media/image18.jpeg)

8.  Eseguire il comando seguente per inizializzare il progetto nella
    directory corrente. Immettere il nome dell'ambiente come
    ''**cosmoschatapp''** e premere Invio.

''azd init ''

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image19.png)

9.  Eseguire il comando seguente per distribuire i servizi in Azure,
    compilare il contenitore. Selezionare i valori seguenti.

> ''azd provision''
>
> **Select an Azure Subscription to use** : selezionare la
> sottoscrizione
>
> **Select an Azure location to use: East us/ west us**(a volte, Stati
> Uniti orientali potrebbe non essere disponibile, scegliere una
> posizione diversa e distribuire).
>
> **Enter a value for the 'existingResourceGroupName' infrastructure
> parameter: ResourceGroup1**

![](./media/image20.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image21.png)

10. Attendere il completamento del provisioning della risorsa. Questo
    processo richiederà 5-10 minuti per creare tutte le risorse
    necessarie.

![](./media/image22.png)

### Attività 4: Distribuire l'applicazione in Azure

1.  Tornare al di Azure portal e fare clic sul riquadro di Resource
    groups nella home page.

![](./media/image23.png)

2.  Fare clic su nome del di resource group.

![](./media/image24.png)

3.  Dovresti vedere le risorse qui sotto

- **Container**

- **Container Registry**

- **Azure Cosmos Db account**

- **AureOpenAI**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image25.png)

4.  Fare clic su Nome **Container registry** .

![](./media/image26.png)

5.  Espandi **Settings** dal menu di navigazione a sinistra, fai clic su
    **Access keys.** Selezionare **Admin user check box.** Copia il
    **Login server**, il **user name** e la **password** in un notepad
    per utilizzarli per distribuire l'app.

![](./media/image27.png)

6.  Duplica la scheda per aprire il Azure portal in una **new** scheda.

![](./media/image28.png)

7.  Fare clic sul menu di spostamento superiore del modulo del nome del
    di resource name.

![](./media/image29.png)

8.  Fare clic su Nome app contenitore.

![](./media/image30.png)

9.  Fai clic sul pulsante **Authorize** sotto Github-Accedi per
    autenticarti con il tuo account GitHub. Autorizza il tuo account
    Github.

10. Seleziona i valori sottostanti

> **Organization : your Github organization**
>
> **Respository:** chat-csharp-cosmos-db-nosql-openai
>
> **Branch :** principale

![](./media/image31.png)

11. Scorri verso il basso fino a **del Registry settings** e inserisci i
    valori sottostanti, quindi fai clic sul pulsante **Start continuous
    deployment**.

- Fonte del repository : **Docker Hub or other registries.**

- URL del server di accesso: il server di accesso copiato dal registro
  dei contenitori (passaggio \# 5)

- Nome utente: la password dal registro contenitori (passaggio \#5)

- Password : La tua password dal registro dei contenitori (passaggio \#
  5)

![](./media/image32.png)

12. Fare clic sul collegamento File del flusso di lavoro. Si apre una
    nuova scheda con Github.

![](./media/image33.png)

13. Fare clic sulla scheda **Actions**.

![](./media/image34.png)

14. Attendere il completamento della deployment.

![](./media/image35.png)

15. Non chiudere alcuna scheda.

### Compito 5 : Accedi all'app di chat

1.  Tornare al portale di Azure e fare clic su **Overview** nel riquadro
    di spostamento a sinistra, quindi fare clic su **Application**
    **URL**. Apre una nuova app per il caricamento.

![](./media/image36.png)

2.  Fai clic sul pulsante **Create New chat**.

![](./media/image37.png)

3.  Immettere il prompt seguente.

''Qual è la capacità di posti a sedere per Lumen a Seattle?''

![](./media/image38.jpeg)

4.  Immettere il prompt sottostante. Esplora l'app con diversi
    suggerimenti.

''è più grande dello stadio Dogger?? \`\`

![](./media/image39.jpeg)

### Compito 6 : Pulisci tutte le risorse

Per pulire tutte le risorse create da questo esempio:

1.  Torna alla scheda del portale Github e aggiorna la pagina.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image40.png)

2.  Clicca su Codice, seleziona il ramo creato per questo lab e clicca
    su **Delete**.

![](./media/image41.png)

3.  Confermare l'eliminazione del ramo facendo clic sul pulsante
    **Delete**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image42.png)

5.  Tornare al di **Azure portal -\> Resource group- \> Resource group
    name.**

![](./media/image43.png)

6.  Seleziona tutta la risorsa e quindi fai clic su Elimina come
    mostrato nell'immagine sottostante. (**DO NOT DELETE** resource
    group)

![](./media/image44.png)

7.  Digita **''delete''** nella casella di testo e poi clicca su
    **Delete**.

> ![](./media/image45.png)

8.  Conferma l'eliminazione cliccando su **Delete**.

![](./media/image46.png)

**Sommario:**

Sono state implementate le classi di servizio usando i pacchetti
Microsoft.Azure.Cosmos e Azure.AI.OpenAI in NuGet. Sono stati inviati
prompt all'interfaccia di conversazione Azure OpenAI insieme ai prefissi
contestuali e sono state analizzate le proprietà di utilizzo e corpo
della risposta. È stato anche usato Azure Cosmos DB per NoSQL per
archiviare le sessioni di conversazione e i messaggi all'interno di un
singolo contenitore.
