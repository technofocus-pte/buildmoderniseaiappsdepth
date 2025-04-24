# Anwendungsfall 04: Erstellen einer TODO-Liste ASP.NET-App, Bereitstellen in Azure App Service durch Herstellen einer Verbindung mit SQL-Datenbank

**Geschätzte Dauer:** 40 Minuten

**Labtyp:** Von einem Kursleiter geleitet

**Objektiv:**

Azure App Service bietet einen hochgradig skalierbaren, selbstpatchenden
Webhostingdienst. In dieser Übung erfahren Sie, wie Sie eine
datengesteuerte ASP.NET-App in App Service bereitstellen und mit Azure
SQL-Datenbank verbinden. Wenn Sie fertig sind, verfügen Sie über eine
ASP.NET-App, die in Azure ausgeführt und mit SQL-Datenbank verbunden
ist.

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

    - **Password** – Password to the Azure login. Let us call this
      Username and password as Azure login credentials. We will use
      these creds wherever we mention Azure login credentials.

    - **Resource Group** – Die **Ressourcengruppe**, die Ihnen
      zugewiesen ist.

\[!Alert\] **Wichtig:** Stellen Sie sicher, dass Sie alle Ressourcen
unter dieser Ressourcengruppe erstellen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  Die Registerkarte **Help** enthält die Supportinformationen. Der
    **ID-**Wert ist hier die **Lab-Instanc-ID**, die während der
    Lab-Ausführung verwendet wird.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## Übung 1: Bereitstellen einer ASP.NET-App in Azure mit Azure SQL-Datenbank

### Aufgabe 1: Einrichten von Visual Studio 2022 und Ausführen der Anwendung

1.  Geben Sie in der Windows**-Suchleiste** +++**Visual studio**+++ und
    wählen Sie Visual Studio 2022 aus. Wenn Sie aufgefordert werden,
    sich anzumelden, fahren Sie mit den Schritten 2 und 3 fort, oder
    fahren Sie mit Schritt 4 fort.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.jpeg)

1.  Klicken Sie auf **Sign in** und **sign in** mit dem
    **Username** und **password** Im Rahmen der **User Credentials** auf
    der Registerkarte "Ressourcen" der VM.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

2.  Wählen Sie **Start Visual Studio** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

3.  Wählen Sie **Open a local folder** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

4.  Wählen Sie den **webappwithsqldb** folder in **C:\Labfiles** aus und
    klicken Sie auf **Select Folder**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

5.  Sobald der Ordner geöffnet ist, doppelklicken Sie auf
    **DotNetAppSqlDb.sln** im **Solution Explorer**.

**Anmerkung:** Wenn der Projektmappen-Explorer nicht automatisch
geöffnet wird, klicken Sie auf **View -\> Solution Explorer.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

6.  Klicken Sie auf **Build** -\> **Build Solution**.

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image10.jpeg)

7.  Sobald der Build abgeschlossen ist, wählen Sie **Debug -\>** **Start
    Debugging**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

8.  Dadurch wird ein Browser geöffnet, in dem die **Todos-Web-App**
    ausgeführt wird.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.jpeg)

9.  Fügen Sie der App einige Elemente hinzu, indem Sie auf **Create
    New** klicken, wie in den folgenden Screenshots zu sehen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.jpeg)

![A screenshot of a application AI-generated content may be
incorrect.](./media/image15.jpeg)

10. Fügen Sie der Liste einige weitere Elemente hinzu.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.jpeg)

11. Klicken Sie im Visual Studio 2022 auf **Debug -\>** **Stop
    Debugging**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

### Aufgabe 2: Veröffentlichen ASP.NET Anwendung in Azure

1.  Im **Solution Explorer**, klicken Sie mit der rechten Maustaste auf
    Ihr **DotNetAppSqlDb** Projekt und wählen Sie **Publish**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

2.  Wählen Sie **Azure** aus und klicken Sie auf **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.jpeg)

3.  Wählen Sie den **Azure App Service (Windows)** Bildschirm in **Which
    Azure service would you like to use to host your application?**  und
    klicken Sie auf **Next**.

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image21.jpeg)

4.  Klicken Sie im Dialogfeld Veröffentlichen auf **Sign In** und dann
    bei Ihrem Azure-Abonnement anmelden, falls Sie noch nicht angemeldet
    sind.

**Anmerkung:** Wenn Sie bereits bei einem Microsoft-Konto angemeldet
sind, stellen Sie sicher, dass dieses Konto Ihr Azure-Abonnement
enthält. Wenn das angemeldete Microsoft-Konto nicht über Ihr
Azure-Abonnement verfügt, klicken Sie darauf, um das richtige Konto
hinzuzufügen.

