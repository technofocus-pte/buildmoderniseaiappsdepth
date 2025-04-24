# Caso d'uso 04 - Creare un elenco di cose da fare ASP.NET'app, distribuirlo nel servizio app di Azure connettendosi al database SQL

**Durata stimata:** 40 minuti

**Tipo di laboratorio:** Guidato dall'istruttore

**Obiettivo:**

Il servizio app di Azure offre un servizio di hosting Web altamente
scalabile con patch automatiche. In questo lab si apprenderà come
distribuire un'app ASP.NET basata sui data nel servizio app e
connetterla al database SQL di Azure. Al termine, è presente un'app
ASP.NET in esecuzione in Azure e connessa al database SQL.

## Esercizio 0: Informazioni sulla macchina virtuale e sulle credenziali

In questa attività, identificheremo e comprenderemo le credenziali che
utilizzeremo in tutto il laboratorio.

1.  **Disposizioni** tab Tenere la guida del laboratorio con le
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

    - **Resource Group**: il **Resource group** assegnato all'utente.

\[! Vigile\] **Importante:** assicurarsi di creare tutte le risorse in
questo Resource Group

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image1.png)

3.  **Help** tab contiene le informazioni di supporto. Il **ID value** è
    del **Lab** **instance ID** che verrà utilizzato durante
    l'esecuzione del lab.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image2.png)

## Esercizio 1: Distribuzione di un'app ASP.NET in Azure con il Database SQL di Azure

### Attività 1: Configurare Visual Studio 2022 ed eseguire l'applicazione

1.  Dalla barra di Windows **Search** , digita +++**Visual studio**+++ e
    seleziona Visual Studio 2022. Se ti viene chiesto di accedere,
    continua con i passaggi 2 e 3 o continua dal passaggio 4.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image3.jpeg)

2.  Fare clic su **Sign In** e **Sign In** con il **Username** e la
    **password** nella sezione **User Credentials** nella scheda
    Resources della VM.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image4.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image5.png)

3.  Selezionare **Start Visual Studio**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image6.jpeg)

4.  Seleziona **Open a Local Folder**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image7.jpeg)

5.  Seleziona la cartella **webappwithsqldb** in **C:\Labfiles** e fai
    clic su **Select Folder**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image8.jpeg)

6.  Una volta aperta la cartella, fare doppio clic su
    **DotNetAppSqlDb.sln** da **Solution Explorer**.

**Nota:** Se Esplora soluzioni non viene aperto automaticamente, fare
clic su **View -\> Solution Explorer.**

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image9.jpeg)

7.  Fare clic su **Build** -\> **Build Solution**.

![Schermata del computer di una schermata nera I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image10.jpeg)

8.  Al termine della compilazione, selezionare **Debug -\> Start
    Debugging**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image11.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image12.jpeg)

9.  Si opens up browser con l' **Todos Web app** in esecuzione.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image13.jpeg)

10. Aggiungi alcuni elementi nell'app facendo clic su **Create New**
    come negli screenshot qui sotto.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image14.jpeg)

![Uno screenshot di un'applicazione I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image15.jpeg)

11. Aggiungi qualche altro elemento all'elenco.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image16.jpeg)

12. Da Visual Studio 2022, fare clic su **Debug -\> Stop Debugging**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image17.jpeg)

### Attività 2: Pubblicare ASP.NET'applicazione in Azure

1.  In **Solution Explorer** fare clic con il pulsante destro del mouse
    sul **DotNetAppSqlDb** project e scegliere **Publish**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image18.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image19.jpeg)

2.  Selezionare **Azure** e fare clic su **Next**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image20.jpeg)

3.  Selezionare **Azure App Service (Windows)** in **Which Azure service
    would you like to use to host your application?** e fare clic su
    **Next**.

![Uno screenshot di un'applicazione per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image21.jpeg)

4.  Nella finestra di dialogo Publish fare clic su **Sign in** e **Sign
    in** alla sottoscrizione di Azure, se non è già stato effettuato
    l'accesso.

**Nota:** se hai già effettuato l'accesso a un account Microsoft,
assicurati che l'account contenga la tua sottoscrizione di Azure. Se
l'account Microsoft connesso non dispone della sottoscrizione di Azure,
fare clic su di esso per aggiungere l'account corretto.

5.  Fare clic su **Create new** per creare un nuovo App Service.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image22.jpeg)

