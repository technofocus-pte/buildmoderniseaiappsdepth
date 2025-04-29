# 

# Anwendungsfall 10: Bereitstellen einer Chat-Anwendung zur Beantwortung von Fragen des Benutzers und zur Verfolgung des Chat-Verlaufs über Konversationen hinweg

**Objektiv:**

Dieser Anwendungsfall führt Sie durch die Schritte zum Verbinden einer
vorhandenen Blazor-Anwendung mit einem Azure Cosmos DB für NoSQL-Konto
und einem Azure OpenAI-Konto. Ihre Anwendung sendet Prompts an das
Modell in Azure OpenAI und analysiert die Antworten. Ihre Anwendung
speichert auch verschiedene Unterhaltungssitzungen und die
entsprechenden Nachrichten als Elemente, die in einem einzelnen
Container in Azure Cosmos DB für NoSQL zusammengefasst sind.

Kurz gesagt, die Anwendung wird:

- **Herstellen einer Verbindung** mit dem Azure OpenAI-Modell mithilfe
  des .NET SDK

- **Senden von** Prompts an das Modell und Analysieren der Antwort auf
  die Completion

- **Herstellen einer Verbindung** mit Azure Cosmos DB für NoSQL mithilfe
  des .NET SDK

- **Verwalten von** Elementen mit einzelnen Vorgängen, Abfragen und
  Transaktionsbatches

Diese Beispiel-Chatanwendung beantwortet Fragen des Benutzers und
verfolgt den Chatverlauf über Unterhaltungen hinweg.

![](./media/image1.jpeg)

**Verwendete Schlüsseltechnologien**-- Csharp, nosql, asp-net, blazor,
azure-cosmos-db,

**Geschätzte Dauer**: 45 Minuten

**Labtyp:** Von einem Kursleiter geleitet

**Voraussetzungen:**

GitHub-Konto – Es wird erwartet, dass Sie über Ihre eigenen
GitHub-Anmeldeinformationen verfügen. Wenn Sie noch keine haben,
erstellen Sie bitte eine von hier -''
**https://github.com/signup?user_email=&source=form-home-signupobjectives''**

### Aufgabe 1: Ausführen von Docker

1.  Geben Sie in Ihrem Windows-Suchfeld **Docker** ein und klicken Sie
    dann auf **Docker Desktop**.

![](./media/image2.jpeg)

### Aufgabe 2 : Registrieren des Dienstanbieters

1.  Öffnen Sie einen Browser, wechseln Sie zu
    <https://portal.azure.com>, und melden Sie sich mit Ihren
    Azure-Anmeldeinformationen an, die auf der Registerkarte
    **Resource** Ihrer virtuellen Maschine(VM) verfügbar sind.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image3.png)

2.  Klicken Sie auf der Startseite des Azure-Portals auf die Kachel
    **Resource groups**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image4.png)

3.  Kopieren Sie den Namen der Ressourcengruppe, und speichern Sie ihn
    im Editor, um die nächste Aufgabe zum Bereitstellen der
    erforderlichen Ressourcen in dieser Ressourcengruppe zu verwenden.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image5.png)

4.  Navigieren Sie zurück zur Startseite und klicken Sie auf die Kachel
    **Subscription**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.png)

5.  Klicken Sie auf den Namen des Abonnements.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image7.png)

6.  Klicken Sie im linken Navigationsmenü auf **Settings - \> Resource
    provider.**

![](./media/image8.png)

7.  Geben Sie ''**Microsoft.AlertsManagement**'' ein und drücken Sie die
    Eingabetaste. Wählen Sie es aus und klicken Sie dann auf
    **Register**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image9.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image10.png)

### Aufgabe 3: Bereitstellen von Diensten und Anwendungen in Azure

1.  Öffnen Sie einen Browser und gehen Sie zu ''https:\\github.com'' und
    melden Sie sich mit Ihrem Github-Konto an. Suchen Sie nach dem
    folgenden Repository

