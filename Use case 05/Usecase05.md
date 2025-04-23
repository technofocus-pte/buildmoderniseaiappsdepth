# Caso d'uso 05 - Distribuzione di un'app Web Python Restaurant basata sui data con il database di Azure per PostgreSQL

**Obiettivo:**

Questo caso d'uso distribuisce un'app Web Python usando il framework
Flask e il servizio di database relazionale Database di Azure per
PostgreSQL. L'app Flask è ospitata in un servizio app di Azure
completamente gestito. Questa app è progettata per essere eseguita in
locale e quindi distribuita in Azure

![Schema di un piano di assistenza Descrizione generata
automaticamente](./media/image1.jpeg)

Si distribuirà un'app Web Python basata sui data (**Django** o
**Flask**) nel **Azure App Service** con il servizio di database
relazionale **Azure Database for PostgreSQL**. Il servizio app di Azure
supporta Python in un ambiente server Linux.

**Principali tecnologie utilizzate**: Java 17, Database di Azure per
PostgreSQL

**Durata stimata**: 45 minuti

**Lab Type:** Guidato dall'istruttore

**Prerequisiti:**

Account GitHub: è necessario che tu abbia le tue credenziali di accesso
a GitHub. Se non ne hai, creane uno da qui -
**https://github.com/signup?user_email=&source=form-home-signupobjectives**

Il **requirements.txt** include i seguenti pacchetti, tutti utilizzati
da una tipica applicazione Flask basata sui data:

[TABLE]

### Compito 1 : Registrare il fornitore di servizi

1.  Aprire un browser e passare a <https://portal.azure.com> e accedere
    con l'account Cloud Slice disponibile nella scheda Risorse della VM.

> ![](./media/image2.png)

2.  Nella home page del portale di Azure fare clic sul riquadro
    **Resources groups**.

![](./media/image3.png)

3.  Copiare il nome del gruppo di risorse e salvarlo nel notepad per
    usare l'attività successiva per distribuire le risorse necessarie in
    questo **resource group.**

![](./media/image4.png)

4.  Nella navigazione in alto , fai clic su **Home**.

![](./media/image5.png)

5.  Fare clic sul riquadro **Subscriptions**.

![](./media/image6.png)

6.  Fare clic sul **subscription name**.

![](./media/image7.png)

7.  Espandi Impostazioni dal menu di navigazione a sinistra. Fare clic
    su **Resource providers** , immettere Microsoft.AlertsManagement e
    selezionarlo, quindi fare clic su **Register**.

> ![](./media/image8.png)
>
> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image9.png)

### Attività 2: Creare lo spazio di codice Github per avviare il modello dell'interfaccia della riga di comando di Azure Developer

Questo caso d'uso ha una configurazione del contenitore di sviluppo, che
semplifica lo sviluppo di app in locale, la distribuzione in Azure e il
monitoraggio. Usiamo i modelli dell'interfaccia della riga di comando di
sviluppo di Azure per distribuire le app

1.  Apri un browser e vai su **''https:\\github.com''** e accedi con il
    suo account Github.

2.  Esegui il fork di questo repository
    https://github.com/technofocus-pte/msdocs-flask-postgresql-sample-app
    sul suo account facendo clic su **Fork** come mostrato nell'immagine
    sottostante.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image10.jpeg)

3.  Inserisci un nome univoco e quindi fai clic su **Create repo**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image11.jpeg)

4.  Dalla radice del repository del fork, seleziona **Code** \>
    **Codespaces** \> **+**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image12.jpeg)

5.  Attendi la configurazione dell'area di lavoro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image13.jpeg)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image14.jpeg)

6.  Nel terminale dello codespace, esegui i seguenti comandi:

> \# Install requirements

''python3 -m pip install -r requirements.txt''

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image15.jpeg)

7.  Esegui il comando seguente per creare la variabile d'ambiente

> \# Create .env with environment variables

''cp .env.sample.devcontainer .env''

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image16.jpeg)

8.  Esegui il comando seguente per la migrazione dei data

> \# Run database migrations 

''python3 -m flask db upgrade''

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image17.jpeg)

9.  Esegui sotto il comando per

> \# Start the development server

''python3 -m flask run''

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image18.jpeg)

10. Quando viene visualizzato il messaggio L'applicazione in esecuzione
    sulla porta è disponibile.,fare clic su **Open in Browser**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image18.jpeg)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image19.jpeg)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image20.jpeg)

11. Fare clic sul pulsante **Add new restaurant**.

![Una schermata bianca con testo nero Descrizione generata
automaticamente](./media/image21.jpeg)

12. Inserisci i dettagli qui sotto e fai clic sul pulsante **Submit**.

Name : **''Contoso Rica''**

Street address - **''3A, 8th cross, Ferns** **street, Singapore''**

Description - **''This is a medium to high priced restaurant in the city
shopping center”**

![Uno screenshot di un ristorante Descrizione generata
automaticamente](./media/image22.jpeg)

13. Fare clic sul pulsante **Add new review**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image23.jpeg)

14. Inserisci la tua recensione e poi clicca sul pulsante. **Save
    Changes**

**Your name : your name**

**Rating : your rating**

''Questo è un ristorante di prezzo medio-alto nel centro commerciale
della città. Il servizio era un po' confuso perché avevamo almeno 6
camerieri che venivano a chiederci le cose. Il cibo ha impiegato un po'
di tempo per arrivare. Avevamo 2 menu: uno indiano e uno tailandese. Il
tailandese è il 30% più economico, quindi siamo andata per alcuni
antipasti e curry rosso tailandese. Il cibo ha richiesto un po' di
tempo, ma ne è valsa la pena. Era delizioso e molto ben preparato. Nel
complesso, questo è un buon cibo".

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image24.jpeg)