5.  Klicken Sie auf **Create new**, um einen neuen App-Dienst zu
    erstellen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.jpeg)

6.  Geben Sie die folgenden Details ein.

[TABLE]

7.  Klicken Sie auf **New** im **Hosting Plan**.

8.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image23.png)

9.  Klicken Sie auf die Option **New** bei der Option **Hosting-Plan,**
    geben Sie die folgenden Details ein und klicken Sie auf **OK**.

[TABLE]

10. ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image24.png)

11. Klicken Sie im App Service-Fenster auf **Create**, und warten Sie,
    bis Azure-Ressourcen erstellt wurden.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image25.png)

12. Im Dialogfeld **Publish** werden die Ressourcen angezeigt, die Sie
    konfiguriert haben. Klicken Sie auf **Finish**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

13. Klicken Sie auf **Close**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image27.jpeg)

1.  Scrollen Sie nach unten zum Abschnitt Serverabhängigkeiten und
    klicken Sie auf das + sign to add dependency.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

2.  Wählen Sie auf der Seite **add Dependancy** die Option **Azure SQL
    Database** aus, und klicken Sie auf **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.jpeg)

3.  Wählen Sie **Create New** neben SQL-Datenbanken, in der Spalte
    **Connect to Azure SQL Database** Dialogfeld aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

1.  In **Azure SQL Database Create new**, klicken Sie auf **New** neben
    dem Datenbankserver.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

4.  Füllen Sie die folgenden Details aus und klicken Sie auf **OK**.

[TABLE]

5.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image32.png)

6.  Klicken Sie **Create** in Create new dialog.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

### Aufgabe 3: Konfigurieren der Datenbankverbindung

1.  Wenn der Assistent die Erstellung der Datenbankressourcen
    abgeschlossen hat, klicken Sie auf **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

2.  Geben Sie die folgenden Details in das Feld **Connect to Azure SQL
    Database** und klicken Sie auf **Finish**.

[TABLE]

3.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image35.jpeg)

4.  Klicken Sie auf **Finish,** nachdem Sie die **summary of changes**
    überprüft haben.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

5.  Warten Sie, bis der Konfigurationsassistent abgeschlossen ist, und
    klicken Sie auf **Close**. Die Azure SQL-Datenbank ist jetzt mit
    Ihrer App **verbunden**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

6.  Klicken Sie auf der Seite Publish in der oberen rechten Ecke auf
    **Publish**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

**Anmerkung:** Dies dauert ca. 5 Minuten.

7.  Nachdem Ihre ASP.NET-App in Azure bereitgestellt wurde, wird Ihr
    Standardbrowser mit der URL der bereitgestellten App
    gestartet. **Add a few to-do items**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

### Aufgabe 4: Lokaler Zugriff auf die Datenbank

Mit Visual Studio können Sie Ihre neue Datenbank in Azure einfach in der
**SQL Server Object Explorer**. Die Firewall der neuen Datenbank für die
App Service-App, die Sie erstellt haben, hat bereits geöffnet. Um jedoch
von Ihrem lokalen Computer (z. B. von Visual Studio) aus darauf
zugreifen zu können, müssen Sie eine Firewall für die öffentliche
IP-Adresse Ihres lokalen Computers öffnen. Wenn Ihr
Internetdienstanbieter Ihre öffentliche IP-Adresse ändert, müssen Sie
die Firewall neu konfigurieren, um wieder auf die Azure-Datenbank
zugreifen zu können.

1.  Wählen Sie im Menü **View** von Visual Studio 2022 die Option **SQL
    Server Object Explorer** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

1.  Wählen Sie oben **SQL Server Object Explorer** aus, Klicken Sie auf
    die Schaltfläche **Add SQL Server**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.jpeg)

### Aufgabe 5: Konfigurieren der Datenbankverbindung

1.  Erweitern Sie im Dialogfeld **Connect** den Knoten **Azure**. Alle
    Ihre SQL-Datenbank-Instanzen in Azure sind hier aufgelistet.

2.  Wählen Sie die Datenbank aus, die Sie zuvor erstellt haben
    (**dotnetappsqldbdbserver98**). Die Verbindung, die Sie zuvor
    erstellt haben, wird automatisch unten ausgefüllt.

3.  Geben Sie das Datenbankadministratorkennwort ein, das Sie zuvor
    erstellt haben **(+++PassWord98+++),** und klicken Sie auf Connect.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

