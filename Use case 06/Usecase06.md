# Usecase 06 – Bereitstellen von Chat-Apps in Azure Container Apps mit PostgreSQL Flexible Server

**Objektiv:**

- Um die Entwicklungsumgebung unter Windows zu konfigurieren, indem Sie
  die Azure CLI Node.js, das Zuweisen von Azure-Abonnementrollen, das
  Starten von Docker Desktop und das Aktivieren der Erweiterung Visual
  Studio Code mit der Erweiterung "Dev Containers" konfigurieren.

- Zum Bereitstellen und Testen der benutzerdefinierten Chatanwendung mit
  PostgreSQL und OpenAI in Azure.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image1.jpeg)

In diesem Anwendungsfall richten Sie eine umfassende
Entwicklungsumgebung ein, stellen eine in PostgreSQL integrierte
Chatanwendung bereit und überprüfen deren Bereitstellung in Azure. Dazu
gehören die Installation wichtiger Tools wie Azure CLI, Docker und
Visual Studio Code ( wir haben dies bereits für Sie auf der Hostumgebung
getan ), das Konfigurieren von Benutzerrollen in Azure, das
Bereitstellen der Anwendung mithilfe der Azure Developer CLI und die
Interaktion mit den bereitgestellten Ressourcen, um die Funktionalität
sicherzustellen.

**Verwendete Schlüsseltechnologien**: Python, FastAPI, Azure
OpenAI-Modelle, Azure Database for PostgreSQL und
azure-container-apps,ai-azd-templates.

**Geschätzte Dauer**: 45 Minuten

**Labortyp:** Von einem Kursleiter geleitet

**Voraussetzungen:**

GitHub-Konto – Es wird erwartet, dass Sie über Ihre eigenen
GitHub-Anmeldeinformationen verfügen. Wenn Sie noch keine haben,
erstellen Sie bitte eine von hier aus -
**https://github.com/signup?user_email=&source=form-home-signupobjectives**

## Übung 1: Bereitstellen, Bereitstellung der Anwendung und Testen über den Browser

### Aufgabe 1: Kopieren des Namens der vorhandenen Ressourcengruppe

