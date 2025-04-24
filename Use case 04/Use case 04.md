# Cas d'usage 04 - Créez une liste de tâches à ASP.NET application, déployez-la sur Azure App Service en vous connectant à SQL Database

**Durée estimée :** 40 minutes

**Type de Lab :** Dirigé par un instructeur

**Objectif :**

Azure App Service fournit un service d'hébergement web hautement
évolutif et à correction automatique. Dans cet atelier, vous allez
apprendre à déployer une application ASP.NET pilotée par les données
dans App Service et à la connecter à Azure SQL Database. Lorsque vous
avez terminé, vous disposez d'une application ASP.NET en cours
d'exécution dans Azure et connectée à SQL Database.

## Exercice 0 : Comprendre la machine virtuelle et les informations d'identification

Dans cette tâche, nous identifierons et comprendrons les informations
d'identification que nous utiliserons tout au long du Lab.

1.  L'onglet **Instructions** contient le guide de Lab avec les
    instructions à suivre tout au long du Lab.

2.  L'onglet **Resources** contient les informations d'identification
    nécessaires à l'exécution du Lab.

    - **URL** – URL du portail Azure

    - **Subscription** – Il s'agit de l'ID de l'abonnement qui vous a
      été attribué

    - **Username** : ID utilisateur avec lequel vous devez vous
      connecter aux services Azure.

    - **Password** : mot de passe pour la connexion Azure. Appelons ce
      nom d'utilisateur et ce mot de passe en tant qu'identifiants de
      connexion Azure. Nous utiliserons ces crédits chaque fois que nous
      mentionnerons les identifiants de connexion Azure.

    - **Resource Group** : **Resource Group** qui vous est attribué.

\[ ! Alerte\] **Important :** Assurez-vous de créer toutes vos
ressources sous ce groupe de ressources

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image1.png)

3.  L'onglet **Help** contient les informations d'assistance. La valeur
    **ID** ici est l'ID de **Lab instance** qui sera utilisé lors de
    l'exécution du labo.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image2.png)

## Exercice 1 : Déploiement d'une application ASP.NET sur Azure avec Azure SQL Database

### Tâche 1 : Configurer Visual Studio 2022 et exécuter l'application

1.  Dans la barre de **recherche (search)** Windows, tapez +++**Visual
    studio**+++ et sélectionnez Visual Studio 2022. S'il vous demande de
    vous connecter, passez aux étapes 2 et 3 ou à l'étape 4.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image3.jpeg)

2.  Cliquez sur **Sign in**, puis **Sign in** avec le **nom
    d'utilisateur (Username)** et le **mot de passe** (**password**)dans
    la section **User Credentials** dans l'onglet Ressources de la
    machine virtuelle.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image4.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image5.png)

3.  Sélectionnez **Start Visual Studio**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image6.jpeg)

4.  Sélectionnez **Open a local folder**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image7.jpeg)

5.  Sélectionnez le dossier **webappwithsqldb** dans **C :\Labfiles** et
    cliquez sur **Select Folder**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image8.jpeg)

6.  Une fois le dossier ouvert, double-cliquez sur
    **DotNetAppSqlDb.sln** dans la **Solution Explorer**.

**Remarque :** Si l'Explorateur de solutions ne s'ouvre pas
automatiquement, cliquez sur **View -\> Solution Explorer.**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image9.jpeg)

7.  Cliquez sur **Build** -\> **Build Solution**.

![Capture d'écran d'ordinateur d'un écran noir Le contenu généré par
l'IA peut être incorrect.](./media/image10.jpeg)

8.  Une fois la build terminée, sélectionnez **Debug -\> Start
    Debugging**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image11.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image12.jpeg)

9.  Cela ouvre un navigateur dans lequel **Todos web app** s'exécute.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image13.jpeg)

10. Ajoutez quelques éléments dans l'application en cliquant sur
    **Create New** comme dans les captures d'écran ci-dessous.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image14.jpeg)

![Une capture d'écran d'une application Le contenu généré par l'IA peut
être incorrect.](./media/image15.jpeg)

11. Ajoutez quelques éléments supplémentaires à la liste.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image16.jpeg)

12. À partir de Visual Studio 2022, cliquez sur **Debug -\> Stop
    Debugging**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image17.jpeg)

### Tâche 2 : Publier ASP.NET application sur Azure

