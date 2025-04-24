# Cas d'usage 02 - Créer une application web Quarkus de liste de fruits (Fruits List) avec Azure App Service sur Linux et PostgreSQL

**Durée estimée :** 40 minutes

**Type de Lab :** Dirigé par un instructeur

**Objectif :**

Ce cas d'usage montre comment créer, configurer et déployer une
application Quarkus sécurisée dans Azure App Service qui est connectée à
une base de données PostgreSQL (à l'aide d'Azure Database pour
PostgreSQL). Azure App Service est un service d'hébergement web
hautement évolutif et autocorrectif qui peut facilement déployer des
applications sur Windows ou Linux. Lorsque vous avez terminé, une
application Quarkus s'exécute sur Azure App Service sur Linux.

**Pré-requis :**

**Compte GitHub** : vous devez disposer de vos propres identifiants de
connexion GitHub. Si vous n'en avez pas, créez-en-un d'ici -
+++<https://github.com/signup?user_email=&source=form-home-signup+++>

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

    - **Password** : mot de passe pour la connexion Azure.

Appelons ce nom d'utilisateur et ce mot de passe en tant qu'identifiants
de connexion Azure. Nous utiliserons ces crédits chaque fois que nous
mentionnerons les identifiants de connexion Azure.

- **Resource Group** : Le **Resource group** qui vous est attribué.

\[ ! Alerte\] **Important :** Assurez-vous de créer toutes vos
ressources sous ce groupe de ressources

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image1.png)

3.  L'onglet **Help** contient les informations d'assistance. La valeur
    de l’**ID** ici est l'ID de l'**instance du Lab** qui sera utilisé
    lors de l'exécution du Lab.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image2.png)

## Exercice 1 : Exécuter l'échantillon

Tout d'abord, vous configurez un exemple d'application pilotée par les
données comme point de départ. L'exemple de référentiel que nous
utilisons ici inclut une configuration de conteneur de développement. Le
conteneur de développement contient tout ce dont vous avez besoin pour
développer une application, y compris la base de données, le cache et
toutes les variables d'environnement nécessaires à l'exemple
d'application. Le conteneur dev peut s'exécuter dans un codespace
GitHub, ce qui signifie que vous pouvez exécuter l'exemple sur n'importe
quel ordinateur doté d'un navigateur web.

1.  À partir d'un navigateur, connectez-vous à votre compte GitHub
    +++\*\*<https://github.com/login**+++>.

2.  Ouvrez cette url à partir d'un nouvel onglet,
    +++\*\*<https://github.com/technofocus-pte/msdocs-quarkus-postgresql-sample-app**+++>.

3.  Sélectionnez **Fork -\> Create a new fork**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image3.jpeg)

4.  Cliquez sur **Create fork** dans la page Créer une nouvelle page
    fork.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image4.jpeg)

5.  Dans la page dupliquée du dépôt, sélectionnez **Code** \> **Create
    codespace on main**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image5.jpeg)

**Remarque :** Si l'option Créer un codespace sur l'option principale ne
s'affiche pas, cliquez sur le symbole + à côté de Codespaces.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image6.jpeg)

**Remarque :** La création du codespace prend environ 10 minutes à
configurer.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image7.jpeg)

6.  Exécutez +++mvn quarkus :dev+++ dans le Terminal. Cliquez sur
    **Allow** dans la fenêtre contextuelle.

![Une capture d'écran d'un navigateur Le contenu généré par l'IA peut
être incorrect.](./media/image8.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image9.jpeg)

7.  Lorsque la notification indique que **Your application running on
    port 8080** est disponible, sélectionnez **Open in Browser**.
    L'exemple d'application doit s'afficher dans un nouvel onglet du
    navigateur.

Si vous voyez une **notification** avec le port **5005**, ignorez-la.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image10.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image11.jpeg)

8.  Pour arrêter le serveur de développement Quarkus, tapez **Ctrl+C**
    dans le terminal Codespace.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image12.jpeg)

## Exercice 2 : Création d'App Service et de PostgreSQL

Tout d'abord, vous créez les ressources Azure. Les étapes utilisées dans
cet atelier créent un ensemble de ressources sécurisées par défaut qui
incluent App Service et Azure Database pour PostgreSQL.

1.  Ouvrez le portail Azure à l'adresse
    +++<https://portal.azure.com/+++> et **connectez-vous** avec les
    informations d'identification de connexion Azure à partir de
    l’onglet **Resources** de la machine virtuelle.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image13.png)

2.  Sélectionnez **Cancel** ou le bouton de fermeture sur la page
    d'accueil.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image14.jpeg)

3.  Entrez +++**web app database**+++ dans la barre de recherche en haut
    du portail Azure. Sélectionnez l'élément intitulé **Web App + Base
    de données** sous l'en-tête **Marketplace**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image15.jpeg)

4.  Dans **Create Web App + Database**, renseignez les détails
    ci-dessous et sélectionnez **Review + create**

[TABLE]

5.  ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA
    peut être incorrect.](./media/image16.png)