### Aufgabe 6: Zulassen einer Clientverbindung von Ihrem Computer aus

Das Dialogfeld Neue Firewall-Regel erstellen wird geöffnet.
Standardmäßig lässt ein Server nur Verbindungen mit seinen Datenbanken
von Azure-Diensten zu, z. B. von Ihrer Azure-App. Wenn Sie von außerhalb
von Azure eine Verbindung mit Ihrer Datenbank herstellen möchten,
erstellen Sie eine Firewallregel auf Serverebene. Die Firewallregel
lässt die öffentliche IP-Adresse Ihres lokalen Computers zu.

Das Dialogfeld ist bereits mit der öffentlichen IP-Adresse Ihres
Computers gefüllt.

1.  Stellen Sie sicher, dass Add my client IP markiert ist, und klicken
    Sie auf OK.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.jpeg)

2.  Nachdem Visual Studio die Erstellung der Firewalleinstellung für
    Ihre SQL-Datenbankinstanz abgeschlossen hat, wird Ihre Verbindung in
    **SQL Server Object Explorer** hergestellt.

3.  Erweitern Sie Ihre **connection \> Databases \> \< YOUR DATABASE \>
    \> Tables**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

4.  Klicken Sie mit der rechten Maustaste auf die **Todos-Tabelle** und
    wählen Sie **View Data** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

5.  Zeigen Sie den Inhalt der Tabelle an. Die Daten, die über die
    App-Benutzeroberfläche hinzugefügt wurden, sollten hier aufgelistet
    werden.

![](./media/image46.jpeg)

## Übung 2: Aktualisieren der App mit Code First-Migrationen

1.  Öffnen Sie im **Solution Explorer** \> **Modelle\Todo.cs** im
    Code-Editor aus. Fügen Sie der **ToDo-Klasse** die folgende
    Eigenschaft als letzte Zeile hinzu **(**nach der **public DateTime
    CreatedDate { get; set; }** Zeile) und klicken Sie auf
    **Speichern**.

+++**public bool Done { get; set; }**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.jpeg)

### Aufgabe 1: Lokales Ausführen von Code First-Migrationen

Führen Sie einige Befehle aus, um Aktualisierungen an Ihrer lokalen
Datenbank vorzunehmen.

1.  Klicken Sie im Menü **Tools** auf **NuGet Package
    Manager** \> **Package Manager Console**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image48.jpeg)

2.  Aktivieren Sie im Fenster Package Manager Console \> Code
    First-Migrationen, indem Sie diesen Befehl ausführen.

+++**Enable-Migrations**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.jpeg)

3.  Fügen Sie eine Migration hinzu, indem Sie den folgenden Befehl
    ausführen.

+++**Add-Migration AddProperty**+++

![](./media/image50.jpeg)

4.  Aktualisieren Sie die lokale Datenbank, indem Sie den folgenden
    Befehl ausführen.

+++**Update-Database**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

5.  Geben Sie **Strg+F5** ein, um die App auszuführen, oder klicken Sie
    auf **Debug -\> Start without Debugging**. Testen der Bearbeitung,
    Details und Erstellen von Verknüpfungen.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image52.jpeg)

6.  Die Anwendungsseite wird geöffnet und sieht immer noch gleich aus,
    da Ihre Anwendungslogik diese neue Eigenschaft noch nicht verwendet.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

### Aufgabe 2: Verwenden der neuen Eigenschaft

Nehmen Sie einige Änderungen im Code vor, um die Done-Eigenschaft zu
verwenden.

1.  Öffnen Sie in Visual Studio **Controllers\TodosController.cs**.
    Suchen Sie die Methode **Create ()** in Zeile 52 und fügen Sie
    +++**Done**+++ zur Liste der Eigenschaften im Bind-Attribut hinzu.
    Wenn Sie fertig sind, sieht die Signatur der Create ()-Methode wie
    der folgende Code aus:

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

1.  Öffnen Sie **Views\Todos\Create.cshtml**. Fügen Sie den folgenden
    Code nach dem \< div class="form-group" \> für **CreatedDate** ein.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image55.jpeg)

\<div class="form-group"\>

@Html.LabelFor(model =\> model.Done, htmlAttributes: new { @class =
"control-label col-md-2" })

\<div class="col-md-10"\>

\<div class="checkbox"\>

@Html.EditorFor(model =\> model.Done)

@Html.ValidationMessageFor(model =\> model.Done, "", new { @class =
"text-danger" })

\</div\>

