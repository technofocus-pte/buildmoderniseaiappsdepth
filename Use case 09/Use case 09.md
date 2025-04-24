# Anwendungsfall 09 – Erstellen einer Chatbot-Erfahrung mit Azure Cosmos DB für MongoDB und Azure OpenAI Service

**Ziel:**

Mit diesem Anwendungsfall wird eine intelligente Lösung erstellt, die
die V-Kern-basierte Azure Cosmos DB für die Vektorsuche und den
Dokumentabruf in MongoDB mit Azure OpenAI-Diensten kombiniert, um eine
Chatbot-Erfahrung zu erstellen.

![A diagram of a software application AI-generated content may be
incorrect.](./media/image1.jpeg)

**Eingesetzte Schlüsseltechnologien**-- Azure OpenAI Service, Azure
Cosmos DB, ChatGPT model

**Geschätzte Dauer**: 60 Minuten

**Labtyp** – von einem Kursleiter geleitet

**Wichtig:** Wenn einer der Befehle nicht in der **PowerShell**
**eingefügt** wird, öffnen Sie bitte einen Notizblock, halten Sie den
Cursor an einer leeren Stelle des Notizblocks und klicken Sie dann auf
die T-Schaltfläche des einzufügenden Befehls. Der Inhalt wird in den
Editor kopiert und Sie können ihn dann aus dem Editor kopieren und in
die PowerShell einfügen.

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

    - **Password** – Kennwort für die Azure-Anmeldung. Nennen wir diesen
      Benutzernamen und dieses Kennwort als Azure-Anmeldeinformationen.
      Wir werden diese Creds überall dort verwenden, wo wir
      Azure-Anmeldeinformationen erwähnen.

    - **Resource Group** – Die **Ressourcengruppe**, die Ihnen
      zugewiesen ist.

\[! Alert\] **wichtig:** Stellen Sie sicher, dass Sie alle Ressourcen
unter dieser Ressourcengruppe erstellen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  Die Registerkarte **Help** enthält die Supportinformationen. Der
    **ID-Wert** ist hier die **Lab-Instanz-ID**, die während der
    Lab-Ausführung verwendet wird.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

## Übung 1: Bereitstellen von Azure-Ressourcen

### Aufgabe 1: Erstellen von Azure-Ressourcen mithilfe eines Skripts

1.  Melden Sie sich beim Azure-Portal unter +++\*\* an, und melden Sie
    sich mit Ihren Azure-Anmeldeinformationen auf der Registerkarte
    **Ressourcen** an.

2.  Wählen Sie im Azure-Portal Ihr Abonnement aus. Wählen Sie im linken
    Fensterbereich Resource providers unter Settings aus, wählen
    Sie+++**Microsoft.Alertsmanagement**+++ und klicken Sie auf
    **Register**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  Suchen Sie auf der VM nach +++**power shell**+++, Klicken Sie mit
    der rechten Maustaste auf **Windows PowerShell** und wählen Sie
    **Run as administrator** aus. Klicken Sie im Bestätigungsdialog auf
    Yes.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  Führen Sie den folgenden Befehl aus, um Az in der PowerShell zu
    installieren.

+++**Install-Module Az**+++

Wählen Sie **A** (Ja zu allen), wenn Sie dazu aufgefordert werden.

**Hinweis:** Dies dauert bis zu 5 Minuten.

![A computer screen with white text AI-generated content may be
incorrect.](./media/image7.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

5.  Wenn Sie fertig sind, führen Sie den folgenden Befehl aus, um das
    Az-Modul zu importieren.

+++**Import-Module Az**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

1.  Führen Sie den folgenden Befehl aus, um die browserbasierte
    Anmeldung zu verwenden

+++Update-AzConfig -EnableLoginByWam $false+++

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image10.jpeg)

1.  Führen Sie den folgenden Befehl aus, und wählen Sie Ihre
    Azure-Anmeldung aus, wenn Sie dazu aufgefordert werden, um sich bei
    Azure anzumelden.

+++Connect-AzAccount+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

6.  Führen Sie die folgenden Befehle aus, um zum **LabFiles-Ordner** zu
    navigieren.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![A blue rectangular sign with white text AI-generated content may be
incorrect.](./media/image12.jpeg)

7.  Führen Sie den folgenden Befehl aus, um **Microsoft Bicep** mit
    **winget** zu installieren.

+++winget install -e --id Microsoft.Bicep+++

Geben Sie **Y** ein, wenn Sie dazu aufgefordert werden.