6.  Inserisci i dettagli seguenti.

[TABLE]

7.  Fai clic su **New** sul **Hosting Plan**.

8.  ![Uno screenshot di un computer I contenuti generati
    dall'intelligenza artificiale potrebbero non essere
    corretti.](./media/image23.png)

9.  Fare clic **New** sul **Hosting Plan** option e inserire i dettagli
    sottostanti e fare clic su **OK**.

[TABLE]

10. ![Uno screenshot di un computer I contenuti generati
    dall'intelligenza artificiale potrebbero non essere
    corretti.](./media/image24.png)

11. Fare clic su **Create** nella finestra App Service e attendere la
    creazione delle Resources di Azure.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image25.png)

12. La finestra di dialogo **Publish** mostra le risorse configurate.
    Fare clic su **Finish**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image26.png)

13. Fare clic su **Close**.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image27.jpeg)

14. Scorri verso il basso fino alla sezione Server Dependencies e fai
    clic sul **+** **sign** per aggiungere dipendenza.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image28.jpeg)

15. Selezionare **Azure SQL Database** nella pagina **Add dependency** e
    fare clic su **Next**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image29.jpeg)

16. Fare clic su **Create New** accanto a Database SQL nella finestra di
    dialogo **Connect to Azure SQL Database.**![Uno screenshot di un
    computer I contenuti generati dall'intelligenza artificiale
    potrebbero non essere corretti.](./media/image30.jpeg)

17. Nella finestra di dialogo **Azure SQL Database Create New** fare
    clic su **New** accanto al server di Database.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image31.png)

18. Compila i dettagli sottostanti e fai clic su **OK.**

[TABLE]

19. ![Uno screenshot di un computer I contenuti generati
    dall'intelligenza artificiale potrebbero non essere
    corretti.](./media/image32.png)

20. Fare clic su **Create** nella finestra di dialogo Create new.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image33.png)

### Attività 3: Configurare la connessione al Database

1.  Al termine della creazione delle risorse del database, fare clic su
    **Next**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image34.png)

2.  Immettere i dettagli seguenti nella finestra di dialogo **Connect to
    Azure SQL Database** e fare clic su **Finish**.

[TABLE]

3.  ![Uno screenshot di un computer I contenuti generati
    dall'intelligenza artificiale potrebbero non essere
    corretti.](./media/image35.jpeg)

4.  Fare clic su **Finish** dopo aver esaminato il **Summary of
    changes**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image36.png)

5.  Attendi il completamento della configurazione guidata e fai clic su
    **Close**. Il database SQL di Azure è ora **connected** all'app.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image37.jpeg)

6.  Dalla pagina Publish, fai clic su **Publish** nell'angolo in alto a
    destra.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image38.png)

**Nota:** ci vorranno circa 5 minuti

7.  Dopo aver distribuito l'app ASP.NET in Azure, viene avviato il
    browser predefinito con l'URL dell'app deployed. **Add a few to-do
    items**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image39.jpeg)

### Attività 4: Accedere al database in locale

Visual Studio consente di esplorare e gestire facilmente il nuovo
database in Azure in **SQL Server Object Explorer**. Il nuovo database
ha già aperto il firewall per l'app del App Service creata. Tuttavia,
per accedervi dal computer locale, (ad esempio da Visual Studio), è
necessario aprire un firewall per l'indirizzo IP pubblico del computer
locale. Se il provider di servizi Internet modifica l'indirizzo IP
pubblico, è necessario riconfigurare il firewall per accedere nuovamente
al database di Azure.

1.  Dal **View Menu** di Visual Studio 2022 selezionare **SQL Server
    Object Explorer**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image40.jpeg)

2.  Nella parte superiore di **SQL Server Object Explorer** fare clic
    sul pulsante **Add SQL Server**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image41.jpeg)

### Attività 5: Configurare la connessione al database

1.  Nella finestra di dialogo **Connect** espandere il **Azure** node.
    Tutte le istanze del Database SQL in Azure sono elencate qui.

