# Anwendungsfall 05 – Bereitstellen einer datengesteuerten Python-Restaurant-Web-App mit der Azure Database for PostgreSQL

**Objektiv:**

In diesem Anwendungsfall wird eine Python-Web-App mithilfe des
Flask-Frameworks und des relationalen Datenbankdiensts Azure Database
for PostgreSQL bereitgestellt. Die Flask-App wird in einem vollständig
verwalteten Azure App Service gehostet. Diese App ist so konzipiert,
dass sie lokal ausgeführt und dann in Azure bereitgestellt wird

![Ein Diagramm eines Serviceplans Beschreibung wird automatisch
generiert](./media/image1.jpeg)

Sie stellen eine datengesteuerte Python-Web-App (**Django** oder
**Flask**) in **Azure App Service** mit dem relationalen Datenbankdienst
**Azure Database for PostgreSQL** bereit . Azure App Service unterstützt
Python in einer Linux-Serverumgebung.

**Verwendete Schlüsseltechnologien**: Java 17, Azure Database for
PostgreSQL

**Geschätzte Dauer**: 45 Minuten

**Labtyp:** Von einem Kursleiter geleitet

**Voraussetzungen:**

GitHub-Konto – Es wird erwartet, dass Sie über Ihre eigenen
GitHub-Anmeldeinformationen verfügen. Wenn Sie noch keine haben,
erstellen Sie bitte eine von hier aus -
**https://github.com/signup?user_email=&source=form-home-signupobjectives**

Das **requirements.txt** verfügt über die folgenden Pakete, die alle von
einer typischen datengesteuerten Flask-Anwendung verwendet werden:

[TABLE]

### Aufgabe 1: Registrieren des Dienstanbieters

1.  Öffnen Sie einen Browser, wechseln Sie zu
    <https://portal.azure.com>, [und melden Sie sich mit Ihrem Cloud
    Slice-Konto an, das auf der Registerkarte "Resource" Ihrer VM
    verfügbar ist.](https://portal.azure.com)

> ![](./media/image2.png)

2.  Klicken Sie auf der Startseite des Azure-Portals auf die Kachel
    **Resource groups**.

![](./media/image3.png)

3.  Kopieren Sie den Namen der Ressourcengruppe, und speichern Sie ihn
    im Editor, um die nächste Aufgabe zum Bereitstellen der
    erforderlichen Ressourcen in dieser Ressourcengruppe zu verwenden.

![](./media/image4.png)

4.  Klicken Sie in der oberen Navigation auf Home.

![](./media/image5.png)

5.  Klicken Sie auf die Kachel **Subscriptions**.

![](./media/image6.png)

6.  Klicken Sie auf den Namen des Abonnements.

![](./media/image7.png)

7.  Erweitern Sie Einstellungen im linken Navigationsmenü. Klicken Sie
    auf **Resource providers**, geben Sie Microsoft.AlertsManagement
    ein, wählen Sie es aus und klicken Sie dann auf **Register**.

> ![](./media/image8.png)
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image9.png)

### Aufgabe 2: Erstellen von GitHub Codespace zum Initiieren der Azure Developer CLI-Vorlage

Dieser Anwendungsfall verfügt über eine
Entwicklungscontainerkonfiguration, die es einfacher macht, Apps lokal
zu entwickeln, in Azure bereitzustellen und zu überwachen. Wir verwenden
CLI-Vorlagen für die Azure-Entwicklung, um Apps bereitzustellen

1.  Öffnen Sie einen Browser und gehen Sie zu '' **https:\\github.com**
    '' und melden Sie sich mit Ihrem Github-Konto an.

2.  Forken Sie diese Repository-
    https://github.com/technofocus-pte/msdocs-flask-postgresql-sample-app 
    in Ihr Konto, indem Sie auf **Fork** klicken, wie in der Abbildung
    unten gezeigt.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image10.jpeg)

3.  Geben Sie den eindeutigen Namen ein und klicken Sie dann auf
    **Create repo**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image11.jpeg)

4.  Wählen Sie im Repository-Stammverzeichnis Ihres Forks **Code** \>
    **Codespaces** \> **+** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image12.jpeg)

5.  Warten Sie, bis der Arbeitsbereich eingerichtet ist.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image13.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image14.jpeg)

6.  Führen Sie im Codespace-Terminal die folgenden Befehle aus:

