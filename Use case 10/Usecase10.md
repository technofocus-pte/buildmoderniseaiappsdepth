# Cas d'usage 10 : Déploiement d'une application de chat pour répondre aux questions de l'utilisateur et suivre l'historique des chats dans les conversations

**Objectif :**

Ce cas d'usage vous guide tout au long des étapes de connexion d'une
application Blazor existante à un compte Azure Cosmos DB pour NoSQL et à
un compte Azure OpenAI. Votre application envoie des invites au modèle
dans Azure OpenAI et analyse les réponses. Votre application stocke
également diverses sessions de conversation et leurs messages
correspondants sous forme d'éléments colocalisés dans un conteneur
unique au sein d'Azure Cosmos DB pour NoSQL.

En bref, l'application :

- **Se connectera** au modèle d'Azure OpenAI à l'aide du Kit de
  développement logiciel (SDK) .NET

- **Envoyera** des invites au modèle et analyser la réponse d'achèvement

- **Se connectera** à Azure Cosmos DB pour NoSQL à l'aide du Kit de
  développement logiciel (SDK) .NET

- **Gérera les** articles avec des opérations individuelles, des
  requêtes et des lots transactionnels

Cet exemple d'application de chat répond aux questions de l'utilisateur
et suit l'historique des conversations de chat.

![](./media/image1.jpeg)

**Les principales technologies utilisées** --, Csharp, nosql, asp-net,
blazor, azure-cosmos-db,

**Durée estimée** -- 45 minutes

**Type de Lab :** Dirigé par un instructeur

**Pré-requis :**

