# Cas d'usage 11 - Création d'un Copilot à l'aide d'Azure OpenAI, Azure Cosmos DB pour NoSQL

Dans ce cas d'usage, vous allez connecter une application web Blazor à
Azure Cosmos DB pour NoSQL et Azure OpenAI à l'aide de kits de
développement logiciel .NET. Votre code gère et interroge les éléments
d'une API pour conteneur NoSQL. Votre code envoie également des invites
à Azure OpenAI et analyse les réponses.

**Durée de l'atelier :** 45 minutes

**Type de Lab** : Dirigé par un instructeur

**Objectif**

- Pour configurer l'environnement de développement pour Blazor,
  PostgreSQL et OpenAI.

- Créer un projet Blazor et concevoir une interface de chat réactive.

- Pour configurer la base de données PostgreSQL sur Azure et la
  connecter à l'application Blazor.

- Intégrer Azure OpenAI pour des fonctionnalités de chat améliorées.

- Déployer l'application Blazor et la base de données PostgreSQL sur
  Azure.

- Tester l'application afin d'assurer une interaction transparente entre
  les composants.

- Pour surveiller et résoudre les problèmes liés à l'application
  déployée sur Azure.

**Principales technologies utilisées :** Azure Cosmos DB pour NoSQL,
Azure OpenAI

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

    - **Resource Group** : groupe de **Resources** qui vous est
      attribué.

\[ ! Alerte\] **Important :** Assurez-vous de créer toutes vos
ressources sous ce groupe de ressources

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image1.png)

3.  L'onglet **Help** contient les informations d'assistance. La valeur
    **ID** ici est l'ID de l'**instance de lab (Lab instance)** qui sera
    utilisé lors de l'exécution du labo.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image2.png)

## Exercice 1 : Déployer l'infrastructure et terminer la configuration initiale

Pour mener à bien ce projet, vous avez besoin d'un compte Azure Cosmos
DB pour NoSQL et d'un compte Azure OpenAI. Pour simplifier ce processus,
déployez un modèle Bicep sur Azure avec ces deux comptes.

### Tâche 1 : Déployer l'infrastructure à partir d'un modèle

1.  Ouvrez le fichier à partir du chemin d'accès **C:\Labfiles\Build and
    Test a custom chat application Using Azure Cosmos DB and
    AzureOpenAI** et mettez à jour la version d'Azure OpenAI à la ligne
    96 pour qu'elle soit +++0125+++. **Save** le fichier.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image3.png)

2.  Ouvrez un nouveau navigateur et entrez l'URL suivante dans la barre
    d'adresse : +++<https://portal.azure.com/+++> pour ouvrir le portail
    Azure.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image4.jpeg)

3.  Dans le portail Azure, cliquez sur le bouton **\[\>\_\] (Cloud
    Shell)** en haut de la page à droite de la zone de recherche. Un
    volet Cloud Shell s'ouvre en bas du portail. La première fois que
    vous ouvrez Cloud Shell, vous pouvez être invité à choisir le type
    de shell que vous souhaitez utiliser (**Bash** ou **PowerShell**).
    Sélectionnez **Bash**. Si vous ne voyez pas cette option, ignorez
    cette étape.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image5.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image6.jpeg)

4.  Dans la boîte de dialogue **Getting Started**, sélectionnez **Mount
    storage account**, sélectionnez votre **subscription,** puis cliquez
    sur **Apply**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image7.jpeg)

5.  Dans la boîte de dialogue **Mount storage account,** sélectionnez
    **we will create a storage account for you** et cliquez sur
    **Next**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image8.jpeg)

![Gros plan d'un écran d'ordinateur Le contenu généré par l'IA peut être
incorrect.](./media/image9.jpeg)

6.  Assurez-vous que le type de shell indiqué en haut à gauche du volet
    Cloud Shell est bash sur **Bash**. S'il s'agit de **PowerShell**,
    basculez vers **Bash** à l'aide du menu déroulant.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image10.jpeg)

7.  Une fois le terminal démarré, cliquez sur **Manage files \>
    Upload**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image11.jpeg)