1.  Dans **Solution Explorer**, cliquez avec le bouton droit sur votre
    projet **DotNetAppSqlDb** et sélectionnez **Publish**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image18.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image19.jpeg)

2.  Sélectionnez **Azure,** puis cliquez sur **Next**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image20.jpeg)

3.  Sélectionnez **Azure App Service (Windows)** dans **Which Azure
    service would you like to use to host your application?** écran et
    cliquez sur **Next**.

![Capture d'écran d'une application informatique Le contenu généré par
l'IA peut être incorrect.](./media/image21.jpeg)

4.  Dans la boîte de dialogue Publier, cliquez sur **Sign In** et Se
    connecter à votre abonnement Azure, si vous n'êtes pas déjà
    connecté.

**Remarque :** Si vous êtes déjà connecté à un compte Microsoft,
assurez-vous que ce compte contient votre abonnement Azure. Si le compte
Microsoft connecté n'a pas votre abonnement Azure, cliquez dessus pour
ajouter le compte approprié.

5.  Cliquez sur **Create new** pour créer un nouveau service
    d'application.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image22.jpeg)

6.  Entrez les détails ci-dessous.

[TABLE]

7.  Cliquez sur **New** sur **Hosting Plan**.

8.  ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA
    peut être incorrect.](./media/image23.png)

9.  Cliquez sur **New** sur l'option **Hosting Plan**, entrez les
    détails ci-dessous et cliquez sur **OK**.

[TABLE]

10. ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA
    peut être incorrect.](./media/image24.png)

11. Cliquez sur **Create** dans la fenêtre App Service et attendez que
    les ressources Azure soient créées.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image25.png)

12. La boîte de dialogue **Publish** affiche les ressources que vous
    avez configurées. Cliquez sur **Finish**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image26.png)

13. Cliquez sur **Close**.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image27.jpeg)

14. Faites défiler jusqu'à la section Dépendances du serveur et cliquez
    sur **+ sign** pour ajouter une dépendance.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image28.jpeg)

15. Sélectionnez **Azure SQL Database** dans la page **Add dependency**
    et cliquez sur **Next**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image29.jpeg)

16. Cliquez sur **Create New** en regard des bases de données SQL, dans
    la **Connect to Azure SQL Database** Se connecter à Azure SQL
    Database.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image30.jpeg)

17. Dans la boîte de **Azure SQL Database Create new**, cliquez sur
    **New** en regard du serveur de base de données.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image31.png)

18. Remplissez les détails ci-dessous et cliquez sur **OK.**

[TABLE]

19. ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA
    peut être incorrect.](./media/image32.png)

20. Cliquez sur **Create** dans la boîte de dialogue Créer.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image33.png)

### Tâche 3 : Configurer la connexion à la base de données

1.  Lorsque l'assistant a terminé de créer les ressources de base de
    données, cliquez sur **Next**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image34.png)

2.  Renseignez les détails ci-dessous dans la boîte de dialogue
    **Connect to Azure SQL Database** et cliquez sur **Finish**.

[TABLE]

3.  ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA
    peut être incorrect.](./media/image35.jpeg)

4.  Cliquez sur **Finish** après avoir consulté le **summary of
    changes**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image36.png)

5.  Attendez la fin de l'assistant de configuration et cliquez sur
    **Close**. La base de données SQL Azure est maintenant **connected**
    à votre application.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image37.jpeg)

6.  Sur la page Publier, cliquez sur **Publish** dans le coin supérieur
    droit.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image38.png)

**Remarque :** Cela prendra environ 5 minutes

7.  Une fois que votre application ASP.NET est déployée sur Azure, votre
    navigateur par défaut est lancé avec l'URL de l'application
    déployée. **Add a few to-do items**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image39.jpeg)

### Tâche 4 : Accéder à la base de données localement

Visual Studio vous permet d'explorer et de gérer facilement votre
nouvelle base de données dans Azure dans **SQL Server Object Explorer**.
La nouvelle base de données a déjà ouvert son pare-feu à l'application
App Service que vous avez créée. Mais pour y accéder à partir de votre
ordinateur local (par exemple, à partir de Visual Studio), vous devez
ouvrir un pare-feu pour l'adresse IP publique de votre ordinateur local.
Si votre fournisseur d'accès Internet modifie votre adresse IP publique,
vous devez reconfigurer le pare-feu pour accéder à nouveau à la base de
données Azure.

