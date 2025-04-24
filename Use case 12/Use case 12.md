# Anwendungsfall 12: Integrieren von generativen AI-Funktionen in Azure Database for PostgreSQL Flexible Server, um Überprüfungen bestimmter AI-Einträge auszuwerten

**Dauer des Labs -** 40 Minuten

**Labtyp –** von einem Kursleiter geleitet

**Einleitung**

In diesem Lab erfahren Sie, wie Sie Azure-AI-Dienste in PostgreSQL
integrieren, um Ihre Datenbank mit erweiterten AI-Funktionen zu
erweitern. Durch die Nutzung der Leistungsfähigkeit von Azure OpenAI-
und PostgreSQL-Erweiterungen wie pgvector und PostGIS ermöglichen Sie
ausgefeilte Textanalysen, Vektorähnlichkeitssuchen und räumliche
Abfragen direkt in Ihrer Datenbank. Dieses Lab führt Sie durch die
Bereitstellung der erforderlichen Azure-Ressourcen, die Konfiguration
Ihrer Datenbank und das Ausführen komplexer Abfragen, die AI-gesteuerte
Erkenntnisse mit Geodaten kombinieren.

**Ziele**

1.  Zum Bereitstellen und Konfigurieren von Azure Database for
    PostgreSQL Flexible Server.

2.  Zum Erstellen und Verwalten von Vektoreinbettungen mithilfe des
    Azure OpenAI-Diensts.

3.  Um Vektorähnlichkeitssuchen durchzuführen, um semantisch ähnliche
    Textdaten zu finden.

4.  Um die Erweiterung PostGIS für die Analyse von Geodaten zu nutzen.

&nbsp;

1.  Integrieren von Azure AI Language Services für die Meinungsanalyse
    und andere kognitive Funktionen.

- So optimieren und analysieren Sie die Abfrageleistung mithilfe von
  Indizierungs- und Abfrageplanungstools.

**Wichtig:** Wenn einer der Befehle nicht in **CloudShell** eingefügt
wird, öffnen Sie bitte einen Editor, halten Sie den Cursor an einer
leeren Stelle des Editors und klicken Sie dann auf die T-Schaltfläche
des einzufügenden Befehls. Der Inhalt wird in den Editor kopiert und Sie
können ihn dann aus dem Editor kopieren und in die CloudShell einfügen.

## Übung 0: Grundlegendes zum virtuellen Computer und den Anmeldeinformationen

In dieser Aufgabe identifizieren und verstehen wir die
Anmeldeinformationen, die wir im gesamten Lab verwenden werden.

1.  Auf der Registerkarte **Instructions** befindet sich der
    Laborleitfaden mit den Anweisungen, die im gesamten Labor zu
    befolgen sind.

2.  Die Registerkarte **Resources** enthält die Anmeldeinformationen,
    die zum Ausführen des Labs erforderlich sind.

    - **URL** – URL zum Azure-Portal

    - **Subscription** – Dies ist die ID des Abonnements, das Ihnen
      zugewiesen wurde

    - **Username** – Die Benutzer-ID, mit der Sie sich bei den
      Azure-Diensten anmelden müssen.

    - **Password** – Kennwort für die Azure-Anmeldung. Nennen wir diesen
      Benutzernamen und dieses Kennwort als Azure-Anmeldeinformationen.
      Wir werden diese Creds überall dort verwenden, wo wir
      Azure-Anmeldeinformationen erwähnen.

    - **Resource Group** – Die **Ressourcengruppe**, die Ihnen
      zugewiesen ist.

\[!Alert\] **Wichtig:** Stellen Sie sicher, dass Sie alle Ressourcen
unter dieser Ressourcengruppe erstellen.

![](./media/image1.png)

3.  Die Registerkarte **Help** enthält die Supportinformationen. Der
    **ID-Wert** ist hier die **Lab instance ID**, die während der
    Lab-Ausführung verwendet wird.

![](./media/image2.png)

## Übung 1: Bereitstellen eines flexiblen Azure Database for PostgreSQL-Servers

### Aufgabe 0: Registrieren von Ressourcenanbietern

