# Caso d'uso 06 - Distribuzione dell'app di chat in Azure Container Apps con server flessibile PostgreSQL

**Obiettivo:**

- Per configurare l'ambiente di sviluppo in Windows installando
  l'interfaccia della riga di comando di Azure, Node.js, assegnando
  ruoli di sottoscrizione di Azure, avviando Docker Desktop e abilitando
  Visual Studio Code con l'estensione Dev Containers.

- Per distribuire e testare l'applicazione di chat personalizzata con
  PostgreSQL e OpenAI in Azure.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image1.jpeg)

In questo caso d'uso, si configurerà un ambiente di sviluppo completo,
si distribuirà un'applicazione di chat integrata con PostgreSQL e si
verificherà la distribuzione in Azure. Ciò comporta l'installazione di
strumenti essenziali come l'interfaccia della riga di comando di Azure,
Docker e Visual Studio Code (l'abbiamo già fatto per te su host env), la
configurazione dei ruoli utente in Azure, la distribuzione
dell'applicazione utilizzando l'interfaccia della riga di comando di
Azure Developer e l'interazione con le risorse distribuite per garantire
la funzionalità.

**Principali tecnologie utilizzate**: Python, FastAPI, modelli Azure
OpenAI, Database di Azure per PostgreSQL e
azure-container-apps,ai-azd-templates.

**Durata stimata**: 45 minuti

**Tipo di laboratorio:** Guidato dall'istruttore

**Prerequisiti:**

Account GitHub: è necessario che tu abbia le tue credenziali di accesso
a GitHub. Se non ne hai, creane uno da qui -
**https://github.com/signup?user_email=&source=form-home-signupobjectives**

## Esercizio 1 : Provisioning, distribuzione dell'applicazione e test dal browser

### Attività 1: Copiare il nome del gruppo di risorse esistente

1.  Aprire il browser, aprire il portale di Azure
    ''https:\\portal.azure.com''.  Accedere con l'account Azure Slice
    (Credenziali di Azure***)*** disponibile nella sezione
    Istruzioni/Risorse dell'ambiente host.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image2.jpeg)

2.  Nella home page fare clic sul riquadro **Resource groups** .

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image3.png)

3.  Assicurarsi di avere già un gruppo di risorse creato per il lavoro.
    Non eliminare mai questo gruppo di risorse. È invece possibile
    eliminare le risorse all'interno del gruppo di risorse, ma non il
    gruppo di risorse stesso.

4.  Fare clic sul nome del gruppo di risorse

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image4.png)

5.  Copiare il nome del gruppo di risorse e salvarlo in Blocco note per
    usarlo per la distribuzione di tutte le risorse in **resource
    group**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image5.png)

### Attività 2 : Eseguire la finestra mobile

1.  Sul desktop, fare doppio clic su **Docker Desktop**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.jpeg)

2.  Eseguire il desktop Docker.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image7.jpeg)

### Compito 3 : Registrare il fornitore di servizi

1.  Tornare alla scheda Portale di Azure, fare clic sul riquadro
    **Subscription**.

![](./media/image8.png)

2.  Fare clic sul nome dell'abbonamento.

![](./media/image9.png)

3.  Fare clic su **Settings - \> Resource provider** dal menu di
    spostamento a sinistra.

![](./media/image10.png)

4.  Digita ''**Microsoft.AlertsManagement**'' e premi invio. Selezionalo
    e poi clicca su **Register**.

![](./media/image11.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image12.png)

### 

### Compito 4 : Ambiente di sviluppo aperto

1.  Apri il browser, vai alla barra degli indirizzi, digita o incolla il
    seguente URL: Si apre la scheda
    ''https://github.com/technofocus-pte/rag-postgres-openai-python.git''
    e ti chiede di aprirla nel codice di Visual Studio. Selezionare
    **Apri codice Visual Studio.**

![](./media/image13.jpeg)

2.  Fare clic sulla **forchetta** per biforcare il repository. Assegna
    un nome univoco al repository e fai clic sul **pulsante Crea
    repository**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image14.jpeg)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image15.jpeg)

3.  Clicca su **Code spaces -\> Codespaces+ Codespaces**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image16.jpeg)

4.  Attendi la configurazione dell'ambiente **Codespaces**. Ci vogliono
    pochi minuti per la configurazione completa

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image17.jpeg)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image18.jpeg)

### Attività 5: Effettuare il provisioning dei servizi e distribuire l'applicazione in Azure

1.  Esegui il seguente comando sul Terminale. Genera il codice da
    copiare. Copia il codice e premi **Enter**.

''azd auth login''

![](./media/image19.png)

2.  Si apre il browser predefinito per inserire il codice generato da
    verificare. Inserisci il codice e fai clic su **Next**.

![](./media/image20.png)

3.  Accedere con le credenziali di Azure.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image21.png)

4.  Per creare un ambiente per le risorse di Azure, eseguire il comando
    dell'interfaccia della riga di comando di Azure Developer seguente.
    Ti chiede di inserire il nome dell'ambiente . Inserisci un nome a
    tua scelta e premi invio (es. :**ragpgpy**)