1.  Dans le menu **View** de Visual Studio 2022, sélectionnez **SQL
    Server Object Explorer**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image40.jpeg)

2.  En haut de **SQL Server Object Explorer**, cliquez sur le bouton
    **Add SQL Server**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image41.jpeg)

### Tâche 5 : Configurer la connexion à la base de données

1.  Dans la boîte de dialogue **Connect**, développez le nœud **Azure**.
    Toutes vos instances SQL Database dans Azure sont répertoriées ici.

2.  Sélectionnez la base de données que vous avez créée précédemment
    (**dotnetappsqldbdbserver98**). La connexion que vous avez créée
    précédemment est automatiquement renseignée en bas.

3.  Tapez **password** de l'administrateur de base de données que vous
    avez créé précédemment (+++**PassWord98**+++) et cliquez sur
    Connecter.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image42.jpeg)

### Tâche 6 : Autoriser la connexion client à partir de votre ordinateur

La boîte de dialogue Créer une nouvelle règle de pare-feu s'ouvre. Par
défaut, un serveur autorise uniquement les connexions à ses bases de
données à partir de services Azure, tels que votre application Azure.
Pour vous connecter à votre base de données à partir de l'extérieur
d'Azure, créez une règle de pare-feu au niveau du serveur. La règle de
pare-feu autorise l'adresse IP publique de votre ordinateur local.

La boîte de dialogue est déjà remplie avec l'adresse IP publique de
votre ordinateur.

1.  Assurez-vous que l'option Ajouter l'adresse IP de mon client est
    sélectionnée et cliquez sur OK.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image43.jpeg)

2.  Une fois que Visual Studio a terminé de créer le paramètre de
    pare-feu pour votre instance de base de données SQL, votre connexion
    s'affiche dans **SQL Server Object Explorer**.

3.  Étendez votre **connection \> Databases \> \< YOUR DATABASE \> \>
    Tables**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image44.jpeg)

4.  Cliquez avec le bouton droit de la souris sur la table **Todoes** et
    sélectionnez **View Data**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image45.jpeg)

5.  Affichez le contenu du tableau. Les données ajoutées à partir de
    l'interface utilisateur de l'application doivent être répertoriées
    ici.

![](./media/image46.jpeg)

## Exercice 2 : Mettre à jour l'application à l'aide de Code First Migrations

1.  Dans **Solution Explorer**, ouvrez **Models\Todo.cs** dans l'éditeur
    de code. Ajoutez la propriété suivante à la classe **ToDo** en tant
    que dernière ligne (après le **public DateTime CreatedDate { get ;
    set ; }** ) et cliquez sur **Save**.

+++**public bool Done { get ; set ; }**+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image47.jpeg)

### Tâche 1 : Exécuter les migrations Code First localement

Exécutez quelques commandes pour effectuer des mises à jour de votre
base de données locale.

1.  Dans le menu **Tools**, cliquez sur **NuGet Package Manager \>
    Package Manager Console**.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image48.jpeg)

2.  Dans la fenêtre de la console du gestionnaire de packages, activez
    les migrations Code First en exécutant cette commande.

+++ **Enable-Migrations** +++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image49.jpeg)

3.  Ajoutez une migration en exécutant la commande ci-dessous.

+++**Add-Migration** **AddProperty**+++

![](./media/image50.jpeg)

4.  Mettez à jour la base de données locale en exécutant la commande
    ci-dessous.

+++ **Update-Database** +++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image51.jpeg)

5.  Tapez **Ctrl+F5** pour exécuter l'application ou cliquez sur **Debug
    -\> Start without Debugging**. Testez l'édition, les détails et
    créez des liens.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image52.jpeg)

6.  La page de l'application s'ouvre et elle a toujours le même aspect,
    car votre logique d'application n'utilise pas encore cette nouvelle
    propriété.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image53.jpeg)

### Tâche 2 : Utiliser la nouvelle propriété

Apportez quelques modifications à votre code pour utiliser la propriété
Done.

1.  À partir de Visual Studio, ouvrez
    **Controllers\TodosController.cs**. Trouvez la méthode **Create()**
    à la ligne 52 et ajoutez +++**Done**+++ à la liste des propriétés
    dans l'attribut Bind. Lorsque vous avez terminé, la signature de
    votre méthode Create() ressemble au code suivant :

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image54.jpeg)