1.  Melden Sie sich beim **Azure-Portal** an -
    +++[https://portal.azure.com+++](https://portal.azure.com+++/) mit
    Ihren Azure-Anmeldedaten.

2.  Wählen Sie **Subscriptions** und wählen Sie**Resource
    Providers** unter **Settings** aus dem linken Fensterbereich.

3.  Suchen Sie nach +++Microsoft.DBforPostgreSQL+++ und klicken Sie auf
    **Register**, um diesen Ressourcenanbieter zu registrieren.

![](./media/image3.png)

### Aufgabe 1: Bereitstellen eines flexiblen Azure Database for PostgreSQL-Servers

1.  Öffnen Sie einen Webbrowser und navigieren Sie zum
    +++[https://portal.azure.com+++](https://portal.azure.com+++/)

2.  Wählen Sie auf der Symbolleiste des Azure-Portals das **Cloud
    Shell-Symbol** aus, um oben im Browserfenster einen neuen Cloud
    Shell-Bereich zu öffnen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  Wenn Sie Cloud Shell zum ersten Mal öffnen, werden Sie
    möglicherweise aufgefordert, den Typ der Shell auszuwählen, die Sie
    verwenden möchten (**Bash** oder **PowerShell**). Wählen Sie
    **Bash**.

![](./media/image5.jpeg)

4.  Wählen Sie im Dialogfeld **Get Started** die Option **Mount storage
    account** und dann Ihr Azure-Abonnement aus. Klicken Sie auf die
    Schaltfläche Apply.

![](./media/image6.png)

5.  Wählen Sie im Dialogfeld **Mount storage account** die Option **we
    will create a storage account for you** aus, und klicken Sie auf die
    Schaltfläche Next.

![](./media/image7.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

6.  Führen Sie an der Cloud Shell-Eingabeaufforderung die folgenden
    Befehle aus, um Variablen zum Erstellen von Ressourcen zu
    definieren. Die Variablen stellen die Namen dar, die Ihrer
    Ressourcengruppe und Datenbank zugewiesen werden sollen, und geben
    die Azure-Region an, in der Ressourcen bereitgestellt werden sollen.

7.  Ersetzen Sie den Namen der Ressourcengruppe im folgenden Befehl
    durch die zugewiesene Ressourcengruppe, und führen Sie den Befehl
    aus.

+++RG_NAME= \< Resource group Name \>+++

![](./media/image9.png)

8.  Ersetzen Sie im Datenbanknamen das Token {SUFFIX} durch Ihre **Lab
    instance ID**, z. B. Ihre Initialen, um sicherzustellen, dass der
    Name des Datenbankservers global eindeutig ist.

+++DATABASE_NAME=<pgsql-flex-@lab.LabInstance.Id>+++

![](./media/image10.jpeg)

9.  Führen Sie den folgenden Befehl aus, um den Wert für Region
    festzulegen.

+++REGION=@lab.CloudResourceGroup(ResourceGroup1).Location+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

1.  Bereitstellen einer Azure Database for PostgreSQL-Datenbankinstanz
    innerhalb der zugewiesenen Ressourcengruppe, indem Sie den folgenden
    Azure CLI-Befehl ausführen (der Ausführung dieses Befehls dauert 10
    Minuten)

> \`\`\`
>
> az postgres flexible-server create --name $DATABASE_NAME --location
> $REGION --resource-group $RG_NAME \\
>
> --admin-user s2admin --admin-password Seattle123Seattle123
> --database-name airbnb \\
>
> --public-access 0.0.0.0-255.255.255.255 --version 16 \\
>
> --sku-name Standard_D2s_v3 --storage-size 32 --yes
>
> \`\`\`

![](./media/image12.jpeg)

### Aufgabe 2: Herstellen einer Verbindung mit der Datenbank mithilfe von psql in der Azure Cloud Shell

In dieser Aufgabe verwenden Sie das Befehlszeilenprogramm psql aus Azure
Cloud Shell, um eine Verbindung mit Ihrer Datenbank herzustellen.

1.  Öffnen Sie einen Browser, gehen Sie zu
    +++[https://portal.azure.com+++](https://portal.azure.com+++/) und
    melden Sie sich mit Ihrem Azure-Abonnementkonto an.

&nbsp;

1.  Klicken Sie auf der **Startseite** auf **Ressource Group**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.jpeg)

2.  Klicken Sie auf **your assigned resource group** name.

![](./media/image14.png)

1.  Wählen Sie in der Ressourcengruppe die Ressource **PostgreSQL
    Flexible Server** aus

![](./media/image15.png)

3.  Wählen Sie im linken Navigationsmenü unter **Settings** die Option
    **Connect** aus.

![](./media/image16.jpeg)

4.  Über die Seite **Connect** der Datenbank im Azure-Portal, Wählen Sie
    **airbnb** als **Database name** aus, Kopieren Sie dann den
    **Connection details,** und fügen Sie ihn in den Editor ein, um die
    Informationen in den anstehenden Aufgaben zu verwenden.

![](./media/image17.jpeg)

1.  Klicken Sie auf der Startseite von Azure Database for PostgresSQL im
    linken Navigationsmenü auf **Overview**, kopieren Sie den
    Servernamen, fügen Sie ihn in Editor ein, und **speichern** Sie dann
    den Editor, um die Informationen in der nächsten Übung zu verwenden.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.jpeg)

5.  Wählen Sie auf der Startseite von Azure Database for PostgreSQL
    unter **Settings** die Option **Networking** aus und wählen **Allow
    public access from any Azure service within Azure to this server**
    aus. Klicken Sie auf die Schaltfläche **Save**.

![](./media/image19.jpeg)

![](./media/image20.jpeg)

6.  Wählen Sie auf der Symbolleiste des Azure-Portals das **Cloud
    Shell-Symbol** aus, um oben im Browserfenster einen neuen Cloud
    Shell-Bereich zu öffnen.

7.  Fügen Sie **Connection details** in die Cloud Shell ein.

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image21.jpeg)

1.  Ersetzen Sie an der Cloud Shell-Eingabeaufforderung das Token
    **{your_password}** durch das Kennwort, das Sie dem Benutzer
    **s2admin** beim Erstellen Ihrer Datenbank zugewiesen haben. Das
    Kennwort sollte +++**Seattle123Seattle123**+++ lauten.

![](./media/image22.jpeg)

8.  Stellen Sie mithilfe des Befehlszeilenprogramms psql eine Verbindung
    zu Ihrer Datenbank her, indem Sie an der Eingabeaufforderung
    Folgendes eingeben:

+++psql+++

![](./media/image23.jpeg)

Um eine Verbindung mit der Datenbank über Cloud Shell herzustellen, muss
das Kontrollkästchen Öffentlichen Zugriff von jedem Azure-Dienst in
Azure auf den Server zulassen auf der **Seite Netzwerk** der Datenbank
aktiviert sein. Wenn Sie die Meldung erhalten, dass Sie keine Verbindung
herstellen können, überprüfen Sie bitte, ob diese Option aktiviert ist,
und versuchen Sie es erneut.

### Aufgabe 3: Hinzufügen von Daten zur Datenbank

Mit der psql-Eingabeaufforderung erstellen Sie Tabellen und füllen sie
mit Daten für die Verwendung im Lab.

1.  Führen Sie die folgenden Befehle aus, um temporäre Tabellen zum
    Importieren von JSON-Daten aus einem öffentlichen Blob Storage-Konto
    zu erstellen.

> CREATE TABLE temp_calendar (data jsonb);
>
> CREATE TABLE temp_listings (data jsonb);
>
> CREATE TABLE temp_reviews (data jsonb);

![](./media/image24.jpeg)

2.  Füllen Sie jede temporäre Tabelle mit dem Befehl COPY mit Daten aus
    JSON-Dateien in einem öffentlichen Speicherkonto auf.

+++\COPY temp_calendar (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/calendar.json'+++>

+++\COPY temp_listings (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/listings.json'+++>

+++\COPY temp_reviews (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/reviews.json'+++>

![](./media/image25.jpeg)

![](./media/image26.jpeg)

3.  Führen Sie den folgenden Befehl aus, um die Tabellen zum Speichern
    von Daten in der von diesem Lab verwendeten Form zu erstellen:

> CREATE TABLE listings (
>
> listing_id int,
>
> name varchar(50),
>
> street varchar(50),
>
> city varchar(50),
>
> state varchar(50),
>
> country varchar(50),
>
> zipcode varchar(50),
>
> bathrooms int,
>
> bedrooms int,
>
> latitude decimal(10,5),
>
> longitude decimal(10,5),
>
> summary varchar(2000),
>
> description varchar(2000),
>
> host_id varchar(2000),
>
> host_url varchar(2000),
>
> listing_url varchar(2000),
>
> room_type varchar(2000),
>
> amenities jsonb,
>
> host_verifications jsonb,
>
> data jsonb
>
> );

![](./media/image27.jpeg)

> CREATE TABLE reviews (
>
> id int,
>
> listing_id int,
>
> reviewer_id int,
>
> reviewer_name varchar(50),
>
> date date,
>
> comments varchar(2000)
>
> );
>
> CREATE TABLE calendar (
>
> listing_id int,
>
> date date,
>
> price decimal(10,2),
>
> available boolean
>
> );

![](./media/image28.jpeg)

4.  Führen Sie abschließend die folgenden **INSERT**
    **INTO**-Anweisungen aus, um Daten aus den temporären Tabellen in
    die Haupttabellen zu laden, indem Sie Daten aus dem JSON-Datenfeld
    in einzelne Spalten extrahieren:

> INSERT INTO listings
>
> SELECT
>
> data\['id'\]::int,
>
> replace(data\['name'\]::varchar(50), '"', ''),
>
> replace(data\['street'\]::varchar(50), '"', ''),
>
> replace(data\['city'\]::varchar(50), '"', ''),
>
> replace(data\['state'\]::varchar(50), '"', ''),
>
> replace(data\['country'\]::varchar(50), '"', ''),
>
> replace(data\['zipcode'\]::varchar(50), '"', ''),
>
> data\['bathrooms'\]::int,
>
> data\['bedrooms'\]::int,
>
> data\['latitude'\]::decimal(10,5),
>
> data\['longitude'\]::decimal(10,5),
>
> replace(data\['description'\]::varchar(2000), '"', ''),
>
> replace(data\['summary'\]::varchar(2000), '"', ''),
>
> replace(data\['host_id'\]::varchar(50), '"', ''),
>
> replace(data\['host_url'\]::varchar(50), '"', ''),
>
> replace(data\['listing_url'\]::varchar(50), '"', ''),
>
> replace(data\['room_type'\]::varchar(50), '"', ''),
>
> data\['amenities'\]::jsonb,
>
> data\['host_verifications'\]::jsonb,
>
> data::jsonb
>
> FROM temp_listings;
>
> INSERT INTO reviews
>
> SELECT
>
> data\['id'\]::int,
>
> data\['listing_id'\]::int,
>
> data\['reviewer_id'\]::int,
>
> replace(data\['reviewer_name'\]::varchar(50), '"', ''),
>
> to_date(replace(data\['date'\]::varchar(50), '"', ''), 'YYYY-MM-DD'),
>
> replace(data\['comments'\]::varchar(2000), '"', '')
>
> FROM temp_reviews;
>
> INSERT INTO calendar
>
> SELECT
>
> data\['listing_id'\]::int,
>
> to_date(replace(data\['date'\]::varchar(50), '"', ''), 'YYYY-MM-DD'),
>
> data\['price'\]::decimal(10,2),
>
> replace(data\['available'\]::varchar(50), '"', '')::boolean
>
> FROM temp_calendar;

![](./media/image29.jpeg)

## Übung 2: Hinzufügen von Azure AI- und Vector-Erweiterungen zur Zulassungsliste

In dieser Übung verwenden Sie die Erweiterungen azure_ai und pgvector,
um Ihrer PostgreSQL-Datenbank generative AI-Funktionen hinzuzufügen. In
dieser Übung fügen Sie diese Erweiterungen der *Zulassungsliste Ihres
Servers* hinzu, wie unter Verwenden von PostgreSQL-Erweiterungen
beschrieben.

1.  Klicken Sie auf der Startseite auf **Resource Groups**.

![](./media/image30.jpeg)

1.  Klicken Sie auf den Namen Ihrer Ressourcengruppe.

![](./media/image14.png)

2.  Wählen Sie in der Ressourcengruppe **PostgreSQL Flexible
    Server** Ressource

![](./media/image15.png)

1.  Wählen Sie im linken Navigationsmenü der Datenbank die Option
    **Server parameters** unter **Settings**, geben Sie dann
    +++**azure.extensions**+++ in das Suchfeld ein. Erweitern Sie die
    Dropdown-Liste **VALUE**, suchen und aktivieren Sie das
    Kontrollkästchen neben jeder der folgenden Erweiterungen:

    - AZURE_AI

    - POSTGIS

    - VECTOR

![](./media/image31.jpeg)

![](./media/image32.jpeg)

![](./media/image33.jpeg)

3.  Wählen Sie auf der Symbolleiste Save aus, um eine Bereitstellung in
    der Datenbank auszulösen.

![](./media/image34.jpeg)

## Übung 3: Erstellen einer Azure OpenAI-Ressource

Für die azure_ai Erweiterung ist ein zugrunde liegender Azure
OpenAI-Dienst erforderlich, um Vektoreinbettungen zu erstellen. In
dieser Übung stellen Sie eine Azure OpenAI-Ressource im Azure-Portal
bereit und stellen ein Einbettungsmodell in diesen Dienst bereit.

### Aufgabe 1: Bereitstellen eines Azure OpenAI-Diensts

In dieser Aufgabe erstellen Sie einen neuen Azure OpenAI-Dienst.

1.  Klicken Sie auf der Startseite des Azure-Portals auf das
    **Menu** **Azure-Portals,** das durch drei horizontale Balken auf
    der linken Seite der Microsoft Azure-Befehlsleiste dargestellt wird,
    wie in der folgenden Abbildung gezeigt.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.jpeg)

2.  Navigieren Sie und klicken Sie auf**+ Create a resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

3.  Auf **Create a resource** Seite, in der **Search services and
    marketplace** Suchleiste, schreiben Sie +++**Azure OpenAI**+++, und
    drücken Sie dann **Enter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

4.  Navigieren Sie auf der Seite **Marketplace** zum Abschnitt **Azure
    OpenAI**, klicken Sie auf die Dropdownliste Schaltfläche Erstellen,
    und wählen Sie **dann Azure OpenAI** aus, wie im Bild gezeigt.
    (Falls Sie bereits auf die **Azure OpenAI-Kachel** geklickt haben,
    klicken Sie auf der Azure OpenAI-Seite auf die Schaltfläche
    **Create**).

![A screenshot of a software page AI-generated content may be
incorrect.](./media/image38.png)

5.  Geben Sie auf der Registerkarte Azure OpenAI-**Basics** erstellen
    die folgenden Informationen ein, und klicken Sie auf die
    Schaltfläche **Next**.

[TABLE]

6.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image39.png)

7.  Lassen Sie auf der Registerkarte **Network** alle Optionsfelder im
    Standardzustand und klicken Sie auf die Schaltfläche **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

8.  Lassen Sie auf der Registerkarte **Tags** alle Felder im
    Standardzustand und klicken Sie auf die Schaltfläche **Weiter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

1.  Klicken Sie auf der Registerkarte **Review+submit,** sobald die
    Validierung bestanden ist, auf die Schaltfläche **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

9.  Warten Sie, bis die Bereitstellung abgeschlossen ist. Die
    Bereitstellung dauert ca. 2-3 Minuten.

\[!Note\] **Anmerkung:** Wenn über ein Anwendungsformular eine Meldung
angezeigt wird, dass der Azure OpenAI-Dienst derzeit für Kunden
verfügbar ist. Das ausgewählte Abonnement wurde für den Dienst nicht
aktiviert und verfügt nicht über ein Kontingent für Tarife. Sie müssen
auf den Link klicken, um Zugriff auf den Azure OpenAI-Dienst
anzufordern, und das Antragsformular ausfüllen.

### Aufgabe 2: Abrufen des Schlüssels und Endpunkts des Azure OpenAI-Diensts

1.  Wählen Sie auf der Seite **Overview** der Ressource die Schaltfläche
    **Go to resource** aus. Wenn Sie dazu aufgefordert werden, wählen
    Sie die Lab-Anmeldeinformationen aus:

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.jpeg)

1.  In Ihrem **Azure OpenAI home** Fenster, navigieren Sie zum
    **Resource Management** Abschnitt, und klicken Sie auf **Keys and
    Endpoints**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

1.  Kopieren Sie auf der Seite **Keys and Endpoints** l und Endpunkte
    die Werte **KEY1, KEY 2** und **Endpoint,** fügen Sie sie in einen
    Editor ein, wie in der folgenden Abbildung gezeigt, und
    **speichern** Sie dann den Editor, um die Informationen in den
    anstehenden Aufgaben zu verwenden.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.jpeg)

**Anmerkung:** Sie können entweder KEY1 oder KEY2 verwenden. Wenn Sie
immer zwei Schlüssel haben, können Sie Schlüssel sicher rotieren und neu
generieren, ohne dass es zu einer Dienstunterbrechung kommt.

### Aufgabe 3: Bereitstellen eines Einbettungsmodells

Die Erweiterung azure_ai ermöglicht die Erstellung von
Vektoreinbettungen aus Text. Zum Erstellen dieser Einbettungen ist ein
bereitgestelltes text-embedding-ada-002-Modell (Version 2) in Ihrem
Azure OpenAI-Dienst erforderlich. In dieser Aufgabe verwenden Sie Azure
OpenAI Studio, um eine Modellbereitstellung zu erstellen, die Sie
verwenden können.

1.  Auf der **Azure OpenAI** Seite, klicken Sie auf **Overview** im
    linken Navigationsmenü, scrollen Sie nach unten und klicken Sie auf
    die **Go to Azure OpenAI Studio** Schaltfläche, wie in der folgenden
    Abbildung gezeigt.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

2.  Auf der **Azure AI Foundry | Azure Open AI Service** Homepage,
    navigieren Sie zum Abschnitt **Components**, und klicken Sie auf
    **Deployments**.

3.  Öffnen Sie im Fenster **Deployments** das **+Deploy-Modell,** und
    wählen Sie **Deploy base model** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

4.  In der Dialogbox **Select a model**, navigieren und sorgfältig
    auswählen**text-embedding-ada-002**, Klicken Sie dann auf die
    Schaltfläche **Confirm**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

5.  Legen Sie im Dialogfeld **Deploy model** folgendes fest, und wählen
    Sie **Create** aus, um das Modell bereitzustellen.

    - **Select a model**: Wählen Sie **text-embedding-ada-002** aus der
      Liste.

    - **Model version**: Stellen Sie sichher **2 (Default)** ist
      ausgewählt.

    - **Deployment name**: Enter +++**embeddings**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

1.  Im **Deployments** Fenster, kopieren Sie den **Deployment name** und
    fügen Sie sie in einen Notizblock ein (wie im Bild gezeigt),
    **speichern** Sie dann den Editor, um die Informationen in der
    nächsten Aufgabe zu verwenden.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

## Übung 4: Installieren und Konfigurieren der Erweiterung "azure_ai"

In dieser Übung installieren Sie die azure_ai-Erweiterung in Ihrer
Datenbank und konfigurieren sie so, dass sie eine Verbindung mit Ihrem
Azure OpenAI-Dienst herstellt.

### Aufgabe 1: Herstellen einer Verbindung mit der Datenbank mithilfe von psql in Azure Cloud Shell

In dieser Aufgabe verwenden Sie das Befehlszeilenprogramm psql aus Azure
Cloud Shell, um eine Verbindung mit Ihrer Datenbank herzustellen.

1.  Wählen Sie auf der Symbolleiste des Azure-Portals das **Cloud
    Shell-Symbol** aus, um oben im Browserfenster einen neuen Cloud
    Shell-Bereich zu öffnen.

2.  Fügen Sie **Connection details** in die Cloud Shell ein.

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image21.jpeg)

1.  Ersetzen Sie an der Cloud Shell-Eingabeaufforderung das Token
    **{your_password}** durch das Kennwort, das Sie dem Benutzer
    **s2admin** beim Erstellen Ihrer Datenbank zugewiesen haben. Das
    Kennwort sollte **Seattle123Seattle123** lauten.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.jpeg)

3.  Stellen Sie mithilfe des Befehlszeilenprogramms psql eine Verbindung
    zu Ihrer Datenbank her, indem Sie an der Eingabeaufforderung
    Folgendes eingeben:

+++**psql**+++

![A black background with a black square AI-generated content may be
incorrect.](./media/image23.jpeg)

### Aufgabe 2: Installieren der azure_ai Erweiterung

Mit der Erweiterung azure_ai können Sie Azure OpenAI und Azure Cognitive
Services in Ihre Datenbank integrieren. Um die Erweiterung in Ihrer
Datenbank zu aktivieren, führen Sie die folgenden Schritte aus:

1.  Stellen Sie sicher, dass die Erweiterung erfolgreich zur
    Zulassungsliste hinzugefügt wurde, indem Sie an der
    psql-Eingabeaufforderung Folgendes ausführen:

+++SHOW azure.extensions;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

2.  Installieren Sie die Erweiterung azure_ai mit dem Befehl CREATE
    EXTENSION.

+++CREATE EXTENSION IF NOT EXISTS azure_ai;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.jpeg)

### Aufgabe 3: Überprüfen der in der azure_ai Erweiterung enthaltenen Objekte

Das Überprüfen der Objekte in der azure_ai Erweiterung kann zu einem
besseren Verständnis ihrer Funktionen führen. In dieser Aufgabe
überprüfen Sie die verschiedenen Schemas, benutzerdefinierten Funktionen
(User-Defined Functions, UDFs) und zusammengesetzten Typen, die der
Datenbank durch die Erweiterung hinzugefügt wurden.

1.  Sie können den \dx-Metabefehl an der **psql-**Eingabeaufforderung
    verwenden, um die in der Erweiterung enthaltenen Objekte
    aufzulisten.

\[!Note\] **Anmerkung:** Klicken Sie auf eine beliebige Taste, um
fortzufahren, wenn die Cloud Shell mit **Mehr...**

+++\dx+ azure_ai+++

![](./media/image56.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image57.jpeg)

Die Ausgabe des Metabefehls zeigt, dass die Erweiterung azure_ai drei
Schemas, mehrere benutzerdefinierte Funktionen (User-Defined Functions,
UDFs) und mehrere zusammengesetzte Typen in der Datenbank erstellt. In
der folgenden Tabelle sind die von der Erweiterung hinzugefügten Schemas
aufgeführt und die einzelnen Schemas.

[TABLE]

2.  Die Funktionen und Typen sind alle mit einem der Schemas verknüpft.
    Um die im azure_ai Schema definierten Funktionen zu überprüfen,
    verwenden Sie den Metabefehl \df und geben Sie das Schema an, dessen
    Funktionen angezeigt werden sollen. Mit dem \x auto-Befehl vor \df
    kann die erweiterte Anzeige bei Bedarf automatisch angewendet
    werden, um die Anzeige der Ausgabe des Befehls in Azure Cloud Shell
    zu erleichtern.

+++\x auto+++ +++\df+ azure_ai.\*+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

Mit der Funktion azure_ai.set_setting() können Sie den Endpunkt und die
Schlüsselwerte für Azure AI-Dienste festlegen. Es akzeptiert einen
**Keys** und den **Value**, dem er zugewiesen werden soll. Die Funktion
azure_ai.get_setting() bietet eine Möglichkeit, die Werte abzurufen, die
Sie mit der Funktion set_setting() festgelegt haben. Es akzeptiert den
**Key** der Einstellung, die Sie anzeigen möchten. Für beide Methoden
muss der Schlüssel einer der folgenden sein:

### Aufgabe 4: Festlegen des Azure OpenAI-Endpunkts und -Schlüssels

Bevor Sie die azure_openai Funktionen verwenden, konfigurieren Sie die
Erweiterung für Ihren Azure OpenAI-Dienstendpunkt und -schlüssel.

1.  Ersetzen Sie im folgenden Befehl die Token**{endpoint}** und
    **{api-key}** durch Werte, die Sie aus dem Azure-Portal abgerufen
    haben, und führen Sie dann die Befehle an der
    psql-Eingabeaufforderung im Cloud Shell-Bereich aus, um Ihre Werte
    der Konfigurationstabelle hinzuzufügen.

&nbsp;

1.  Wählen Sie
    azure_ai.set_setting('azure_openai.endpoint','{endpoint}');

2.  Wählen Sie azure_ai.set_setting('azure_openai.subscription_key',
    '{api-key}');

![A computer screen with white text AI-generated content may be
incorrect.](./media/image59.jpeg)

3.  Überprüfen Sie die in der Konfigurationstabelle geschriebenen
    Einstellungen mit den folgenden Abfragen:

4.  Wählen Sie azure_ai.get_setting('azure_openai.endpoint');

5.  Wählen Sie azure_ai.get_setting('azure_openai.subscription_key');

Die azure_ai Erweiterung ist jetzt mit Ihrem Azure OpenAI-Konto
verbunden und kann Vektoreinbettungen generieren.

![A computer screen with white text AI-generated content may be
incorrect.](./media/image60.jpeg)

## Übung 5: Generieren von Vektoreinbettungen mit Azure OpenAI

Das azure_openai Schema der azure_ai-Erweiterung ermöglicht es Azure
OpenAI, Vektoreinbettungen für Textwerte zu erstellen. Mithilfe dieses
Schemas können Sie Einbettungen mit Azure OpenAI direkt aus der
Datenbank generieren, um Vektordarstellungen von Eingabetext zu
erstellen, die dann in Vektorähnlichkeitssuchen verwendet und von
Machine Learning-Modellen genutzt werden können.

Einbettungen sind ein Konzept des maschinellen Lernens und der
Verarbeitung natürlicher Sprache (NLP), bei dem Objekte wie Wörter,
Dokumente oder Entitäten als Vektoren in einem mehrdimensionalen Raum
dargestellt werden. Einbettungen ermöglichen es Machine
Learning-Modellen, zu bewerten, wie eng Informationen miteinander
verbunden sind. Diese Technik identifiziert effizient Beziehungen und
Ähnlichkeiten zwischen Daten und ermöglicht es Algorithmen, Muster zu
erkennen und genaue Vorhersagen zu treffen.

### Aufgabe 1: Aktivieren der Vektorunterstützung mit der Erweiterung pgvector

Mit der Erweiterung azure_ai können Sie Einbettungen für Eingabetext
generieren. Damit die generierten Vektoren zusammen mit den restlichen
Daten in der Datenbank gespeichert werden können, müssen Sie die
pgvector-Erweiterung installieren, indem Sie den Anweisungen in der
Dokumentation zum Aktivieren der Vektorunterstützung in Ihrer Datenbank
folgen.

1.  Installieren Sie die Erweiterung pgvector mit dem Befehl CREATE
    EXTENSION.

+++CREATE EXTENSION IF NOT EXISTS vector; +++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image61.jpeg)

2.  Wenn Sie die Vektorunterstützung zu Ihrer Datenbank hinzugefügt
    haben, fügen Sie der Listentabelle eine neue Spalte hinzu, indem Sie
    den Datentyp vector verwenden, um Einbettungen in der Tabelle zu
    speichern. Das Modell text-embedding-ada-002 erzeugt Vektoren mit
    1536 Dimensionen, daher müssen Sie 1536 als Vektorgröße angeben.

3.  ALTER TABLE listings

4.  ADD COLUMN description_vector vector(1536);

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image62.jpeg)

### Aufgabe 2: Generieren und Speichern von Vektoreinbettungen

Die Listings-Tabelle ist jetzt bereit, Einbettungen zu speichern. Mit
der Funktion azure_openai.create_embeddings() erstellen Sie Vektoren für
das Beschreibungsfeld und fügen sie in die neu erstellte Spalte
description_vector in der Angebotstabelle ein.

1.  Bevor Sie die Funktion create_embeddings() verwenden, führen Sie den
    folgenden Befehl aus, um sie zu überprüfen und die erforderlichen
    Argumente zu überprüfen:

+++\df+ azure_openai.\* +++

![A computer screen with white text AI-generated content may be
incorrect.](./media/image63.jpeg)

Die Eigenschaft Argument data types in der Ausgabe des Befehls \df+
azure_openai.\* zeigt die Liste der Argumente an, die die Funktion
erwartet.

[TABLE]

1.  Führen Sie unter Verwendung des Bereitstellungsnamens die folgende
    Abfrage aus, um jeden Datensatz in der Listings-Tabelle zu
    aktualisieren, und fügen Sie die generierten Vektoreinbettungen für
    das Beschreibungsfeld mit der Funktion
    azure_openai.create_embeddings() in die Spalte description_vector
    ein. Ersetzen Sie {your-deployment-name} durch den Wert für den
    **Deployment name,** den Sie von der Seite Azure OpenAI Studio-
    **Deployment** kopiert haben. Beachten Sie, dass diese Abfrage etwa
    fünf Minuten dauert.

> DO $$
>
> DECLARE counter integer := (SELECT COUNT(\*) FROM listings WHERE
> description \<\> '' AND description_vector IS NULL);
>
> DECLARE r record;
>
> BEGIN
>
> RAISE NOTICE 'Total descriptions to embed: %', counter;
>
> WHILE counter \> 0 LOOP
>
> BEGIN
>
> FOR r IN
>
> SELECT listing_id FROM listings WHERE description \<\> '' AND
> description_vector IS NULL
>
> LOOP
>
> BEGIN
>
> UPDATE listings
>
> SET description_vector =
> azure_openai.create_embeddings('{your-deployment-name}', description)
>
> WHERE listing_id = r.listing_id;
>
> EXCEPTION
>
> WHEN OTHERS THEN
>
> RAISE NOTICE 'Waiting 1 second before trying again...';
>
> PERFORM pg_sleep(1);
>
> END;
>
> counter := (SELECT COUNT(\*) FROM listings WHERE description \<\> ''
> AND description_vector IS NULL);
>
> IF counter % 25 = 0 THEN
>
> RAISE NOTICE 'Remaining descriptions to embed: %', counter;
>
> END IF;
>
> END LOOP;
>
> END;
>
> END LOOP;
>
> END;
>
> $$;

![A computer screen with white text AI-generated content may be
incorrect.](./media/image64.jpeg)

Die obige Abfrage verwendet eine WHILE-Schleife, um Datensätze aus der
Listings-Tabelle abzurufen, in der das Feld description_vector null ist
und das Beschreibungsfeld keine leere Zeichenfolge ist. Die Abfrage
versucht dann, die description_vector Spalte mithilfe der Funktion
azure_openai.create_embeddings mit einer Vektordarstellung der
Beschreibungsspalte zu aktualisieren. Die Schleife wird beim Ausführen
dieses Updates verwendet, um zu verhindern, dass Aufrufe zum Erstellen
von Einbettungsfunktionen den Grenzwert für die Aufrufrate des Azure
OpenAI-Diensts überschreiten. Wenn das Limit für die Anrufrate
überschritten wird, werden in der Ausgabe Warnungen ähnlich der
folgenden angezeigt:

\[!Note\] **ANMERKUNG**: Warten Sie 1 Sekunde, bevor Sie es erneut
versuchen...

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image65.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image66.jpeg)

2.  Sie können überprüfen, ob die Spalte description_vector für alle
    Angebotsdatensätze ausgefüllt wurde, indem Sie die folgende Abfrage
    ausführen:

+++SELECT COUNT(\*) FROM listings WHERE description_vector IS NULL AND
description \<\> '';+++

Das Ergebnis der Abfrage sollte den Wert 0 haben.

![A black screen with white text AI-generated content may be
incorrect.](./media/image67.jpeg)

### Aufgabe 3: Durchführen einer Vektorähnlichkeitssuche

Die Vektorähnlichkeit ist eine Methode, die verwendet wird, um die
Ähnlichkeit zweier Elemente zu messen, indem sie als Vektoren, eine
Reihe von Zahlen, dargestellt werden. Vektoren werden häufig verwendet,
um Suchen mit LLMs durchzuführen. Die Vektorähnlichkeit wird häufig
anhand von Entfernungsmetriken wie der euklidischen Distanz oder der
Kosinusähnlichkeit berechnet. Die euklidische Distanz misst den
geradlinigen Abstand zwischen zwei Vektoren im n-dimensionalen Raum,
während die Kosinusähnlichkeit den Kosinus des Winkels zwischen zwei
Vektoren misst. Jede Einbettung ist ein Vektor von Gleitkommazahlen, so
dass der Abstand zwischen zwei Einbettungen im Vektorraum mit der
semantischen Ähnlichkeit zwischen zwei Eingaben im Originalformat
korreliert.

1.  Führen Sie vor dem Ausführen einer Vektorähnlichkeitssuche die
    folgende Abfrage mit der IALIKE-Klausel aus, um die Ergebnisse der
    Suche nach Datensätzen mit einer Abfrage in natürlicher Sprache ohne
    Verwendung der Vektorähnlichkeit zu beobachten:

+++SELECT listing_id, name, description FROM listings WHERE description
ILIKE '%Properties with a private room near Discovery Park%';+++

![A black background with white text AI-generated content may be
incorrect.](./media/image68.jpeg)

Die Abfrage gibt keine Ergebnisse zurück, da sie versucht, den Text im
Beschreibungsfeld mit der bereitgestellten Abfrage in natürlicher
Sprache abzugleichen.

1.  Führen Sie nun eine Kosinus-Ähnlichkeitssuchabfrage für die
    Listings-Tabelle aus, um eine Vektorähnlichkeitssuche für
    Listing-Beschreibungen durchzuführen. Die Einbettungen werden für
    eine Eingabefrage generiert und dann in ein Vektorarray (::vector)
    umgewandelt, wodurch sie mit den in der Listings-Tabelle
    gespeicherten Vektoren verglichen werden können. Ersetzen Sie
    {your-deployment-name} durch den Wert für den **Deployment name,**
    den Sie von der Seite Azure OpenAI Studio- **Deployment** kopiert
    haben.

+++SELECT listing_id, name, description FROM listings ORDER BY
description_vector \<=\>
azure_openai.create_embeddings('{your-deployment-name}', 'Properties
with a private room near Discovery Park')::vector LIMIT 3;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image69.jpeg)

Die Abfrage verwendet den [Vektoroperator
\<=\>](https://github.com/pgvector/pgvector#vector-operators), der den
Operator \cosine distance\\ darstellt, der zum Berechnen des Abstands
zwischen zwei Vektoren in einem mehrdimensionalen Raum verwendet wird.

1.  Führen Sie dieselbe Abfrage erneut mit der EXPLAIN ANALYZE-Klausel
    aus, um die Planungs- und Ausführungszeiten der Abfrage anzuzeigen.
    Ersetzen Sie **{your-deployment-name}** mit dem **Deployment
    name** Wert, den Sie von der Seite Azure OpenAI **Studio-
    Deployment** kopiert haben.

> EXPLAIN ANALYZE
>
> SELECT listing_id, name, description FROM listings
>
> ORDER BY description_vector \<=\>
> azure_openai.create_embeddings('{your-deployment-name}', 'Properties
> with a private room near Discovery Park')::vector
>
> LIMIT 3;

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image70.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image71.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image72.jpeg)

Beachten Sie in der Ausgabe den Abfrageplan, der mit etwas Ähnlichem wie
folgt beginnt:

Limit (cost=1098.54..1098.55 rows=3 width=261) (actual
time=10.505..10.507 rows=3 loops=1) -\> Sort (cost=1098.54..1104.10
rows=2224 width=261) (actual time=10.504..10.505 rows=3 loops=1)

…

Sort Method: top-N heapsort Memory: 27kB -\> Seq Scan on listings
(cost=0.00..1069.80 rows=2224 width=261) (actual time=0.005..9.997
rows=2224 loops=1) Die Abfrage verwendet eine sequenzielle
Scansortierung, um die Suche durchzuführen. Die Planungs- und
Ausführungszeiten werden am Ende der Ergebnisse aufgelistet und sollten
in etwa wie folgt aussehen: Planungszeit: 62.020 ms Execution Time:
10.530 ms

2.  Um eine effizientere Suche über das Vektorfeld zu ermöglichen,
    erstellen Sie einen Index für Einträge mit Kosinusabstand und
    [HNSW](https://github.com/pgvector/pgvector#hnsw), der Abkürzung für
    Hierarchical Navigable Small World. HNSW ermöglicht es pgvector, die
    neuesten graphenbasierten Algorithmen zu verwenden, um Abfragen des
    nächsten Nachbarn zu approximieren.

+++CREATE INDEX ON listings USING hnsw (description_vector
vector_cosine_ops);+++

![](./media/image73.jpeg)

1.  Um die Auswirkungen des hnsw-Indexes auf die Tabelle zu beobachten,
    führen Sie die Abfrage erneut mit der EXPLAIN ANALYZE-Klausel aus,
    um die Planungs- und Ausführungszeiten der Abfrage zu vergleichen.
    Ersetzen Sie **{your-deployment-name}** mit dem **Deployment
    name** Wert, den Sie von der Seite Azure OpenAI **Studio-
    Deployment** kopiert haben.

> EXPLAIN ANALYZE
>
> SELECT listing_id, name, description FROM listings
>
> ORDER BY description_vector \<=\>
> azure_openai.create_embeddings('{your-deployment-name}', 'Properties
> with a private room near Discovery Park')::vector
>
> LIMIT 3;

![A computer screen with white text AI-generated content may be
incorrect.](./media/image74.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image75.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image76.jpeg)

Beachten Sie, dass der Abfrageplan in der Ausgabe jetzt einen
effizienteren Indexscan enthält:

Limit (cost=116.48..119.33 rows=3 width=261) (actual time=1.112..1.130
rows=3 loops=1) -\> Index Scan using listings_description_vector_idx on
listings (cost=116.48..2228.28 rows=2224 width=261) (actual
time=1.111..1.128 rows=3 loops=1)

Die Ausführungszeiten der Abfrage sollten eine deutliche Verkürzung der
Zeit widerspiegeln, die für die Planung und Ausführung der Abfrage
benötigt wurde:

Planning Time: 56.802 ms

Execution Time: 1.167 ms

## Übung 6: Integrieren von Azure AI Services

Die Azure AI Services-Integrationen, die im azure_cognitive Schema der
azure_ai Erweiterung enthalten sind, bieten eine Vielzahl von
AI-Sprachfeatures, auf die direkt über die Datenbank zugegriffen werden
kann. Zu den Funktionen gehören Stimmungsanalyse, Spracherkennung,
Extraktion von Schlüsselphrasen, Entitätserkennung und
Textzusammenfassung. Diese Funktionen werden über den Azure AI
Language-Dienst aktiviert.

Eine vollständige Liste der Azure AI-Funktionen, auf die über die
Erweiterung zugegriffen werden kann, finden Sie in der Dokumentation
Integrieren von Azure Database for PostgreSQL Flexible Server in Azure
Cognitive Services.

### Aufgabe 1: Bereitstellen eines Azure AI-Sprachdiensts

Ein Azure AI Languageservice ist erforderlich, um die kognitiven
Funktionen der azure_ai Erweiterungen nutzen zu können. In dieser Übung
erstellen Sie einen Azure AI-Sprachdienst.

1.  Klicken Sie auf der Startseite des Azure-Portals auf das **Azure
    portal menu,** das durch drei horizontale Balken auf der linken
    Seite der Microsoft Azure-Befehlsleiste dargestellt wird, wie in der
    folgenden Abbildung gezeigt.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image77.jpeg)

2.  Auf der Seite **Create a resource** wählen Sie **AI + Machine
    Learning** aus dem Menü auf der linken Seite und wählen Sie
    dann **Language service**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image78.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image79.jpeg)

3.  Klicken Sie im Dialogfeld **Select additional features**, wählen
    Sie **Continue to create your resource** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image80.jpeg)

4.  Geben Sie auf der Create Language **Basics** erstellen Folgendes
    ein:

[TABLE]

5.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image81.png)

6.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image82.jpeg)

7.  Die Standardeinstellungen werden für die verbleibenden
    Registerkarten der Sprachdienstkonfiguration verwendet, wählen Sie
    also die Schaltfläche **Review + create** aus.

8.  Wählen Sie die Schaltfläche **Create** auf der Schaltfläche
    **Review + create** zum Bereitstellen des Sprachdienstes.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image83.png)

9.  Wählen Sie auf der Bereitstellungsseite die Option **Go to resource
    group** aus, wenn die Bereitstellung des Sprachdiensts abgeschlossen
    ist.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image84.jpeg)

### Aufgabe 2: Festlegen des Endpunkts und des Schlüssels für den Azure AI-Sprachdienst

Wie bei den azure_openai-Funktionen müssen Sie den Endpunkt und einen
Schlüssel für Ihren Azure AI-Sprachdienst bereitstellen, um mithilfe der
azure_ai-Erweiterung erfolgreich Aufrufe für Azure AI-Dienste zu
tätigen.

1.  Wählen Sie auf der Startseite für Sprachen im linken Navigationsmenü
    unter **Resource Management** den Eintrag **Keys and Endpoint** aus.

2.  Kopieren Sie auf der Seite **Keys and Endpoints** die Werte **KEY1,
    KEY 2** und **Endpoint,** fügen Sie sie in einen Editor ein, wie in
    der folgenden Abbildung gezeigt, und **speichern** Sie dann den
    Editor, um die Informationen in den anstehenden Aufgaben zu
    verwenden.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image85.jpeg)

3.  Kopieren Sie die Werte für den Endpunkt und den Zugriffsschlüssel,
    und ersetzen Sie dann im folgenden Befehl die Token {endpoint} und
    {api-key} durch Werte, die Sie aus dem Azure-Portal abgerufen haben.
    Führen Sie die Befehle über die psql-Eingabeaufforderung in Cloud
    Shell aus, um Ihre Werte zur Konfigurationstabelle hinzuzufügen.

\[!Note\] **Anmerkung:** Stellen Sie eine Verbindung zur
psql-Eingabeaufforderung her, bevor Sie die folgenden Befehle ausführen.

SELECT azure_ai.set_setting('azure_cognitive.endpoint','{endpoint}');

SELECT azure_ai.set_setting('azure_cognitive.subscription_key',
'{api-key}');

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image86.jpeg)

### Aufgabe 3: Analysieren Sie die Stimmung von Bewertungen

In dieser Aufgabe verwendest du die Funktion
azure_cognitive.analyze_sentiment, um Bewertungen von Airbnb-Inseraten
auszuwerten.

1.  Um eine Stimmungsanalyse mithilfe des azure_cognitive Schemas in der
    azure_ai-Erweiterung durchzuführen, verwenden Sie die Funktion
    analyze_sentiment. Führen Sie den folgenden Befehl aus, um diese
    Funktion zu überprüfen:

+++\df azure_cognitive.analyze_sentiment+++

![A computer screen with white text AI-generated content may be
incorrect.](./media/image87.jpeg)

Die Ausgabe zeigt das Schema, den Namen, den Ergebnisdatentyp und die
Argumentdatentypen der Funktion an. Diese Informationen helfen dabei,
ein Verständnis für die Verwendung der Funktion zu erlangen.

2.  Es ist auch wichtig, die Struktur des Ergebnisdatentyps zu
    verstehen, den die Funktion ausgibt, damit Sie den Rückgabewert
    korrekt verarbeiten können. Führen Sie den folgenden Befehl aus, um
    den Typ sentiment_analysis_result:

+++\dT+ azure_cognitive.sentiment_analysis_result+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image88.jpeg)

3.  Die Ausgabe des obigen Befehls zeigt, dass der
    sentiment_analysis_result Typ ein Tupel ist. Um die Struktur dieses
    Tupels zu verstehen, führen Sie den folgenden Befehl aus, um die
    Spalten zu betrachten, die im zusammengesetzten Typ
    sentiment_analysis_result enthalten sind:

+++\d+ azure_cognitive.sentiment_analysis_result+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image89.jpeg)

Die Ausgabe dieses Befehls sollte in etwa wie folgt aussehen: Composite
type "azure_cognitive.sentiment_analysis_result"

Column | Type | Collation | Nullable | Default | Storage | Description
----------------+------------------+-----------+----------+---------+----------+-------------

sentiment | text | | | | extended |

positive_score | double precision | | | | plain |

neutral_score | double precision | | | | plain |

negative_score | double precision | | | | plain |

Der azure_cognitive.sentiment_analysis_result ist ein zusammengesetzter
Typ, der die Stimmungsvorhersagen des Eingabetexts enthält. Es enthält
die Stimmung, die positiv, negativ, neutral oder gemischt sein kann, und
die Bewertungen für positive, neutrale und negative Aspekte, die im Text
zu finden sind. Die Punktzahlen werden als reelle Zahlen zwischen 0 und
1 dargestellt. Zum Beispiel ist die Stimmung in (neutral,0,26,0,64,0,09)
neutral mit einem positiven Wert von 0,26, neutral von 0,64 und negativ
bei 0,09.

## Übung 7: Ausführen einer abschließenden Abfrage, um alles miteinander zu verknüpfen

In dieser Übung stellen Sie eine Verbindung mit Ihrer Datenbank in
**pgAdmin** her und führen eine abschließende Abfrage aus, die Ihre
Arbeit mit den Erweiterungen azure_ai, postgis und pgvector in den Labs
3 und 4 verknüpft.

### Aufgabe 1: Installieren von pgAdmin

1.  Öffnen Sie einen Webbrowser und navigieren Sie
    zum <https://www.pgadmin.org/download/pgadmin-4-windows/>

&nbsp;

1.  Klicken Sie auf die neueste Version von **pgAdmin**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.jpeg)

2.  Wählen Sie **pgadmin4-8.9-x64.exe**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image91.jpeg)

1.  Heruntergeladene Datei ausführen und installieren

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image92.jpeg)

3.  Wählen Sie auf der Registerkarte Setupinstallationsmodus auswählen
    die Option **Install for me only(recommended)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image93.jpeg)

1.  Klicken Sie auf die Schaltfläche **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image94.jpeg)

4.  Wählen Sie die Schaltfläche **I accept the agreement** und klicken
    Sie auf **Next** Knopf.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image95.jpeg)

5.  Wählen Sie den Pfad aus und klicken Sie auf **Next** Knopf

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image96.jpeg)

1.  Klicken Sie im Fenster **Setup-pgAdmin 4** auf die Schaltfläche
    **Next**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image97.jpeg)

1.  Klicken Sie auf die Schaltfläche **Install**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image98.jpeg)

1.  Klicken Sie im Fenster **Setup-pgAdmin 4** auf die Schaltfläche
    **Finish** stellen

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image99.jpeg)

### Aufgabe 2: Herstellen einer Verbindung mit der Datenbank mithilfe von pgAdmin

In dieser Aufgabe öffnen Sie pgAdmin und stellen eine Verbindung zu
Ihrer Datenbank her.

1.  Geben Sie in Ihrem Windows-Suchfeld Folgendes ein +++**pgAdmin**+++,
    klicken Sie dann auf **pgAdmin**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image100.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image101.jpeg)

2.  Registrieren Sie Ihren **Server**, indem Sie im Objekt-Explorer mit
    der rechten Maustaste auf Server klicken und **\> Register \>
    Server** auswählen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image102.jpeg)

3.  Fügen Sie im Dialogfeld **Register - Server** den Namen Ihres Azure
    Database for PostgreSQL Flexible Server-Servers (den Sie in Übung
    1\> Aufgabe 1 gespeichert haben) in das Feld **Name** auf der
    Registerkarte **General** ein.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image103.jpeg)

4.  Wählen Sie als Nächstes die **Registerkarte Verbindung** und fügen
    Sie Ihren Servernamen in das Feld **Hostname/Adresse** ein. Enter
    +++**s2admin**+++ in den **Username** Feld, geben Sie
    +++**Seattle123Seattle123**+++ in das Feld **Passwort**, und wählen
    Sie optional **Save password**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image104.jpeg)

5.  Wählen Sie abschließend die Registerkarte **Parameter** und stellen
    Sie den **SSL-Modus** so ein, dass er benötigt. Wählen Sie **Save**
    aus, um den Server zu registrieren.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image105.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image106.jpeg)

1.  Sobald du mit deinem Server verbunden bist, erweitere den Knoten
    **Database** und wähle die **Airbnb-**Database aus. Klicken Sie mit
    der rechten Maustaste auf die **airbnb**-Database und wählen Sie
    **Query Tool** aus dem Kontextmenü.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image107.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image108.jpeg)

### Aufgabe 3: Überprüfen, ob die Erweiterung PostGIS in der Datenbank installiert ist

Um die postgis-Erweiterung in Ihrer Datenbank zu installieren, verwenden
Sie den Befehl CREATE EXTENSION.

1.  Führen Sie in dem Abfragefenster, das Sie oben geöffnet haben, den
    Befehl CREATE EXTENSION mit der IF NOT EXISTS-Klausel aus, um die
    postgis-Erweiterung in Ihrer Datenbank zu installieren.

+++CREATE EXTENSION IF NOT EXISTS postgis;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image109.jpeg)

Nachdem die PostGIS-Erweiterung nun geladen ist, können Sie mit der
Arbeit mit Geodaten in der Datenbank beginnen. Die Listentabelle, die
Sie oben erstellt und ausgefüllt haben, enthält den Breiten- und
Längengrad aller aufgelisteten Immobilien. Um diese Daten für die
Geodatenanalyse zu verwenden, müssen Sie die Auflistungstabelle ändern,
um eine Geometriespalte hinzuzufügen, die den Datentyp "Punkt"
akzeptiert. Diese neuen Datentypen sind in der postgis-Erweiterung
enthalten.

2.  Um Punktdaten aufzunehmen, fügen Sie der Tabelle, die Punktdaten
    akzeptiert, eine neue Geometriespalte hinzu. Kopieren Sie die
    folgende Abfrage, und fügen Sie sie in das geöffnete
    pgAdmin-Abfragefenster ein.:

&nbsp;

1.  ALTER TABLE-Auflistungen

+++ADD COLUMN listing_location geometry(point, 4326); +++

3.  Aktualisieren Sie als Nächstes die Tabelle mit Geodaten, die den
    einzelnen Auflistungen zugeordnet sind, indem Sie die Werte für
    Längen- und Breitengrad in die Geometriespalte einfügen.

4.  UPDATE Auflistungen

+++SET listing_location = ST_SetSRID(ST_Point(longitude, latitude),
4326);+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image110.jpeg)

### Aufgabe 4: Ausführen einer Abfrage und Anzeigen der Ergebnisse auf einer Karte

1.  Kopieren Sie die folgende Abfrage, fügen Sie sie in den Editor für
    geöffnete Abfragen ein, und führen Sie sie dann aus, um die in der
    Spalte **listing_location** gespeicherten Daten anzuzeigen.

+++SELECT listing_id, name, listing_location FROM listings LIMIT 50;+++

Wählen Sie im Bereich Datenausgabe die Schaltfläche **View all
geometries** in dieser Spalte anzeigen aus, die in der
**listing_location Spalte** der Abfrageergebnisse angezeigt wird.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image111.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image112.jpeg)

1.  Führen Sie nun die folgende Abfrage aus, um eine räumliche
    **geospatial proximity query** durchzuführen, und geben Sie
    Eigenschaften zurück, die für die Woche vom 13. Januar 2016
    verfügbar sind, unter 75,00 USD pro Nacht liegen und sich in der
    Nähe des Discovery Park in Seattle befinden. Die Abfrage verwendet
    die Funktion ST_DWithin, die von der PostGIS-Erweiterung
    bereitgestellt wird, um Einträge innerhalb einer bestimmten
    Entfernung vom Park zu identifizieren, die einen Längengrad von
    -122,410347 und einen Breitengrad von 47,655598 hat.

> SELECT name, listing_location, summary
>
> FROM listings l
>
> INNER JOIN calendar c ON l.listing_id = c.listing_id
>
> WHERE ST_DWithin(
>
> listing_location,
>
> ST_GeomFromText('POINT(-122.410347 47.655598)', 4326),
>
> 0.025
>
> )
>
> AND c.date = '2016-01-13'
>
> AND c.available = 't'
>
> AND c.price \<= 75.00;

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image113.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image114.jpeg)

**Zusammenfassung**

In diesem Lab haben Sie Azure-KI-Dienste erfolgreich in PostgreSQL
integriert, um eine leistungsstarke AI-gestützte Datenbankumgebung zu
erstellen. Sie haben mit der Bereitstellung von Azure-Ressourcen und der
Konfiguration Ihrer PostgreSQL-Datenbank mit den erforderlichen
Erweiterungen begonnen. Anschließend haben Sie Vektoreinbettungen für
Textdaten generiert und Vektorähnlichkeitssuchen durchgeführt, um
semantisch ähnliche Datensätze zu finden. Darüber hinaus haben Sie die
PostGIS-Erweiterung für die Analyse von Geodaten und den Azure AI
Language-Dienst für die Meinungsanalyse verwendet. Schließlich haben Sie
Ihre Abfragen mithilfe der Indizierung optimiert und ihre Leistung
analysiert, um die Effizienz und Leistungsfähigkeit dieser integrierten
Lösung für die erweiterte Datenanalyse zu demonstrieren.

 