8.  Sélectionnez **azuredeploy. JSON** à partir du chemin d'accès
    **C :\Labfiles\Générer et tester une application de conversation
    personnalisée à l'aide d'Azure Cosmos DB et d'AzureOpenAI**, puis
    sélectionnez **Open**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image12.jpeg)

Vous devriez recevoir un message de réussite pour le téléchargement du
fichier.

![Un fond blanc avec du texte noir Le contenu généré par l'IA peut être
incorrect.](./media/image13.jpeg)

9.  Créez une variable shell nommée **resourceGroupName** avec le nom du
    groupe de ressources Azure que vous créez (mslearn-cosmos-openai).

+++resourceGroupName="ResourceGroup1"+++(Obtenir le nom du groupe de
ressources à partir de l'onglet Ressources)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image14.png)

10. Déployez le fichier de modèle **azuredeploy.json** dans le groupe de
    ressources à l'aide de az group deployment create. Ensuite, exécutez
    la commande suivante.

+++az groupe de déploiement create --resource-group $resourceGroupName
--name zero-touch-deployment --template-file azuredeploy.json+++

**Remarque :** Ce déploiement peut prendre environ 5 à 10 minutes.

![Une capture d'écran d'un écran d'ordinateur Le contenu généré par l'IA
peut être incorrect.](./media/image15.jpeg)

![Une capture d'écran d'un écran d'ordinateur Le contenu généré par l'IA
peut être incorrect.](./media/image16.jpeg)

### Tâche 2 : Obtenir les informations d'identification du compte Azure Cosmos DB pour NoSQL et Azure OpenAI

Le déploiement ci-dessus a déployé Azure Cosmos DB pour les comptes
NoSQL et Azure OpenAI, puis stocké leurs informations d'identification
dans la configuration de l'application web Azure App Service. À présent,
vous avez le choix d'utiliser le portail Azure ou Azure CLI pour
récupérer les informations d'identification de chaque service.

1.  Dans la page d'accueil du portail Azure, cliquez sur **Resource
    groups.**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image17.jpeg)

2.  Sélectionnez votre groupe de ressources.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image18.png)

3.  Sur la page **Resource Groups**, développez le panneau
    **Essentiels** et observez l’en-tête **Deployments**. L'état du
    déploiement doit être **Réussi (Succeeded)** à ce stade.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image19.png)

4.  À présent, sélectionnez le compte **Azure Cosmos DB** pour accéder à
    la page de la ressource.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image20.png)

5.  Sélectionnez l'option **Keys** dans la section **Settings** du menu
    de navigation des ressources. Enregistrez la valeur des champs
    **URI** et **PRIMARY KEY**. Vous utiliserez ces valeurs
    ultérieurement.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image21.jpeg)

6.  Revenez à la page **Resource Groups**. Sélectionnez le compte
    **Azure OpenAI**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image22.png)

7.  Dans votre fenêtre **Azure Open AI**, accédez à la section
    **Resource Management**, puis cliquez sur **Keys and Endpoints**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image23.jpeg)

8.  Dans la page **Keys and Endpoints**, copiez **KEY1** (*vous pouvez
    utiliser KEY1 ou KEY2)* et **Endpoint**, puis **Save** le bloc-notes
    pour utiliser les informations dans les tâches à venir.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image24.jpeg)

### Tâche 3 : Exécuter le Docker

1.  Dans votre champ de recherche Windows, tapez +++Docker+++ , puis
    cliquez sur **Docker Desktop**.

![Une capture d'écran d'un ordinateur de bureau Le contenu généré par
l'IA peut être incorrect.](./media/image25.jpeg)

2.  Exécutez le Docker Desktop.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image26.jpeg)

## Exercice 2 - Configurer et générer l'application de démarrage

1.  Dans la barre de recherche de la machine virtuelle, recherchez
    +++Visual Studio+++ et sélectionnez **Visual Studio Code**.

2.  Cliquez sur **File** -\> **Open Folder**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image27.jpeg)

3.  Sélectionnez **cosmosdb-chatgpt** dans **C :\LabFiles** et cliquez
    sur **Select Folder**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image28.jpeg)

4.  Cliquez sur l'option **Yes, I trust the authors** dans la boîte de
    dialogue **Do you trust the authors**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image29.jpeg)

5.  Dans l'éditeur **Visual Studio Code**, cliquez sur **Terminal**,
    ouvrez un **New Terminal**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image30.jpeg)

