# Anwendungsfall 11 - Erstellen eines Copiloten mit Azure OpenAI, Azure Cosmos DB für NoSQL

In diesem Anwendungsfall verbinden Sie eine Blazor-Webanwendung mithilfe
von .NET Software Development Kits mit Azure Cosmos DB für NoSQL und
Azure OpenAI. Ihr Code verwaltet und fragt Elemente in einem API für
NoSQL-Container ab. Ihr Code sendet auch Eingabeaufforderungen an Azure
OpenAI und analysiert die Antworten.

**Dauer des Labs:** 45 Minuten

**Labtyp**: Von einem Kursleiter geleitet

**Ziel**

1.  Einrichten der Entwicklungsumgebung für Blazor, PostgreSQL und
    OpenAI.

2.  Um ein Blazor-Projekt zu erstellen und eine responsive
    Chat-Oberfläche zu entwerfen.

3.  So konfigurieren Sie die PostgreSQL-Datenbank in Azure und verbinden
    sie mit der Blazor-App.

4.  Integration von Azure OpenAI für erweiterte Chatfunktionen.

5.  Zum Bereitstellen der Blazor-Anwendung und der PostgreSQL-Datenbank
    in Azure.

6.  Um die Anwendung zu testen, um ein nahtloses Zusammenspiel zwischen
    den Komponenten zu gewährleisten.

7.  Zum Überwachen und Beheben von Problemen mit der bereitgestellten
    Anwendung in Azure.

**Verwendete Schlüsseltechnologien:** Azure Cosmos DB für NoSQL, Azure
OpenAI

## Übung 0: Grundlegendes zum virtuellen Computer und den Anmeldeinformationen

In dieser Aufgabe identifizieren und verstehen wir die
Anmeldeinformationen, die wir im gesamten Lab verwenden werden.

1.  Auf der Registerkarte **Instructions** befindet sich der
    Laborleitfaden mit den Anweisungen, die im gesamten Lab zu befolgen
    sind.

&nbsp;

1.  Die Registerkarte **Resources** enthält die Anmeldeinformationen,
    die zum Ausführen des Labs erforderlich sind.

    - **URL** – URL zum Azure-Portal

    - **Subscription** – Dies ist die ID des Abonnements, das Ihnen
      zugewiesen wurde

    - **Username** – Die Benutzer-ID, mit der Sie sich bei den
      Azure-Diensten anmelden müssen.

    1.  **Password** – Kennwort für die Azure-Anmeldung. Nennen wir
        diesen Benutzernamen und dieses Kennwort als
        Azure-Anmeldeinformationen. Wir werden diese Creds überall dort
        verwenden, wo wir Azure-Anmeldeinformationen erwähnen.

    - **Resource Group** – Die **Resource Group**, die Ihnen zugewiesen
      ist.

\[!Alert\] **Wichtig:** Stellen Sie sicher, dass Sie alle Ressourcen
unter dieser Ressourcengruppe erstellen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

1.  Die Registerkarte **Help** enthält die Supportinformationen. Der
    **ID-**Wert ist hier die **Lab instance ID**, die während der
    Lab-Ausführung verwendet wird.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## Übung 1: Bereitstellen der Infrastruktur und Abschließen der Ersteinrichtung

Zum Abschließen dieses Projekts benötigen Sie ein Azure Cosmos DB für
NoSQL-Konto und ein Azure OpenAI-Konto. Um diesen Prozess zu optimieren,
stellen Sie eine Bicep-Vorlage mit diesen beiden Konten in Azure bereit.

### Aufgabe 1: Bereitstellen der Infrastruktur aus der Vorlage

1.  Öffnen Sie die Datei aus dem Pfad **C:\Labfiles\Build, und testen
    Sie eine benutzerdefinierte Chatanwendung mit Azure Cosmos DB und
    AzureOpenAI**, und aktualisieren Sie die Azure OpenAI-Version in
    Zeile 96 auf +++0125+++. **Speichern Sie** die Datei.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