**Nota:** Quando si crea un ambiente, assicurarsi che il nome sia
composto da lettere minuscole.

''azd env new''

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image22.png)

5.  Eseguire il comando dell'interfaccia della riga di comando di Azure
    Developer seguente per effettuare il provisioning delle risorse di
    Azure e distribuire il codice.

''azd provision'

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image23.png)

6.  Quando richiesto, selezionare una **subscription** per creare le
    risorse e selezionare l'area più vicina alla propria posizione. In
    questo lab è stata scelta l' area **East US2**.

![](./media/image24.png)

7.  Ti verrà chiesto "**Enter a value for the
    'existingResourceGroupName' infrastructure parameter:**" inserisci
    il gruppo di risorse copiato nell'attività 1 (ad esempio:
    **ResourceGroup1 used for the development slice).**È possibile
    copiare il nome del gruppo di risorse dalla sezione **Resources**,
    come illustrato nell'immagine seguente

> ![](./media/image25.png)

8.  Quando richiesto, **enter a value for the 'openAILocation'
    infrastructure parameter** seleziona la regione più vicina alla tua
    posizione; in questo laboratorio, abbiamo scelto la regione **North
    Central US.**

![](./media/image26.png)

9.  Il provisioning delle risorse richiederà circa 5-10 minuti. Fare
    clic su **Yes** se richiesto.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image27.png)

10. Attendere che il modello esegua correttamente il provisioning di
    tutte le risorse.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image28.png)

11. Esegui il comando seguente per impostare il **resource group**

''azd env set AZURE_RESOURCE_GROUP {your resource group name }''

![](./media/image29.png)

12. Eseguire il comando seguente per distribuire l'app in Azure.

''azd deploy''

![](./media/image30.png)

13. Attendere il completamento della distribuzione. La distribuzione
    richiede \<5

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image31.png)

14. Fare clic sul collegamento dell'endpoint dell'app Web distribuito.

![](./media/image32.png)

15. Fare clic su **Open**. Si apre una nuova scheda con l'app

![](./media/image33.png)

16. L'app si apre.

![Uno screenshot di una chat Descrizione generata
automaticamente](./media/image34.png)

### Attività 6: Utilizzare l'app di chat per ottenere risposte dai file

1.  Negli **RAG on database |**Pagina dell'app Web
    **OpenAI+PoastgreSQL**, **click on Best shoe for hiking?** pulsante
    e osservare l'uscita

![](./media/image35.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image36.png)

2.  Fai clic sulla **chat clear**

![](./media/image37.png)

3.  Negli **RAG on database |**Pagina dell'app Web **OpenAI +
    PoastgreSQL**, fare clic sul pulsante **Climbing gear cheaper than
    \\30** e osservare l'output

![](./media/image38.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image39.png)

4.  Fai clic sulla **clear** **chat.**

### Attività 7: Verificare le risorse distribuite nel portale di Azure

1.  Nella home page del portale di Azure fare clic su **Resource
    Groups.**

![](./media/image40.png)

2.  Fare clic sul nome del **resource name**

![](./media/image41.png)

3.  Assicurarsi che la risorsa seguente sia stata distribuita
    correttamente

    - App contenitore

    - Application Insights

    - Ambiente di app contenitore

    - Area di lavoro Log Analytics

    - Azure OpenAI

    - Server flessibile di Database di Azure per PostgreSQL

    - Registro contenitori

![](./media/image42.png)

4.  Fare clic su Nome risorsa **Azure OpenAI.**

![](./media/image43.png)

5.  In **Overview** nel menu di spostamento a sinistra fare clic su **Go
    to Azure AI Foundry Portal** e selezionare per aprire una nuova
    scheda.

![](./media/image44.png)

6.  Fare clic su **Shared resources -\> Deployments** dal menu di
    navigazione a sinistra e assicurarsi che **gpt-35-turbo**,
    **text-embedding-ada-002** venga distribuito correttamente

![](./media/image45.png)

### Compito 8 : Pulisci tutte le risorse

Per pulire tutte le risorse create da questo esempio:

1.  Tornare al di **Azure portal -\> Resource Group- \> Resource group
    name.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image46.png)

2.  Seleziona tutta la risorsa e quindi fai clic su Elimina come
    mostrato nell'immagine sottostante. (**DO NOT DELETE** Resource
    group)

![](./media/image47.png)

3.  Digita **''delete''** nella casella di testo e poi clicca su
    **Delete**.

![](./media/image48.png)

4.  Conferma l'eliminazione cliccando su **Delete**.

![](./media/image49.png)

5.  Torna alla scheda del portale Github e aggiorna la pagina.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image50.png)

1.  Clicca su Codice, seleziona il ramo creato per questo lab e clicca
    su **Delete**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image51.png)

2.  Confermare l'eliminazione del ramo facendo clic sul pulsante
    **Delete**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image52.png)