2.  Selezionare il database creato in precedenza
    (**dotnetappsqldbdbserver98**). La connessione creata in precedenza
    viene compilata automaticamente nella parte inferiore.

3.  Digita la **password** dell'amministratore del database che hai
    creato in precedenza (+++**PassWord98**+++) e fai clic su
    **Connect**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image42.jpeg)

### 

### Attività 6: Consentire la connessione client dal computer

Viene visualizzata la finestra di dialogo Crea una nuova regola del
firewall. Per impostazione predefinita, un server consente solo le
connessioni ai database dai servizi di Azure, ad esempio l'app Azure.
Per connettersi al database dall'esterno di Azure, creare una regola del
firewall a livello di server. La regola del firewall consente
l'indirizzo IP pubblico del computer locale.

La finestra di dialogo è già compilata con l'indirizzo IP pubblico del
computer.

1.  Assicurati che l'opzione **Add my client IP** sia selezionata e fai
    clic su **OK**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image43.jpeg)

2.  Al termine della creazione dell'impostazione del firewall per
    l'istanza del database SQL, la connessione viene visualizzata in
    **SQL Server Object Explorer**.

3.  Espandi la tua **Connection \> Databases \> \< Your DATABASE \> \>
    Tables**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image44.jpeg)

4.  Fare clic con il pulsante destro del mouse sulla **Todoes** table e
    selezionare **View Data**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image45.jpeg)

5.  Visualizza il contenuto della tabella. I data aggiunti
    dall'interfaccia utente dell'app dovrebbero essere elencati qui.

![](./media/image46.jpeg)

## Esercizio 2: Aggiornare l'app con le migrazioni Code First

1.  In **Solution Explorer** aprire **Models\Todo.cs** nell'editor di
    codice. Aggiungi la seguente proprietà alla class **ToDo** come
    ultima riga (dopo il **public DateTime CreatedDate { get; set; }** )
    e fare clic su **Save**.

+++**public bool Done { get; set; }**+++

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image47.jpeg)

### Attività 1: Eseguire le migrazioni Code First in locale

Esegui alcuni comandi per apportare aggiornamenti al database locale.

1.  Dal menu **Tools** fare clic su **NuGet Package Manager** \>
    **Package Manager Console**.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image48.jpeg)

2.  Nella finestra Package Manager Console, abilitare Code First
    Migrations eseguendo questo comando.

+++**Enable-Migrations**+++

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image49.jpeg)

3.  Aggiungere una migrazione eseguendo il comando seguente.

+++**Add-Migration** **AddProperty**+++

![](./media/image50.jpeg)

4.  Aggiorna il database locale eseguendo il comando seguente.

+++**Update-Database**+++

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image51.jpeg)

5.  Digita **CTRL+F5** per eseguire l'app o fai clic su **Debug -\>
    Start without Debugging**. Verifica la modifica, i dettagli e crea i
    collegamenti.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image52.jpeg)

6.  Viene visualizzata la pagina dell'applicazione che ha ancora lo
    stesso aspetto perché la logica dell'applicazione non utilizza
    ancora questa nuova proprietà.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image53.jpeg)

### Attività 2: Utilizzare la nuova proprietà

Apportare alcune modifiche al codice per utilizzare la proprietà Done.

1.  Da Visual Studio aprire **Controllers\TodosController.cs**. Trova il
    metodo **Create()** alla riga 52 e aggiungi +++**Done**+++
    all'elenco delle proprietà nell'attributo Bind. Al termine, la firma
    del metodo Create() è simile al seguente codice:

![Schermata di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image54.jpeg)

2.  Aprire **Views\Todos\Create.cshtml**. Aggiungi il seguente codice
    dopo il \< div class="form-group" \> for **CreatedDate**.

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image55.jpeg)

\<div class="form-group"\>

@Html.LabelFor(model =\> model.Done, htmlAttributes: new { @class =
"control-label col-md-2" })

\<div class="col-md-10"\>

\<div class="checkbox"\>

@Html.EditorFor(model =\> model.Done)

@Html.ValidataonMessageFor(model =\> model.Done, "", new { @class =
"text-danger" })