1.  Öffnen Sie einen neuen Browser, und geben Sie die folgende URL in
    die Adressleiste ein: +++<https://portal.azure.com/+++>, um das
    Azure-Portal zu öffnen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

1.  Klicken Sie im Azure-Portal auf die **Schaltfläche \[\>\_\] (Cloud
    Shell)** oben auf der Seite rechts neben dem Suchfeld. Am unteren
    Rand des Portals wird ein Cloud Shell-Bereich geöffnet. Wenn Sie
    Cloud Shell zum ersten Mal öffnen, werden Sie möglicherweise
    aufgefordert, den Typ der Shell auszuwählen, die Sie verwenden
    möchten (**Bash** oder **PowerShell**). Wählen Sie **Bash** aus.
    Wenn diese Option nicht angezeigt wird, überspringen Sie diesen
    Schritt.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

2.  Wählen Sie im Dialogfeld **Getting Started** die Option **Mount
    storage account** aus, Wählen Sie Ihre **Subscription** aus und
    klicken Sie dann auf **Apply**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

3.  Wählen Sie im **Dialogfeld Mount storage account** die Option **we
    will create a storage account for you** und klicken Sie
    auf **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

![A close-up of a computer screen AI-generated content may be
incorrect.](./media/image9.jpeg)

1.  Stellen Sie sicher, dass der oben links im Cloud Shell-Bereich
    angegebene Shell-Typ auf **Bash** umgeschaltet ist. Wenn es sich um
    **PowerShell** handelt, wechseln Sie über das Dropdownmenü zu
    **Bash**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.jpeg)

4.  Sobald das Terminal gestartet ist, klicken Sie auf **Manage files
    -\> Upload**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

5.  Wählen Sie **azuredeploy aus. JSON-Datei** aus dem Pfad
    **C:\Labfiles\Build and Test a custom chat application Using Azure
    Cosmos DB and AzureOpenAI** und wählen Sie **Open** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

Sie sollten eine Erfolgsmeldung für den Datei-Upload erhalten.

![A white background with black text AI-generated content may be
incorrect.](./media/image13.jpeg)

6.  Erstellen Sie eine neue Shellvariable mit dem Namen
    **resourceGroupName** mit dem Namen der Azure-Ressourcengruppe, die
    Sie erstellen (mslearn-cosmos-openai).

+++resourceGroupName="ResourceGroup1"+++(Abrufen des Namens der
Ressourcengruppe auf der Registerkarte Ressourcen)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

7.  Stellen Sie die **azuredeploy.json** Vorlagendatei mit az group
    deployment create in der Ressourcengruppe bereit. Führen Sie dann
    den folgenden Befehl aus.

+++az deployment group create --resource-group $resourceGroupName --name
zero-touch-deployment --template-file azuredeploy.json+++

**Anmerkung:** Diese Bereitstellung kann ca. 5 bis 10 Minuten dauern.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image15.jpeg)

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image16.jpeg)

### Aufgabe 2: Abrufen von Anmeldeinformationen für Azure Cosmos DB für NoSQL- und Azure OpenAI-Konten

Bei der obigen Bereitstellung wurden Azure Cosmos DB für NoSQL- und
Azure OpenAI-Konten bereitgestellt und deren Anmeldeinformationen dann
in der Konfiguration der Azure App Service-Web-App gespeichert. Jetzt
haben Sie die Wahl, die Anmeldeinformationen für jeden Dienst über das
Azure-Portal oder die Azure CLI abzurufen.

1.  Klicken Sie auf der Startseite des Azure-Portals auf **Resource
    groups.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

2.  Wählen Sie Ihre Ressourcengruppe aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

1.  Erweitern Sie auf der Seite **Ressource Groups** den Bereich
    **Essentials**, und beachten Sie die Kopfzeile Deployments. Der
    Status für die Bereitstellung sollte zu diesem Zeitpunkt
    **Succeeded** sein.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

3.  Wählen Sie nun das **Azure Cosmos DB-Konto** aus, um zur Seite der
    Ressource zu navigieren.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

