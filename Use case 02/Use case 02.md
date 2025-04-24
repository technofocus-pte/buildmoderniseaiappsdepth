# Anwendungsfall 02 - Erstellen einer Fruits List Quarkus Web-App mit Azure App Service unter Linux und PostgreSQL

**Geschätzte Dauer:** 40 Minuten

**Labtyp:** Von einem Kursleiter geleitet

**Ziel:**

Dieser Anwendungsfall zeigt, wie Sie eine sichere Quarkus Anwendung in
Azure App Service erstellen, konfigurieren und bereitstellen, die mit
einer PostgreSQL-Datenbank verbunden ist (unter Verwendung von Azure
Database for PostgreSQL). Azure App Service ist ein hochgradig
skalierbarer, selbstpatchender Webhostingdienst, mit dem Apps problemlos
unter Windows oder Linux bereitgestellt werden können. Wenn Sie fertig
sind, haben Sie eine Quarkus App, die auf Azure App Service unter Linux
ausgeführt wird.

**Voraussetzungen:**

**GitHub Konto** -- Es wird erwartet, dass Sie über Ihre eigenen
GitHub-Anmeldeinformationen verfügen. Wenn Sie noch keine haben,
erstellen Sie bitte hier eine
- +++<https://github.com/signup?user_email=&source=form-home-signup+++>

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

    - **Password** – Kennwort für die Azure-Anmeldung.

Nennen wir diesen Benutzernamen und dieses Kennwort als
Azure-Anmeldeinformationen. Wir werden diese Creds überall dort
verwenden, wo wir Azure-Anmeldeinformationen erwähnen.

- **Resource Group** – Die **Ressourcengruppe**, die Ihnen zugewiesen
  ist.

\[!Alert\] **Wichtig:** Stellen Sie sicher, dass Sie alle Ressourcen
unter dieser Ressourcengruppe erstellen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Die Registerkarte **Help** enthält die Supportinformationen. Der
    **ID-Wert** ist hier die **Lab-Instance-ID,** die während der
    Lab-Ausführung verwendet wird.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## Übung 1: Ausführen des Beispiels

Zuerst richten Sie eine datengesteuerte Beispiel-App als Ausgangspunkt
ein. Das Beispielrepository, das wir hier verwenden, enthält eine
Entwicklungscontainerkonfiguration. Der Entwicklungscontainer enthält
alles, was Sie zum Entwickeln einer Anwendung benötigen, einschließlich
der Datenbank, die Caches und aller Umgebungsvariablen, die von der
Beispielanwendung benötigt werden. Der Entwicklungscontainer kann in
einem GitHub-Codespace ausgeführt werden, was bedeutet, dass Sie das
Beispiel auf jedem Computer mit einem Webbrowser ausführen können.

1.  Melden Sie sich in einem Browser bei Ihrem GitHub-Konto
    an+++\*\*<https://github.com/login**+++>.

2.  Öffnen Sie diese URL von einem neuen Tab
    aus, +++\*\*<https://github.com/technofocus-pte/msdocs-quarkus-postgresql-sample-app**+++>.

3.  Wählen Sie **Fork -\> Create a new fork** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.jpeg)

4.  Klicken Sie auf **Create fork** auf der Seite Neuen Fork erstellen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

5.  Wählen Sie auf der geforkten Seite des Repositorys
    **Code** \> **Create codespace on main**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

**Anmerkung:** Wenn die Option Create Codespace on main nicht angezeigt
wird, klicken Sie auf das +-Symbol neben Codespaces.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

**Anmerkung:** Die Einrichtung der Codespace-Erstellung dauert ca. 10
Minuten.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

6.  Führen Sie +++mvn quarkus:dev+++ im Terminal aus. Klicken **Sie im
    Pop-up** auf Zulassen.

![A screenshot of a browser AI-generated content may be
incorrect.](./media/image8.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

1.  Wenn die Benachrichtigung **Your application running on port 8080**
    angezeigt wird, wählen sie **Open in Browser** aus. Die
    Beispielanwendung sollte in einer neuen Browserregisterkarte
    angezeigt werden.

Wenn eine **Benachrichtigung** mit Port **5005** angezeigt wird,
**überspringen** Sie diese.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

7.  Um den Quarkus Entwicklungsserver zu stoppen, geben Sie **Strg+C**
    im Codespace-Terminal ein.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

## Übung 2: Erstellen von App Service und PostgreSQL

Zuerst erstellen Sie die Azure-Ressourcen. Mit den in dieser Übung
verwendeten Schritten wird eine Reihe von standardmäßig sicheren
Ressourcen erstellt, die App Service und Azure Database for PostgreSQL
enthalten.

1.  Öffnen Sie das Azure-Portal unter
    +++<https://portal.azure.com/+++> und melden Sie sich mit den
    Azure-Anmeldeinformationen auf der Registerkarte **Resources** der
    VM an.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

2.  Wählen Sie **Abbrechen** oder die Schaltfläche "Schließen" auf der
    Willkommensseite aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.jpeg)

1.  Enter +++**web app database**+++ in der Suchleiste oben im
    Azure-Portal. Wählen Sie das Element mit der Bezeichnung **Web App +
    Database** unter der Überschrift **Marketplace** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.jpeg)