6.  Dans une application .NET, il est courant d'utiliser les
    fournisseurs de configuration pour injecter de nouveaux paramètres
    dans votre application. Pour cette application, utilisez
    **appsettings. Development.json** fichier pour fournir les valeurs
    les plus récentes pour le point de terminaison et la clé Azure
    OpenAI.

7.  Ouvrez les paramètres de l'**application. Development.JSON**
    fichier. Remplacez les espaces réservés pour les valeurs d'URI et de
    clé des ressources **Azure Cosmos DB** et **Azure OpenAI** dans le
    fichier par les valeurs que nous avons enregistrées précédemment.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image31.jpeg)

8.  **Créez** le projet .NET en exécutant la commande ci-dessous.

+++dotnet build+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image32.jpeg)

## Exercice 3 : Comprendre le code

### Tâche 1 : Ajouter les membres requis et une instance client

1.  Ouvrez le fichier **Services/OpenAiService.cs**. Ce fichier
    implémente les variables de classe requises pour utiliser le client
    Azure OpenAI. Il implémente quelques invites statiques et crée une
    nouvelle instance de la classe OpenAIClient.

2.  Ce bloc de code crée une nouvelle variable de string nommé
    \_systemPromptText avec un bloc de texte statique à envoyer à
    l'assistant IA avant chaque invite.

> private readonly string \_systemPrompt = @"
>
> Vous êtes un assistant IA qui aide les gens à trouver des
> informations.
>
> Fournissez des réponses concises, polies et professionnelles. " +
> Environment.NewLine;

3.  Ce bloc de code crée une autre nouvelle variable de string nommé
    \_summarizePrompt avec un bloc de texte statique à envoyer à
    l'assistant IA avec des instructions sur la façon de résumer une
    conversation.

> private readonly string \_summarizePrompt = @"
>
> Résumez cette invite en un ou deux mots à utiliser comme étiquette
> dans un bouton sur une page Web.
>
> N'utilisez pas de ponctuation. » + Environment.NewLine ;

4.  Ce bloc de code crée une nouvelle instance de la classe OpenAIClient
    à l'aide du point de terminaison pour générer un URI et de la clé
    pour générer un AzureKeyCredential.

> Uri uri = new(endpoint);
>
> AzureKeyCredential credential = new(key);
>
> \_client = new(
>
> endpoint: uri,
>
> keyCredential: credential
>
> );

**Tâche 2 : Poser une question au modèle d'IA**

Tout d'abord, implémentez une conversation question-réponse en envoyant
une invite système, une question et un ID de session afin que le modèle
d'IA puisse fournir une réponse dans le contexte de la conversation en
cours. Assurez-vous de mesurer le nombre de jetons nécessaires pour
analyser l'invite et renvoyer une réponse (ou une complétion dans ce
contexte).

1.  Ce bloc de code crée une nouvelle variable nommée options de type
    ChatCompletionsOptions. Ajoute les deux variables de message à la
    liste Messages et définit la valeur User sur le paramètre de
    constructeur sessionId.

> ChatCompletionsOptions options = new()
>
> {
>
> DeploymentName = "chatmodel",
>
> Messages = {
>
> new ChatRequestSystemMessage(\_systemPrompt),
>
> new ChatRequestUserMessage(userPrompt)
>
> },
>
> User = sessionId,
>
> MaxTokens = 4000,
>
> Temperature = 0.3f,
>
> NucleusSamplingFactor = 0.5f,
>
> FrequencyPenalty = 0,
>
> PresencePenalty = 0
>
> };

2.  La méthode GetChatCompletionsAsync de la variable cliente Azure
    OpenAI (\_client) est appelée de manière asynchrone. Le résultat est
    stocké dans une variable nommée completions de type ChatCompletions.

> Response\<ChatCompletions\> completionsResponse =
> await_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;

3.  Enfin, le bloc de code ci-dessous renvoie un tuple à la suite de la
    méthode GetChatCompletionAsync avec le contenu de la saisie
    semi-automatique sous forme de chaîne, le nombre de jetons associés
    à l'invite et le nombre de jetons pour la réponse.

