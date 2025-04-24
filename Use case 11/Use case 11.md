# Caso d'uso 11 - Creazione di un Copilot con Azure OpenAI, Azure Cosmos DB per NoSQL

In questo caso d'uso, si connetterà un'applicazione Web Blazor ad Azure
Cosmos DB per NoSQL e Azure OpenAI usando i kit di sviluppo software
.NET. Il codice gestisce ed esegue query sugli elementi in un
contenitore API per NoSQL. Il codice invia anche prompt ad Azure OpenAI
e analizza le risposte.

**Durata del laboratorio:** 45 minuti

**Tipo di laboratorio**: Guidato da un istruttore

**Obiettivo**

- Per configurare l'ambiente di sviluppo per Blazor, PostgreSQL e
  OpenAI.

- Per creare un progetto Blazor e progettare un'interfaccia di chat
  reattiva.

- Per configurare il database PostgreSQL in Azure e connetterlo all'app
  Blazor.

- Per integrare Azure OpenAI per funzionalità di chat avanzate.

- Per distribuire l'applicazione Blazor e il database PostgreSQL in
  Azure.

- Testare l'applicazione al fine di garantire un'interazione senza
  soluzione di continuità tra i componenti.

- Per monitorare e risolvere i problemi dell'applicazione distribuita in
  Azure.

**Tecnologie chiave utilizzate:** Azure Cosmos DB per NoSQL, Azure
OpenAI

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

    - **Resource group**: il **Resource Group** assegnato all'utente.

\[! Alert\] **Importante:** assicurarsi di creare tutte le risorse in
questo Resource group

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image1.png)

3.  **Help Tab** contiene le informazioni di supporto. Il valore **ID**
    è l'ID i **Lab instance** che verrà utilizzato durante l'esecuzione
    del lab.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image2.png)

## Esercizio 1: Distribuire l'infrastruttura e completare la configurazione iniziale

Per completare questo progetto, è necessario un account Azure Cosmos DB
per NoSQL e un account Azure OpenAI. Per semplificare questo processo,
distribuire un modello Bicep in Azure con entrambi questi account.

### Attività 1: Distribuire l'infrastruttura dal modello

1.  Aprire il file dal percorso **C:\Labfiles\Build and Test custom chat
    application Using Azure Cosmos DB and AzureOpenAI** e aggiornare la
    versione di Azure OpenAI alla riga 96 in modo che sia +++0125+++.
    **Save** il file.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image3.png)

2.  Aprire un nuovo browser e immettere l'URL seguente nella barra degli
    indirizzi: +++<https://portal.azure.com/+++> per aprire il Azure
    Portal.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image4.jpeg)

3.  Nel Azure Portal, fare clic sul pulsante **\[\>\_\] (Cloud Shell)**
    nella parte superiore della pagina a destra della casella di
    ricerca. Verrà aperto un riquadro Cloud Shell nella parte inferiore
    del portale. La prima volta che si apre Cloud Shell, potrebbe essere
    richiesto di scegliere il tipo di shell che si desidera utilizzare
    (**Bash** or **PowerShell**). Seleziona **Bash**. Se non vedi questa
    opzione, salta questo passaggio.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image5.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image6.jpeg)

4.  Nella finestra di dialogo **Getting Started**, selezionare **Mount
    storage account**, selezionare la **subscription** e quindi fare
    clic su **Apply**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image7.jpeg)

5.  Nella finestra di dialogo **Mount storage account**, selezionare
    **we will create a storage account for you** e fare clic su
    **Next**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image8.jpeg)

![Un primo piano dello schermo di un computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image9.jpeg)

6.  Assicurarsi che il tipo di shell indicato nella parte superiore
    sinistra del riquadro Cloud Shell sia impostato su **Bash**. Se si
    tratta di **PowerShell**, passare a **Bash** utilizzando il menu a
    discesa.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image10.jpeg)

7.  Una volta avviato il terminale, fai clic su **Manage files -\>
    Upload**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image11.jpeg)