3.  Auf **Create Web App + Database**, geben Sie die folgenden Details
    ein, und wählen Sie **Review + create**

[TABLE]

4.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image16.png)

5.  ![A screenshot of a web application AI-generated content may be
    incorrect.](./media/image17.jpeg)

6.  Sobald die Validierung bestanden ist, klicken Sie auf **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

**Anmerkung:** Die Erstellung der App dauert ca. 15 Minuten.

7.  Sobald die Bereitstellung abgeschlossen ist, klicken Sie auf **Go to
    resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

8.  Sie gelangen direkt zum **App Service Seite.** Klicken Sie oben
    links auf Home.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.jpeg)

9.  Klicken Sie auf das Menü Portal und wählen Sie dort **Resource
    Groups** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.jpeg)

10. Wählen Sie die Ressourcengruppe aus, die Ihnen zugewiesen ist, und
    stellen Sie sicher, dass die folgenden Ressourcen aus der
    Bereitstellung erstellt werden, die wir gerade ausgeführt haben.

> \- App Service plan
>
> \- App Service
>
> \- Virtual network
>
> \- Azure Database for PostgreSQL flexible server
>
> \- Private DNS zone

![A screenshot of a group AI-generated content may be
incorrect.](./media/image22.png)

## Übung 3: Überprüfen der Verbindungseinstellungen

Der Erstellungsassistent hat die Konnektivitätsvariablen bereits als
App-Einstellungen für Sie generiert. In diesem Schritt erfahren Sie, wo
Sie die App-Einstellungen finden und wie Sie Ihre eigenen erstellen
können.

1.  Klicken Sie in der Liste der Ressourcen in der **App Service** auf
    App Service.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image23.jpeg)

2.  Wählen Sie auf der Seite App Service im linken Menü die Option
    **Environment variables** unter **Settings**.

3.  Überprüfen Sie auf der Registerkarte **App-Einstellungen** der Seite
    **Environment variables**, ob **AZURE_POSTGRESQL_CONNECTIONSTRING**
    vorhanden ist. Sie wird zur Laufzeit als Umgebungsvariable
    eingefügt.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

4.  Wählen Sie **+ Add**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.jpeg)

5.  Nennen Sie die Einstellung +++**PORT**+++ und setzen Sie den Wert
    auf +++**8080**+++, was der Standardport der Quarkus-Anwendung ist.
    Wählen Sie **Anwenden** aus.

![A screenshot of a login AI-generated content may be
incorrect.](./media/image26.jpeg)

6.  Wählen Sie **Apply** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

7.  Wählen Sie **Confirm** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

8.  Sie erhalten eine Benachrichtigung, dass die App-Einstellungen
    aktualisiert wurden.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image29.jpeg)

## Übung 4: Bereitstellen von Beispielcode

In diesem Schritt konfigurieren Sie die GitHub-Bereitstellung mithilfe
von GitHub Actions. Dies ist nur eine von vielen Möglichkeiten für die
Bereitstellung in App Service, aber auch eine hervorragende Möglichkeit,
Continuous Integration in Ihren Bereitstellungsprozess zu ermöglichen.
Standardmäßig wird bei jedem Git-Push an Ihr GitHub-Repository die
Build- und Bereitstellungsaktion gestartet.

1.  Auf der App Service Seite, wählen Sie im linken Menü **Deployment
    Center** unter **Deployment** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

1.  Wählen Sie unter Quelle die Option **GitHub**. Standardmäßig ist
    GitHub Actions als Buildanbieter ausgewählt.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

2.  Klicken Sie auf **Authorize**, melden Sie sich bei Ihrem
    GitHub-Konto an, und folgen Sie der Anweisung, um Azure zu
    autorisieren.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image32.jpeg)

3.  Geben Sie die Details wie folgt ein, belassen Sie den Rest auf der
    Standardeinstellung und klicken Sie auf **Save**.

[TABLE]

4.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image33.jpeg)

&nbsp;

1.  Sobald Sie auf Save geklickt haben, committet App Service eine
    Workflowdatei in das ausgewählte GitHub-Repository im Verzeichnis
    .github/workflows.

&nbsp;

5.  Führen Sie im GitHub-Codespace Ihres Beispielforks +++git pull
    origin main+++ aus. Dadurch wird die neu committete Workflowdatei in
    Ihren Codespace gezogen.

\[!Note\] **Anmerkung:** Wenn Sie feststellen, dass Testfälle noch im
Terminal ausgeführt werden, können Sie Strg+C drücken und dann den
obigen Befehl ausführen.

![A screenshot of a computer code AI-generated content may be
incorrect.](./media/image34.jpeg)

1.  Öffnen Sie **src/main/resources/application.properties** im
    Explorer. Quarkus verwendet diese Datei, um Java-Eigenschaften zu
    laden.

&nbsp;

1.  Suchen des Codes (lines 10-11). Dieser Code legt die
    Produktionsvariable **%prod.quarkus.datasource.jdbc.url** auf die
    App-Einstellung fest, die der Erstellungsassistent für Sie
    verwendet. **quarkus.package.type** ist so festgelegt, dass ein
    Uber-Jar erstellt wird, dass Sie in App Service ausführen müssen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.jpeg)