2.  Ouvrez **Views\Todos\Create.cshtml**. Ajoutez le code suivant après
    le \> div \< class="form-group » pour **CreatedDate**.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image55.jpeg)

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

3.  Ouvrez **Views\Todos\Index.cshtml**. Ajoutez le code suivant dans
    l’élément **th** vide, après le **th** élément pour **CreatedDate**.

<+++@Html.DisplayNameFor>(model =\> model.Done)+++

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image56.jpeg)

4.  Add this code just above the html.ActionLink() helper methods.

5.  \<td\>

6.  @Html.DisplayFor(modelItem =\> item.Done)

\`\`\`

! \[\](./media/image53.jpeg)

5.  Tapez **Ctrl+F5** pour exécuter l'application.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image57.jpeg)

### Tâche 3 : Activer les migrations Code First dans Azure

1.  Faites un clic droit sur le projet et sélectionnez **Publish**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image58.jpeg)

2.  Cliquez sur **More actions \> Edit** pour ouvrir les paramètres de
    publication.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image59.jpeg)

3.  Dans le menu déroulant **MyDatabaseContext**, sélectionnez la
    connexion à la base de données de votre base de données SQL Azure.

4.  Sélectionnez **Execute Code First Migrations** d'abord (s'exécute au
    démarrage de l'application), puis cliquez sur **Save**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image60.jpeg)

5.  Sur la page Publier, cliquez sur **Publish**.

![Un objet rectangulaire noir avec du texte blanc Le contenu généré par
l'IA peut être incorrect.](./media/image61.jpeg)

6.  L'application mise à jour est désormais disponible sur Azure.

7.  Essayez à nouveau d'ajouter des tâches et sélectionnez **Done**, et
    ils devraient apparaître sur votre page d'accueil en tant qu'élément
    terminé.

![Une capture d'écran d'une application Le contenu généré par l'IA peut
être incorrect.](./media/image62.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image63.jpeg)

## Exercice 3 : Journaux d'application de flux

1.  Sur la page de publication, faites défiler jusqu'à la section
    **Hosting**. Dans le coin droit, cliquez sur **...** \> **View
    Streaming Logs**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image64.jpeg)

2.  Les journaux sont maintenant diffusés dans la fenêtre Sortie.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image65.jpeg)

3.  Vous ne voyez pas encore les messages de suivi, car, lorsque vous
    sélectionnez Afficher les journaux de streaming pour la première
    fois, votre application Azure définit le niveau de suivi sur Erreur,
    qui consigne uniquement les événements d'erreur.

\[ ! Remarque\] **Remarque :** Redémarrez le flux de journalisation à
partir de Visual Studio si vous ne les voyez pas encore.

### Tâche 1 : Modifier les niveaux de trace

1.  Accédez à la page de publication. Dans la section Hébergement,
    cliquez sur **... \> Open in Azure portal**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image66.jpeg)

2.  Dans la page Portail Azure – application, sélectionnez **App Service
    logs** le volet gauche sous la section **Monitoring**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image67.jpeg)

3.  Sous **Application Logging** (système de fichiers), sélectionnez
    **Verbose** dans Niveau. Cliquez sur **Save**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image68.jpeg)

4.  À partir de votre navigateur, accédez à l'application web sur Azure
    et effectuez certaines activités.

![Une capture d'écran d'une application Le contenu généré par l'IA peut
être incorrect.](./media/image69.jpeg)

5.  Les messages de suivi sont désormais diffusés en continu dans la
    fenêtre Sortie de Visual Studio.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image70.jpeg)

6.  Pour arrêter le service de diffusion en continu des journaux,
    cliquez sur le bouton **Stop monitoring** dans la fenêtre Sortie.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image71.jpeg)

7.  Fermez Visual Studio.

## Exercice 4 : Nettoyer les ressources

1.  À partir du portail Azure, ouvrez le groupe de ressources qui vous a
    été attribué.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image72.png)

2.  Sélectionnez toutes les ressources et cliquez sur Supprimer.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image73.png)

3.  Tapez +++delete+++ dans la zone de texte et sélectionnez Supprimer.
    Sélectionnez delete dans la boîte de dialogue de confirmation.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image74.png)

![Une capture d'écran d'une erreur informatique Le contenu généré par
l'IA peut être incorrect.](./media/image75.png)

**Résumé**

Dans cet atelier, vous avez appris à déployer une application ASP.NET
pilotée par les données dans App Service et à la connecter à Azure SQL
Database.