1.  Wählen Sie die Option **Keys** im Abschnitt **Settings** des
    Ressourcennavigationsmenüs aus. Notieren Sie sich den Wert der
    Felder **URI** und **PRIMARY KEY**. Sie verwenden diese Werte
    später.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.jpeg)

4.  Kehren Sie zur Seite **Resource Groups** zurück. Wählen Sie das
    **Azure OpenAI-Konto** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

1.  Navigieren Sie in Ihrem **Azure Open AI-**Fenster zum Abschnitt
    **Resource Management,** und klicken Sie auf **Keys and Endpoints.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.jpeg)

5.  Kopieren Sie auf der Seite **Keys and Endpoints** \> **KEY1** (*Sie
    können entweder KEY1 oder KEY2 verwenden)* und **Endpoint**, und
    **speichern** Sie dann den Editor, um die Informationen in den
    anstehenden Aufgaben zu verwenden.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

### Aufgabe 3: Ausführen von Docker

1.  Geben Sie in Ihrem Windows-Suchfeld +++Docker+++ ein und klicken Sie
    dann auf **Docker Desktop**.

![A screenshot of a desktop AI-generated content may be
incorrect.](./media/image25.jpeg)

2.  Ausführen des Docker-Desktops.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.jpeg)

## Übung 2: Einrichten und Erstellen der Startanwendung

1.  Suchen Sie in der VM-Suchleiste nach +++Visual Studio+++, und wählen
    Sie **Visual Studio Code aus.**

2.  Klicken Sie **File** -\> **Open Folder**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

3.  Wählen Sie **cosmosdb-chatgpt** aus **C:\LabFiles** und klicken Sie
    auf **Select Folder**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

4.  Klicken Sie auf **Yes, I trust the authors** in der Option **Do you
    trust the authors dialog** ![A screenshot of a computer AI-generated
    content may be incorrect.](./media/image29.jpeg)

5.  Klicken Sie im **Visual Studio Code** -Editor auf **Terminal,**
    öffnen Sie ein **new Terminal**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

1.  In einer .NET-Anwendung ist es üblich, die Konfigurationsanbieter zu
    verwenden, um neue Einstellungen in die Anwendung einzufügen.
    Verwenden Sie für diese Anwendung die **appsettings.
    Development.json** Datei, um die aktuellen Werte für den Azure
    OpenAI-Endpunkt und -Schlüssel bereitzustellen.

&nbsp;

1.  Öffnen Sie die **appsettings.Development.JSON**  Datei. Ersetzen Sie
    die Platzhalter für URI und Schlüsselwerte von **Azure Cosmos DB-**
    und **Azure OpenAI-Ressourcen** in der Datei durch die Werte, die
    wir zuvor gespeichert haben.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

6.  **Erstellen Sie** das .NET-Projekt, indem Sie den folgenden Befehl
    ausführen.

+++dotnet build+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image32.jpeg)

## Übung 3: Grundlegendes zum Code

### Aufgabe 1: Hinzufügen der erforderlichen Member und einer Clientinstanz

1.  Öffnen Sie die **Datei Services/OpenAiService.cs**. Diese Datei
    implementiert die Klassenvariablen, die für die Verwendung des Azure
    OpenAI-Clients erforderlich sind. Es implementiert einige statische
    Eingabeaufforderungen und erstellt eine neue Instanz der
    OpenAIClient-Klasse.

&nbsp;

1.  Dieser Codeblock erstellt eine neue Zeichenfolgenvariable mit dem
    Namen \_systemPromptText mit einem statischen Textblock, der vor
    jeder Eingabeaufforderung an den AI-Assistenten gesendet wird.

> private readonly-Zeichenkette \_systemPrompt = @"
>
> Du bist ein AI-Assistent, der Menschen hilft, Informationen zu finden.
>
> Geben Sie prägnante Antworten, die höflich und professionell sind." +
> Umwelt.NewLine;

1.  Dieser Codeblock erstellt eine weitere neue Zeichenfolgenvariable
    mit dem Namen \_summarizePrompt mit einem statischen Textblock, der
    mit Anweisungen zum Zusammenfassen einer Konversation an den
    AI-Assistenten gesendet wird.