8.  Selezionare **azuredeploy.JSON** file dal percorso **C:\Labfiles\\
    Build and Test a custom chat application Using Azure Cosmos DB e
    AzureOpenAI** e selezionare **Open**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image12.jpeg)

Dovresti ricevere un messaggio di successo per il caricamento del file.

![Uno sfondo bianco con testo nero I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image13.jpeg)

9.  Creare una nuova variabile della shell denominata
    **resourceGroupName** con il nome del Azure resource group creato
    (mslearn-cosmos-openai).

+++resourceGroupName="ResourceGroup1"+++( Get the Resource Group name
from the Resources tab) 

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image14.png)

10. Distribuire il file modello di **azuredeploy.json** nel gruppo di
    risorse usando az group deployment create. Quindi, esegui il
    seguente comando.

+++az deployment group create --resource-group $resourceGroupName --name
zero-touch-deployment --template-file azuredeploy.json+++

**Nota:** Questa distribuzione può richiedere circa 5-10 minuti.

![Uno screenshot dello schermo di un computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image15.jpeg)

![Uno screenshot dello schermo di un computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image16.jpeg)

### Attività 2: Ottenere le credenziali dell'account Azure Cosmos DB per NoSQL e Azure OpenAI

La distribuzione precedente ha distribuito gli account Azure Cosmos DB
per NoSQL e Azure OpenAI e quindi ha archiviato le credenziali nella
configurazione dell'app Web del servizio app di Azure. A questo punto, è
possibile scegliere di usare il portale di Azure o l'interfaccia della
riga di comando di Azure per recuperare le credenziali per ogni
servizio.

1.  Nella home page del portale di Azure fare clic su **Resource
    Groups.**

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image17.jpeg)

2.  Selezionare il resource group.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image18.png)

3.  Nella pagina **Resource Groups** espandere il pannello
    **Essentials** e osservare l' intestazione **Deployments**. A questo
    punto, lo stato della distribuzione dovrebbe essere **Succeeded**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image19.png)

4.  A questo punto, selezionare l' account **Azure Cosmos DB** per
    passare alla pagina della resource.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image20.png)

5.  Selezionare l' opzione **Keys** nella sezione **Settings** del menu
    di navigazione delle risorse. Registrare il valore dei campi **URI**
    e **PRIMARY KEY**. Questi valori verranno utilizzati in un secondo
    momento.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image21.jpeg)

6.  Tornare alla pagina **Resource Groups**. Selezionare l' account
    **Azure OpenAI**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image22.png)

7.  Nella finestra **Azure Open AI** passare alla sezione **Resource
    Management** e fare clic su **Keys and Endpoints**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image23.jpeg)

8.  Nella pagina **Keys and Endpoints ,** copiare **KEY1** (*è possibile
    utilizzare KEY1 o KEY2)* e **Endpoint**, quindi **Save** il notepad
    per utilizzare le informazioni nelle attività future.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image24.jpeg)

### Attività 3: Eseguire la finestra mobile

1.  Nella casella di ricerca di Windows, digita +++Docker+++ , quindi
    fai clic su **Docker Desktop**.

![Uno screenshot di un desktop I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image25.jpeg)

2.  **Run** il desktop Docker.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image26.jpeg)

## Esercizio 2 - Configurare e creare l'applicazione iniziale

1.  Dalla barra di ricerca della macchina virtuale cercare +++Visual
    Studio+++ e selezionare **Visual Studio Code**.

2.  Fare clic su **File** -\> **Open Folder**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image27.jpeg)

3.  Seleziona **cosmosdb-chatgpt** da **C:\LabFiles** e fai clic su
    **Select Folder**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image28.jpeg)

4.  Fare clic sull'opzione **Yes, I trust the authors** nella **Do you
    trust the authors dialog.** ![Uno screenshot di un computer I
    contenuti generati dall'intelligenza artificiale potrebbero non
    essere corretti.](./media/image29.jpeg)

5.  Nell'editor di **Visual Studio Code**, fare clic su **Terminal**,
    aprire un **New Terminal**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image30.jpeg)

