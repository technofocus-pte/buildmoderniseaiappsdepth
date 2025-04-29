# Anwendungsfall 07 – Aktivieren der semantischen Suche in Azure Database for PostgreSQL Flexible Server, um Azure OpenAI zum Generieren von Vektoreinbettungen zu verwenden.

**Objektiv** :

In diesem Anwendungsfall implementieren Sie die semantische Suche, um
Einbettungen zu generieren und zu speichern, installieren die Vektor-
und azure_ai-Erweiterungen auf einem flexiblen Azure Database for
PostgreSQL-Server und wenden dann die Erweiterungen an, um von Azure
OpenAI generierte Einbettungsvektoren zu speichern

**Verwendete Schlüsseltechnologien**: Azure OpenAI, Azure Database for
PostgreSQL, Azure AI-Erweiterung

**Geschätzte Dauer**: 45 Minuten

**Labtyp:** Von einem Kursleiter geleitet

## Übung 1: Generieren von Vektoreinbettungen mit Azure OpenAI

Um semantische Suchen durchzuführen, müssen Sie zunächst
Einbettungsvektoren aus einem Modell generieren, sie in einer
Vektordatenbank speichern und dann die Einbettungen abfragen. Sie
erstellen eine Datenbank, füllen sie mit Beispieldaten und führen
semantische Suchen für diese Einträge durch.

Am Ende dieser Übung verfügen Sie über eine flexible Azure Database for
PostgreSQL-Serverinstanz, für die die Erweiterungen vector und azure_ai
aktiviert sind. Sie werden Embeddings für die listings-Tabelle des
Seattle Airbnb Open Data-Datensatzes generieren. Sie führen auch
semantische Suchen für diese Auflistungen durch, indem Sie den
Einbettungsvektor einer Abfrage generieren und eine
Vektor-Cosine-Abstandssuche durchführen.