> private readonly Zeichenkette \_summarizePrompt = @"
>
> Fassen Sie diese Eingabeaufforderung in ein oder zwei Wörtern
> zusammen, um sie als Beschriftung in einer Schaltfläche auf einer
> Webseite zu verwenden.
>
> Verwenden Sie keine Interpunktion." + Umgebung.NewLine;

1.  Dieser Codeblock erstellt eine neue Instanz der OpenAIClient-Klasse,
    wobei der Endpunkt zum Erstellen eines URI und der Schlüssel zum
    Erstellen eines AzureKeyCredential verwendet werden.

> Uri uri = new(endpoint);
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

**Aufgabe 2: Stellen Sie dem AI-Modell eine Frage**

Implementieren Sie zunächst eine Frage-Antwort-Konversation, indem Sie
eine Systemansage, eine Frage und eine Sitzungs-ID senden, damit das
AI-Modell eine Antwort im Kontext der aktuellen Konversation
bereitstellen kann. Stellen Sie sicher, dass Sie die Anzahl der Token
messen, die zum Analysieren der Eingabeaufforderung und zum Zurückgeben
einer Antwort (oder eines Abschlusses in diesem Kontext) erforderlich
sind.

1.  Dieser Codeblock erstellt eine neue Variable mit dem Namen options
    vom Typ ChatCompletionsOptions. Fügt die beiden Nachrichtenvariablen
    der Liste Messages hinzu und legt den Wert von User auf den
    sessionId-Konstruktorparameter fest.

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
> Temperature = 0.3f,
>
> NucleusSamplingFactor = 0.5f,
>
> FrequencyPenalty = 0,
>
> PresencePenalty = 0
>
> };

2.  Die GetChatCompletionsAsync-Methode der Azure OpenAI-Clientvariablen
    (\_client) wird asynchron aufgerufen. Das Ergebnis wird in einer
    Variablen mit dem Namen completions vom Typ ChatCompletions
    gespeichert.

> Response\<ChatCompletions\> completionsResponse =
> await_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;

3.  Schließlich gibt der folgende Codeblock ein Tupel als Ergebnis der
    GetChatCompletionAsync-Methode mit dem Inhalt der Vervollständigung
    als Zeichenfolge, der Anzahl der Token, die der Eingabeaufforderung
    zugeordnet sind, und der Anzahl der Token für die Antwort zurück.

> return (
>
> completionText: completions.Choices\[0\].Message.Content,
>
> completionTokens: completions.Usage.CompletionTokens
>
> );

**Aufgabe 3: Bitten Sie das AI-Modell, eine Konversation
zusammenzufassen**

Senden Sie nun dem AI-Modell eine andere Systemansage, Ihre aktuelle
Konversation und die Sitzungs-ID, damit das AI-Modell die Konversation
in ein paar Worten zusammenfassen kann.

1.  Mit dem folgenden Code wird eine ChatCompletionsOptions-Variable mit
    dem Namen options erstellt, wobei die beiden Nachrichtenvariablen in
    der Nachrichtenliste, User auf den sessionId-Konstruktorparameter,
    MaxTokens auf 200 und die restlichen Eigenschaften festgelegt sind.

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
> Temperature = 0.0f,
>
> NucleusSamplingFactor = 1.0f,
>
> FrequencyPenalty = 0,
>
> PresencePenalty = 0
>
> };

2.  Der folgende Code ruft die \_client auf. GetChatCompletionsAsynchron
    mit dem Modellnamen (\_modelName) und der Variablen options als
    Parameter und speichert das Ergebnis in einer Variablen mit dem
    Namen completions vom Typ ChatCompletions. Der Inhalt der
    Vervollständigung wird als Zeichenfolge als Ergebnis der
    SummarizeAsync-Methode zurückgegeben.

> Response\<ChatCompletions\> completionsResponse = await
> \_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;
>
> string completionText = completions.Choices\[0\].Message.Content;
>
> return completionText;

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image33.jpeg)