6.  In un'applicazione .NET è comune usare i provider di configurazione
    per inserire nuove impostazioni nell'applicazione. Per questa
    applicazione, utilizzare le **appsettings. Development.json** file
    per fornire i valori più aggiornati per l'endpoint e la chiave di
    Azure OpenAI.

7.  Apri le **appsettings.Development.JSON** file. Sostituire i
    segnaposto per i valori uri e chiave delle **Azure Cosmos DB** e
    **Azure OpenAI** risorse nel file con i valori salvati in
    precedenza.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image31.jpeg)

8.  **Build** il progetto .NET eseguendo il comando seguente.

+++dotnet build+++

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image32.jpeg)

## Esercizio 3: Comprendere il codice

### Attività 1: Aggiungere membri necessari e un'istanza client

1.  Apri il file **Services/OpenAiService.cs**. Questo file implementa
    le variabili di classe necessarie per usare il client Azure OpenAI.
    Implementa alcuni prompt statici e crea una nuova istanza della
    classe OpenAIClient.

2.  Questo blocco di codice crea una nuova variabile stringa denominata
    \_systemPromptText con un blocco statico di testo da inviare
    all'assistente di intelligenza artificiale prima di ogni prompt.

> private readonly string \_systemPrompt = @"
>
> Sei un assistente AI che aiuta le persone a trovare informazioni.
>
> Fornisci risposte concise, educate e professionali." +
> Environment.NewLine;

3.  Questo blocco di codice crea un'altra nuova variabile stringa
    named_summarizePrompt con un blocco statico di testo da inviare
    all'assistente AI con le istruzioni su come riassumere una
    conversazione.

> private readonly string \_summarizePrompt = @"
>
> Riassumi questo prompt in una o due parole da utilizzare come
> etichetta in un pulsante in una pagina web.
>
> Non usare alcuna punteggiatura." + Environment.NewLine;

4.  Questo blocco di codice crea una nuova istanza della classe
    OpenAIClient usando l'endpoint per creare un URI e la chiave per
    creare un AzureKeyCredential.

> Uri uri =new(endpoint);
>
> AzureKeyCredential credential = new(key);
>
> \_client = new(
>
> endpoint: uri,
>
> keyCredential: credential
>
> );

**Attività 2: Porre una domanda al modello di AI**

Innanzitutto, implementa una conversazione domanda-risposta inviando un
prompt di sistema, una domanda e un ID sessione in modo che il modello
di AI possa fornire una risposta nel contesto della conversazione
corrente. Assicurati di misurare il numero di token necessari per
analizzare il prompt e restituire una risposta (o un completamento in
questo contesto).

1.  Questo blocco di codice crea una nuova variabile denominata options
    di tipo ChatCompletionsOptions. Aggiunge le due variabili di
    messaggio all'elenco Messaggi e imposta il valore di User sul
    parametro del costruttore sessionId.

> ChatCompletionsOptions options = new()
>
> {
>
> DeploymentName = "chatmodel",
>
> Messages = {
>
> new ChatRequestSystemMessage(\_systemPrompt),
>
> new ChatRequestUserMessage(userPrompt)
>
> },
>
> User = sessionId,
>
> MaxTokens = 4000,
>
> Temperature = 0,3f,
>
> NucleusSamplingFactor = 0,5f,
>
> FrequencyPenalty = 0,
>
> PresencePenalty= 0
>
> };

2.  Il metodo GetChatCompletionsAsync della variabile client Azure
    OpenAI (\_client) viene richiamato in Asynchronously. Il risultato
    viene archiviato in una variabile denominata completions di tipo
    ChatCompletions.

> Response\<ChatCompletions\> completionsResponse= await_client.
> GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;

3.  Infine, il blocco di codice seguente, restituisce una tupla come
    risultato del metodo GetChatCompletionAsync con il contenuto del
    completamento come stringa, il numero di token associati al prompt e
    il numero di token per la risposta.

> return (
>
> completionText: completions.Choices\[0\]. Message.Content,
>
> completionTokens: completions.Usage.CompletionTokens
>
> );