Compte GitHub : vous devez disposer de vos propres identifiants de
connexion GitHub. Si vous n'en avez pas, créez-en un à partir d'ici
-\`\` **https://github.com/signup?user_email=&source=form-home-signupobjectives\`\`**

### Tâche 1: Exécuter le docker

1.  Dans votre champ de recherche Windows, tapez **Docker** , puis
    cliquez sur **Docker Desktop**.

![](./media/image2.jpeg)

### Tâche 2 : Enregistrer le prestataire de services

1.  Ouvrez un navigateur, accédez à <https://portal.azure.com> et
    connectez-vous à l'aide de vos informations d'identification Azure
    disponibles dans l'onglet **Ressource** de votre machine virtuelle.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image3.png)

2.  Sur la page d'accueil du portail Azure, cliquez sur la vignette
    **Resource groups**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image4.png)

3.  Copiez le nom du groupe de ressources et enregistrez-le dans le
    bloc-notes pour utiliser la tâche suivante afin de déployer les
    ressources requises dans ce groupe de ressources (Resource groups).

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image5.png)

4.  Revenez à la page d'accueil, cliquez sur la vignette
    **Subscription**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.png)

5.  Cliquez sur le nom de l'abonnement (subscription name) .

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image7.png)

6.  Cliquez sur **Settings - \> Resource provider** dans le menu de
    navigation de gauche.

![](./media/image8.png)

7.  Tapez \`\`**Microsoft.AlertsManagement**\`\` et appuyez sur Entrée.
    Sélectionnez-le puis cliquez sur **Register**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image9.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image10.png)

### Tâche 3 : Fournir des services et des applications à Azure

1.  Ouvrez un navigateur et allez dans \`\`https:\\github.com\`\` et
    connectez-vous avec votre compte Github. Recherchez le référentiel
    ci-dessous

![](./media/image11.jpeg)

2.  Recherchez le référentiel ci-dessous et cliquez sur **Fork**.

\`\`https://github.com/technofocus-pte/chat-csharp-cosmos-db-nosql-openai\`\`![](./media/image12.jpeg)

3.  Entrez le nom du dépôt, puis cliquez sur **Create repository**.

![](./media/image13.jpeg)

4.  Cliquez sur **Code -\> Code space -\> Open Code space.**

![](./media/image14.jpeg)

5.  Attendez que le conteneur Dev soit configuré . Cela prend 3-5 min

![](./media/image15.jpeg)

6.  Exécutez la commande ci-dessous pour vous connecter à AZD. Copiez le
    code généré et appuyez sur Entrée.

> ''**azd auth login''**

![](./media/image16.jpeg)

7.  Collez le code généré et connectez-vous avec vos informations
    d'identification Azure.

![](./media/image17.jpeg)

![](./media/image18.jpeg)

8.  Exécutez la commande ci-dessous pour initialiser le projet dans le
    répertoire courant. Entrez le nom de l'environnement
    \`\`**cosmoschatapp\`\`** et appuyez sur Entrée.

\`\`azd init \`\`![Une capture d'écran d'un ordinateur Description
générée automatiquement](./media/image19.png)

9.  Exécutez la commande ci-dessous pour déployer les services sur
    Azure, générez votre conteneur. Sélectionnez les valeurs ci-dessous.

> \`\`azd provision\`\`
>
> **Sélectionnez un abonnement Azure à utiliser** : sélectionnez votre
> abonnement
>
> **Sélectionnez un emplacement Azure à utiliser** : **East us/west us**
> (Parfois, USA Est peut ne pas être disponible, choisissez un autre
> emplacement et déployez.)
>
> **Entrez une valeur pour le paramètre d'infrastructure
> 'existingResourceGroupName' : ResourceGroup1**

![](./media/image20.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image21.png)

10. Attendez que la ressource soit complètement provisionnée. Ce
    processus prendra 5 à 10 minutes pour créer toutes les ressources
    requises.

![](./media/image22.png)

### Tâche 4 : Déployer l'application sur Azure

1.  Revenez au portail Azure et cliquez sur la vignette Groupes de
    ressources sur la page d'accueil.

![](./media/image23.png)

2.  Cliquez sur le nom du groupe de ressources (Resource group).

![](./media/image24.png)

3.  Vous devriez voir les ressources ci-dessous

- **Container**

- **Container Registry**

- **Azure Cosmos Db account**

- **AureOpenAI**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image25.png)

4.  Cliquez sur **Nom du registre de conteneurs (Container registry)**.

![](./media/image26.png)

5.  Développez **Setting** dans le menu de navigation de gauche, cliquez
    sur **Admin user check box.** Cochez **la case Utilisateur
    administrateur.** Copiez le **Login server, user name** et le
    **password** dans un bloc-notes pour l'utiliser pour déployer
    l'application.

![](./media/image27.png)

6.  Dupliquez l'onglet pour ouvrir le portail Azrue dans un nouvel
    onglet.

![](./media/image28.png)

7.  Cliquez sur le menu de navigation supérieur du formulaire Nom du
    groupe de ressources.

![](./media/image29.png)

8.  Cliquez sur Nom de l'application conteneur (Container App).

![](./media/image30.png)

9.  Cliquez sur le bouton **Authorize** sous Github-Sign in pour vous
    authentifier avec votre compte GitHub. Autorisez votre compte
    Github.

10. Sélectionnez les valeurs ci-dessous

> **Organization: votre organisation Github**
>
> **Repository:** chat-csharp-cosmos-db-nosql-openai
>
> **Branch :** main

![](./media/image31.png)

11. Faites défiler jusqu'aux **Registry settings** et entrez les valeurs
    ci-dessous, puis cliquez sur le bouton **Start continuous
    deployment**.

- Repository source : **Docker Hub or other registries.**

- Login server URL : Votre serveur de connexion a copié le formulaire
  Registre de conteneurs (étape \#5)

- Username : votre mot de passe depuis le registre des conteneurs (étape
  \#5)

- Password : Votre mot de passe du registre des conteneurs (étape \#5)

![](./media/image32.png)

12. Cliquez sur le lien du fichier de flux de travail. Il ouvre un
    nouvel onglet avec Github.

![](./media/image33.png)

13. Cliquez sur l'onglet **Actions**.

![](./media/image34.png)

14. Attendez la fin du déploiement.

![](./media/image35.png)

15. Ne fermez aucun onglet.

### Tâche 5 : Accéder à l'application de chat

1.  Revenez au portail Azure et cliquez sur **Overview** dans le volet
    de navigation gauche, puis cliquez sur **Application Url**. Il ouvre
    une nouvelle application à charger.

![](./media/image36.png)

2.  Cliquez sur le bouton **Create New Chat**.

![](./media/image37.png)

3.  Entrez l'invite ci-dessous.

« Quelle est la capacité d'accueil de Lumen à Seattle ? »

![](./media/image38.jpeg)

4.  Entrez l'invite ci-dessous. Explorez l'application à l'aide de
    différentes invites.

\`\`is that bigger than Dogger stadium??\`\`\`\`

![](./media/image39.jpeg)

### Tâche 6 : Nettoyer toutes les ressources

Pour nettoyer toutes les ressources créées par cet exemple :

1.  Revenez à l'onglet du portail Github et actualisez la page.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image40.png)

2.  Cliquez sur Code , sélectionnez la branche créée pour ce labo et
    cliquez sur **Delete**.

![](./media/image41.png)

3.  Confirmez la suppression de la branche en cliquant sur le bouton
    **Delete**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image42.png)

5.  Revenez au **Azure portal -\> Resource group- \> Resource group
    name.**

![](./media/image43.png)

6.  Sélectionnez toutes les ressources, puis cliquez sur Supprimer comme
    indiqué dans l'image ci-dessous. (**NE SUPPRIMER PAS groupe de
    ressources**)

![](./media/image44.png)

7.  Tapez \`\`**delete**\`\` dans la zone de texte, puis cliquez sur
    **Delete**.

> ![](./media/image45.png)

8.  Confirmez la suppression en cliquant sur **Delete**.

![](./media/image46.png)

**Résumé :**

Vous avez implémenté des classes de service à l'aide des packages
Microsoft.Azure.Cosmos et Azure.AI.OpenAI sur NuGet. Vous avez envoyé
des invites à l'interface conversationnelle Azure OpenAI avec des
préfixes contextuels et analysé l'utilisation et les propriétés de corps
de la réponse. Vous avez également utilisé Azure Cosmos DB pour NoSQL
pour stocker les sessions de conversation et les messages dans un seul
conteneur.
