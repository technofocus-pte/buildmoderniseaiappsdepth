# Caso d'uso 07 - Abilitazione della ricerca semantica in Database di Azure per il server flessibile PostgreSQL per l'uso di Azure OpenAI per generare incorporamenti vettoriali.

**Obiettivo** :

In questo caso d'uso, si implementa la ricerca semantica per generare e
archiviare gli incorporamenti, installare il vettore e le estensioni
azure_ai in un server flessibile di Database di Azure per PostgreSQL,
quindi applicare le estensioni per archiviare i vettori di
incorporamento generati da Azure OpenAI

**Principali tecnologie utilizzate**: Azure OpenAI, Azure Database per
PostgreSQL, estensione Azure AI

**Durata stimata**: 45 minuti

**Tipo di laboratorio:** Guidato dall'istruttore

## Esercizio 1 :Generare incorporamenti vettoriali con Azure OpenAI

Per eseguire ricerche semantiche, è necessario prima generare vettori di
incorporamento da un modello, archiviarli in un database vettoriale e
quindi eseguire una query sugli incorporamenti. Si creerà un database,
lo si popolerà con di esempio ed eseguirai ricerche semantiche su tali
elenchi.

Al termine di questo esercizio, si avrà un'istanza del server flessibile
di Database di Azure per PostgreSQL con le estensioni vettore e azure_ai
abilitate. Genererai incorporamenti per la tabella degli annunci del set
di Open Data di Seattle Airbnb. Eseguirai anche ricerche semantiche su
questi elenchi generando il vettore di incorporamento di una query ed
eseguendo una ricerca della distanza del coseno vettoriale.

1.  Apri un browser Web e vai a ''https:\\portal.azure.com/'' e accedi
    con le tue credenziali Azure.

2.  Selezionare l' icona **Cloud Shell** nella barra degli strumenti del
    di Azure Portal per aprire un nuovo riquadro Cloud Shell nella parte
    inferiore della finestra del browser. Seleziona **Bash**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image1.jpeg)

3.  Selezionare il pulsante di opzione **No storage account required **,
    selezionare la sottoscrizione e quindi fare clic su **Apply**.

![](./media/image2.png)

4.  Al prompt di Cloud Shell, eseguire il comando seguente per clonare
    il progetto

''git clone https://github.com/technofocus-pte/postgresql-case''

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image3.jpeg)

5.  Passare alla cartella del progetto.

**''cd postgresql-case''**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image4.jpeg)

6.  Successivamente, si eseguono tre comandi per definire le variabili
    per ridurre la digitazione ridondante quando si usano i comandi
    dell'interfaccia della riga di comando di Azure per creare risorse
    di Azure. Le variabili rappresentano il nome da assegnare al gruppo
    di risorse (RG_NAME), l'area di Azure (REGION) in cui verranno
    distribuite le risorse e una password generata in modo casuale per
    l'account di accesso dell'amministratore PostgreSQL
    (ADMIN_PASSWORD).

7.  Nel primo comando, la regione assegnata alla variabile
    corrispondente è eastus o westus, **\[but you can replace it with a
    location of your preference.\]{.mark}** Tuttavia, se si sostituisce
    l'impostazione predefinita, è necessario selezionare un'altra \[area
    di Azure che supporta il riepilogo astratto\]{.underline} per
    assicurarsi di poter completare tutte le attività nei moduli in
    questo percorso di apprendimento.

''REGION=westus''

8.  Il comando seguente assegna il nome del gruppo di risorse esistente
    da utilizzare per il gruppo di risorse che ospiterà tutte le risorse
    utilizzate in questo esercizio.

> ''RG_NAME= Your existing resource group name ''

![](./media/image5.png)

9.  Il comando finale genera in modo casuale una password per l'accesso
    dell'amministratore di PostgreSQL. **Make sure you copy it** in un
    luogo sicuro per utilizzarlo in seguito per connetterti al suoserver
    flessibile PostgreSQL.

> a=()
>
> for i in {a.. z} {A.. Z} {0..9};
>
> do
>
> a\[$RANDOM\]=$i
>
> done
>
> ADMIN_PASSWORD=$(IFS=; echo "${a\[\*\]::18}")
>
> echo "Your randomly generated PostgreSQL admin user’s password is:"

echo $ADMIN_PASSWORD

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.jpeg)

### Attività 1: Assegnare un collaboratore di Servizi cognitivi