**Attività 3: Chiedere al modello di AI di riassumere una
conversazione**

A questo punto, invia al modello di AI un prompt di sistema diverso, la
conversazione corrente e l'ID della sessione in modo che il modello di
AI possa riassumere la conversazione in un paio di parole.

2.  Il codice seguente crea una variabile ChatCompletionsOptions
    denominata options con le due variabili di messaggio nell'elenco
    Messaggi, l'utente impostato sul parametro del costruttore
    sessionId, MaxTokens impostato su 200 e le proprietà rimanenti.

> ChatCompletionsOptions options = new()
>
> {
>
> DeploymentName = "chatmodel",
>
> Messages = {
>
> new ChatRequestSystemMessage(\_systemPrompt),
>
> new ChatRequestUserMessage(conversationText)
>
> },
>
> User = sessionId,
>
> MaxTokens = 200,
>
> Temperature = 0,0 f,
>
> NucleusSamplingFactor = 1.0f,
>
> FrequencyPenality = 0,
>
> PresencePenality = 0
>
> };

3.  Il codice seguente richiama il \_client. GetChatCompletionsAsync
    asynchronously con il nome del modello (\_modelName) e la variabile
    options come parametro e archivia il risultato in una variabile
    denominata completions di tipo ChatCompletions. Restituisce il
    contenuto del completamento come stringa come risultato del metodo
    SummarizeAsync.

> Response\<ChatCompletions\> completionsResponse = await \_client.
> GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;
>
> string completionText = completions. Choices\[0\]. Message.Content;
>
> return completionText;

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image33.jpeg)

**Attività 4 - Connettersi ad Azure Cosmos DB per NoSQL**

La classe CosmosDbService contiene un'implementazione stub di un
servizio simile alla classe OpenAiService su cui si è lavorato in
precedenza in questo modulo. Al contrario, questa classe usa .NET SDK
per Azure Cosmos DB, che funziona in modo leggermente diverso.

Questa sezione illustra l'implementazione delle variabili di classe e
del client necessarie per accedere ad Azure Cosmos DB per NoSQL usando
il client.

1.  Apri il file **Services/CosmosDbService.cs**.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image34.jpeg)

2.  Il codice seguente crea una variabile denominata options di tipo
    CosmosSerializationOptions e imposta la proprietà
    PropertyNamingPolicy della variabile su
    CosmosPropertyNamingPolicy.CamelCase.

> CosmosSerializationOptions options= new()
>
> {
>
> PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
>
> };

**Nota:** l'impostazione di questa proprietà garantirà che il codice
JSON prodotto dall'SDK sia serializzato e deserializzato nel caso camel,
indipendentemente dal modo in cui la proprietà corrispondente è
racchiusa nella classe .NET.

3.  Il codice seguente crea una nuova istanza di tipo CosmosClient
    denominata client usando la classe CosmosClientBuilder, l'endpoint,
    la chiave e le opzioni di serializzazione specificate in precedenza.

> CosmosClient client = new CosmosClientBuilder(endpoint, key)
>
> . WithSerializerOptions(options)
>
> . Build();

4.  Il codice seguente crea una nuova variabile nullable di tipo
    Database denominata database chiamando il metodo GetDatabase della
    variabile client.

**Database? database = client?. GetDatabase(databaseName);**

5.  Il codice seguente assegna la variabile contenitore del costruttore
    alla variabile class’\_container solo se non è null. Se è null,
    generare un'eccezione ArgumentException.

> \_container = container ??
>
> throw new ArgumentException("Unable to connect to existing Azure
> Cosmos DB container or database."); 

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image35.jpeg)

**Attività 5 - Implementare il servizio Azure Cosmos DB per NoSQL**

Il servizio Azure Cosmos DB (CosmosDbService) gestisce l'esecuzione di
query, la creazione, l'eliminazione e l'aggiornamento di sessioni e
messaggi nell'applicazione dell'assistente di AI. Per gestire tutte
queste operazioni, il servizio deve implementare più metodi per ogni
potenziale operazione usando varie funzionalità di .NET SDK.