\</div\>

\</div\>

\`\`\`

3.  Aprire **Views\Todos\Index.cshtml**. Aggiungere il codice seguente
    nell' elemento **th** vuoto , dopo l' elemento **th** per
    **CreatedDate**.

<+++@Html.DisplayNameFor>(model =\> model.Done)+++

![Uno screenshot di un programma per computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image56.jpeg)

4.  Aggiungi questo codice appena sopra Metodi helper html.ActionLink().

5.  \<td\>

6.  @Html.DisplayFor(modelItem =\>item.Done)

\`\`\`

! \[\](./media/image53.jpeg)

5.  Digita **CTRL+F5** per eseguire l'app.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image57.jpeg)

### Attività 3: Abilitare le migrazioni Code First in Azure

1.  Fare clic con il pulsante destro del mouse sul progetto e
    selezionare **Publish**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image58.jpeg)

2.  Fai clic su **More actions** \> **Edit** per aprire le impostazioni
    di pubblicazione.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image59.jpeg)

3.  Nell'elenco a discesa **MyDatabaseContext** selezionare la
    connessione al database per il Azure SQL Database.

4.  Selezionare **Execute Code First Migrations** (viene eseguito
    all'avvio dell'applicazione), quindi fare clic su **Save**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image60.jpeg)

5.  Nella pagina Publish, fai clic su **Publish**.

![Un oggetto rettangolare nero con testo bianco I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image61.jpeg)

6.  L'app aggiornata è ora disponibile in Azure.

7.  Prova ad aggiungere di nuovo le cose da fare e seleziona **Done**, e
    dovrebbero apparire nella tua homepage come elementi completati.

![Uno screenshot di un'applicazione I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image62.jpeg)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image63.jpeg)

## 

## Esercizio 3: Trasmettere i log delle applicazioni

1.  Nella pagina di pubblicazione, scorri verso il basso fino alla
    sezione **Hosting**. Nell'angolo destro, fai clic su **...** \>
    **View Streaming Logs**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image64.jpeg)

2.  I log vengono ora trasmessi nella **Output Window**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image65.jpeg)

3.  Non viene ancora visualizzato alcun messaggio di traccia perché,
    quando si seleziona per la prima volta Visualizza Streaming Logs,
    l'app Azure imposta il livello di traccia su Errore, che registra
    solo gli eventi di errore.

\[! Nota\] **Nota:** Riavviare il flusso di registrazione da Visual
Studio se non vengono ancora visualizzati.

### Attività 1: Modificare i livelli di traccia

1.  Vai alla pagina di pubblicazione. Nella sezione Hosting, fai clic su
    **...\> Open in Azure Portal**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image66.jpeg)

2.  Nella pagina **Azure Portal** – App, selezionare **App Service
    Logs** nel riquadro sinistro nella sezione **Monitoring**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image67.jpeg)

3.  In **Application Logging** (File system), selezionare Livello
    **Verbose**. Fai clic su **Save**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image68.jpeg)

4.  Dal browser, accedere all'app Web in Azure ed eseguire alcune
    attività.

![Uno screenshot di un'applicazione I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image69.jpeg)

5.  I messaggi di traccia vengono ora trasmessi alla finestra Output in
    Visual Studio.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image70.jpeg)

6.  Per arrestare il servizio di streaming dei registri, fare clic sul
    pulsante **Stop monitoring** nella finestra **Output**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image71.jpeg)

7.  Chiudere Visual Studio.

## Esercizio 4: Pulire le risorse

1.  Dal Azure Portal, aprire il ResourceGroup assegnato.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image72.png)

2.  Seleziona tutte le Resources e clicca su **Delete**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image73.png)

3.  Digita +++**delete**+++ nella casella di testo e seleziona
    **Delete**. Seleziona **Delete** nella finestra di dialogo di
    conferma.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image74.png)

![Uno screenshot di un errore del computer I contenuti generati
dall'intelligenza artificiale potrebbero non essere
corretti.](./media/image75.png)

**Sommario**

In questo lab, si è appreso come distribuire un'app ASP.NET basata sui
data nel App Service e connetterla al Azure SQL Database.