> \# Install requirements

\`\`python3 -m pip install -r requirements.txt\`\`

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image15.jpeg)

7.  Führen Sie den folgenden Befehl aus, um eine Umgebungsvariable zu
    erstellen

\# Create .env with environment variables

\`\`cp .env.sample.devcontainer .env\`\`

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image16.jpeg)

8.  Führen Sie den folgenden Befehl für die Datenmigration aus

\# Run database migrations

\`\`python3 -m flask db upgrade\`\`

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image17.jpeg)

9.  Führen Sie den folgenden Befehl aus, um

\# Start the development server

\`\`python3 -m flask run\`\`

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image18.jpeg)

10. Wenn die Meldung “Your application running on port is available”
    angezeigt wird, klicken Sie auf **Open in Browser**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image18.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image19.jpeg)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image20.jpeg)

11. Klicken Sie auf die Schaltfläche **Add new restaurant**.

![Ein weißer Bildschirm mit schwarzem Text Beschreibung wird automatisch
generiert](./media/image21.jpeg)

12. Geben Sie die unten angegebenen Details ein und klicken Sie auf die
    Schaltfläche **Submit**.

Name : ''**Contoso Rica**''

Adresse - ''**3A ,8th cross, Ferns street , Singapore** ''

Beschreibung - '' **This is a medium to high priced restaurant in the
city shopping center** ''

![Ein Screenshot eines Restaurants Beschreibung wird automatisch
generiert](./media/image22.jpeg)

13. Klicken Sie auf die Schaltfläche **Add new review**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image23.jpeg)

1.  Geben Sie Ihre Bewertung ein und klicken Sie dann auf die
    Schaltfläche. **Save changes**

**Your Name: Ihr Name**

**Rating : Ihre Bewertung**

" This is a medium to high priced restaurant in the city shopping
center. Service was a little bit confusing as we had at least 6 waiters
coming to ask us things. Food took some time to come. We had 2 menus:
one indian and one thai. The thai is 30% cheaper so we went for some
appetizers and thai red curry. Food took some time but it was worth it.
It was delicious and very well prepared. Overall, this is a good eat."

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image24.jpeg)

![Eine weiße Karte mit schwarzem Text Beschreibung wird automatisch
generiert](./media/image25.jpeg)

2.  Fügen Sie weitere Bewertungen und ein neues Restaurant mit
    Kommentaren hinzu.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image26.jpeg)

### Aufgabe 3: Bereitstellen der erforderlichen Ressource in Azure.

Dieses Projekt ist so konzipiert, dass es gut mit der Azure
Developer-CLI zusammenarbeitet, die es einfacher macht, Apps lokal zu
entwickeln, in Azure bereitzustellen und zu überwachen.

1.  Wechseln Sie zurück zur Registerkarte Github Code Space,Führen Sie
    den folgenden Befehl aus, um eine neue azd-Umgebung zu
    initialisieren:

> \`\`azd init\`\`

![](./media/image27.jpeg)

2.  Sie werden aufgefordert, einen Umgebungsnamen anzugeben (z. B**.
    flask-app**XXXX (XXXX kann eine eindeutige Nummer sein)), der später
    im Namen der bereitgestellten Ressourcen verwendet wird.

![](./media/image28.jpeg)

3.  Melden Sie sich bei Bedarf an '' **azd auth login** '' ,kopieren Sie
    den Code und drücken Sie die Eingabe-Taste.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image29.jpeg)

4.  Geben Sie den Code ein, und melden Sie sich dann mit Ihren
    Azure-Anmeldeinformationen an.

![Ein Screenshot eines Computerfehlers Beschreibung wird automatisch
generiert](./media/image30.jpeg)

![](./media/image31.jpeg)

![Ein Screenshot eines Computerfehlers Beschreibung wird automatisch
generiert](./media/image32.jpeg)

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image33.jpeg)

5.  Wechseln Sie zurück zur Registerkarte Gtihub-Codespace und führen
    Sie den folgenden Befehl aus, um alle Ressourcen bereitzustellen .
    Sie werden aufgefordert, Ihr Azure-Abonnement auszuwählen. Geben Sie
    **1** ein, um Ihr Abonnement auszuwählen, und drücken Sie die
    Eingabetaste.

**''azd provision''**

![Ein Computer-Screenshot eines Computercodes Beschreibung wird
automatisch generiert](./media/image34.png)

6.  Wählen Sie den Standort als **WestUS/eastus** aus. Anschließend
    werden die Ressourcen in Ihrem Konto bereitgestellt und der neueste
    Code bereitgestellt. Wenn Sie einen Fehler bei der Bereitstellung
    erhalten, kann es hilfreich sein, den Speicherort (z. B. in
    "westus") zu ändern, da für einige der Ressourcen möglicherweise
    Verfügbarkeitseinschränkungen auftreten.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image35.png)

7.  Geben Sie den Namen der Ressourcengruppe aus Ihrem Azure-Portal ein
    (kopiert in der vorherigen Aufgabe), und drücken Sie die
    Eingabe-Taste.

![](./media/image36.png)

8.  Die Bereitstellung dauert **20 bis 30 Minuten**. Sie können den
    Status der Bereitstellung auch über den generierten Link oder über
    das **Azure-Portal \> Resource group-\> Deployments** überprüfen.

![](./media/image37.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image38.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image39.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image40.png)

### Aufgabe 4: Bereitstellen der Anwendung über Github

1.  Führen Sie den folgenden Befehl aus, um die Umgebungsvariable für
    die Ressourcengruppe festzulegen.

> \`\`azd env set AZURE_RESOURCE_GROUP {Name of existing resource group}
> \`\`

Hinweis: Ersetzen Sie {Name der vorhandenen Ressourcengruppe} durch den
Namen Ihrer Ressourcengruppe, der im Abschnitt Resources auf Ihrer VM
verfügbar ist.

![](./media/image41.png)

2.  Führen Sie den folgenden Befehl aus, um alle Ressourcen
    bereitzustellen, und warten Sie, bis die Bereitstellung erfolgreich
    abgeschlossen wurde.

''azd deploy''

![](./media/image42.png)

3.  Klicken Sie auf die generierte Endpunkt-URL

![](./media/image43.png)

4.  Klicken Sie auf **Open**, um die externe Website zu öffnen.

![](./media/image44.png)

5.  App wird in einem neuen Tab geöffnet.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image45.png)

### Aufgabe 5: Streamen von Diagnostic-Logs

Azure App Service erfasst alle Nachrichten, die an die Konsole
ausgegeben werden, um Sie bei der Diagnose von Problemen mit Ihrer
Anwendung zu unterstützen. Die App enthält print()-Anweisungen, um diese
Funktion zu demonstrieren, wie unten gezeigt.

@app.route('/', methods=\['GET'\])

def index():

print('Request for index page received')

restaurants = Restaurant.query.all()

return render_template('index.html', restaurants=restaurants)

1.  Wechseln Sie zurück zum **Azure-Portal – \> Resource group,** und
    klicken Sie auf **App Service**.

![](./media/image46.png)

2.  Auf der App Service-Seite. Wählen Sie im linken Menü **Monitoring
    -\>** **App Service logs** aus.

![](./media/image47.png)

3.  Vergewissern Sie sich, dass unter **Application logging** die Option
    **File System** ausgewählt ist. Wählen Sie es bei Bedarf aus. Wählen
    Sie im oberen Menü **Save** aus.

![](./media/image48.png)

4.  Wählen Sie im linken Menü die Option **Log stream** aus. Sie sehen
    die Logs für Ihre App, einschließlich Plattform-Logs und Logs aus
    dem Container.

![](./media/image49.png)

### Aufgabe 6: Bereinigen Sie Ressourcen in Github.

1.  Wechseln Sie zurück zu Github, klicken Sie auf **repo -\> Code -\>
    Codespaces.** Wählen Sie den richtigen Zweig aus

![](./media/image50.png)

2.  Wählen Sie den Zweig aus und klicken Sie auf **Delete**.

![](./media/image51.png)

3.  Klicken Sie auf **Delete**.

![](./media/image52.png)

4.  Wechseln Sie zurück zum **Azure-Portal – \> Resource group.**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image53.png)

5.  Wählen Sie alle Ressourcen aus und klicken Sie dann auf **Delete**
    (Ressourcengruppe NICHT löschen)

![](./media/image54.png)

6.  Geben Sie '' **Delete** '' ein und klicken Sie dann auf **Delete**.

![](./media/image55.png)

7.  Klicken Sie auf **Delete** , um den Löschvorgang zu bestätigen.

![](./media/image56.png)