\</div\>

\`\`\`

3.  Öffnen Sie **Views\Todos\Index.cshtml**. Fügen Sie den folgenden
    Code in das leere **th-**Element nach dem **th**-Element für das
    **CreatedDate ein**.

<+++@Html.DisplayNameFor>(model =\> model.Done)+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image56.jpeg)

4.  Fügen Sie diesen Code direkt über dem HTML-Code hinzu.
    ActionLink()-Hilfsmethoden.

5.  \<td\>

6.  @Html.DisplayFor(modelItem =\> item.Done)

\`\`\`

\![\](./media/image53.jpeg)

5.  Geben Sie **STRG+F5 ein**, um die App auszuführen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.jpeg)

### Aufgabe 3: Aktivieren von Code First-Migrationen in Azure

1.  Klicken Sie mit der rechten Maustaste auf das Projekt und wählen Sie
    **Publish**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

2.  Klicken Sie auf **More actions** \> **Edit**, um die publish
    settings zu öffnen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

3.  Wählen Sie in der Dropdownliste **MyDatabaseContext** die
    Datenbankverbindung für Ihre Azure SQL-Datenbank aus.

4.  Wählen Sie **Execute Code First Migrations** (Wird beim Start der
    Anwendung ausgeführt), Klicken Sie dann auf **Save**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image60.jpeg)

5.  Auf der Publish Seite, klicken Sie auf **Publish**.

![A black rectangular object with white text AI-generated content may be
incorrect.](./media/image61.jpeg)

6.  Die aktualisierte App ist jetzt in Azure verfügbar.

&nbsp;

1.  Versuchen Sie erneut, To-Do-Elemente hinzuzufügen, und wählen Sie
    **Done**, und sie sollten auf Ihrer Homepage als abgeschlossenes
    Element angezeigt werden.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image62.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image63.jpeg)

## Übung 3: Streamen von Anwendungsprotokollen

1.  Scrollen Sie auf der Veröffentlichungsseite nach unten zum Abschnitt
    **Hosting**. Klicken Sie in der rechten Ecke auf**...** \> **View
    Streaming Logs**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image64.jpeg)

2.  Die Protokolle werden nun in das Ausgabefenster gestreamt.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image65.jpeg)

3.  Es werden noch keine Ablaufverfolgungsmeldungen angezeigt, da Ihre
    Azure-App beim ersten Auswählen von Streamingprotokolle anzeigen die
    Ablaufverfolgungsebene auf Fehler festlegt, wodurch nur
    Fehlerereignisse protokolliert werden.

\[!Note\] **Anmerkung:** Starten Sie den Protokolldatenstrom über Visual
Studio neu, wenn sie noch nicht angezeigt werden.

### Aufgabe 1: Ändern von Ablaufverfolgungsebenen

1.  Rufen Sie die Veröffentlichungsseite auf. Klicken Sie im Abschnitt
    Hosting auf**… \> Open in Azure portal**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image66.jpeg)

2.  Wählen Sie auf der Seite Azure-Portal – App die Option **App Service
    Logs** im linken Fensterbereich unter dem Abschnitt **Monitoring**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image67.jpeg)

3.  Unter **Application Logging** (File System), Wählen Sie **Verbose**
    in Ebene aus. Klicken Sie dann auf **Save**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image68.jpeg)

4.  Greifen Sie in Ihrem Browser auf die Web-App in Azure zu, und führen
    Sie einige Aktivitäten aus.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image69.jpeg)

5.  Die Ablaufverfolgungsmeldungen werden jetzt an das Ausgabefenster in
    Visual Studio gestreamt.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image70.jpeg)

1.  Um den Protokollstreamingdienst zu beenden, klicken Sie **im**
    Ausgabefenster auf die Schaltfläche **Stop Monitoring**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image71.jpeg)

6.  Schließen Sie Visual Studio.

## Übung 4: Bereinigen von Ressourcen

1.  Öffnen Sie im Azure-Portal die Ihnen zugewiesene Ressourcengruppe.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image72.png)

2.  Wählen Sie alle Ressourcen aus und klicken Sie auf Delete.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image73.png)

3.  Geben Sie +++delete+++ in das Textfeld ein und wählen Sie Delete
    aus. Wählen Sie im Bestätigungsdialogfeld Delete aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image74.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image75.png)

**Zusammenfassung**

In dieser Übung haben Sie gelernt, eine datengesteuerte ASP.NET-App in
App Service bereitzustellen und sie mit Azure SQL-Datenbank zu
verbinden.
