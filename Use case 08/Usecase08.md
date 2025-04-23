# Cas d'usage 08 - Création et déploiement de l'application de chat Contoso Real Estate pour aider les clients.

**Objectif**

Ce cas d'utilisation présente quelques approches pour créer des
expériences de type ChatGPT sur vos propres données à l'aide du modèle
de génération augmentée de récupération. Il utilise le service Azure
OpenAI pour accéder au modèle ChatGPT (gpt-35-turbo) et Azure AI Search
pour l'indexation et la récupération des données.

![Schéma d'un processus logiciel Description générée
automatiquement](./media/image1.jpeg)

Le cas d'utilisation inclut des exemples de données afin d'être prêt à
être essayé de bout en bout. Dans cet exemple d'application, nous
utilisons une société fictive appelée Contoso Real Estate, et
l'expérience permet à ses clients de poser des questions d'assistance
sur l'utilisation de ses produits. L'exemple de données comprend un
ensemble de documents qui décrivent ses conditions d'utilisation, sa
politique de confidentialité et un guide d'assistance.

L'application est composée de plusieurs composants, notamment :

- **Service de recherche** : le service principal qui fournit les
  capacités de recherche et d'extraction.

- **Service d'indexation** : le service qui indexe les données et crée
  les index de recherche.

- **Application Web** : l'application Web frontale qui fournit
  l'interface utilisateur et orchestre l'interaction entre l'utilisateur
  et les services backend.

![Schéma d'un système logiciel Description générée
automatiquement](./media/image2.jpeg)

- Interfaces de chat et de questions-réponses

- Explore diverses options pour aider les utilisateurs à évaluer la
  fiabilité des réponses avec des citations, le suivi du contenu source,
  etc.

- Montre les approches possibles pour la préparation des données, la
  construction d'invites et l'orchestration de l'interaction entre le
  modèle (ChatGPT) et l'extracteur (Azure AI Search)

- Paramètres directement dans l'UX pour modifier le comportement et
  expérimenter les options

- Suivi et surveillance des performances en option avec Application
  Insights

**Principales technologies utilisées** : Azure OpenAI Service, modèle
ChatGPT (gpt-35-turbo) et Azure AI Search

**Durée estimée --** 40 minutes

## Exercice 1 : Déployer l'application et la tester depuis le navigateur

### Tâche 1: Environnement de développement ouvert

1.  Ouvrez votre navigateur, naviguez jusqu'à la barre d'adresse, tapez
    ou collez l'URL suivante :
    \`\`https://github.com/technofocus-pte/azure-search-openai-javascript\`\`
    et connectez-vous avec votre compte Github.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image3.jpeg)

2.  Cliquez sur **Fork**.

![Une capture d'écran d'une page web Description générée
automatiquement](./media/image4.jpeg)

3.  Entrez le nom du référentiel puis cliquez sur **Create fork**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image5.jpeg)

4.  Click on **Code -\> Codespaces -\> +**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.jpeg)

5.  Attendez que l'environnement soit configuré. Cela prend 5 à 10
    minutes.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image7.jpeg)

### Tâche 2 : Fournir les services requis pour créer et déployer l'application de conversation sur Azure

1.  Exécutez la commande suivante sur le terminal. Copiez le code et
    appuyez sur Entrée.

\`\`azd auth login\`\`![Une capture d'écran d'un ordinateur Description
générée automatiquement](./media/image8.png)

2.  Le navigateur par défaut s'ouvre pour saisir un code. Entrez le code
    copié et cliquez sur **Next**.

![](./media/image9.png)

3.  Connectez-vous à l'aide de vos informations d'identification Azure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image10.png)

![Une capture d'écran d'une erreur informatique Description générée
automatiquement](./media/image11.png)

6.  Revenez à l'onglet Github Codespace. Exécutez la commande ci-dessous
    pour initialiser l'environnement du projet dans le répertoire
    actuel. Entrez le nom de l'environnement comme ''**ragpgpy ''** et
    appuyez sur Entrée.

Note : env name should be unique

\`\` azd env new\`\`![](./media/image12.png)

7.  Exécutez la commande ci-dessous pour provisionner les services sur
    Azure, créez votre conteneur.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image13.png)

8.  Sélectionnez les valeurs ci-dessous.

> \`\`azd provision\`\`

- **Select an Azure Subscription to use** : sélectionnez votre
  abonnement

- **Select an Azure location to use** : **East us2/west us2** (Parfois,
  East US peut ne pas être disponible, choisissez un emplacement dans la
  liste mentionnée ci-dessous.)

- Sélectionnez un groupe de ressources existant : Votre groupe de
  ressources existant (par exemple :**ResourceGroup1 )**

![](./media/image14.png)

9.  Attendez que la ressource soit complètement provisionnée. Ce
    processus prendra 5 à 10 minutes pour créer toutes les ressources
    requises.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image15.png)

### Tâche 3 : Déployer l'application de chat et l'explorer

10. Exécutez la commande ci-dessous pour déployer l'application.

\`\`azd deploy\`\`![](./media/image16.png)

11. Attendez le déploiement . Cela prend \< 5 minutes.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image17.png)

12. Cliquez sur l'URL du point de terminaison générée.

![](./media/image18.png)

13. Cliquez sur **Open**.

![](./media/image19.png)

14. Il ouvre l'application dans un nouvel onglet.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image20.png)

15. Sélectionnez **How to search and book rental?** Conteneur, puis
    cliquez sur le bouton Entrée à côté de la zone de texte.

![](./media/image21.png)

### Tâche 4 : Nettoyer toutes les ressources

1.  Revenez au **Azure portal -\> Resource group- \> Resource group
    name.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image22.png)

2.  Sélectionnez toutes les ressources, puis cliquez sur Supprimer comme
    indiqué dans l'image ci-dessous. (**NE SUPPRIMEZ** **PAS** resource
    group)

![](./media/image23.png)

3.  Tapez \`\`**delete**\`\` dans la zone de texte, puis cliquez sur
    **Delete**.

![](./media/image24.png)

4.  Confirmez la suppression en cliquant sur **Delete**.

![](./media/image25.png)

5.  Revenez à l'onglet du portail Github et actualisez la page.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image26.png)

6.  Cliquez sur Code , sélectionnez la branche créée pour ce labo et
    cliquez sur **Delete**.

![](./media/image27.png)

7.  Confirmez la suppression de la branche en cliquant sur le bouton
    **Delete**.

![](./media/image28.png)

### Résumé:

Ce cas d'utilisation vous a pensé, déployer une application de
conversation pour le modèle de génération augmentée de récupération
s'exécutant sur Azure, utiliser Azure AI Search pour la récupération et
les modèles de langage volumineux (LLM) Azure OpenAI et LangChain pour
alimenter les expériences de type ChatGPT et de questions-réponses