![A computer screen with white text AI-generated content may be
incorrect.](./media/image13.jpeg)

8.  **Schließen** Sie die PowerShell, und **öffnen** Sie sie erneut.

9.  Führen Sie den folgenden Befehl aus, und wählen Sie Ihre
    Azure-Anmeldung aus, wenn Sie dazu aufgefordert werden, um sich bei
    Azure anzumelden.

+++Connect-AzAccount+++

12. Führen Sie die folgenden Befehle aus, um zum **LabFiles-Ordner** zu
    navigieren.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![A blue rectangular sign with white text AI-generated content may be
incorrect.](./media/image12.jpeg)

13. Führen Sie den folgenden Befehl aus, um die Abonnement-ID
    festzulegen.

+++Set-AzContext -SubscriptionId @lab. CloudSubscription.Id+++

![A computer screen shot of a blue screen AI-generated content may be
incorrect.](./media/image14.jpeg)

1.  Öffnen Sie die Datei **azuredeploy. bicep** im Pfad
    **C:\LabFiles\Build a Chat bot\Labs\deploy**, und ersetzen Sie die
    Buchstaben **dgxxxxxxx** in Zeile 35 durch
    [+++dg@lab.LabInstance.Id+](mailto:+++dg@lab.LabInstance.Id)++.
    Aktualisieren Sie in Zeile **74** die Version als +++**0125**+++.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

![](./media/image16.png)

14. Führen Sie den folgenden Befehl aus, um die Ressourcen wie den Azure
    Cosmos DB-Arbeitsbereich und Azure OpenAI in Azure bereitzustellen.