**Aufgabe 4: Herstellen einer Verbindung mit Azure Cosmos DB für NoSQL**

Die CosmosDbService-Klasse enthält eine Stubimplementierung eines
Diensts, der der OpenAiService-Klasse ähnelt, an der Sie zuvor in diesem
Modul gearbeitet haben. Im Gegensatz dazu wird in dieser Klasse das .NET
SDK für Azure Cosmos DB verwendet, das etwas anders funktioniert.

In diesem Abschnitt wird die Implementierung der Klassenvariablen und
des Clients erläutert, die für den Zugriff auf Azure Cosmos DB für NoSQL
mithilfe des Clients erforderlich sind.

1.  Öffnen Sie die **Datei** Services/CosmosDbService.cs.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image34.jpeg)

2.  Der folgende Code erstellt eine Variable mit dem Namen options vom
    Typ CosmosSerializationOptions und legt die
    PropertyNamingPolicy-Eigenschaft der Variablen auf
    CosmosPropertyNamingPolicy.CamelCase fest.

> CosmosSerializationOptions options = new()
>
> {
>
> PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
>
> };

**Anmerkung:** Durch das Festlegen dieser Eigenschaft wird
sichergestellt, dass der vom SDK erzeugte JSON-Code sowohl serialisiert
als auch deserialisiert wird, unabhängig davon, wie die entsprechende
Eigenschaft in der .NET-Klasse geschrieben wird.

1.  Mit dem folgenden Code wird eine neue Instanz vom Typ CosmosClient
    mit dem benannten Client erstellt, wobei die
    CosmosClientBuilder-Klassen-, Endpunkt-, Schlüssel- und
    Serialisierungsoptionen verwendet werden, die Sie zuvor angegeben
    haben.

> CosmosClient client = new CosmosClientBuilder(endpoint, key)
>
> .WithSerializerOptions(options)
>
> .Build();

3.  Mit dem folgenden Code wird eine neue NULL-zulässige Variable vom
    Typ Database mit dem Namen database erstellt, indem die
    GetDatabase-Methode der Clientvariablen aufgerufen wird.

**Database? database = client?.GetDatabase(databaseName);**

4.  Der folgende Code weist die Containervariable des Konstruktors nur
    dann der Variablen \_container Klasse zu, wenn sie nicht NULL ist.
    Wenn es null ist, lösen Sie eine ArgumentException aus.

> \_container = container ??
>
> throw new ArgumentException("Unable to connect to existing Azure
> Cosmos DB container or database.");

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.jpeg)

**Aufgabe 5: Implementieren des Azure Cosmos DB für NoSQL-Diensts**

Der Azure Cosmos DB-Dienst (CosmosDbService) verwaltet das Abfragen,
Erstellen, Löschen und Aktualisieren von Sitzungen und Nachrichten in
Ihrer AI-Assistentenanwendung. Um all diese Vorgänge zu verwalten, muss
der Dienst mehrere Methoden für jeden potenziellen Vorgang mithilfe
verschiedener Features des .NET SDK implementieren.

Es gibt mehrere wichtige Anforderungen, die in dieser Übung zu
bewältigen sind:

1.  Implementieren von Vorgängen zum Erstellen einer Sitzung oder
    Nachricht

2.  Implementieren von Abfragen zum Abrufen mehrerer Sitzungen oder
    Nachrichten

3.  Implementieren eines Vorgangs zum Aktualisieren einer einzelnen
    Sitzung oder zum Aktualisieren mehrerer Nachrichten im Batch

4.  Implementieren eines Vorgangs zum Abfragen und Löschen mehrerer
    verwandter Sitzungen und Nachrichten

Azure Cosmos DB für NoSQL speichert Daten im JSON-Format, sodass wir
viele Arten von Daten in einem einzigen Container speichern können.
Diese Anwendung speichert sowohl eine Chat-"Sitzung" mit dem
AI-Assistenten als auch die einzelnen "Nachrichten" innerhalb jeder
Sitzung. Mit der API für NoSQL kann die Anwendung beide Datentypen im
selben Container speichern und dann anhand eines einfachen Typfelds
zwischen diesen Typen unterscheiden.