![](./media/image11.jpeg)

2.  Suchen Sie nach dem folgenden Repo und klicken Sie auf **Fork**.

> ''https://github.com/technofocus-pte/chat-csharp-cosmos-db-nosql-openai''

![](./media/image12.jpeg)

3.  Geben Sie den Namen des Repositorys ein und klicken Sie dann auf
    **Create repository**.

![](./media/image13.jpeg)

4.  Klicken Sie auf **Code -\> Code space -\> Open Code space.**

![](./media/image14.jpeg)

5.  Warten Sie, bis der Dev-Container eingerichtet ist. dauert 3-5 min

![](./media/image15.jpeg)

6.  Führen Sie den folgenden Befehl aus, um sich bei AZD anzumelden.
    Kopieren Sie den generierten Code, und drücken Sie die Eingabetaste.

> \`\`**azd auth login\`\`**

![](./media/image16.jpeg)

7.  Fügen Sie den generierten Code ein, und melden Sie sich mit Ihren
    Azure-Anmeldeinformationen an.

![](./media/image17.jpeg)

![](./media/image18.jpeg)

8.  Führen Sie den folgenden Befehl aus, um das Projekt im aktuellen
    Verzeichnis zu initialisieren. Geben Sie den Umgebungsnamen als
    ''**cosmoschatapp''** ein , und drücken Sie die Eingabetaste.

\`\`azd init \`\`

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image19.png)

9.  Führen Sie den folgenden Befehl aus, um die Dienste in Azure
    bereitzustellen, und erstellen Sie Ihren Container. Wählen Sie die
    folgenden Werte aus.

\`\`azd provision\`\`

> **Wählen Sie ein zu verwendendes Azure-Abonnement** aus: Wählen Sie
> Ihr Abonnement aus
>
> **Wählen Sie einen zu verwendenden Azure-Standort aus** : **East
> us/west us** (Manchmal ist "East US" nicht verfügbar, wählen Sie einen
> anderen Standort aus, und stellen Sie ihn bereit.)
>
> **Geben Sie einen Wert für den Infrastrukturparameter
> "existingResourceGroupName" ein: ResourceGroup1**

![](./media/image20.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image21.png)

10. Warten Sie, bis die Ressource vollständig bereitgestellt wurde.
    Dieser Vorgang dauert 5-10 Minuten, um alle erforderlichen
    Ressourcen zu erstellen.

![](./media/image22.png)

### Aufgabe 4: Bereitstellen der Anwendung in Azure

1.  Wechseln Sie zurück zum Azure-Portal, und klicken Sie auf der
    Startseite auf die Kachel Resource groups.

![](./media/image23.png)

2.  Klicken Sie auf Ressourcengruppenname .

![](./media/image24.png)

3.  Die folgenden Ressourcen sollten angezeigt werden

- **Container**

- **Container Registry**

- **Azure Cosmos Db account**

- **AureOpenAI**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image25.png)

4.  Klicken Sie auf Name der **Container Registry**.

![](./media/image26.png)

5.  Erweitern Sie **Setting** im linken Navigationsmenü und klicken Sie
    auf **Access keys.** Aktivieren Sie das **Kontrollkästchen Admin
    user.** Kopieren Sie den **Login server**, **user name** und
    **password** in einen Editor, um ihn zum Bereitstellen der App zu
    verwenden.

![](./media/image27.png)

6.  Duplizieren Sie die Registerkarte, um das Azrue-Portal auf einer
    neuen Registerkarte zu öffnen.

![](./media/image28.png)

7.  Klicken Sie im oberen Navigationsmenü auf den Namen der
    Ressourcengruppe.

![](./media/image29.png)

8.  Klicken Sie auf Container App Name .

![](./media/image30.png)

9.  Klicken Sie unter Github-Sign in auf die Schaltfläche **Authorize**,
    um sich mit Ihrem GitHub-Konto zu authentifizieren. Autorisieren Sie
    Ihr Github-Konto.

10. Wählen Sie die folgenden Werte aus

> **Organization: Ihre Github-Organisation**
>
> **Repository:** chat-csharp-cosmos-db-nosql-openai
>
> **Branch :** main

![](./media/image31.png)

11. Scrollen Sie nach unten zu **Registry settings**, geben Sie die
    folgenden Werte ein und klicken Sie dann auf die Schaltfläche
    **Start continuous deployment**.

- Repository-Quelle: **Docker Hub or other registries.**

- Login server URL: Ihr Anmeldeserver wurde aus der Container-Registry
  kopiert (Schritt \#5)

- Username: Ihr Passwort aus der Container-Registry (Schritt \#5)

- Password: Ihr Passwort aus der Container-Registry (Schritt \# 5)

![](./media/image32.png)

12. Klicken Sie auf den Link Workflow-Datei. Es öffnet einen neuen Tab
    mit Github.

![](./media/image33.png)

13. Klicken Sie auf die Registerkarte **Actions**.

![](./media/image34.png)

14. Warten Sie, bis die Bereitstellung abgeschlossen ist.

![](./media/image35.png)

15. Schließen Sie keine Registerkarten.

### Aufgabe 5 : Zugreifen auf die Chat-App

1.  Wechseln Sie zurück zum Azure-Portal, klicken Sie im linken
    Navigationsbereich auf **Overview** und dann auf
    **Application-URL**. Es öffnet sich eine neu zu ladende App.

![](./media/image36.png)

2.  Klicken Sie auf die Schaltfläche **Create New Chat**.

![](./media/image37.png)

3.  Geben Sie die folgende Prompt ein.

\`\`What is the seating capacity for Lumen in Seattle?\`\`

![](./media/image38.jpeg)

4.  Geben Sie die untenstehende Prompt ein. Erkunden Sie die App mit
    verschiedenen Prompts.

> \`\`is that bigger than Dogger stadium??\`\`

![](./media/image39.jpeg)

### Aufgabe 6 : Bereinigen aller Ressourcen

So bereinigen Sie alle Ressourcen, die in diesem Beispiel erstellt
wurden:

1.  Wechseln Sie zurück zur Registerkarte Github-Portal, und
    aktualisieren Sie die Seite.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image40.png)

2.  Klicken Sie auf Code , wählen Sie den für dieses Lab erstellten
    Zweig aus und klicken Sie auf **Delete**.

![](./media/image41.png)

3.  Bestätigen Sie das Löschen des Zweigs, indem Sie auf die
    Schaltfläche **Delete** klicken.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image42.png)

5.  Wechseln Sie zurück zum **Azure-Portal -\> Resource group- \>
    Resource group name.**

![](./media/image43.png)

6.  Wählen Sie die gesamte Ressource aus und klicken Sie dann auf
    Delete, wie in der folgenden Abbildung gezeigt. (Ressourcengruppe
    **NICHT LÖSCHEN**)

![](./media/image44.png)

7.  Geben Sie \`\`**delete**\`\` in das Textfeld ein und klicken Sie
    dann auf **Delete**.

> ![](./media/image45.png)

8.  Bestätigen Sie den Löschvorgang mit einem Klick auf **Delete**.

![](./media/image46.png)

**Zusammenfassung:**

Sie haben Dienstklassen mit den Paketen Microsoft.Azure.Cosmos und
Azure.AI.OpenAI in NuGet implementiert. Sie haben Prompts zusammen mit
kontextbezogenen Präfixen an die Azure OpenAI-Konversationsschnittstelle
gesendet und die Verwendungs- und Texteigenschaften der Antwort
analysiert. Sie haben auch Azure Cosmos DB für NoSQL verwendet, um die
Unterhaltungssitzungen und Nachrichten in einem einzelnen Container zu
speichern.
