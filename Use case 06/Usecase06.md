# Cas d'usage 06 - Déploiement d'une application de conversation sur Azure Container Apps avec PostgreSQL Flexible Server

**Objectif:**

- Pour configurer l'environnement de développement sur Windows en
  installant Azure CLI, Node.js, en attribuant des rôles d'abonnement
  Azure, en démarrant Docker Desktop et en activant l'extension Visual
  Studio Code avec Dev Containers.

- Déployer et tester l'application de chat personnalisée avec PostgreSQL
  et OpenAI sur Azure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image1.jpeg)

Dans ce cas d'utilisation, vous allez configurer un environnement de
développement complet, déployer une application de chat intégrée à
PostgreSQL et vérifier son déploiement sur Azure. Cela implique
l'installation d'outils essentiels tels qu'Azure CLI, Docker et Visual
Studio Code (nous l'avons déjà fait pour vous sur host env), la
configuration des rôles d'utilisateur dans Azure, le déploiement de
l'application à l'aide d'Azure Developer CLI et l'interaction avec les
ressources déployées pour garantir la fonctionnalité.

**Principales technologies utilisées** : Python, FastAPI, modèles Azure
OpenAI, Azure Database pour PostgreSQL et
azure-container-apps,ai-azd-templates.

**Durée estimée** -- 45 minutes

**Type de Lab :** Dirigé par un instructeur

**Pré-requis :**

Compte GitHub : vous devez disposer de vos propres identifiants de
connexion GitHub. Si vous n'en avez pas, créez-en un d'ici
- **https://github.com/signup?user_email=&source=form-home-signupobjectives**

## Exercice 1 : Provisionner, déployer l'application et la tester depuis le navigateur

### Tâche 1 : Copier le nom du groupe de ressources existant

1.  Ouvrez votre navigateur, ouvrez le portail Azure
    \`\`https:\\portal.azure.com\`\`.  Connectez-vous à l'aide de votre
    compte de tranche Azure (informations d'identification Azure***)***
    disponible dans la section instructions/Ressources de votre
    environnement hôte.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image2.jpeg)

2.  Sur la page d'accueil, cliquez sur la vignette **Resource groups**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image3.png)

3.  Assurez-vous qu'un groupe de ressources a déjà été créé pour que
    vous puissiez l'utiliser. Ne supprimez jamais ce groupe de
    ressources. Au lieu de cela, vous pouvez supprimer des ressources au
    sein du groupe de ressources, mais pas le groupe de ressources
    lui-même.

4.  Cliquez sur le nom du groupe de ressources (resource group name)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image4.png)

5.  Copiez le nom du groupe de ressources et enregistrez-le dans le
    Bloc-notes pour l'utiliser pour déployer toutes les ressources dans
    ce groupe de ressources

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image5.png)

### Tâche 2 : Exécuter le docker

1.  Sur le bureau, double-cliquez sur **Docker Desktop**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.jpeg)

2.  Exécutez le Docker Desktop.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image7.jpeg)

### Tâche 3 : Enregistrer le prestataire de services

1.  Revenez à l'onglet du portail Azure, cliquez sur la vignette
    **Subscription**.

![](./media/image8.png)

2.  Cliquez sur le nom de l'abonnement (subscription name).

![](./media/image9.png)

3.  Cliquez sur **Settings - \> Resource provider** dans le menu de
    navigation de gauche.

![](./media/image10.png)

4.  Tapez \`\`**Microsoft.AlertsManagement**\`\` et appuyez sur Entrée.
    Sélectionnez-le puis cliquez sur **Register**.

![](./media/image11.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image12.png)

### Tâche 4 : Environnement de développement ouvert

1.  Ouvrez votre navigateur, naviguez jusqu'à la barre d'adresse, tapez
    ou collez l'URL suivante : L'onglet
    \`\`https://github.com/technofocus-pte/rag-postgres-openai-python.git\`\`
    s'ouvre et vous demande d'ouvrir dans Visual Studio code.
    Sélectionnez **Open Visual Studio Code.**

![](./media/image13.jpeg)

2.  Cliquez sur **fork** pour dupliquer le dépôt. Donnez un nom unique
    au dépôt et cliquez sur le bouton **Create repo**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image14.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image15.jpeg)

3.  Click on **Code -\> Codespaces -\> Codespaces+**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image16.jpeg)

4.  Attendez que l'environnement Codespaces soit configuré.
    L'installation complète prend quelques minutes

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image17.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image18.jpeg)

### Tâche 5 : Approvisionner des services et déployer l'application sur Azure

1.  Exécutez la commande suivante sur le terminal. Il génère le code à
    copier. Copiez le code et appuyez sur Entrée.

\`\`azd auth login\`\`

![](./media/image19.png)

2.  Le navigateur par défaut s'ouvre pour entrer le code généré à
    vérifier. Entrez le code et cliquez sur **Next**.

![](./media/image20.png)

3.  Connectez-vous à l'aide de vos informations d'identification Azure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image21.png)

4.  Pour créer un environnement pour les ressources Azure, exécutez la
    commande Azure Developer CLI suivante. Il vous demande d'entrer le
    nom de l'environnement . Entrez le nom de votre choix et appuyez sur
    Entrée (par exemple :**ragpgpy**)

**Remarque :** Lors de la création d'un environnement, assurez-vous que
le nom est composé de lettres minuscules.

\`\`azd env new\`\`![Une capture d'écran d'un ordinateur Description
générée automatiquement](./media/image22.png)

5.  Exécutez la commande Azure Developer CLI suivante pour
    approvisionner les ressources Azure et déployer le code.

\`\`azd provision \`\`![Une capture d'écran d'un ordinateur Description
générée automatiquement](./media/image23.png)

6.  Lorsque vous y êtes invité, sélectionnez un **subscription** pour
    créer les ressources et sélectionnez la région la plus proche de
    votre emplacement ; dans cet atelier, nous avons choisi la région
    **East US2**.

![](./media/image24.png)

7.  Il vous demandera “**Enter a value for the
    'existingResourceGroupName' infrastructure parameter:**” entrez le
    groupe de ressources copié dans la tâche 1 (par exemple :
    **ResourceGroup1 used for the development slice).** Vous pouvez
    copier le nom du groupe de ressources à partir de la section
    **Ressources**, comme indiqué dans l'image ci-dessous

> ![](./media/image25.png)

8.  Lorsque vous y êtes invité, **enter a value for the 'openAILocation'
    infrastructure parameter** et sélectionnez la région la plus proche
    de votre emplacement. Dans cet atelier, nous avons choisi la région
    **North Central US**

![](./media/image26.png)

9.  L'approvisionnement des ressources prend environ 5 à 10 min. Cliquez
    sur **Yes** si vous y êtes invité.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image27.png)

10. Attendez que le modèle provisionne toutes les ressources avec
    succès.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image28.png)

11. Exécutez la commande suivante pour définir le groupe de ressources

\`\`azd env set AZURE_RESOURCE_GROUP {your resource group
name}\`\`![](./media/image29.png)

12. Exécutez la commande ci-dessous pour déployer l'application sur
    Azure.

\`\`azd deploy\`\`

![](./media/image30.png)

13. Attendez la fin du déploiement. Le déploiement prend \<5

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image31.png)

14. Cliquez sur le lien du point de terminaison (Endpoint) de
    l'application web déployée.

![](./media/image32.png)

15. Cliquez sur **Open**. Il ouvre un nouvel onglet avec l'application

![](./media/image33.png)

16. L'application s'ouvre.

![Une capture d'écran d'un chat Description générée
automatiquement](./media/image34.png)

### Tâche 6 : Utiliser l'application de chat pour obtenir des réponses à partir de fichiers

1.  Dans le **RAG on database |** Page de l'application Web
    **OpenAI+PoastgreSQL**, **Cliquez sur Best shoe for hiking?** bouton
    et observez la sortie

![](./media/image35.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image36.png)

2.  Cliquez sur le **clear chat.**

![](./media/image37.png)

3.  Dans le **RAG on database |**Page de l'application Web
    **OpenAI+PoastgreSQL**, cliquez sur le bouton **Climbing gear
    cheaper than \\30** et observez la sortie

![](./media/image38.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image39.png)

4.  Cliquez sur le **clear chat.**

### Tâche 7 : Vérifier les ressources déployées dans le portail Azure

1.  Sur la page d'accueil du portail Azure, cliquez sur **Resource
    Groups**.

![](./media/image40.png)

2.  Cliquez sur le nom de votre groupe de ressources (resource group
    name)

![](./media/image41.png)

3.  Assurez-vous que la ressource ci-dessous a été déployée avec succès

    - Application de conteneur

    - Informations sur les applications

    - Environnement des applications de conteneur

    - Espace de travail Log Analytics

    - Azure OpenAI

    - Serveur flexible Azure Database pour PostgreSQL

    - Registre de conteneurs

![](./media/image42.png)

4.  Cliquez sur le nom de la ressource (resource name) **Azure OpenAI**.

![](./media/image43.png)

5.  Dans **Overview** dans le menu de navigation de gauche, cliquez sur
    **Go to Azure AI Foundry portal** et sélectionnez pour ouvrir un
    nouvel onglet.

![](./media/image44.png)

6.  Cliquez sur **Shared resources -\>** **Deployments** dans le menu de
    navigation de gauche et assurez-vous que
    **gpt-35-turbo**, **text-embedding-ada-002** doit être déployé avec
    succès

![](./media/image45.png)

### Tâche 8 : Nettoyer toutes les ressources

Pour nettoyer toutes les ressources créées par cet exemple :

1.  Revenez à **Azure portal -\> Resource group- \> Resource group
    name.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image46.png)

2.  Sélectionnez toutes les ressources, puis cliquez sur Supprimer comme
    indiqué dans l'image ci-dessous. (**NE SUPPRIMEZ PAS les groupe de
    ressources**)

![](./media/image47.png)

3.  Tapez ''supprimer'' dans la zone de texte, puis cliquez sur
    **Delete**.

![](./media/image48.png)

4.  Confirmez la suppression en cliquant sur **Delete**.

![](./media/image49.png)

5.  Revenez à l'onglet du portail Github et actualisez la page.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image50.png)

1.  Cliquez sur Code, sélectionnez la branche créée pour ce labo et
    cliquez sur **Delete**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image51.png)

2.  Confirmez la suppression de la branche en cliquant sur le bouton
    **Delete**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image52.png)