Ci sono diversi requisiti chiave da affrontare in questo esercizio:

- Implementare le operazioni per creare una sessione o un messaggio

- Implementare query per recuperare più sessioni o messaggi

- Implementare un'operazione per aggiornare una singola sessione o
  aggiornare in batch più messaggi

- Implementare un'operazione per eseguire query ed eliminare più
  sessioni e messaggi correlati

Azure Cosmos DB per NoSQL archivia i data in formato JSON, consentendo
di archiviare molti tipi di data in un unico contenitore. Questa
applicazione memorizza sia una "sessione" di chat con l'assistente AI
che i singoli "messaggi" all'interno di ogni sessione. Con l'API per
NoSQL, l'applicazione può archiviare entrambi i tipi di data nello
stesso contenitore e quindi distinguere tra questi tipi utilizzando un
semplice campo di tipo.

1.  Apri il file **Services/CosmosDbService.cs**.

2.  Il codice seguente crea una nuova variabile denominata partitionKey
    di tipo PartitionKey usando la proprietà SessionId della sessione
    corrente come parametro.

**PartitionKey partitionKey = new(session. SessionId);**

3.  Il codice seguente richiama il metodo CreateItemAsync del
    contenitore passando il parametro session e la variabile
    partitionKey. Restituisce la risposta come risultato del metodo
    InsertSessionAsync.

> Return await \_container. CreateItemAsync\<Session\>(
>
> item: session,
>
> partitionKey: partitionKey
>
> );

4.  Il codice seguente crea una variabile PartitionKey usando session.
    SessionId come valore della chiave di partizione. Crea una nuova
    variabile di messaggio denominata newMessage con la proprietà
    Timestamp aggiornata al timestamp UTC corrente. Richiamare
    CreateItemAsync passando sia il nuovo messaggio che le variabili
    della chiave di partizione. Restituisci la risposta come risultato
    di InsertMessageAsync.

> PartitionKey partitionKey = new(message. SessionId);
>
> Message newMessage = message with { TimeStamp = DateTime.UtcNow };
>
> return await \_container. CreateItemAsync\<Message\>(
>
> item: newMessage,
>
> partitionKey: partitionKey
>
> );

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image36.jpeg)

**Attività 6: Recuperare più sessioni o messaggi**

Esistono due casi d'uso principali in cui l'applicazione deve recuperare
più elementi dal contenitore. Innanzitutto, l'applicazione recupera
tutte le sessioni per l'utente corrente filtrando gli elementi in base a
quelli in cui type = Session. In secondo luogo, l'applicazione recupera
tutti i messaggi per una sessione eseguendo un filtro simile in cui type
= Session & sessionId = . Entrambe le query vengono implementate qui
usando .NET SDK e un iteratore di feed.

1.  Il codice seguente crea una nuova variabile denominata query di tipo
    QueryDefinition. Utilizza il metodo fluente WithParameter per
    assegnare il nome della classe Session come valore per il parametro.
    Richiama quindi il metodo generico GetItemQueryIterator\<\> sulla
    variabile \_container passando il tipo generico Session e la
    variabile di query come parametro. Archiviare il risultato in una
    variabile di tipo FeedIterator denominata response.