New-AzResourceGroupDeployment -ResourceGroupName @lab.
CloudResourceGroup (ResourceGroup1).Name -TemplateFile
.\azuredeploy.bicep -TemplateParameterFile .\azuredeploy.parameters.json
-c \`\`\`

\>\[!Note\] \*\*Note:\*\* Die Bereitstellung dauert etwa 10 bis 15
Minuten.

Wenn es ein Problem mit der Bereitstellung gibt und ein Fehler auftritt,
versuchen Sie, den Namen in Schritt 14 auf einen anderen Namen zu
aktualisieren, und versuchen Sie es erneut.

\>\[!Note\] \*\*Note:\*\* Geben Sie Y ein, wenn Sie dazu aufgefordert
werden.

![A computer screen shot of a blue screen AI-generated content may be
incorrect.](./media/image17.jpeg)

![](./media/image18.jpeg)

\>\[!Note\] \*\*Hinweis:\*\* Wenn nach 15 bis 20 Minuten kein Update in
der PowerShell vorhanden ist, überprüfen Sie im Azure-Portal unter
Ressourcengruppe -\> Bereitstellungen, oder drücken Sie im Fenster
\*\*PowerShell\*\* die Eingabetaste.

### Aufgabe 2: Überprüfen der erstellten Ressourcen in Azure

1.  Melden Sie sich beim **Azure-Portal** unter
    +++<https://portal.azure.com/+++> mit Ihren **Azure-Anmeldedaten
    an.** Wählen Sie **Resource groups** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

2.  Wählen Sie in der Liste Ressourcengruppen die Option **assigned
    Resource Group**.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image20.png)

3.  Beachten Sie, dass eine Reihe von Ressourcen, einschließlich **Azure
    OpenAI-Ressource**, **App Service** und **Azure Cosmos DB für
    MongoDB,** erstellt werden.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

4.  Klicken Sie auf die **Azure OpenAI-**Ressource.

![](./media/image22.png)

5.  Wählen Sie **Keys and Endpoint** unter **Resource Management** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.jpeg)

6.  Kopieren und Speichern von **Key 1** und **Endpoint** in einem
    Editor zur späteren Referenz.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

7.  Wählen Sie auf der Ressourcengruppenseite die **Ressource** **Azure
    Cosmos DB for Mongo DB** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

1.  Klicken Sie unter Settings auf **Connection strings**. Kopieren Sie
    den Wert von Self (immer diese Cluster).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.jpeg)

1.  Kopieren Sie die Verbindungszeichenfolge, und fügen Sie sie in einen
    Editor ein. Ersetzen Sie das \< **Kennwort** \> durch
    +++**myMongoDB98**+++ in der kopierten Verbindungszeichenfolge und
    speichern Sie es im Notepad.

## Übung 2: Erkunden und Verwenden von Azure OpenAI-Modellen aus Code

### Aufgabe 1: Einrichten der Umgebung

1.  Suchen Sie in der Windows-Suchleiste der Lab-VM nach +++Visual
    Studio Code++, und öffnen Sie **Visual Studio Code**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

2.  Klicken Sie auf **Open Folder**. (Wenn es nicht angezeigt wird,
    wählen Sie **File -\> Open Folder**)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

3.  Navigieren Sie zu **C:\Labfiles**, klicken Sie auf **Build a Chat
    bot** ordner und wählen Sie **Select Folder** aus**.**

![A screenshot of a chat bot AI-generated content may be
incorrect.](./media/image29.jpeg)

4.  Klicken Sie im Pop-up auf **Yes, I trust the Authors**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

5.  Öffnen Sie in Visual Studio Code **lab_0_explore_and_use_models.
    ipynb** aus dem Ordner **Labs**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

6.  Klicken Sie auf **Select Kernel.**

7.  Wählen Sie **Install** im **Do you want to install the recommended
    extensions for Python** Pop Up.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image32.jpeg)

8.  Klicken Sie **Allow access,** wenn Sie dazu aufgefordert werden.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.jpeg)

1.  Klicken Sie auf **Select Kernel**. Wählen Sie **Python
    Environments** und dann **Python 3.12.3** oder höher aus, das als
    Option **Suggested** oder **Recommodation** aufgeführt wird.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.jpeg)

9.  Öffnen Sie das. env file

10. Ersetzen Sie **DB_CONNECTION
    STRING**, **AOAI_KEY** und **AOAI_Endpoint** die wir **weiter** oben
    in Aufgabe 2 von Übung 1 im **Notepad** gespeichert haben.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

Jetzt sind die Umgebungsvariablen so festgelegt, dass sie auf die
Azure-Ressourcen verweisen, die wir bereits erstellt haben.

### Aufgabe 2: Ausführen des Codes

1.  Zurück in den Ordner **Lab 0 ipynb**, **Führen Sie** die **erste
    Zelle** aus, indem Sie auf die Schaltfläche Abspielen klicken, um
    die neueste OpenAI-Client-Bibliothek zu installieren.

![A black screen with a black background AI-generated content may be
incorrect.](./media/image37.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image38.jpeg)

1.  **Führen Sie** die nächste Zelle aus, um die **Python-dotenv** zu
    installieren**.**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image39.jpeg)

2.  Drücke **Strg+Umschalt+P**, tippe +++Fenster neu laden+++ und wähle
    die Option Entwickler: Fenster neu laden, die aufgelistet wird.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

3.  Erneut aus der **ersten Zelle** ausführen.

4.  **Führen Sie** die nächste Zelle aus, um die erforderliche
    OpenAI-Bibliothek zu importieren, os, um auf die Umgebungsvariablen
    zuzugreifen, und dotenv, um die Umgebungsvariablen aus der
    .env-Datei zu laden.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image41.jpeg)

5.  **Führen Sie** die nächste Zelle aus, um den **Azure OpenAI-Client**
    zu erstellen, um die Azure OpenAI-Chat-Abschluss-API aufzurufen:

![A computer screen with text AI-generated content may be
incorrect.](./media/image42.jpeg)

6.  **Führen Sie** die nächste Zelle aus, um die
    **.chat.completions.create()-**Methode auf dem Client aufzurufen und
    einen **Chat-Abschluss durchzuführen**. Sie sollten eine
    Chat-Antwort erhalten.

![A computer screen with text on it AI-generated content may be
incorrect.](./media/image43.jpeg)

## Übung 3: Erste Cosmos DB für MongoDB-API-Anwendung

In dieser Übung erfahren Sie, wie Sie Ihr erstes Cosmos DB-Projekt
erstellen. Wir verwenden ein Notebook, um die grundlegenden
CRUD-Vorgänge zu veranschaulichen.

1.  Öffnen Sie die Datei **lab_1_first_application. ipynb** aus dem
    **Ordner** "Labs".

2.  Klicken Sie auf **Select kernel** und wählen Sie die **Python
    version**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

3.  **Führen Sie** die erste Zelle aus, um **pymongo zu installieren**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

1.  **Führen Sie** die nächste Zelle aus, um die erforderlichen
    **Importe durchzuführen**

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image46.jpeg)

4.  Führen Sie die nächste Zelle aus, um **eine Datenbank zu
    erstellen.**

\[! Note\] **Hinweis:** Dabei wird die Verbindungszeichenfolge
verwendet, die wir in der. env-Datei aktualisiert haben.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image47.jpeg)

5.  **Ausführen** der nächsten Zelle, um eine Sammlung zu erstellen.

![A black screen with white text AI-generated content may be
incorrect.](./media/image48.jpeg)

6.  **Führen Sie** die nächste Zelle aus, um ein Dokument zu erstellen.
    Eine Methode zum Erstellen eines Dokuments ist die Verwendung der
    insert_one Methode. Bei dieser Methode wird ein einzelnes Dokument
    in die Datenbank eingefügt.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image49.jpeg)

7.  **Führen Sie** die nächste Zelle aus, um **ein einzelnes Dokument**
    aus der Datenbank abzurufen. Hierfür wird die **find_one** Methode
    verwendet.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.jpeg)

8.  **Führen Sie** die nächste Zelle aus, in der **find_one_and_update**
    Methode verwendet wird, um ein einzelnes Dokument in der Datenbank
    zu aktualisieren.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

9.  **Führen Sie** die nächste Zelle aus, in **der delete_one** Methode
    verwendet wird, um ein einzelnes Dokument aus der Datenbank zu
    löschen.

![](./media/image52.jpeg)

1.  Die **find-Methode** wird verwendet, um mehrere Dokumente in der
    Datenbank abzufragen. **Führen Sie** die **nächsten 3 Zellen**
    nacheinander aus, um es in Aktion zu sehen.

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image53.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image55.jpeg)

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image56.jpeg)

1.  In der folgenden Zelle werden die Datenbank und die Sammlung
    **gelöscht,** die in dieser Übung erstellt wurden. Dies erfolgt
    mithilfe der **drop_database**-Methode für das Datenbankobjekt

![A computer screen with text AI-generated content may be
incorrect.](./media/image57.jpeg)

## Übung 4: Laden von Daten in Cosmos DB mithilfe der MongoDB-API

In der vorherigen Übung wurde veranschaulicht, wie Sie einer Sammlung
einzeln Daten hinzufügen. In dieser Übung wird veranschaulicht, wie
Daten mithilfe von Massenvorgängen in mehrere Sammlungen geladen werden.
Diese Daten werden in nachfolgenden Labs verwendet, um die Funktionen
der Azure Cosmos DB-API für MongoDB in Bezug auf KI näher zu erläutern.

In diesem Notebook wird veranschaulicht, wie Sie mithilfe der
MongoDB-API Daten aus Cosmic Works-JSON-Dateien in die Datenbank in
Cosmos DB laden.

1.  Öffnen Sie die Datei **lab_2_load_data. ipynb** aus dem **Ordner**
    "Labs". Klicken Sie auf **Select Kernel** und wählen Sie die
    **Python-Version** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

1.  Führen Sie die erste Zelle aus, um Anforderungen zu installieren.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

1.  **Führen** Sie die nächste Zelle aus, um die erforderlichen
    **Importe** durchzuführen.

![A computer screen with green text AI-generated content may be
incorrect.](./media/image60.jpeg)

2.  **Führen** Sie die nächste Zelle aus, die eine **Verbindung** mit
    der **Datenbank** herstellt.

![A computer screen with text AI-generated content may be
incorrect.](./media/image61.jpeg)

![A computer screen shot of text AI-generated content may be
incorrect.](./media/image62.jpeg)

3.  **Führen** Sie die nächste Zelle aus, um die **Produkte** zu laden.

![A screen shot of a computer screen AI-generated content may be
incorrect.](./media/image63.jpeg)

4.  **Führen** Sie die nächsten Zellen aus, um die **Kunden- und
    Verkaufsrohdaten zu laden**. In diesem Repository werden die Kunden-
    und Verkaufsdaten in derselben Datei gespeichert. Das Typfeld wird
    verwendet, um zwischen den beiden Arten von Dokumenten zu
    unterscheiden.

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image64.jpeg)

![](./media/image65.jpeg)

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image66.jpeg)

5.  **Ausführen** der nächsten Zelle, die bereinigt werden soll.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image67.jpeg)

## Übung 5: Vektorsuche mit der V-Kern-basierten Azure Cosmos DB für MongoDB

1.  Öffnen Sie die Datei**lab_3_mongodb_vector_search. ipynb** aus **dem
    Labs-Ordner**.

2.  Klicken Sie auf **Select Kernel** und wählen Sie die
    **Python-Version** aus.

![](./media/image68.jpeg)

3.  **Führen** Sie die erste Zelle aus, um **Tenacity** zu installieren.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image69.jpeg)

4.  **Führen Sie** die nächste Zelle aus, um die erforderlichen
    **Importe** durchzuführen.

![A computer screen with text AI-generated content may be
incorrect.](./media/image70.jpeg)

1.  **Führen** Sie die nächste Zelle aus, um die **Einstellungen** aus
    der. env-Datei zu laden.

![A computer screen with text AI-generated content may be
incorrect.](./media/image71.jpeg)

5.  Führen Sie die nächste Zelle aus, um **eine Verbindung** zur
    **Datenbank** herzustellen.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image72.jpeg)

6.  **Ausführen** der nächsten Zelle, um **Azure OpenAI-Konnektivität**
    herzustellen.

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image73.jpeg)

1.  Das Erstellen eines Vektoreinbettungsfelds in jedem Dokument muss
    nur einmal durchgeführt werden. Wenn sich jedoch ein Dokument
    ändert, muss das Feld für die Vektoreinbettung mit einem
    aktualisierten Vektor aktualisiert werden. Dies geschieht in den
    nächsten beiden Zellen. **Führen** Sie die nächsten beiden Zellen
    aus und beobachten Sie die **Einbettungen,** die Sie als Ausgabe in
    der zweiten erhalten haben.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image74.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image75.jpeg)

1.  **Führen Sie** die nächste Zelle aus, um **zu vektorisieren und alle
    Dokumente in der Cosmic Works-Datenbank zu aktualisieren.**

![A computer screen shot of a program code AI-generated content may be
incorrect.](./media/image76.jpeg)

1.  **Führen Sie** die nächsten **3** Zellen aus, um **Vektorfelder** zu
    **Produkten, Kunden- und Verkaufsdokumenten hinzuzufügen.**

**Hinweis:** Die erste Zelle dauert etwa 5 Minuten, die zweite etwa 3
Minuten und die dritte etwa 20 Minuten, bis die Ausführung abgeschlossen
ist.

! \[\] (./media/image77.jpeg)

11. **Führen Sie** die nächste Zelle aus, um einen
    **Produktvektorindex** zu erstellen.

![A screen shot of a computer screen AI-generated content may be
incorrect.](./media/image77.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image78.jpeg)

1.  Da nun jedem Dokument die Vektoreinbettung zugeordnet ist und die
    Vektorindizes für jede Sammlung erstellt wurden, können wir jetzt
    die Vektorsuchfunktionen der V-Kern-basierten Azure Cosmos DB für
    MongoDB verwenden. **Führen** Sie die nächsten **3** Zellen aus.

![](./media/image79.jpeg)

![](./media/image80.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image81.jpeg)

1.  **Führen** Sie die nächsten Zellen aus, um die Verwendung von
    **Vektorsuchergebnissen** in einem RAG-Muster mit Chat GPT-3.5 zu
    beobachten

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image82.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image83.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image84.jpeg)

12. Beobachten Sie die Ausgabe der folgenden Zellen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image85.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image86.jpeg)

## Übung 6: Löschen der bereitgestellten Ressourcen

1.  Wählen Sie im Azure-Portal
    (+++[https://portal.azure.com+++](https://portal.azure.com+++/)) die
    zugewiesene Ressourcengruppe aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image87.png)

1.  Wählen Sie alle darunter liegenden Ressourcen aus, klicken Sie auf
    die **drei Punkte** im Menü und wählen Sie **Delete**, um alle
    Ressourcen zu löschen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image88.png)

1.  Geben Sie +++delete+++ in das Textfeld ein und klicken Sie auf die
    Schaltfläche delete.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image89.png)

1.  Nachdem die Ressourcen gelöscht wurden, suchen Sie auf der
    Startseite des Azure-Portals nach +++**Azure AI Services**+, und
    wählen Sie es aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.jpeg)

1.  Wählen Sie **im** linken Bereich **Azure OpenAI** und dann **manage
    deleted resources** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image91.jpeg)

2.  Wählen Sie die Ressource aus, die dort aufgelistet wird, und klicken
    Sie dann auf **Purge**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image92.jpeg)

3.  Klicken Sie auf **Yes**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image93.jpeg)

**Zusammenfassung:**

Sie haben erfolgreich eine Lösung mit Azure Cosmos DB für die
MongoDB-Vektorsuche und den Dokumentabruf mit Azure OpenAI-Diensten
erstellt.