1.  Öffnen Sie einen Webbrowser, navigieren Sie zu
    \`\`https:\\portal.azure.com/\`\`, und melden Sie sich mit Ihren
    Azure-Anmeldeinformationen an.

2.  Wählen Sie auf der Symbolleiste des Azure-Portals das **Cloud
    Shell**-Symbol aus, um am unteren Rand des Browserfensters einen
    neuen Cloud Shell-Bereich zu öffnen. Wählen Sie **Bash** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image1.jpeg)

3.  Wählen Sie das Optionsfeld **No storage account required** aus,
    wählen Sie Ihr Abonnement aus, und klicken Sie dann auf **Apply**.

![](./media/image2.png)

4.  Führen Sie an der Cloud Shell-Prompt den folgenden Befehl aus, um
    das Projekt zu klonen

> \`\`git clone https://github.com/technofocus-pte/postgresql-case\`\`

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image3.jpeg)

5.  Navigieren Sie zum Projektordner.

**\`\`cd postgresql-case\`\`**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image4.jpeg)

6.  Als Nächstes führen Sie drei Befehle aus, um Variablen zu
    definieren, um redundante Eingaben bei der Verwendung von Azure
    CLI-Befehlen zum Erstellen von Azure-Ressourcen zu reduzieren. Die
    Variablen stellen den Namen dar, der Ihrer Ressourcengruppe
    (RG_NAME) zugewiesen werden soll, die Azure-Region (REGION), in der
    Ressourcen bereitgestellt werden, und ein zufällig generiertes
    Passwort für die PostgreSQL-Administratoranmeldung (ADMIN_PASSWORD).

7.  Im ersten Befehl ist die Region, die der entsprechenden Variablen
    zugewiesen ist, eastus oder westus **\[aber Sie können ihn durch
    einen Ort Ihrer Wahl ersetzen.\]{.mark}** Wenn Sie jedoch die
    Standardeinstellung ersetzen, müssen Sie eine andere \[Azure-Region,
    die abstrakte Zusammenfassung unterstützt\]{.underline} auswählen,
    um sicherzustellen, dass Sie alle Aufgaben in den Modulen in diesem
    Lernpfad ausführen können.

\`\`REGION=westus\`\`

8.  Mit dem folgenden Befehl wird der vorhandene Ressourcengruppenname
    zugewiesen, der für die Ressourcengruppe verwendet werden soll, in
    der alle in dieser Übung verwendeten Ressourcen gespeichert werden.

\`\`RG_NAME=Your existing resource group name\`\`

![](./media/image5.png)

9.  Der letzte Befehl generiert nach dem Zufallsprinzip ein Passwort für
    die PostgreSQL-Admin-Anmeldung. **Stellen Sie sicher, dass Sie es**
    an einen sicheren Ort **kopieren**, um es später für die Verbindung
    mit Ihrem PostgreSQL Flexible Server zu verwenden.

> a=()
>
> for i in {a..z} {A..Z} {0..9};
>
> do
>
> a\[$RANDOM\]=$i
>
> done
>
> ADMIN_PASSWORD=$(IFS=; echo "${a\[\*\]::18}")
>
> echo "Your randomly generated PostgreSQL admin user's password is:"

echo $ADMIN_PASSWORD

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image6.jpeg)

### Aufgabe 1: Zuweisen von Cognitive Services-Contributor

1.  Öffnen Sie einen neuen Tab und gehen Sie zu
    \`\`**https://portal.azure.com\`\`** . Melden Sie sich mit Ihren
    Azure-Credentials an, und klicken Sie dann auf die Kachel
    **Subscription**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image7.jpeg)

2.  Klicken Sie auf Abonnementname .

![](./media/image8.png)

3.  Klicken Sie im linken Navigationsmenü auf Access control (IAM).
    Klicken Sie auf **Add**, und wählen Sie **Add role assignment** aus.

![](./media/image9.png)

4.  Suchen Sie nach \`\`Cognitive Services Contributor\`\` , wählen Sie
    es aus und klicken Sie dann auf die Schaltfläche **Next**.

![Ein Screenshot eines Serviceauftrags Beschreibung wird automatisch
generiert](./media/image10.jpeg)

5.  Wählen Sie **User, group or service principal** aus, und klicken Sie
    auf den Link **select member**. Suchen Sie nach Ihrem
    Azure-Abonnementkonto, und wählen Sie es aus. Klicken Sie
    abschließend auf die Schaltfläche **Select**.

![](./media/image11.png)

6.  Klicken Sie auf die Schaltfläche **Review + assign**.

![](./media/image12.png)

7.  Klicken Sie erneut auf die Schaltfläche **Review + assign**.

> ![](./media/image13.png)

### Aufgabe 2: Ausführen des Bicep-Bereitstellungsskripts zum Bereitstellen von Azure-Ressourcen

1.  Wechseln Sie mit der Azure CLI zurück zur 1. Registerkarte des
    Azure-Portals, um ein Bicep-Bereitstellungsskript zum Bereitstellen
    von Azure-Ressourcen in Ihrer Ressourcengruppe auszuführen: Die
    Bereitstellung dauert 3 bis 5 Minuten.

''cd''

\`\`az deployment group create --resource-group $RG_NAME --template-file
"postgresql-case/Allfiles/Labs/Shared/deploy.bicep" --parameters
restore=false adminLogin=pgAdmin
adminLoginPassword=$ADMIN_PASSWORD\`\`![Ein Screenshot eines Computers
Beschreibung wird automatisch generiert](./media/image14.jpeg)

![Ein Computer-Screenshot eines schwarzen Bildschirms Beschreibung wird
automatisch generiert](./media/image15.jpeg)

2.  Das Bicep-Bereitstellungsskript stellt die Azure-Dienste, die zum
    Abschließen dieser Übung erforderlich sind, in Ihrer
    Ressourcengruppe bereit. Zu den bereitgestellten Ressourcen gehört
    ein flexibler Azure Database for PostgreSQL-Server. Sie können
    Ressourcen in Ihrer Ressourcengruppe überprüfen.

- **Azure OpenAI,**

- **Azure AI Language Service.**

![](./media/image16.png)

3.  Klicken Sie auf Open AI-Resource.

![](./media/image17.png)

4.  Klicken Sie im linken Navigationsmenü unter **Ressourcenverwaltung**
    auf **Keys and Endpoint**. Notieren Sie sich Schlüssel 1 und
    Endpunkt, um sie in Aufgabe 5 zu verwenden

![](./media/image18.png)

5.  Das Bicep-Skript führt auch einige Konfigurationsschritte aus, z. B.
    das Hinzufügen der azure_ai- und Vektorerweiterungen zur
    *Zulassungsliste* des PostgreSQL-Servers (über den Serverparameter
    azure.extensions), das Erstellen einer Datenbank mit dem Namen
    rentals auf dem Server und das Hinzufügen einer Bereitstellung mit
    dem Namen embedding mithilfe des **text-embedding-ada-002**-Modells
    zu Ihrem Azure OpenAI-Dienst.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image19.jpeg)

6.  Die Bereitstellung dauert in der Regel mehrere Minuten. Sie können
    sie über Cloud Shell überwachen oder zur Seite **Deployments** für
    die Ressourcengruppe navigieren, die Sie oben erstellt haben, und
    dort den Bereitstellungsfortschritt beobachten.

7.  Schließen Sie den Cloud Shell-Bereich, sobald die
    Ressourcenbereitstellung abgeschlossen ist.

### Aufgabe 3: Herstellen einer Verbindung mit Ihrer Datenbank mithilfe von psql in Azure Cloud Shell

In dieser Aufgabe stellen Sie mithilfe des Befehlszeilenprogramms psql
aus Azure Cloud Shell eine Verbindung mit der rentals-Datenbank auf
Ihrem Azure Database for PostgreSQL-Server her.

1.  Navigieren Sie im Azure-Portal (https://portal.azure.com/) zu Ihrem
    neu erstellten Azure Database for PostgreSQL – Flexible Server.

![](./media/image20.png)

1.  Wählen Sie in der Seitenleiste **Server-Parameters** aus. Suchen Sie
    nach dem Parameter **azure.extensions**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image21.png)

2.  Wählen Sie die Erweiterungen **Vector** und **AZURE_AI** aus, falls
    noch nicht ausgewählt.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image22.png)

3.  Wählen Sie im Ressourcenmenü unter **Settings** die Option
    **Databases** aus, wählen Sie **Connect** für die rentals-Datenbank
    aus.

![](./media/image23.png)

4.  Geben Sie an der Prompt " Password for user pgAdmin " in der Cloud
    Shell das zufällig generierte Passwort für die **pgAdmin**-Anmeldung
    ein .

Nach der Anmeldung wird die psql-Prompt für die rentals-Datenbank
angezeigt.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image24.jpeg)

5.  Im weiteren Verlauf dieser Übung arbeiten Sie weiterhin in Cloud
    Shell, sodass es hilfreich sein kann, den Bereich in Ihrem
    Browserfenster zu erweitern, indem Sie oben rechts im Bereich auf
    die Schaltfläche **Maximize** klicken.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image25.jpeg)

### Aufgabe 4: Konfigurieren von Erweiterungen

Zum Speichern und Abfragen von Vektoren und zum Generieren von
Einbettungen müssen Sie zwei Erweiterungen für Azure Database for
PostgreSQL Flexible Server auf die Zulassungsliste setzen und
aktivieren: vector und azure_ai.

1.  Wechseln Sie zurück auf der Registerkarte des Azure-Portals mit der
    Azure CLI, und führen Sie den folgenden SQL-Befehl aus, um die
    Vektorerweiterung zu aktivieren. Für detaillierte Anweisungen

\`\`CREATE EXTENSION vector;\`\`

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image26.jpeg)

2.  Um die Erweiterung azure_ai zu aktivieren, **aktualisieren Sie** den
    folgenden SQL-Befehl, **und führen Sie** **ihn aus**. Sie benötigen
    den Endpunkt und den API-Schlüssel für die Azure OpenAI-Ressource.

> \`\`CREATE EXTENSION azure_ai;\`\`
>
> \`\`SELECT azure_ai.set_setting('azure_openai.endpoint',
> 'https://\<endpoint\>.openai.azure.com');\`\`
>
> \`\`SELECT azure_ai.set_setting('azure_openai.subscription_key',
> '\<API Key\>');\`\`

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image27.jpeg)

### Aufgabe 5: Auffüllen der Datenbank mit Beispieldaten

Bevor Sie die Erweiterung "azure_ai" untersuchen, fügen Sie der
rentals-Datenbank einige Tabellen hinzu, und füllen Sie sie mit
Beispieldaten, damit Sie Informationen haben, mit denen Sie arbeiten
können, während Sie die Funktionalität der Erweiterung überprüfen.

1.  Führen Sie die folgenden Befehle aus, um die Tabellen **listings**
    und **reviews** zu erstellen, in denen Daten zu Mietobjekten und
    Kundenbewertungen gespeichert werden:

> DROP TABLE IF EXISTS listings;
>
> CREATE TABLE listings (
>
> id int,
>
> name varchar(100),
>
> description text,
>
> property_type varchar(25),
>
> room_type varchar(30),
>
> price numeric,
>
> weekly_price numeric
>
> );

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image28.jpeg)

DROP TABLE IF EXISTS reviews;

CREATE TABLE reviews (

id int,

listing_id int,

date date,

comments text

);

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image29.jpeg)

2.  Verwenden Sie als Nächstes den Befehl COPY, um Daten aus CSV-Dateien
    in jede Tabelle zu laden, die Sie oben erstellt haben. Führen Sie
    zunächst den folgenden Befehl aus, um die Listings-Tabelle zu
    füllen:

''\COPY listings FROM
'postgresql-case/Allfiles/Labs/Shared/listings.csv' CSV HEADER''

Die Befehlsausgabe sollte COPY 50 lauten, was bedeutet, dass 50 Zeilen
aus der CSV-Datei in die Tabelle geschrieben wurden.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image30.jpeg)

3.  Führen Sie abschließend den folgenden Befehl aus, um
    Kundenbewertungen in die Bewertungstabelle zu laden:

> \`\`\COPY reviews FROM
> 'postgresql-case/Allfiles/Labs/Shared/reviews.csv' CSV HEADER\`\`

Die Befehlsausgabe sollte COPY 354 lauten, was bedeutet, dass 354 Zeilen
aus der CSV-Datei in die Tabelle geschrieben wurden.

![](./media/image31.jpeg)

4.  Um Ihre Beispieldaten zurückzusetzen, können Sie DROP
    TABLE-Auflistungen ausführen und diese Schritte wiederholen.

### Aufgabe 6: Erstellen und Speichern von Einbettungsvektoren

Nachdem wir nun einige Beispieldaten haben, ist es an der Zeit, die
Einbettungsvektoren zu generieren und zu speichern. Die azure_ai
Erweiterung erleichtert das Aufrufen der Azure OpenAI-Einbettungs-API.

1.  Fügen Sie die Spalte für den Einbettungsvektor hinzu.

Das Modell text-embedding-ada-002 ist so konfiguriert, dass es 1.536
Dimensionen zurückgibt, also verwenden Sie dies für die Größe der
Vektorspalten.

\`\`ALTER TABLE listings ADD COLUMN listing_vector vector(1536);\`\`

![Ein Computer-Screenshot eines schwarzen Bildschirms Beschreibung wird
automatisch generiert](./media/image32.jpeg)

2.  Generieren Sie einen Einbettungsvektor für die Beschreibung der
    einzelnen Auflistungen, indem Sie Azure OpenAI über die
    create_embeddings benutzerdefinierte Funktion aufrufen, die von der
    azure_ai Erweiterung implementiert wird:

UPDATE listings SET listing_vector =
azure_openai.create_embeddings('embedding', description, max_attempts
=\> 5, retry_delay_ms =\> 500) WHERE listing_vector IS NULL;

Beachten Sie, dass dies je nach verfügbarem Kontingent einige Minuten
dauern kann.

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
generiert](./media/image33.png)

### Aufgabe 7: Ausführen einer semantischen Suchabfrage

Jetzt, da Sie über Auflistungsdaten verfügen, die mit
Einbettungsvektoren angereichert sind, ist es an der Zeit, eine
semantische Suchabfrage auszuführen. Rufen Sie dazu den
Einbettungsvektor der Abfragezeichenfolge ab, und führen Sie dann eine
Kosinussuche durch, um die Auflistungen zu finden, deren Beschreibungen
der Abfrage semantisch am ähnlichsten sind.

1.  Verwenden Sie die Einbettung in eine Kosinussuche (\\=\> stellt den
    Kosinusabstandsvorgang dar) und rufen Sie die Top 10 der ähnlichsten
    Auflistungen zur Abfrage ab.

> SELECT id, name FROM listings ORDER BY listing_vector \<=\>
> azure_openai.create_embeddings('embedding', 'bright natural
> light')::vector LIMIT 10;

Sie erhalten ein ähnliches Ergebnis. Die Ergebnisse können variieren, da
nicht garantiert werden kann, dass die Einbettungsvektoren
deterministisch sind:

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image34.jpeg)

2.  Sie können die Beschreibungsspalte auch projizieren, um den Text der
    übereinstimmenden Zeilen lesen zu können, deren Beschreibungen
    semantisch ähnlich waren. Diese Abfrage gibt z. B. die beste
    Übereinstimmung zurück:

> SELECT id, description FROM listings ORDER BY listing_vector \<=\>
> azure_openai.create_embeddings('embedding', 'bright natural
> light')::vector LIMIT 1;

Das druckt so etwas wie:

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image35.jpeg)

Um die semantische Suche intuitiv zu verstehen, beachten Sie, dass die
Beschreibung nicht die Begriffe "bright" oder "natural" enthält. Aber es
hebt "summer" und "sunlight", "windows" und ein " ceiling window "
hervor.

### Aufgabe 8 : Überprüfen Sie Ihre Arbeit

Nachdem Sie die obigen Schritte ausgeführt haben, enthält die
Listings-Tabelle Beispieldaten von Seattle Airbnb Open Data auf Kaggle.
Die Listings wurden um Einbettungsvektoren erweitert, um semantische
Suchen durchzuführen.

1.  Vergewissern Sie sich, dass die Listings-Tabelle vier Spalten
    enthält: ID, Name, Beschreibung und listing_vector.

> \`\`\d listings\`\`

Es sollte etwas ausgedruckt werden wie:

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image36.jpeg)

2.  Vergewissern Sie sich, dass mindestens eine Zeile über eine
    ausgefüllte listing_vector Spalte verfügt.

> \`\`SELECT COUNT(\*) \> 0 FROM listings WHERE listing_vector IS NOT
> NULL;\`\`

Das Ergebnis muss ein t anzeigen, was wahr bedeutet. Ein Hinweis darauf,
dass mindestens eine Zeile mit Einbettungen der entsprechenden
Beschreibungsspalte vorhanden ist:

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image37.jpeg)

3.  Vergewissern Sie sich, dass der Einbettungsvektor 1536 Dimensionen
    hat:

\`\`SELECT vector_dims(listing_vector) FROM listings WHERE
listing_vector IS NOT NULL LIMIT 1;\`\`

Yielding:

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image38.jpeg)

4.  Vergewissern Sie sich, dass semantische Suchen Ergebnisse
    zurückgeben.

Verwenden Sie die Einbettung in eine Kosinussuche, um die Top 10 der
Listen abzurufen, die der Abfrage am ähnlichsten sind.

\`\`SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;\`\`

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image39.jpeg)

5.  Bleiben Sie auf derselben Seite, um mit der nächsten Aufgabe
    fortzufahren.

## Übung 2 - Erstellen einer Suchfunktion für ein Empfehlungssystem

Lassen Sie uns die Vektoreinbettung-Logic und die API-Aufrufe in eine
Funktion einschließen. In dieser Übung installieren Sie die
Erweiterungen vector und azure_ai auf einem flexiblen Azure Database for
PostgreSQL-Server und erkunden die Funktionen der Erweiterung für die
Integration [von Azure
OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/overview)
in Ihre Datenbank.

### Aufgabe 1 : Erstellen einer Suchfunktion für ein Empfehlungssystem

Lassen Sie uns ein Empfehlungssystem mit semantischer Suche erstellen.
Das System empfiehlt mehrere Auflistungen basierend auf einer
bereitgestellten Beispielliste. Das Beispiel kann aus dem Angebot
stammen, das der Benutzer anzeigt, oder aus seinen Präferenzen. Wir
implementieren das System als PostgreSQL-Funktion und nutzen die
Erweiterung azure_openai.

Am Ende dieser Übung haben Sie eine Funktion recommend_listing
definiert, die höchstens numResults-Auflistungen bereitstellt, die der
angegebenen sampleListingId am ähnlichsten sind. Sie können diese Daten
nutzen, um neue Verkaufschancen zu nutzen, z. B. das Verknüpfen von
empfohlenen Auflistungen mit ermäßigten Auflistungen.

Bereitstellen von Ressourcen in Ihrem Azure-Abonnement

Dieser Schritt führt Sie durch die Verwendung von Azure CLI-Befehlen aus
Azure Cloud Shell, um eine Ressourcengruppe zu erstellen und ein
Bicep-Skript auszuführen, um die Azure-Dienste bereitzustellen, die zum
Abschließen dieser Übung in Ihrem Azure-Abonnement erforderlich sind.

**Hinweis:** Wenn Sie mehrere Module in diesem Lernpfad absolvieren,
können Sie die Azure-Umgebung für sie freigeben. In diesem Fall müssen
Sie diesen Schritt zur Ressourcenbereitstellung nur einmal ausführen.

### Aufgabe 2 : Erstellen der Recommendation-Funktion

1.  Die Recommendation-Funktion verwendet eine sampleListingId und gibt
    die numResults zurück, die anderen Auflistungen am ähnlichsten sind.
    Zu diesem Zweck wird eine Einbettung des Namens und der Beschreibung
    des Beispieleintrags erstellt, und es wird eine semantische Suche
    dieses Abfragevektors anhand der Einbettungen des Auflistungsschemas
    durchgeführt.

> CREATE FUNCTION
>
> recommend_listing(sampleListingId int, numResults int)
>
> RETURNS TABLE(
>
> out_listingName text,
>
> out_listingDescription text,
>
> out_score real)
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

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image40.jpeg)

### Aufgabe 3 : Abfragen der Recommendation-Funktion

1.  Um die Empfehlungsfunktion abzufragen, übergeben Sie ihr eine
    Listing-ID und die Anzahl der Empfehlungen, die sie geben soll.

> select out_listingName, out_score from recommend_listing( (SELECT id
> from listings limit 1), 20); -- search for 20 listing recommendations
> closest to a listing

Das Ergebnis sieht in etwa so aus:

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image41.jpeg)

2.  Um die Funktionslaufzeit anzuzeigen, stellen Sie sicher, dass
    **track_functions** im Azure-Portal im Abschnitt **Server
    Parameters** aktiviert ist (Sie können PL oder ALL verwenden):

![](./media/image42.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image43.png)

### Aufgabe 4 : Überprüfen Sie Ihre Arbeit

1.  Stellen Sie sicher, dass die Funktion mit der richtigen Signatur
    vorhanden ist:

''\df recommend_listing''

Folgendes sollte angezeigt werden:

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image44.jpeg)

2.  Stellen Sie sicher, dass Sie es mit der folgenden Abfrage abfragen
    können:

> select out_listingName, out_score from recommend_listing( (SELECT id
> from listings limit 1), 20); -- search for 20 listing recommendations
> closest to a listing

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image45.jpeg)

### Aufgabe 5 : Aufräumen

Nachdem Sie diese Übung abgeschlossen haben, löschen Sie die von Ihnen
erstellten Azure-Ressourcen. Ihnen wird die konfigurierte Kapazität in
Rechnung gestellt, nicht die Auslastung der Datenbank. Befolgen Sie
diese Anweisungen, um Ihre Ressourcengruppe und alle Ressourcen, die Sie
für dieses Lab erstellt haben, zu löschen.

1.  Suchen Sie auf der Startseite nach **Azure Open AI,** und wählen Sie
    es aus.

![](./media/image46.png)

2.  Wählen Sie die Open AI-Ressource aus und klicken Sie dann auf
    **Delete** .

![](./media/image47.png)

3.  Geben Sie **delete** in das Textfeld ein und klicken Sie dann auf
    **\*\*Delete.** Bestätigen Sie den Löschvorgang.

![](./media/image48.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image49.png)

4.  Klicken Sie auf **Manage deleted resources**, wählen Sie die
    Ressource aus und klicken Sie dann auf die Schaltfläche **Purge**,
    wie in der Abbildung unten gezeigt.

![](./media/image50.png)

5.  Bestätigen Sie die Bereinigung(Purge) mit einem Klick auf **Yes**.

![](./media/image51.png)

6.  Wählen Sie auf der Startseite unter Azure-Services die Option
    **Resource groups** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image52.jpeg)

7.  Klicken Sie auf Name der Ressourcengruppe.

![](./media/image53.png)

8.  Wählen Sie auf der Seite **Overview** Ihrer Ressourcengruppe **die
    gesamte Ressource** aus, und klicken Sie dann auf **Delete . Löschen
    Sie RESOURCE-Gruppe NICHT.**

> ![](./media/image54.png)

9.  Geben Sie **Delete** ein und klicken Sie auf Delete. Bestätigen Sie
    das Löschen der Ressource, indem Sie auf die Schaltfläche **Delete**
    klicken .

![](./media/image55.png)

**Zusammenfassung**:

Sie haben gelernt, wie Sie die semantische Suche in Azure Database for
PostgreSQL Flexible Server verwenden, um Abfragen mithilfe von
Einbettungen durchzuführen, die von Azure OpenAI generiert wurden. Sie
haben diese Suche durchgeführt durch, indem Sie:

- Aktivieren der Erweiterungen vector und azure_ai.

- Erstellen von Vektorspalten zum Speichern von Einbettungen.

- Generieren und Speichern von Einbettungen.

- Abfragen der Datenbank mithilfe eines Abfragevektors.
