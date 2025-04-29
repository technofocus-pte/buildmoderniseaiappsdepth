# Anwendungsfall 08 – Erstellen und Bereitstellen der Chat-App "Contoso Real Estate" zur Unterstützung von Kunden.

**Objektiv**

Dieser Anwendungsfall demonstriert einige Ansätze zum Erstellen von
ChatGPT-ähnlichen Erfahrungen über Ihre eigenen Daten mithilfe des
Retrieval Augmented Generation-Musters. Es verwendet Azure OpenAI
Service für den Zugriff auf das ChatGPT-Modell (gpt-35-turbo) und Azure
AI Search für die Datenindizierung und den Abruf.

![Ein Diagramm eines Softwareprozesses Beschreibung wird automatisch
generiert](./media/image1.jpeg)

Der Anwendungsfall enthält Beispieldaten, sodass er von Anfang bis Ende
ausprobiert werden kann. In dieser Beispielanwendung verwenden wir ein
fiktives Unternehmen namens Contoso Real Estate, und die
Benutzeroberfläche ermöglicht es seinen Kunden, Supportfragen zur
Verwendung seiner Produkte zu stellen. Die Beispieldaten umfassen eine
Reihe von Dokumenten, die die Nutzungsbedingungen, die
Datenschutzrichtlinie und einen Supportleitfaden beschreiben.

Die Anwendung besteht aus mehreren Komponenten, darunter:

- **Suchdienst**: Der Back-End-Dienst, der die Such- und Abruffunktionen
  bereitstellt.

- **Indexerdienst**: Der Dienst, der die Daten indiziert und die
  Suchindizes erstellt.

- **Web-App**: die Frontend-Webanwendung, die die Benutzeroberfläche
  bereitstellt und die Interaktion zwischen dem Benutzer und den
  Backend-Diensten orchestriert.

![Ein Diagramm eines Softwaresystems Beschreibung wird automatisch
generiert](./media/image2.jpeg)

- Chat- und Q&A-Schnittstellen

- Untersucht verschiedene Optionen, um Benutzern zu helfen, die
  Vertrauenswürdigkeit von Antworten mit Zitaten, der Verfolgung von
  Quellinhalten usw. zu bewerten.

- Zeigt mögliche Ansätze für die Datenaufbereitung, die
  Prompt-Erstellung und Orchestrierung der Interaktion zwischen Modell
  (ChatGPT) und Retriever (Azure AI Search)

- Einstellungen direkt in der UX, um das Verhalten zu optimieren und mit
  Optionen zu experimentieren

- Optionale Leistungsablaufverfolgung und -überwachung mit Application
  Insights

**Verwendete Schlüsseltechnologien**: Azure OpenAI Service,
ChatGPT-Modell (gpt-35-turbo) und Azure AI Search

**Geschätzte Dauer:** 40 Minuten

## Übung 1: Bereitstellen der Anwendung und Testen über den Browser

### Aufgabe 1: Entwicklungsumgebung öffnen

1.  Öffnen Sie Ihren Browser, navigieren Sie zur Adressleiste, geben Sie
    die folgende URL ein oder fügen Sie sie ein:
    ''https://github.com/technofocus-pte/azure-search-openai-javascript''
    und melden Sie sich mit Ihrem Github-Konto an.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image3.jpeg)

2.  Klicken Sie auf **Fork**.

![Ein Screenshot einer Webseite Beschreibung wird automatisch
generiert](./media/image4.jpeg)

3.  Geben Sie den Namen des Repositorys ein und klicken Sie dann auf
    **Create fork**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image5.jpeg)

4.  Klicken Sie auf **Code -\> Codespaces -\> +**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.jpeg)

5.  Warten Sie, bis die Umgebung eingerichtet ist. Es dauert 5-10
    Minuten.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image7.jpeg)

### Aufgabe 2: Bereitstellen der erforderlichen Dienste zum Erstellen und Bereitstellen der Chat-App in Azure

1.  Führen Sie den folgenden Befehl auf dem Terminal aus. Kopieren Sie
    den Code und drücken Sie die Eingabetaste.