6.  ![Une capture d'écran d'une application web Le contenu généré par
    l'IA peut être incorrect.](./media/image17.jpeg)

7.  Une fois la validation réussie, cliquez sur **Create**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image18.png)

**Remarque :** la création de l'application prend environ 15 minutes.

8.  Une fois le déploiement terminé, cliquez sur **Go to resource**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image19.jpeg)

9.  Vous êtes directement redirigé vers **App Service page.** Cliquez
    sur **Home** dans le coin supérieur gauche.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image20.jpeg)

10. Cliquez sur le menu Portail et sélectionnez **Resource Groups**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image21.jpeg)

11. Sélectionnez le groupe de ressources qui vous est attribué et
    vérifiez que les ressources suivantes sont créées à partir du
    déploiement que nous venons d'effectuer.

\- App Service plan

\- App Service

\- Virtual network

\- Azure Database for PostgreSQL flexible server

\- Private DNS zone ![Une capture d'écran d'un groupe Le contenu généré
par l'IA peut être incorrect.](./media/image22.png)

## Exercice 3 : Vérification des paramètres de connexion

L'assistant de création a déjà généré les variables de connectivité pour
vous en tant que paramètres d'application. Dans cette étape, vous allez
apprendre où trouver les paramètres de l'application et comment créer
les vôtres.

1.  Cliquez sur **App Service** dans la liste des ressources du groupe
    de ressources.

![Une capture d'écran d'un téléphone Le contenu généré par l'IA peut
être incorrect.](./media/image23.jpeg)

2.  Dans la page App Service, dans le menu de gauche, sélectionnez
    **Environment variables** sous **Settings**.

3.  Dans l’onglet **App settings** de la page **Environment variables**,
    vérifiez que **AZURE_POSTGRESQL_CONNECTIONSTRING** est présent. Il
    est injecté au moment de l'exécution en tant que variable
    d'environnement.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image24.jpeg)

4.  Sélectionnez **+Add**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image25.jpeg)

5.  Nommez le setting +++**PORT**+++ et définissez sa valeur sur
    +++**8080**+++, qui est le port par défaut de l'application Quarkus.
    Sélectionnez **Apply**.

![Une capture d'écran d'un login Le contenu généré par l'IA peut être
incorrect.](./media/image26.jpeg)

6.  Sélectionnez **Apply**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image27.jpeg)

7.  Sélectionnez **Confirm**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image28.jpeg)

8.  Vous recevrez une notification indiquant que les paramètres de
    l'application ont été mis à jour.

![Une capture d'écran d'un téléphone Le contenu généré par l'IA peut
être incorrect.](./media/image29.jpeg)

## Exercice 4 : Déployer un exemple de code

Dans cette étape, vous allez configurer le déploiement de GitHub à
l'aide de GitHub Actions. Il ne s'agit que d'une des nombreuses façons
de déployer sur App Service, mais aussi d'un excellent moyen d'intégrer
en continu votre processus de déploiement. Par défaut, chaque envoi git
vers votre dépôt GitHub lancera l'action de génération et de
déploiement.

1.  Dans la page App Service, dans le menu de gauche, sélectionnez
    **Deployment Center** sous **Deployment**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image30.jpeg)

2.  Dans Source, sélectionnez **GitHub**. Par défaut, GitHub Actions est
    sélectionné comme fournisseur de build.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image31.jpeg)

3.  Cliquez sur **Authorize**, connectez-vous à votre compte GitHub,
    puis suivez l'invite pour autoriser Azure.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image32.jpeg)

4.  Remplissez les détails comme ci-dessous, laissez le reste par défaut
    et cliquez sur **Save**.

[TABLE]

5.  

> ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
> être incorrect.](./media/image33.jpeg)

6.  Une fois que vous avez cliqué sur **Save**, App Service valide un
    fichier de flux de travail dans le référentiel GitHub choisi, dans
    .github/workflows directory.

7.  De retour dans l'espace de code GitHub de votre exemple de
    duplication, exécutez +++**git pull origin main+++.** Cela extrait
    le fichier de flux de travail nouvellement validé dans votre
    codespace.

\[ ! Remarque\] **Remarque :** Si vous trouvez des cas de test toujours
en cours d'exécution dans le terminal, vous pouvez appuyer sur Ctrl+C,
puis exécuter la commande ci-dessus.

![Une capture d'écran d'un code informatique Le contenu généré par l'IA
peut être incorrect.](./media/image34.jpeg)

7.  Ouvrez **src/main/resources/application.properties** dans
    l'explorateur. Quarkus utilise ce fichier pour charger les
    propriétés Java.

8.  Trouvez le code (lignes 10-11). Ce code définit la variable de
    production **%prod.quarkus.datasource.jdbc.url** sur le paramètre
    d'application que l'assistant de création vous propose. Le **fichier
    quarkus.package.type** est configuré pour créer un Uber-Jar, que
    vous devez exécuter dans App Service.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image35.jpeg)

9.  Ouvrez **.github/workflows/main_quarkuwebapp\[ID d'instance de
    lab\].yml** dans l'explorateur. Ce fichier a été créé par
    l'Assistant Création d'App Service.