> return (
>
> completionText: completions.Choices\[0\].Message.Content,
>
> completionTokens: completions.Usage.CompletionTokens
>
> );

**Tâche 3 : Demander au modèle d'IA de résumer une conversation**

Maintenant, envoyez au modèle d'IA une invite système différente, votre
conversation actuelle et l'ID de session afin que le modèle d'IA puisse
résumer la conversation en quelques mots.

2.  Le code ci-dessous crée une variable ChatCompletionsOptions nommée
    options avec les deux variables de message dans la liste Messages,
    User défini sur le paramètre constructeur sessionId, MaxTokens
    défini sur 200 et les propriétés restantes.

> ChatCompletionsOptions options = new()
>
> {
>
> DeploymentName = "chatmodel",
>
> Messages = {
>
> new ChatRequestSystemMessage(\_systemPrompt),
>
> new ChatRequestUserMessage(conversationText)
>
> },
>
> User = sessionId,
>
> MaxTokens = 200,
>
> Temperature = 0.0f,
>
> NucleusSamplingFactor = 1.0f,
>
> FrequencyPenalty = 0,
>
> PresencePenalty = 0
>
> };

3.  Le code ci-dessous invoque le \_client. GetChatCompletionsAsynchrone
    de manière asynchrone avec le nom du modèle (\_modelName) et la
    variable options en tant que paramètre et stocke le résultat dans
    une variable nommée completions de type ChatCompletions. Il renvoie
    le contenu de la saisie semi-automatique sous forme de chaîne à
    l'aide de la méthode SummarizeAsync.

> Response\<ChatCompletions\> completionsResponse = await
> \_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;
>
> string completionText = completions.Choices\[0\].Message.Content;
>
> return completionText;

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image33.jpeg)

**Tâche 4 : se connecter à Azure Cosmos DB pour NoSQL**

La classe CosmosDbService contient une implémentation stub d'un service
similaire à la classe OpenAiService sur laquelle vous avez travaillé
précédemment dans ce module. En revanche, cette classe utilise le Kit de
développement logiciel (SDK) .NET pour Azure Cosmos DB, qui fonctionne
légèrement différemment.

Cette section décrit l'implémentation des variables de classe et du
client requis pour accéder à Azure Cosmos DB pour NoSQL à l'aide du
client.

1.  Ouvrez le fichier **Services/CosmosDbService.cs**.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image34.jpeg)

2.  Le code ci-dessous crée une variable nommée options de type
    CosmosSerializationOptions et définit la propriété
    PropertyNamingPolicy de la variable sur
    CosmosPropertyNamingPolicy.CamelCase.

> CosmosSerializationOptions options = new()
>
> {
>
> PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
>
> };

**Remarque :** La définition de cette propriété garantit que le JSON
produit par le SDK est à la fois sérialisé et désérialisé en casse
camel, quelle que soit la façon dont sa propriété correspondante est
mise en casse dans la classe .NET.

3.  Le code ci-dessous crée une nouvelle instance de type CosmosClient
    nommé client à l'aide de la classe, du point de terminaison, de la
    clé et des options de sérialisation CosmosClientBuilder que vous
    avez spécifiées précédemment.

> Client CosmosClient = new CosmosClientBuilder(point de terminaison,
> clé)
>
> . WithSerializerOptions(options)
>
> . Build() ;

4.  Le code ci-dessous crée une nouvelle variable nullable de type
    Database nommée database en appelant la méthode GetDatabase de la
    variable cliente.

**Database? database = client?.GetDatabase(databaseName);**

5.  Le code ci-dessous attribue la variable conteneur du constructeur à
    la variable \_container de la classe uniquement si elle n'est pas
    null. S'il est null, levez une ArgumentException.

> \_container = container??
>
> throw new ArgumentException("Unable to connect to existing Azure
> Cosmos DB container or database.");

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image35.jpeg)

**Tâche 5 : implémenter le service Azure Cosmos DB pour NoSQL**

Le service Azure Cosmos DB (CosmosDbService) gère l'interrogation, la
création, la suppression et la mise à jour des sessions et des messages
dans votre application d'assistant IA. Pour gérer toutes ces opérations,
le service doit implémenter plusieurs méthodes pour chaque opération
potentielle à l'aide de diverses fonctionnalités du Kit de développement
logiciel (SDK) .NET.