> \`\`azd auth login\`\`

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image8.png)

2.  Der Standardbrowser wird geöffnet, um einen Code einzugeben. Geben
    Sie den kopierten Code ein und klicken Sie auf **Next**.

![](./media/image9.png)

3.  Melden Sie sich mit Ihren Azure-Anmeldeinformationen an.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image10.png)

![Ein Screenshot eines Computerfehlers Beschreibung wird automatisch
generiert](./media/image11.png)

6.  Wechseln Sie zurück zur Registerkarte Github Codespace. Führen Sie
    den folgenden Befehl aus, um die Projektumgebung im aktuellen
    Verzeichnis zu initialisieren. Geben Sie den Umgebungsnamen als
    ''**ragpgpy ''** ein und drücken Sie die Eingabetaste.

Hinweis: Der Name der Umgebung sollte eindeutig sein

\`\` azd env new\`\`

![](./media/image12.png)

7.  Führen Sie den folgenden Befehl aus, um die Dienste in Azure
    bereitzustellen, und erstellen Sie Ihren Container.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image13.png)

8.  Wählen Sie die folgenden Werte aus.

\`\`azd provision\`\`

- **Wählen Sie ein zu verwendendes Azure-Abonnement aus:** Wählen Sie
  Ihr Abonnement aus

- **Wählen Sie einen zu verwendenden Azure-Standort aus** : **East
  us2/west us2** (Manchmal ist "East US" nicht verfügbar. Wählen Sie den
  Standort aus der unten genannten Liste aus.)

- Wählen Sie vorhandene Ressourcengruppe aus: Ihre vorhandene
  Ressourcengruppe (z. B. :**ResourceGroup1 )**

![](./media/image14.png)

9.  Warten Sie, bis die Ressource vollständig bereitgestellt wurde.
    Dieser Vorgang dauert 5-10 Minuten, um alle erforderlichen
    Ressourcen zu erstellen.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image15.png)

### Aufgabe 3: Bereitstellen der Chat-App und Erkunden

10. Führen Sie den folgenden Befehl aus, um die App bereitzustellen.

> \`\`azd deploy\`\`

![](./media/image16.png)

11. Warten Sie auf die Bereitstellung. Es dauert \< 5 Minuten.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image17.png)

12. Klicken Sie auf die generierte Endpunkt-URL.

![](./media/image18.png)

13. Klicken Sie auf **Open**.

![](./media/image19.png)

14. Es öffnet die App in einem neuen Tab.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image20.png)

15. Wählen Sie **How to search and book rental?** Container und klicken
    Sie dann auf die Eingabetaste neben dem Textfeld.

![](./media/image21.png)

### Aufgabe 4: Bereinigen aller Ressourcen

1.  Wechseln Sie zurück zum **Azure-Portal -\> Resource group- \>
    Resource group name.**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image22.png)

2.  Wählen Sie die gesamte Ressource aus und klicken Sie dann auf
    Delete, wie in der folgenden Abbildung gezeigt. (Ressourcengruppe
    **NICHT LÖSCHEN**)

![](./media/image23.png)

3.  Geben Sie \`\`**delete**\`\` in das Textfeld ein und klicken Sie
    dann auf **Delete**.

![](./media/image24.png)

4.  Bestätigen Sie den Löschvorgang mit einem Klick auf **Delete**.

![](./media/image25.png)

5.  Wechseln Sie zurück zur Registerkarte Github-Portal, und
    aktualisieren Sie die Seite.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image26.png)

6.  Klicken Sie auf Code , wählen Sie den für dieses Lab erstellten
    Zweig aus und klicken Sie auf **Delete**.

![](./media/image27.png)

7.  Bestätigen Sie das Löschen des Zweigs, indem Sie auf die
    Schaltfläche **Delete** klicken.

![](./media/image28.png)

### Zusammenfassung:

In diesem Anwendungsfall haben Sie sich gedacht, eine Chatanwendung für
das Retrieval Augmented Generation-Muster bereitzustellen, das in Azure
ausgeführt wird, Azure AI Search für den Abruf und Azure OpenAI und
LangChain Large Language Models (LLMs) zu verwenden, um ChatGPT-ähnliche
und Q&A-Erfahrungen zu ermöglichen