10. Sous l'étape Générer avec Maven, remplacez la commande Maven par
    +++**mvn clean install -DskipTests**+++.

**-DskipTests** ignore les tests de votre projet Quarkus, afin d'éviter
l'échec prématuré du flux de travail GitHub.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image36.jpeg)

11. Sélectionnez l'extension **Source Control**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image37.jpeg)

12. Dans la zone de texte, tapez un message de validation tel que +++
    Configure DB and deployment workflow +++. Sélectionnez **Commit**,
    puis confirmez avec **Yes**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image38.jpeg)

13. Sélectionnez **Sync changes 1**, puis confirmez avec **OK.**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image39.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image40.jpeg)

14. De retour dans la page Centre de déploiement du portail Azure,
    sélectionnez **Logs**. Une nouvelle exécution de déploiement aurait
    déjà démarré à partir de vos modifications validées.

15. Dans l'élément de journal de l'exécution du déploiement,
    sélectionnez **Build/Deploy Logs** avec l'horodatage le plus récent.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image41.jpeg)

16. Vous êtes redirigé vers votre référentiel GitHub et vous voyez que
    l'action GitHub est en cours d'exécution. Le fichier de flux de
    travail définit deux étapes distinctes : la génération et le
    déploiement. Attendez que l'exécution de GitHub affiche l'état
    Terminé. Cela prend environ 5 minutes.

![Une capture d'écran d'une page web Le contenu généré par l'IA peut
être incorrect.](./media/image42.jpeg)

## Exercice 5 : Accéder à l'application

1.  À partir du portail
    Azure(+++https://portal.azure.com[+++](https://portal.azure.com+++/)),
    ouvrez le groupe de ressources **ResourceGroup1** et sélectionnez la
    ressource **App Service**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image43.png)

2.  Dans le menu de gauche, sélectionnez **Overview** et sélectionnez
    l'URL de votre application sous **Default domain**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image44.jpeg)

3.  Collez l'URL copiée dans un nouveau navigateur pour ouvrir
    l'application.

![Une capture d'écran d'une liste de fruits Le contenu généré par l'IA
peut être incorrect.](./media/image45.jpeg)

4.  Ajoutez quelques fruits à la liste. À présent, vous exécutez une
    application web dans Azure App Service, avec une connectivité
    sécurisée à Azure Database pour PostgreSQL.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image46.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image47.jpeg)

## Exercice 6 : Journaux de diagnostic de flux

Azure App Service capture tous les messages générés dans la console pour
vous aider à diagnostiquer les problèmes liés à votre application.
L'exemple d'application inclut des instructions de journalisation JBoss
standard pour démontrer cette capacité, comme indiqué ci-dessous.

1.  Dans la page App Service du portail Azure, dans le menu de gauche,
    sélectionnez **App Service logs** sous **Monitoring**.

![Une capture d'écran d'un téléphone Le contenu généré par l'IA peut
être incorrect.](./media/image48.jpeg)

2.  Sous **Application logging**, sélectionnez **File System**. Dans le
    menu supérieur, sélectionnez **Save**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image49.jpeg)

3.  Dans le menu de gauche, sélectionnez **Log stream**. Vous voyez les
    journaux de votre application, y compris les journaux de plate-forme
    et les journaux à l'intérieur du conteneur.

![Une capture d'écran d'ordinateur d'un écran d'ordinateur Le contenu
généré par l'IA peut être incorrect.](./media/image50.jpeg)

## Exercice 7 : Nettoyer les ressources

1.  Dans la page d'accueil du portail Azure, sélectionnez Groupes de
    ressources.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image51.jpeg)

2.  Sélectionnez **NetworkWatcherRG** et cliquez sur **Delete resource
    group**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image52.png)

3.  Tapez +++NetworkWatcherRG+++ dans la zone de texte et cliquez sur
    **Delete**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image53.png)

![Une capture d'écran d'une erreur informatique Le contenu généré par
l'IA peut être incorrect.](./media/image54.png)

4.  Ensuite, dans la page Groupe de ressources, sélectionnez le groupe
    de ressources attribué.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image55.png)

5.  Sélectionnez toutes les **resources**, puis sélectionnez **Delete**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image56.png)

6.  Tapez +++delete+++ dans la zone de texte et cliquez sur **Delete**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image57.png)

![Une capture d'écran d'une erreur informatique Le contenu généré par
l'IA peut être incorrect.](./media/image58.png)

7.  Une notification de réussite sur les ressources supprimées confirme
    la suppression.

8.  De retour dans GitHub Workspace, cliquez sur le menu déroulant en
    regard de **Code**, sélectionnez les trois points à côté du nom de
    codespace et cliquez sur **Delete**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image59.jpeg)

**Résumé :**

Nous avons appris à déployer une application Quarkus sécurisée dans
Azure App Service, à la connecter à la base de données PostgreSQL pour
ajouter des noms de fruits (Fruit names) à partir de l'interface
utilisateur de l'application.