![Una carta bianca con testo nero Descrizione generata
automaticamente](./media/image25.jpeg)

15. Aggiungi altre recensioni e nuovo ristorante con commenti.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image26.jpeg)

### Attività 3: Effettuare il provisioning della risorsa necessaria in Azure.

Questo progetto è progettato per funzionare bene con l'interfaccia della
riga di comando di Azure Developer, che semplifica lo sviluppo di app in
locale, la distribuzione in Azure e il monitoraggio.

1.  Torna alla scheda dello spazio di codice Github, esegui il comando
    seguente per inizializzare un nuovo ambiente azd:

''azd init''

![](./media/image27.jpeg)

2.  Verrà richiesto di specificare un nome di ambiente (ad esempio
    **flask-app**XXXX (XXXX può essere un numero univoco)), che verrà
    utilizzato in seguito nel nome delle risorse distribuite.

![](./media/image28.jpeg)

3.  Effettua il login se richiesto **''azd auth login''** .copia il
    codice e premi invio.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image29.jpeg)

4.  Immettere il codice e quindi accedere con le credenziali di Azure.

![Uno screenshot di un errore del computer Descrizione generata
automaticamente](./media/image30.jpeg)

![](./media/image31.jpeg)

![Uno screenshot di un errore del computer Descrizione generata
automaticamente](./media/image32.jpeg)

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image33.jpeg)

5.  Torna indietro alla scheda dello spazio di codice Gtihub ed esegui
    il comando seguente per eseguire il provisioning e distribuire tutte
    le risorse. Verrà richiesto di selezionare la sottoscrizione di
    Azure. Immettere **1** per selezionare l'abbonamento e premere
    Invio.

**''azd provision''**

![Schermata di un codice informatico Descrizione generata
automaticamente](./media/image34.png)

6.  Selezionare la località come **WestUS/eastus**. Effettuerà quindi il
    provisioning delle risorse nell'account e distribuirà il codice più
    recente. Se viene visualizzato un errore con la distribuzione, può
    essere utile modificare la posizione (ad esempio in "westus"),
    poiché potrebbero esserci vincoli di disponibilità per alcune
    risorse.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image35.png)

7.  Immettere il nome del gruppo di risorse dal portale di Azure
    (copiato nell'attività precedente) e premere Enter.

![](./media/image36.png)

8.  La distribuzione richiede **20-30 minutes**. È anche possibile
    controllare lo stato della distribuzione nel collegamento generato o
    **in Azure portal-\> Resource group-\> Deployments**.

![](./media/image37.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image38.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image39.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image40.png)

### Attività 4 : Distribuire l'applicazione da Github

1.  Eseguire il comando seguente per impostare la variabile di ambiente
    del gruppo di risorse.

''azd env set AZURE_RESOURCE_GROUP {Name of existing resource group}''

Nota : Sostituire {Name of existing resource group} con il nome del
gruppo di risorse disponibile nella sezione Risorse della VM.

![](./media/image41.png)

2.  Eseguire il comando seguente per distribuire tutte le risorse e
    attendere il completamento della distribuzione.

''azd deploy''

![](./media/image42.png)

3.  Fare clic sull **Endpoint URL** generato

![](./media/image43.png)

4.  Fare clic su **Open** per aprire il sito Web esterno.

![](./media/image44.png)

5.  L'app si apre in una nuova scheda.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image45.png)

### Attività 5 : Streaming dei log di diagnostica

Il servizio app di Azure acquisisce l'output di tutti i messaggi nella
console per facilitare la diagnosi dei problemi con l'applicazione.
L'app include istruzioni print() per dimostrare questa funzionalità,
come illustrato di seguito.

@app.route('/', methods=\['GET'\])

def index():

print(Request for index page received)

restaurants = Restaurant.query.all()

return render_template('index.html', restaurants=restaurants)

1.  Tornare al di **Azure portal - \>** di **Resource group** e fare
    clic su **App service**.

![](./media/image46.png)

2.  Nella pagina Servizio app. Nel menu a sinistra selezionare
    **Monitoring \> App Service logs.**

![](./media/image47.png)

3.  In **Application logging** verificare che l'opzione **File system**
    sia selezionata. Selezionalo se necessario. Nel menu in alto,
    seleziona **Save**.

![](./media/image48.png)

4.  Dal menu a sinistra, seleziona **Log stream**. Vengono visualizzati
    i log per l'app, inclusi i log della piattaforma e i log
    dall'interno del container.

![](./media/image49.png)

### Attività 6 : Pulizia delle risorse in Github.

1.  Torna a Github, fai clic su **repo -\> Code -\> Codespaces.**
    Seleziona il ramo corretto

![](./media/image50.png)

2.  Seleziona il ramo e fai clic su **Delete**.

![](./media/image51.png)

3.  Fare clic su **Delete**.

![](./media/image52.png)

4.  Tornare al di **Azure portal - \> Resource group.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image53.png)

5.  Seleziona tutte le risorse e quindi fai clic su **Delete** (NON
    eliminare il gruppo di risorse)

![](./media/image54.png)

6.  Immettere **''Delete''** e quindi fare clic su **Delete**.

![](./media/image55.png)

7.  Fare clic su **Delete** per confermare l'eliminazione.

![](./media/image56.png)