Il y a plusieurs exigences clés à prendre en compte dans cet exercice :

- Implémenter des opérations pour créer une session ou un message

- Implémenter des requêtes pour récupérer plusieurs sessions ou messages

- Implémenter une opération de mise à jour d'une seule session ou de
  mise à jour par lots de plusieurs messages

- Implémenter une opération d'interrogation et de suppression de
  plusieurs sessions et messages associés

Azure Cosmos DB pour NoSQL stocke les données au format JSON, ce qui
nous permet de stocker de nombreux types de données dans un seul
conteneur. Cette application stocke à la fois une « session » de chat
avec l'assistant IA et les « messages » individuels au sein de chaque
session. Avec l'API pour NoSQL, l'application peut stocker les deux
types de données dans le même conteneur, puis différencier ces types à
l'aide d'un simple champ de type.

1.  Ouvrez le fichier **Services/CosmosDbService.cs**.

2.  Le code ci-dessous crée une nouvelle variable nommée partitionKey de
    type PartitionKey en utilisant la propriété SessionId de la session
    actuelle comme paramètre.

**PartitionKey partitionKey = new(session. SessionId) ;**

3.  Le code ci-dessous appelle la méthode CreateItemAsync du conteneur
    en transmettant le paramètre de session et la variable partitionKey.
    Renvoie la réponse à l'aide de la méthode InsertSessionAsync.

> return await \_container.CreateItemAsync\<Session\>(
>
> item: session,
>
> partitionKey: partitionKey
>
> );

4.  Le code ci-dessous crée une variable PartitionKey à l'aide de
    session. SessionId comme valeur de la clé de partition. Crée une
    variable de message nommée newMessage avec la propriété Timestamp
    mise à jour vers l'horodatage UTC actuel. Appelez le passage
    CreateItemAsync dans le nouveau message et les variables de clé de
    partition. Renvoie la réponse à la suite de InsertMessageAsync.

> PartitionKey partitionKey = new(message.SessionId);
>
> Message newMessage = message with { TimeStamp = DateTime.UtcNow };
>
> return await \_container.CreateItemAsync\<Message\>(
>
> item: newMessage,
>
> partitionKey: partitionKey
>
> );

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image36.jpeg)

**Tâche 6 : Récupérer plusieurs sessions ou messages**

Il existe deux cas d'utilisation principaux où l'application doit
récupérer plusieurs éléments de notre conteneur. Tout d'abord,
l'application récupère toutes les sessions de l'utilisateur actuel en
filtrant les éléments où type = Session. Deuxièmement, l'application
récupère tous les messages d'une session en exécutant un filtre
similaire où type = Session & sessionId = . Les deux requêtes sont ici
implémentées à l'aide du SDK .NET et d'un itérateur de flux.

1.  Le code ci-dessous crée une nouvelle variable nommée query de type
    QueryDefinition. Il utilise la méthode fluent WithParameter pour
    attribuer le nom de la classe Session comme valeur du paramètre.
    Appelle ensuite la méthode générique GetItemQueryIterator\<\> sur la
    variable \_container en transmettant le type générique Session et la
    variable de requête en tant que paramètre. Stockez le résultat dans
    une variable de type FeedIterator nommée response.

> QueryDefinition query = new QueryDefinition("SELECT DISTINCT \* FROM c
> WHERE c.type = @type")
>
> .WithParameter("@type", nameof(Session));
>
> FeedIterator\<Session\> response =
> \_container.GetItemQueryIterator\<Session\>(query);

2.  Le code ci-dessous dans la boucle while permet d'obtenir de manière
    asynchrone la page suivante de résultats en appelant ReadNextAsync
    sur la variable de réponse, puis d'ajouter ces résultats à la
    variable de liste nommée output. En dehors de la boucle while, la
    variable de sortie est renvoyée avec une liste de sessions à la
    suite de la méthode GetSessionsAsync.

> FeedResponse\<Session\> results = await response.ReadNextAsync();
>
> output.AddRange(results);
>
> return output;

3.  Le code ci-dessous utilise la méthode fluent WithParameter pour
    attribuer le paramètre @sessionId à l'identificateur de session
    passé en tant que paramètre, et le paramètre @type au nom de la
    classe Message.

> QueryDefinition query = new QueryDefinition("SELECT \* FROM c WHERE
> c.sessionId = @sessionId AND c.type = @type")
>
> .WithParameter("@sessionId", sessionId)
>
> .WithParameter("@type", nameof(Message));

4.  Créez un \> de messages FeedIterator\< à l'aide de la variable query
    et de la méthode GetItemQueryIterator\<\>.

FeedIterator response = \_container.GetItemQueryIterator(query);

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image37.jpeg)

## Exercice 4 : Exécuter l'application

Votre application dispose désormais d'une implémentation complète
d'Azure OpenAI et d'Azure Cosmos DB. Vous pouvez tester l'application de
bout en bout en déboguant la solution.

1.  À partir du **Visual Studio Code Terminal**, générez le projet à
    l'aide de la commande ci-dessous.

+++**dotnet build**+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image38.jpeg)