1.  Öffnen Sie die **Datei Services/CosmosDbService.cs** .

2.  Mit dem folgenden Code wird eine neue Variable mit dem Namen
    partitionKey vom Typ PartitionKey erstellt, wobei die
    SessionId-Eigenschaft der aktuellen Sitzung als Parameter verwendet
    wird.

**PartitionKey partitionKey = new(session.SessionId);**

3.  Der folgende Code ruft die CreateItemAsync-Methode des Containers
    auf und übergibt den Sitzungsparameter und die
    partitionKey-Variable. Gibt die Antwort als Ergebnis der
    InsertSessionAsync-Methode zurück.

> return await \_container.CreateItemAsync\<Session\>(
>
> item: session,
>
> partitionKey: partitionKey
>
> );

1.  Mit dem folgenden Code wird eine PartitionKey-Variable mithilfe von
    session erstellt. SessionId als Wert des Partitionsschlüssels.
    Erstellt eine neue Nachrichtenvariable mit dem Namen newMessage,
    wobei die Timestamp-Eigenschaft auf den aktuellen UTC-Zeitstempel
    aktualisiert wird. Rufen Sie CreateItemAsync auf, und übergeben Sie
    sowohl die neue Nachricht als auch die Partitionsschlüsselvariablen.
    Gibt die Antwort als Ergebnis von InsertMessageAsync zurück.

> PartitionKey partitionKey = new(message.SessionId);
>
> Message newMessage = message with { TimeStamp = DateTime.UtcNow };
>
> return await \_container.CreateItemAsync\<Message\>(
>
> item: newMessage,
>
> partitionKey: partitionKey
>
> );

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image36.jpeg)

**Aufgabe 6: Abrufen mehrerer Sitzungen oder Nachrichten**

Es gibt zwei Hauptanwendungsfälle, in denen die Anwendung mehrere
Elemente aus unserem Container abrufen muss. Zunächst ruft die Anwendung
alle Sitzungen für den aktuellen Benutzer ab, indem sie die Elemente
nach Elementen filtert, bei denen type = Session ist. Zweitens ruft die
Anwendung alle Nachrichten für eine Sitzung ab, indem sie einen
ähnlichen Filter ausführt, wobei type = Session & sessionId = . Beide
Abfragen werden hier mit dem .NET SDK und einem Feediterator
implementiert.

1.  Mit dem folgenden Code wird eine neue Variable mit dem Namen query
    vom Typ QueryDefinition erstellt. Sie verwendet die fluente
    WithParameter-Methode, um den Namen der Session-Klasse als Wert für
    den Parameter zuzuweisen. Ruft dann die generische
    GetItemQueryIterator\<\>Methode für die \_container Variable auf und
    übergibt den generischen Typ Session und die Abfragevariable als
    Parameter. Speichern Sie das Ergebnis in einer Variablen vom Typ
    FeedIterator mit dem Namen response.

> QueryDefinition query = new QueryDefinition("SELECT DISTINCT \* FROM c
> WHERE c.type = @type")
>
> .WithParameter("@type", nameof(Session));
>
> FeedIterator\<Session\> response =
> \_container.GetItemQueryIterator\<Session\>(query);

1.  Der folgende Code in der while-Schleife ruft asynchron die nächste
    Seite mit Ergebnissen ab, indem ReadNextAsync für die
    Antwortvariable aufgerufen wird, und fügt diese Ergebnisse dann der
    Listenvariablen mit dem Namen output hinzu. Außerhalb der
    while-Schleife wird die Ausgabevariable mit einer Liste von
    Sitzungen als Ergebnis der GetSessionsAsync-Methode zurückgegeben.

> FeedResponse\<Session\> results = await response.ReadNextAsync();
>
> output.AddRange(results);
>
> return output;

2.  Im folgenden Code wird die fluente WithParameter-Methode verwendet,
    um den @sessionId Parameter dem als Parameter übergebenen
    Sitzungsbezeichner und den @type Parameter dem Namen der
    Message-Klasse zuzuweisen.

> QueryDefinition query = new QueryDefinition("SELECT \* FROM c WHERE
> c.sessionId = @sessionId AND c.type = @type")
>
> .WithParameter("@sessionId", sessionId)
>
> .WithParameter("@type", nameof(Message));

3.  Erstellen Sie eine FeedIterator\<Message-\> mithilfe der
    Abfragevariablen und der GetItemQueryIterator\<\>-Methode.

FeedIterator response = \_container.GetItemQueryIterator(query);

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image37.jpeg)

## Übung 4: Ausführen der App

Jetzt verfügt Ihre Anwendung über eine vollständige Implementierung von
Azure OpenAI und Azure Cosmos DB. Sie können die Anwendung durchgängig
testen, indem Sie die Lösung debuggen.

1.  Erstellen Sie das Projekt im **Visual Studio Code Terminal** mit dem
    folgenden Befehl.

+++**dotnet build**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.jpeg)