1.  Apri una nuova scheda e vai su ''**https://portal.azure.com''.**
    Accedere con le credenziali di Azure e quindi fare clic sul riquadro
    **Subscriptions**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image7.jpeg)

2.  Fare clic sul nome dell'abbonamento .

![](./media/image8.png)

3.  Fare clic su Access Control (IAM) dal menu di navigazione a
    sinistra. Fare clic su **Add** e selezionare **Add role
    assignment.**

![](./media/image9.png)

4.  Cercare \`\`Cognitive Services Contributor\`\`  e selezionarlo,
    quindi fare clic sul pulsante **Next.**

![Screenshot di un'assegnazione di servizio Descrizione generata
automaticamente](./media/image10.jpeg)

5.  Selezionare **User, group or service principal ** e fare clic sul
    collegamento **Select member.** Cercare l'account della di Azure
    subscription e selezionarlo. Infine, fai clic sul pulsante
    **Select**.

![](./media/image11.png)

6.  Fare clic sul pulsante **Review + Assign**.

![](./media/image12.png)

7.  Fare nuovamente clic sul pulsante **Review + Assign**.

> ![](./media/image13.png)

### Attività 2: Eseguire lo script di distribuzione Bicep per effettuare il provisioning delle risorse di Azure

1.  Tornare alla scheda 1 del di Azure Portal con l'interfaccia della
    riga di comando di Azure per eseguire uno script di distribuzione
    Bicep per effettuare il provisioning delle risorse di Azure nel
    gruppo di risorse: la distribuzione richiede 3 - 5 minuti

''cd''

''az deployment group create --resource-group $RG_NAME --template-file
"postgresql-case/Allfiles/Labs/Shared/deploy.bicep" --parameters
restore=false adminLogin=pgAdmin adminLoginPassword=$ADMIN_PASSWORD''

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image14.jpeg)

![Schermata del computer di una schermata nera Descrizione generata
automaticamente](./media/image15.jpeg)

2.  Lo script di distribuzione Bicep effettua il provisioning dei
    servizi di Azure necessari per completare questo esercizio nel
    gruppo di risorse. Le risorse distribuite includono un database di
    Azure per PostgreSQL - Server flessibile. È possibile controllare le
    risorse nel gruppo di risorse.

- **Azure OpenAI,**

- **Azure AI Language service.**

![](./media/image16.png)

3.  Fare clic su Open AI resource

![](./media/image17.png)

4.  Fare clic su **Keys and Endpoint** in **Resource Management** dal
    menu di navigazione a sinistra. Prendere nota della chiave 1 e del
    punto finale per utilizzarli nell'attività 5

![](./media/image18.png)

5.  Lo script Bicep esegue anche alcuni passaggi di configurazione, ad
    esempio l'aggiunta delle estensioni azure_ai e vettoriali all'elenco
    consentiti del server PostgreSQL (tramite il parametro del server
    azure.extensions), la creazione di un database denominato rentals
    nel server e l'aggiunta di una distribuzione denominata embedding
    usando il modello **text-embedding-ada-002** al servizio Azure
    OpenAI.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image19.jpeg)

6.  Il completamento della deployment richiede in genere alcuni minuti.
    È possibile monitorarlo da Cloud Shell o passare alla pagina
    **Deployments** per il di resource group creato in precedenza e
    osservare lo stato di avanzamento della distribuzione.

7.  Chiudere il riquadro Cloud Shell al termine della deployment delle
    risorse.

### Attività 3: Connettersi al database usando psql in Azure Cloud Shell

In questa attività ci si connette al database degli affitti nel server
Database di Azure per PostgreSQL usando l'utilità della riga di comando
psql di Azure Cloud Shell.

1.  Nel di Azure portal (https://portal.azure.com/) passare al database
    di Azure per PostgreSQL - Server flessibile appena creato.

![](./media/image20.png)

1.  Nella barra laterale, seleziona **Server Parameters**. Cercare il
    parametro **azure.extensions**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image21.png)

2.  Seleziona le estensioni **Vector** e **AZURE_AI** se non sono già
    selezionate.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image22.png)

2.  Nel menu delle risorse, in **Settings** , seleziona **Database** e
    seleziona **Connect** per il database degli affitti.

![](./media/image23.png)

3.  Al prompt "Password for user pgAdmin" in Cloud Shell, immettere la
    password generata in modo casuale per l' accesso **pgAdmin**.

Una volta effettuato l'accesso, viene visualizzato il prompt psql per il
database degli affitti.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image24.jpeg)

4.  Per il resto di questo esercizio, si continua a lavorare in Cloud
    Shell, quindi può essere utile espandere il riquadro all'interno
    della finestra del browser selezionando il pulsante **Maximize**
    nella parte superiore destra del riquadro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image25.jpeg)

### Attività 4 : Configurare le estensioni

Per archiviare ed eseguire query sui vettori e generare incorporamenti,
è necessario inserire nell'elenco Consentiti e abilitare due estensioni
per il server flessibile di Database di Azure per PostgreSQL: vettore e
azure_ai.

1.  Tornare alla scheda portale di Azure con l'interfaccia della riga di
    comando di Azure ed eseguire il comando SQL seguente per abilitare
    l'estensione vettoriale. Per istruzioni dettagliate

''CREATE EXTENSION vector;''

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image26.jpeg)

3.  Per abilitare l'estensione azure_ai, **update and run** il seguente
    comando SQL. Saranno necessari l'endpoint e la chiave API per la
    risorsa Azure OpenAI.

> “ CREATE EXTENSION azure_ai;''
>
> ''SELEZIONE azure_ai.set_setting('azure_openai.endpoint',
> 'https://\<endpoint\>.openai.azure.com');''

''SELEZIONA azure_ai.set_setting('azure_openai.subscription_key', '\<API
Key\>');''

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image27.jpeg)

### Attività 5 : Popolare il database con di esempio

Prima di esplorare l'estensione azure_ai, aggiungi un paio di tabelle al
database degli affitti e popolale con di esempio in modo da avere
informazioni su cui lavorare mentre esamini le funzionalità
dell'estensione.

1.  Esegui i seguenti comandi per creare le tabelle degli annunci e
    delle recensioni per l'archiviazione dei degli annunci di proprietà
    in affitto e delle recensioni dei clienti:

DROP TABLE IF EXISTS listings;

CREATE TABLE listings(

id int,

name varchar(100),

description text,

property_type varchar(25),

room_type varchar(30),

price numeric,

weekly_price numeric

);

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image28.jpeg)

DROP TABLE IF EXISTS reviews ;

CREATE TABLE reviews (

id int,

listing_id int,

date date,

comments text

);

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image29.jpeg)

2.  Quindi, utilizza il comando COPY per caricare i dai file CSV in ogni
    tabella creata in precedenza. Per iniziare, eseguire il comando
    seguente per popolare la tabella delle inserzioni:

''\COPY listings FROM
'postgresql-case/Allfiles/Labs/Shared/listings.csv' CSV HEADER''

L'output del comando dovrebbe essere COPY 50, a indicare che 50 righe
sono state scritte nella tabella dal file CSV.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image30.jpeg)

3.  Infine, esegui il comando seguente per caricare le recensioni dei
    clienti nella tabella delle recensioni:

''\COPY reviews FROM ‘postgresql-case/Allfiles/Labs/Shared/reviews.csv'
CSV HEADER''

L'output del comando dovrebbe essere COPY 354, a indicare che 354 righe
sono state scritte nella tabella dal file CSV.

![](./media/image31.jpeg)

4.  Per reimpostare i di esempio, puoi eseguire gli elenchi DROP TABLE e
    ripetere questi passaggi.

### Attività 6 : Creare e memorizzare vettori di incorporamento

Ora che abbiamo alcuni di esempio, è il momento di generare e
memorizzare i vettori di incorporamento. L'estensione azure_ai
semplifica la chiamata all'API di incorporamento di Azure OpenAI.

1.  Aggiungere la colonna vettoriale di incorporamento.

Il modello text-embedding-ada-002 è configurato per restituire 1.536
dimensioni, quindi utilizzarlo per le dimensioni della colonna
vettoriale.

''ALTER TABLE listings ADD COLUMN listing_vector vector(1536);''

![Schermata del computer di una schermata nera Descrizione generata
automaticamente](./media/image32.jpeg)

2.  Generare un vettore di incorporamento per la descrizione di ogni
    elenco chiamando Azure OpenAI tramite la funzione create_embeddings
    definita dall'utente, implementata dall'estensione azure_ai:

> UPDATE listings SET listing_vector = azure_openai.create_embeddings
> ('embedding', description, max_attempts =\> 5, retry_delay_ms =\> 500)
> WHERE listing_vector IS NULL;

Si noti che l'operazione potrebbe richiedere alcuni minuti, a seconda
della quota disponibile.

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente](./media/image33.png)

### Attività 7 : Esecuzione di una query di ricerca semantica

Ora che i delle inserzioni sono aumentati con i vettori di
incorporamento, è il momento di eseguire una query di ricerca semantica.
A tale scopo, ottenere il vettore di incorporamento della stringa di
query, quindi eseguire una ricerca del coseno per trovare gli elenchi le
cui descrizioni sono semanticamente più simili alla query.

1.  Usa l'incorporamento in una ricerca del coseno (\\=\> rappresenta
    l'operazione di distanza del coseno), recuperando i primi 10 elenchi
    più simili alla query.

SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;

Otterrai un risultato simile a questo. I risultati possono variare,
poiché non è garantito che i vettori di incorporamento siano
deterministici:

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image34.jpeg)

2.  È inoltre possibile proiettare la colonna della descrizione in modo
    che sia in grado di leggere il testo delle righe corrispondenti le
    cui descrizioni erano semanticamente simili. Ad esempio, questa
    query restituisce la corrispondenza migliore:

SELECT id, description FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 1;

Che stampa qualcosa come:

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image35.jpeg)

Per comprendere intuitivamente la ricerca semantica, si noti che la
descrizione non contiene effettivamente i termini "luminoso" o
"naturale". Ma mette in risalto "l'estate" e la "luce del sole", le
"finestre" e una "finestra sul soffitto".

### Compito 8 : Controlla il suolavoro

Dopo aver eseguito i passaggi precedenti, la tabella degli annunci
contiene di esempio da Seattle Airbnb Open Data su Kaggle. Gli elenchi
sono stati aumentati con vettori di incorporamento per eseguire ricerche
semantiche.

1.  Verifica che la tabella delle inserzioni abbia quattro colonne: id,
    nome, descrizione e listing_vector.

''\d listingsi''

Dovrebbe stampare qualcosa del tipo:

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image36.jpeg)

2.  Verificare che almeno una riga disponga di una colonna
    listing_vector popolata.

''SELECT COUNT(\*) \> 0 from listings WHERE listing_vector IS NOT
NULL;''

Il risultato deve mostrare una t, che significa vero. Indica che è
presente almeno una riga con incorporamenti della colonna di descrizione
corrispondente:

![Schermata di un computer Descrizione generata
automaticamente](./media/image37.jpeg)

3.  Verificare che il vettore di incorporamento abbia 1536 dimensioni:

''SELECT vector_dims(listing_vector) FROM listings WHERE listing_vector
IS NOT NULL LIMIT 1;''

Docile:

![Schermata di un computer Descrizione generata
automaticamente](./media/image38.jpeg)

4.  Verificare che le ricerche semantiche restituiscano risultati.

Usa l'incorporamento in una ricerca del coseno, recuperando i primi 10
elenchi più simili alla query.

''SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;''

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image39.jpeg)

5.  Torna alla stessa pagina per procedere con l'attività successiva.

## Esercizio 2 - Creare una funzione di ricerca per un sistema di raccomandazione

Avvolgiamo la logica di incorporamento vettoriale e le chiamate API in
una funzione. In questo esercizio si installano le estensioni vettoriale
e azure_ai in un server flessibile di Database di Azure per PostgreSQL
ed esploriamo le funzionalità dell'estensione per l'integrazione [di
Azure
OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/overview)
nel database.

### Compito 1 : Creare una funzione di ricerca per un sistema di raccomandazione

Costruiamo un sistema di raccomandazione utilizzando la ricerca
semantica. Il sistema consiglierà diverse inserzioni in base a un elenco
di esempio fornito. Il campione potrebbe provenire dall'elenco che
l'utente sta visualizzando o dalle sue preferenze. Implementeremo il
sistema come funzione PostgreSQL sfruttando l'estensione azure_openai.

Al termine di questo esercizio, avrai definito un recommend_listing di
funzione che fornisce al massimo elenchi numResults più simili a
sampleListingId fornito. Puoi utilizzare questi per promuovere nuove
opportunità, ad esempio unendo le inserzioni consigliate a quelle
scontate.

Distribuire le risorse nella di Azure subscription

Questo passaggio illustra l'uso dei comandi dell'interfaccia della riga
di comando di Azure da Azure Cloud Shell per creare un gruppo di risorse
ed eseguire uno script Bicep per distribuire i servizi di Azure
necessari per completare questo esercizio nella sottoscrizione di Azure.

**Nota:** Se si eseguono più moduli in questo percorso di apprendimento,
è possibile condividere l'ambiente Azure tra di essi. In tal caso, è
necessario completare questo passaggio di distribuzione delle risorse
una sola volta.

### Attività 2 : Creare la funzione di raccomandazione

1.  La funzione di raccomandazione accetta un sampleListingId e
    restituisce il numResults più simile ad altri elenchi. A tale scopo,
    crea un incorporamento del nome e della descrizione dell'elenco di
    esempio ed esegue una ricerca semantica di tale vettore di query
    rispetto agli incorporamenti dell'elenco.

> CREA FUNZIONE
>
> recommend_listing(sampleListingId int, numResults int)
>
> RETURNS TABLE(
>
> out_listingName text,
>
> out_listingDescription text,
>
> out_score reale)
>
> AS $$
>
> DECLARE
>
> queryEmbedding vector(1536);
>
> sampleListingText text;
>
> BEGIN
>
> sampleListingText := (
>
> SELECT
>
> name || ' ' || description
>
> FROM
>
> listings WHERE id = sampleListingId
>
> );
>
> queryEmbedding := (
>
> azure_openai.create_embeddings('embedding', sampleListingText,
> max_attempts =\> 5, retry_delay_ms =\> 500)
>
> );
>
> RETURN QUERY
>
> SELECT
>
> name::text,
>
> description,
>
> -- cosine distance:
>
> (listings.listing_vector \<=\> queryEmbedding)::real AS score
>
> FROM
>
> listings
>
> ORDER BY score ASC LIMIT numResults;
>
> END $$
>
> LANGUAGE plpgsql;

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image40.jpeg)

### Attività 3 : Interrogare la funzione di raccomandazione

1.  Per interrogare la funzione di raccomandazione, passale un ID di
    inserzione e il numero di raccomandazioni che deve fornire.

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommenon closest
to a listing

Il risultato sarà qualcosa del tipo:

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image41.jpeg)

2.  Per visualizzare il runtime della funzione, assicurarsi che
    **track_functions** sia abilitato nella sezione **Server
    Parameters** nel portale di Azure (è possibile usare PL o ALL):

![](./media/image42.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image43.png)

### Compito 4 : Controlla il suolavoro

1.  Assicurati che la funzione esista con la firma corretta:

''\df recommend_listing''

Dovrebbe essere visualizzato quanto segue:

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image44.jpeg)

2.  Assicurarsi di poter eseguire una query utilizzando la query
    seguente:

select out_listingName, out_score from recommend_listing( (SELECT id
from listing limit 1), 20); -- search for 20 listing recommenons closet
to a listing

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image45.jpeg)

### Compito 5 : Pulizia

Al termine di questo esercizio, eliminare le risorse di Azure create.
Viene addebitato il costo della capacità configurata, non la quantità di
utilizzo del database. Seguire queste istruzioni per eliminare il gruppo
di risorse e tutte le risorse create per questo lab.

1.  Nella home page cercare **Azure Open AI** e selezionarlo.

![](./media/image46.png)

2.  Seleziona la risorsa Apri AI e quindi fai clic su **Delete** .

![](./media/image47.png)

3.  Digita **delete** nella casella di testo e quindi fai clic su
    **\*\*Delete.** Confermare l'eliminazione.

![](./media/image48.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image49.png)

4.  Fare clic su **Manage deleted resources**, selezionare la risorsa e
    quindi fare clic sul pulsante **Purge** come mostrato nell'immagine
    sottostante.

![](./media/image50.png)

5.  Confermare l'eliminazione facendo clic su **Yes**.

![](./media/image51.png)

6.  Nella home page selezionare **di Resource Groups** in Servizi di
    Azure.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image52.jpeg)

7.  Fare clic su di Resource group name.

![](./media/image53.png)

8.  Nella pagina **Overview** del gruppo di risorse selezionare **all
    the resource** e quindi fare clic su **Delete . DO NOT delete
    RESOURCE Group.**

> ![](./media/image54.png)

9.  Digita **Delete** e fai clic su Elimina. Confermare l'eliminazione
    della risorsa facendo clic sul pulsante **Delete**.

![](./media/image55.png)

**Sommario**:

Si è appreso come usare la ricerca semantica in Database di Azure per il
server flessibile PostgreSQL per eseguire query usando gli
incorporamenti generati da Azure OpenAI. La ricerca è stata effettuata:

- Abilitazione delle estensioni vettoriale e azure_ai.

- Creazione di colonne vettoriali per memorizzare gli incorporamenti.

- Generazione e memorizzazione degli incorporamenti.

- Interrogazione del database utilizzando un vettore di query.