2.  Démarrez l'application avec les rechargements à chaud activés à
    l'aide de dotnet watch.

+++ **dotnet watch run --non-interactive**+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image39.jpeg)

3.  Visual Studio Code lance le navigateur simple intégré à l'outil avec
    l'application web en cours d'exécution. Dans l'application Web,
    créez une nouvelle session de chat en cliquant sur **+ Create New
    Chat** et posez une question à l'assistant IA. Ensuite, fermez
    l'application Web en cours d'exécution.

![Une capture d'écran d'un chat Le contenu généré par l'IA peut être
incorrect.](./media/image40.jpeg)

4.  Collez le texte suivant dans la zone de texte et cliquez sur l'icône
    **Send.**

+++Combien de victoires faut-il pour être promu en Premier League ?+++

![Une capture d'écran d'un chat Le contenu généré par l'IA peut être
incorrect.](./media/image41.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image42.jpeg)

5.  Collez le texte suivant dans la zone de texte et cliquez sur l'icône
    **Send.**

+++What is Azure OpenAI?+++

![Une capture d'écran d'un chat Le contenu généré par l'IA peut être
incorrect.](./media/image43.jpeg)

![Une capture d'écran d'un chat Le contenu généré par l'IA peut être
incorrect.](./media/image44.jpeg)

6.  Fermez le terminal.

## Exercice 5 : Nettoyer le groupe de ressources

1.  Ouvrez un nouveau navigateur et entrez l'URL suivante dans la barre
    d'adresse : +++<https://portal.azure.com/+++> pour ouvrir le portail
    Azure.

2.  Sur la page Groupe de ressources, sélectionnez le **assigned
    Resource group**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image45.png)

3.  Sélectionnez toutes les **resources**, puis sélectionnez **Delete**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image46.png)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image47.png)

4.  Tapez +++**delete**+++ dans la zone de texte et cliquez sur
    **Delete**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image48.png)

![Une capture d'écran d'une erreur informatique Le contenu généré par
l'IA peut être incorrect.](./media/image49.png)

5.  Une notification de réussite sur les ressources supprimées confirme
    la suppression.

6.  Une fois les ressources supprimées, à partir de la page d'accueil du
    portail Azure, recherchez **Azure AI Services** et sélectionnez-le.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image50.jpeg)

7.  Sélectionnez **Azure OpenAI** dans le volet gauche, puis **Manage
    deleted resources**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image51.jpeg)

8.  Sélectionnez la ressource qui y est répertoriée, puis cliquez sur
    **Purge**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image52.jpeg)

9.  Cliquez sur **Yes**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image53.jpeg)

**Résumé**

Cet atelier a fourni un guide complet sur la création, le déploiement et
le test d'une application de chat personnalisée à l'aide de Blazor,
PostgreSQL et Azure OpenAI. Dans cet atelier, vous avez appris à
configurer l'environnement de développement nécessaire, à créer et à
concevoir une interface de conversation basée sur Blazor, à configurer
et à connecter une base de données PostgreSQL sur Azure, à intégrer
Azure OpenAI pour des fonctionnalités améliorées, et enfin à déployer et
tester l'application sur Azure. Cette expérience pratique vous a permis
d'acquérir les compétences nécessaires pour développer et gérer des
applications Web modernes à l'aide de technologies de pointe et de
services infonuagiques