7.  Öffnen Sie
    **.github/workflows/main_quarkuwebapp\[Lab-Instanz-ID\].yml** im
    Explorer. Diese Datei wurde vom App Service-Assistenten zum
    Erstellen erstellt.

&nbsp;

1.  Ändern Sie im Schritt Build with Maven den Maven-Befehl in +++mvn
    clean install -DskipTests+++.

**-DskipTests** überspringt die Tests in Ihrem Quarkus Projekt, um zu
vermeiden, dass der GitHub-Workflow vorzeitig fehlschlägt.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

8.  Wählen Sie die **Source Control** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

9.  Geben Sie in das Textfeld eine Commit-Nachricht wie folgt
    ein+++**Configure DB and deployment workflow**+++. Wählen Sie
    **Commit**, bestätigen Sie dann mit **Yes**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.jpeg)

10. Wählen Sie **Sync changes 1** und bestätigen Sie mit **OK**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

11. Wählen Sie im Azure-Portal auf der Seite Bereitstellungscenter die
    Option **Logs** aus. Ein neuer Bereitstellungslauf wurde bereits mit
    den festgeschriebenen Änderungen gestartet.

&nbsp;

1.  Wählen Sie im Protokolleintrag für die Bereitstellungsausführung den
    **Build/Deploy Logs** mit dem neuesten Zeitstempel aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.jpeg)

12. Sie werden zu Ihrem GitHub-Repository weitergeleitet und sehen, dass
    die GitHub-Aktion ausgeführt wird. In der Workflowdatei werden zwei
    separate Phasen definiert: Erstellen und Bereitstellen. Warten Sie,
    bis die GitHub-Ausführung den Status abgeschlossen anzeigt. Die
    Fahrt dauert ca. 5 Minuten.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image42.jpeg)

## Übung 5: Navigieren zur App

1.  Über das
    Azure-Portal(+++[https://portal.azure.com+++](https://portal.azure.com+++/)),
    Öffnen Sie die Ressourcengruppe **ResourceGroup1,** und wählen Sie
    die **App Service-Ressource** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

1.  Wählen Sie im linken Menü **Overview** und dann unter **Default
    Domain** die URL Ihrer App aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

2.  Fügen Sie die kopierte URL in einen neuen Browser ein, um die App zu
    öffnen.

![A screenshot of a fruit list AI-generated content may be
incorrect.](./media/image45.jpeg)

3.  Füge der Liste ein paar Früchte hinzu. Jetzt führen Sie eine Web-App
    in Azure App Service mit sicherer Konnektivität mit Azure Database
    for PostgreSQL aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.jpeg)

## Übung 6: Streamen von Diagnoseprotokollen

Azure App Service erfasst alle Nachrichten, die an die Konsole
ausgegeben werden, um Sie bei der Diagnose von Problemen mit Ihrer
Anwendung zu unterstützen. Die Beispielanwendung enthält standardmäßige
JBoss-Protokollierungsanweisungen, um diese Funktion wie unten gezeigt
zu veranschaulichen.

1.  Wählen Sie auf der App Service-Seite des **Azure-Portals** im linken
    Menü **unter** Überwachung die Option **App Service-Logs** aus.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image48.jpeg)

1.  Wählen Sie unter **Application logging** die Option **File System**.
    Wählen Sie im oberen Menü **Save** aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.jpeg)

1.  Wählen Sie im linken Menü die Option **Log stream** aus. Sie sehen
    die Protokolle für Ihre App, einschließlich Plattformprotokolle und
    Protokolle aus dem Container.

![A computer screen shot of a computer screen AI-generated content may
be incorrect.](./media/image50.jpeg)

## Übung 7: Bereinigen von Ressourcen

1.  Wählen Sie auf der Startseite des Azure-Portals die Option
    Ressourcengruppen aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

2.  Wählen Sie den **NetworkWatcherRG** aus und klicken Sie auf **Delete
    resource group**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

3.  Geben Sie +++NetworkWatcherRG+++ in das Textfeld ein und klicken Sie
    auf **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image54.png)

4.  Wählen Sie als Nächstes auf der Seite Resource group die zugewiesene
    Resource group aus.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

5.  Wählen Sie alle **resources**, und wählen Sie dann **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

6.  Geben Sie +++delete+++ in das Textfeld ein und klicken Sie auf
    **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image58.png)

7.  Eine Erfolgsbenachrichtigung auf den gelöschten Ressourcen bestätigt
    den Löschvorgang.

8.  Zurück im GitHub Workspace klicken Sie auf das Dropdown-Menü neben
    **Code**, wählen Sie die drei Punkte neben dem Codespace-Namen aus
    und klicken Sie auf **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

**Zusammenfassung:**

Wir haben gelernt, eine sichere Quarkus-Anwendung in Azure App Service
bereitzustellen und sie mit der PostgreSQL-Datenbank zu verbinden, um
Fruit-Namen über die Benutzeroberfläche der App hinzuzufügen.