1.  Öffnen Sie Ihren Browser, öffnen Sie das Azure-Portal
    \`\`https:\\portal.azure.com\`\`.  Melden Sie sich mit Ihrem
    Azure-Slice-Konto (Azure-Anmeldeinformationen***)** an,* das im
    Abschnitt " instructions/Resources " Ihrer Hostumgebung verfügbar
    ist.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image2.jpeg)

2.  Klicken Sie auf der Startseite auf die Kachel **Resource groups**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image3.png)

3.  Stellen Sie sicher, dass Sie bereits über eine Ressourcengruppe
    verfügen, die Sie verwenden können. Löschen Sie diese
    Ressourcengruppe niemals. Stattdessen können Sie Ressourcen
    innerhalb der Ressourcengruppe löschen, aber nicht die
    Ressourcengruppe selbst.

4.  Klicken Sie auf den Namen der Ressourcengruppe.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image4.png)

5.  Kopieren Sie den Namen der Ressourcengruppe, und speichern Sie ihn
    im Editor, um ihn zum Bereitstellen aller Ressourcen in dieser
    Ressourcengruppe zu verwenden

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image5.png)

### Aufgabe 2: Ausführen von Docker

1.  Doppelklicken Sie auf dem Desktop auf **Docker Desktop**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.jpeg)

2.  Führen Sie den Docker-Desktop aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image7.jpeg)

### Aufgabe 3: Registrieren des Dienstanbieters

1.  Wechseln Sie zurück zur Registerkarte Azure-Portal, und klicken Sie
    auf die Kachel **Subscription**.

![](./media/image8.png)

2.  Klicken Sie auf den Namen des Abonnements.

![](./media/image9.png)

3.  Klicken Sie im linken Navigationsmenü auf **Settings - \> Resource
    provider**.

![](./media/image10.png)

4.  Geben Sie '' **Microsoft.AlertsManagement** '' ein und drücken Sie
    die Eingabetaste. Wählen Sie es aus und klicken Sie dann auf
    **Register**.

![](./media/image11.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image12.png)

### Aufgabe 4 : Entwicklungsumgebung öffnen

1.  Öffnen Sie Ihren Browser, navigieren Sie zur Adressleiste, geben Sie
    die folgende URL ein oder fügen Sie sie ein: Die Registerkarte "
    https://github.com/technofocus-pte/rag-postgres-openai-python.git "
    wird geöffnet und fordert Sie auf, sie in Visual Studio Code zu
    öffnen. Wählen Sie **Open Visual Studio Code** aus.

![](./media/image13.jpeg)

2.  Klicken Sie auf **fork,** um das Repository zu forken. Geben Sie dem
    Repository einen eindeutigen Namen und klicken Sie auf die
    Schaltfläche **Create repo**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image14.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image15.jpeg)

3.  Klicken Sie auf **Code -\> Codespaces -\> Codespaces+**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image16.jpeg)

4.  Warten Sie, bis die Codespaces-Umgebung eingerichtet ist. Es dauert
    einige Minuten, bis die Einrichtung vollständig abgeschlossen ist.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image17.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image18.jpeg)

### Aufgabe 5: Bereitstellen von Diensten und Bereitstellen der Anwendung in Azure

1.  Führen Sie den folgenden Befehl auf dem Terminal aus. Er generiert
    den zu kopierenden Code. Kopieren Sie den Code, und drücken Sie die
    Eingabetaste.

\`\`azd auth login\`\`

![](./media/image19.png)

2.  Der Standardbrowser wird geöffnet, um den generierten Code
    einzugeben, der überprüft werden soll. Geben Sie den Code ein und
    klicken Sie auf **Next**.

![](./media/image20.png)

3.  Melden Sie sich mit Ihren Azure-Anmeldeinformationen an.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image21.png)

4.  Um eine Umgebung für Azure-Ressourcen zu erstellen, führen Sie den
    folgenden Azure Developer CLI-Befehl aus. Sie werden aufgefordert,
    den Umgebungsnamen einzugeben. Geben Sie einen beliebigen Namen ein
    und drücken Sie die Eingabe-Taste (z. B. :**ragpgpy**)

**Hinweis:** Achten Sie beim Erstellen einer Umgebung darauf, dass der
Name aus Kleinbuchstaben besteht.

''azd env new''

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image22.png)

5.  Führen Sie den folgenden Azure Developer CLI-Befehl aus, um die
    Azure-Ressourcen bereitzustellen und den Code bereitzustellen.

> \`\`azd provision \`\`

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image23.png)

6.  Wenn Sie dazu aufgefordert werden, wählen Sie ein **Subscription**
    aus, um die Ressourcen zu erstellen, und wählen Sie die Region aus,
    die Ihrem Standort am nächsten liegt. In diesem Lab haben wir die
    Region **"East US 2**" ausgewählt.

![](./media/image24.png)

7.  Sie werden aufgefordert " **Enter a value for the
    'existingResourceGroupName' infrastructure parameter:**" Geben Sie
    die Ressourcengruppe ein, die in Aufgabe 1 kopiert wurde (z. B**. :
    ResourceGroup1 used for the development slice).**Sie können den
    Namen der Ressourcengruppe aus dem Abschnitt **"Resources"**
    kopieren , wie in der folgenden Abbildung gezeigt

> ![](./media/image25.png)

8.  Wenn Sie dazu aufgefordert werden, **geben Sie einen Wert für den
    Infrastrukturparameter "openAILocation" ein**. Wählen Sie die Region
    aus, die Ihrem Standort am nächsten liegt. In diesem Lab haben wir
    die Region " **North Central US** " ausgewählt.

![](./media/image26.png)

9.  Die Bereitstellung der Ressource dauert etwa 5 bis 10 Minuten.
    Klicken Sie auf **Yes,** wenn Sie dazu aufgefordert werden.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image27.png)

10. Warten Sie, bis die Vorlage alle Ressourcen erfolgreich
    bereitgestellt hat.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image28.png)

11. Führen Sie den folgenden Befehl aus, um die Ressourcengruppe
    festzulegen

azd env set AZURE_RESOURCE_GROUP {your resource group
name}\`\`![](./media/image29.png)

12. Führen Sie den folgenden Befehl aus, um die App in Azure
    bereitzustellen.

> \`\`azd deploy\`\`

![](./media/image30.png)

13. Warten Sie, bis die Bereitstellung abgeschlossen ist. Die
    Bereitstellung dauert \<5

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image31.png)

14. Klicken Sie auf den Link für den bereitgestellten Web-App-Endpunkt.

![](./media/image32.png)

15. Klicken Sie auf **Open**. Es öffnet einen neuen Tab mit App

![](./media/image33.png)

16. Die App wird geöffnet.

![Ein Screenshot eines Chats Beschreibung wird automatisch
generiert](./media/image34.png)

### Aufgabe 6: Verwenden der Chat-App zum Abrufen von Antworten aus Dateien

1.  In der **RAG on database |OpenAI+PoastgreSQL** -Web-App-Seite,
    **klicken Sie auf Best shoe for hiking?** und beobachten Sie die
    Ausgabe

![](./media/image35.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image36.png)

2.  Klicken Sie auf den **Clear chat.**

![](./media/image37.png)

3.  In der **RAG on database |OpenAI+PoastgreSQL**-Web-App-Seite,
    klicken Sie auf die Schaltfläche **Climbing gear cheaper than \\30**
    und beobachten Sie die Ausgabe

![](./media/image38.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image39.png)

4.  Klicken Sie auf den **Clear** **chat.**

### Aufgabe 7: Überprüfen der bereitgestellten Ressourcen im Azure-Portal

1.  Klicken Sie auf der Startseite des Azure-Portals auf **Resource
    Groups**.

![](./media/image40.png)

2.  Klicken Sie auf den Namen Ihrer Ressourcengruppe.

![](./media/image41.png)

3.  Stellen Sie sicher, dass die folgende Ressource erfolgreich
    bereitgestellt wurde

    - Container App

    - Application Insights

    - Container Apps Environment

    - Log Analytics workspace

    - Azure OpenAI

    - Azure Database for PostgreSQL flexible server

    - Container registry

![](./media/image42.png)

4.  Klicken Sie auf **Azure OpenAI**-Ressourcenname.

![](./media/image43.png)

5.  Klicken Sie im linken Navigationsmenü unter **Overview** auf **Go to
    Azure AI Foundry portal,** und wählen Sie aus, um eine neue
    Registerkarte zu öffnen.

![](./media/image44.png)

6.  Klicken Sie im linken Navigationsmenü auf **Shared resources -\>
    Deployments** und stellen Sie sicher, dass **gpt-35-turbo**,
    **text-embedding-ada-002** erfolgreich bereitgestellt werden sollte

![](./media/image45.png)

### Aufgabe 8: Bereinigen aller Ressourcen

So bereinigen Sie alle Ressourcen, die in diesem Beispiel erstellt
wurden:

1.  Wechseln Sie zurück zum **Azure-Portal -\> Resource group- \>
    Resource group name.**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image46.png)

2.  Wählen Sie die gesamte Ressource aus und klicken Sie dann auf
    Delete, wie in der folgenden Abbildung gezeigt. (Ressourcengruppe
    **NICHT LÖSCHEN**)

![](./media/image47.png)

3.  Geben Sie \`\`**delete**\`\` in das Textfeld ein und klicken Sie
    dann auf **Delete**.

![](./media/image48.png)

4.  Bestätigen Sie den Löschvorgang mit einem Klick auf **Delete**.

![](./media/image49.png)

5.  Wechseln Sie zurück zur Registerkarte Github-Portal, und
    aktualisieren Sie die Seite.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image50.png)

1.  Klicken Sie auf Code, wählen Sie den für dieses Lab erstellten Zweig
    aus und klicken Sie auf **Delete**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image51.png)

2.  Bestätigen Sie das Löschen des Zweigs, indem Sie auf die
    Schaltfläche **Delete** klicken.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image52.png)