2.  Starten Sie die Anwendung mit aktiviertem Hot-Reloads mithilfe von
    dotnet watch.

+++**dotnet watch run --non-interactive**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

3.  Visual Studio Code startet den einfachen Browser im Tool, während
    die Webanwendung ausgeführt wird. Erstellen Sie in der Webanwendung
    eine neue Chat-Sitzung, indem Sie auf **+ N+ Create New
    Chat** klicken und stellen Sie dem AI-Assistenten eine Frage.
    Schließen Sie dann die ausgeführte Webanwendung.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image40.jpeg)

4.  Fügen Sie den folgenden Text in das Textfeld ein und klicken Sie auf
    das Symbol **Send**.

+++Wie viele Siege braucht man, um in die Premier League
aufzusteigen?+++

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image41.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

5.  Fügen Sie den folgenden Text in das Textfeld ein und klicken Sie auf
    das Symbol **Send**.

+++Was ist Azure OpenAI?+++

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image43.jpeg)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image44.jpeg)

6.  Schließen Sie den Terminal.

## Übung 5: Bereinigen der Ressourcengruppe

1.  Öffnen Sie einen neuen Browser, und geben Sie die folgende URL in
    die Adressleiste ein: +++<https://portal.azure.com/+++>, um das
    Azure-Portal zu öffnen.

2.  Wählen Sie auf der Seite Ressourcengruppe die **assigned Resource
    group** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  Wählen Sie alle Resources aus, und wählen Sie dann **Delete** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

4.  Geben Sie +++delete+++ in das Textfeld ein und klicken Sie auf
    **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image49.png)

5.  Eine Erfolgsbenachrichtigung auf den gelöschten Ressourcen bestätigt
    den Löschvorgang.

6.  Nachdem die Ressourcen gelöscht wurden, suchen Sie auf der
    Startseite des Azure-Portals nach **Azure AI Services,** und wählen
    Sie es aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.jpeg)

1.  Wählen Sie im linken Bereich **Azure OpenAI** und dann **Manage
    deleted resources** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

7.  Wählen Sie die Ressource aus, die dort aufgelistet wird, und klicken
    Sie dann auf **Purge**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.jpeg)

8.  Klicken Sie auf **Yes**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

**Zusammenfassung**

In diesem Lab wurde ein umfassender Leitfaden zum Erstellen,
Bereitstellen und Testen einer benutzerdefinierten Chatanwendung mit
Blazor, PostgreSQL und Azure OpenAI bereitgestellt. In diesem Lab haben
Sie gelernt, die erforderliche Entwicklungsumgebung einzurichten, eine
Blazor-basierte Chatschnittstelle zu erstellen und zu entwerfen, eine
PostgreSQL-Datenbank in Azure zu konfigurieren und zu verbinden, Azure
OpenAI für erweiterte Funktionalitäten zu integrieren und schließlich
die Anwendung in Azure bereitzustellen und zu testen. Diese praktische
Erfahrung hat Sie mit den Fähigkeiten ausgestattet, moderne
Webanwendungen mit modernsten Technologien und Cloud-Diensten zu
entwickeln und zu verwalten