> QueryDefinition query = new QueryDefinition("SELECT DISTINCT \* FROM c
> WHERE c.type = @type")
>
> . WithParameter("@type", nameof(Session));
>
> FeedIterator\<Session\> response = \_container.
> GetItemQueryIterator\<Session\>(query);

2.  Il codice seguente all'interno del ciclo while, ottiene in modo
    asincrono la pagina successiva di risultati richiamando
    ReadNextAsync sulla variabile di risposta e quindi aggiunge tali
    risultati alla variabile di elenco denominata output. All'esterno
    del while loop, la variabile di output viene restituita con un
    elenco di sessioni come risultato del metodo GetSessionsAsync.

> FeedResponse\<Session\> results = await response. ReadNextAsync();
>
> output. AddRange(results);
>
> return output;

3.  Nel codice seguente viene utilizzato il metodo fluent WithParameter
    per assegnare il parametro @sessionId all'identificatore di sessione
    passato come parametro e il parametro @type al nome della classe
    Message.

> QueryDefinition query = new QueryDefinition("SELECT \* FROM c WHERE
> c.sessionId = @sessionId AND c.type = @type")
>
> . WithParameter("@sessionId", sessionId)
>
> . WithParameter("@type", nameof(Message));

4.  Creare un FeedIterator \<Message \> using the query variable and the
    GetItemQueryIterator\<\>method.

FeedIterator response = \_container.GetItemQueryIterator(query);

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image37.jpeg)

## Esercizio 4: Eseguire l'app

A questo punto l'applicazione dispone di un'implementazione completa di
Azure OpenAI e Azure Cosmos DB. È possibile testare l'applicazione
end-to-end eseguendo il debug della soluzione.

1.  Dal terminale di **Visual Studio Code,** compilare il progetto
    usando il comando seguente.

+++**dotnet build**+++

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image38.jpeg)

2.  Avviare l'applicazione con i ricarichi a caldo abilitati usando
    dotnet watch.

+++**dotnet watch run --non-interactive**+++

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image39.jpeg)

3.  Visual Studio Code avvia il browser semplice nello strumento con
    l'applicazione Web in esecuzione. Nell'applicazione web, crea una
    nuova sessione di chat facendo clic su **+ Create New Chat** e fai
    una domanda all'assistente AI. Quindi, chiudi l'applicazione Web in
    esecuzione.

![Uno screenshot di una chat I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image40.jpeg)

4.  Incolla il seguente testo nella casella di testo e fai clic sull'
    icona **Send**.

+++ How many wins does it take to promote to the Premier League?+++

![Uno screenshot di una chat I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image41.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image42.jpeg)

5.  Incolla il seguente testo nella casella di testo e fai clic sull'
    icona **Send**.

+++What is Azure OpenAI?+++

![Uno screenshot di una chat I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image43.jpeg)

![Uno screenshot di una chat I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image44.jpeg)

6.  Chiudere il terminale.

## Esercizio 5: Pulire il gruppo di risorse

1.  Aprire un nuovo browser e immettere l'URL seguente nella barra degli
    indirizzi: +++<https://portal.azure.com/+++> per aprire il di Azure
    Portal.

2.  Nella pagina Resource group, selezionare il **assigned Resource
    group**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image45.png)

3.  Selezionare tutte le **resources**, quindi selezionare **Delete**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image46.png)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image47.png)

4.  Digita +++**delete**+++ nella casella di testo e fai clic su
    **Delete**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image48.png)

![Uno screenshot di un errore del computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image49.png)

5.  Una notifica di esito positivo per le risorse eliminate conferma
    l'eliminazione.

6.  Dopo aver eliminato le risorse, nella home page del di Azure Portal,
    cercare **Azure AI Services** e selezionarlo.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image50.jpeg)

7.  Selezionare **Azure OpenAI** nel riquadro sinistro e quindi
    selezionare **Manage deleted Resources**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image51.jpeg)

8.  Seleziona la risorsa che viene elencata lì e quindi fai clic su
    **Purge**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image52.jpeg)

9.  Fare clic su **Yes**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image53.jpeg)

**Sommario**

Questo lab ha fornito una guida completa alla creazione, alla
distribuzione e al test di un'applicazione di chat personalizzata usando
Blazor, PostgreSQL e Azure OpenAI. In questo lab hai imparato a
configurare l'ambiente di sviluppo necessario, a creare e progettare
un'interfaccia di chat basata su Blazor, a configurare e connettere un
database PostgreSQL su Azure, a integrare Azure OpenAI per funzionalità
avanzate e infine a distribuire e testare l'applicazione su Azure.
Questa esperienza pratica ti ha fornito le competenze per sviluppare e
gestire moderne applicazioni web utilizzando tecnologie all'avanguardia
e servizi cloud.
