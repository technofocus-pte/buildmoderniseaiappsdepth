# Cas d'usage 09 - Création d'une expérience de bot de conversation à l'aide d'Azure Cosmos DB pour MongoDB et Azure OpenAI Service

**Objectif :**

Ce cas d'usage créera une solution intelligente qui combine Azure Cosmos
DB basé sur vCore pour MongoDB, la recherche vectorielle et la
récupération de documents avec les services Azure OpenAI pour créer une
expérience de chatbot.

![Schéma d'une application logicielle Le contenu généré par l'IA peut
être incorrect.](./media/image1.jpeg)

**Principales technologies utilisées** : Azure OpenAI Service, Azure
Cosmos DB, modèle ChatGPT

**Durée estimée** -- 60 minutes

**Type de Lab** -- Dirigé par un instructeur

**Important :** Si l'une des commandes n'est pas **pasted** dans le
**PowerShell**, veuillez ouvrir un bloc-notes, maintenir le curseur dans
un espace vide du bloc-notes, puis cliquer sur le bouton T de la
commande à coller. Le contenu sera copié dans le bloc-notes, puis vous
pourrez le copier et le coller à partir du bloc-notes sur le PowerShell.

## Exercice 0 : Comprendre la machine virtuelle et les informations d'identification

Dans cette tâche, nous identifierons et comprendrons les informations
d'identification que nous utiliserons tout au long du laboratoire.

1.  L'onglet **Instructions** contient le guide de laboratoire avec les
    instructions à suivre tout au long du laboratoire.

2.  L'onglet **Resources** contient les informations d'identification
    nécessaires à l'exécution du laboratoire.

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
être incorrect.](./media/image2.png)

3.  L'onglet **Help** contient les informations d'assistance. La valeur
    **ID** ici est l'ID de **Lab instance** qui sera utilisé lors de
    l'exécution du labo.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image3.png)

## Exercice 1 : Approvisionner des ressources Azure

### Tâche 1 : Créer des ressources Azure à l'aide d'un script

1.  Connectez-vous au portail Azure à l'adresse
    +++\*\*https://portal.azure.com et connectez-vous à l'aide de vos
    identifiants de connexion Azure à partir de l'onglet **Resources**.

2.  Dans le portail Azure, sélectionnez votre abonnement. Dans le volet
    gauche, sélectionnez Fournisseurs de ressources sous Paramètres,
    sélectionnez +++**Microsoft.Alertsmanagement**+++ et cliquez sur
    **Register**.

![Une capture d'écran d'un écran d'ordinateur Le contenu généré par l'IA
peut être incorrect.](./media/image4.jpeg)

3.  À partir de la machine virtuelle, recherchez +++**power shell**+++,
    faites un clic droit sur **Windows PowerShell** et sélectionnez
    **Run as administrator**. Cliquez sur **Yes** dans la boîte de
    dialogue de confirmation.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image5.jpeg)

![Une capture d'écran d'une erreur informatique Le contenu généré par
l'IA peut être incorrect.](./media/image6.jpeg)

4.  Exécutez la commande ci-dessous pour installer Az dans PowerShell.

+++**Install-Module Az**+++

Sélectionnez **A** (Oui à tous) lorsque vous y êtes invité.

**Remarque :** Cela prendra jusqu'à 5 minutes.

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image7.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image8.jpeg)

5.  Une fois cela fait, exécutez la commande ci-dessous pour importer le
    module Az.

+++**Import-Module Az**+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image9.jpeg)

6.  Exécutez la commande ci-dessous pour utiliser la connexion basée sur
    le navigateur

+++Update-AzConfig -EnableLoginByWam $false+++

![Une capture d'écran d'un écran d'ordinateur Le contenu généré par l'IA
peut être incorrect.](./media/image10.jpeg)

7.  Exécutez la commande ci-dessous et sélectionnez votre identifiant
    Azure si vous y êtes invité pour vous connecter à Azure.

+++Connect-AzAccount+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image11.jpeg)

8.  Exécutez les commandes ci-dessous pour accéder au dossier
    **LabFiles**.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![Un panneau rectangulaire bleu avec du texte blanc Le contenu généré
par l'IA peut être incorrect.](./media/image12.jpeg)

9.  Exécutez la commande ci-dessous pour installer **Microsoft Bicep** à
    l'aide **de winget**.

+++winget install -e --id Microsoft.Bicep+++

Tapez **Y** si vous y êtes invité.

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image13.jpeg)

10. **Close** le PowerShell et **open.**

11. Exécutez la commande ci-dessous et sélectionnez votre identifiant
    Azure si vous y êtes invité pour vous connecter à Azure.

+++Connect-AzAccount+++

12. Exécutez les commandes ci-dessous pour accéder au dossier
    **LabFiles**.

+++cd\\++

+++ cd LabFiles\\Build a Chat bot'\Labs\deploy +++

![Un panneau rectangulaire bleu avec du texte blanc Le contenu généré
par l'IA peut être incorrect.](./media/image12.jpeg)

13. Exécutez la commande ci-dessous pour définir l'ID d'abonnement.

+++Set-AzContext -SubscriptionId @lab.CloudSubscription.Id+++

![Capture d'écran d'ordinateur d'un écran bleu Le contenu généré par
l'IA peut être incorrect.](./media/image14.jpeg)

14. Ouvrez le fichier **azuredeploy.bicep** dans le chemin
    **C :\LabFiles\Build a Chat bot\Labs\deploy**, et remplacez les
    lettres **dgxxxxxxx** à la ligne 35 par
    <+++dg@lab.LabInstance.Id>+++. À la ligne **74**, mettez à jour la
    version sous la forme +++**0125**+++.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image15.png)

![](./media/image16.png)

15. Exécutez la commande ci-dessous pour déployer les ressources telles
    que l'espace de travail Azure Cosmos DB, Azure OpenAI dans Azure.

New-AzResourceGroupDeployment -ResourceGroupName
@lab.CloudResourceGroup(ResourceGroup1).Name -TemplateFile
.\azuredeploy.bicep -TemplateParameterFile .\azuredeploy.parameters.json
-c \`\`\`

\>\[! Remarque\] \*\*Remarque :\*\* Le déploiement prendra environ 10 à
15 minutes.

S'il y a un problème avec le déploiement et qu'il échoue, essayez de
mettre à jour le nom à l'étape 14 pour un autre, puis réessayez.

\>\[! Remarque\] \*\*Remarque :\*\* Tapez Y lorsque vous y êtes invité.

![Capture d'écran d'ordinateur d'un écran bleu Le contenu généré par
l'IA peut être incorrect.](./media/image17.jpeg)

![](./media/image18.jpeg)

\>\[! Remarque\] \*\*Remarque :\*\* S'il n'y a pas de mise à jour dans
PowerShell après 15 à 20 minutes, vérifiez sous **Resource Group** -\>
Déploiements dans le portail Azure ou appuyez sur \*\*Enter\*\* dans la
fenêtre \*\*PowerShell\*\*.

### Tâche 2 : Vérifier les ressources créées dans Azure

1.  Connectez-vous au portail **Azure** à l'adresse
    +++<https://portal.azure.com/+++> à l'aide de vos **identifiants de
    connexion Azure (Azure login credentials)**. Sélectionnez **Resource
    groups**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image19.jpeg)

2.  Dans la liste Groupes de ressources, sélectionnez **assigned
    Resource Group**.

![Une capture d'écran d'une page web Le contenu généré par l'IA peut
être incorrect.](./media/image20.png)

3.  Notez qu'un ensemble de ressources, y compris **Azure OpenAI
    resource, App Service, Azure Cosmos DB for MongoDB**, est créé.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image21.png)

4.  Cliquez sur la ressource **Azure OpenAI**.

![](./media/image22.png)

5.  Sélectionnez **Keys and Endpoint** sous **Resource Management**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image23.jpeg)

6.  Copiez et enregistrez **Key 1** and **Endpoint** dans un bloc-notes
    pour référence ultérieure.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image24.jpeg)

7.  De retour dans la page du groupe de ressources, sélectionnez la
    ressource **Azure Cosmos DB for Mongo DB**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image25.png)

8.  Cliquez sur **Connection strings** sous **Settings**. Copiez la
    valeur de Self (toujours ce cluster.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image26.jpeg)

9.  Copiez la chaîne de connexion et collez-la dans un bloc-notes.
    Remplacez le \< **password** \> par +++**myMongoDB98**+++ dans la
    chaîne de connexion copiée et enregistrez-le dans le bloc-notes.

## Exercice 2 : Explorer et utiliser des modèles Azure OpenAI à partir du code

### Tâche 1: Configuration de l'environnement

1.  Dans la barre de recherche des fenêtres VM du labo, recherchez
    +++Visual studio code+++ et ouvrez **Visual Studio Code**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image27.jpeg)

2.  Cliquez sur **Open Folder**. (S'il n'apparaît pas, sélectionnez
    **File -\> Open Folder)**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image28.jpeg)

3.  Accédez à **C :\Labfiles**, cliquez sur **Build a Chat bot** et
    sélectionnez **Sélect Folder.**

![Une capture d'écran d'un bot de chat Le contenu généré par l'IA peut
être incorrect.](./media/image29.jpeg)

4.  Cliquez sur **Yes,** **I trust the Authors** dans la fenêtre
    contextuelle.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image30.jpeg)

5.  À partir de Visual Studio Code, ouvrez
    **lab_0_explore_and_use_models.ipynb** à partir du dossier **Labs**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image31.jpeg)

6.  Cliquez sur **Select Kernel.**

7.  Sélectionnez **Install** dans la fenêtre contextuelle **Do you want
    to install the recommended extensions for Python**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image32.jpeg)

8.  Cliquez sur **Allow access** si vous y êtes invité.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image33.jpeg)

9.  Cliquez sur **Select Kernel**. Sélectionnez **Python Environments**,
    puis **Python 3.12.3** ou version ultérieure qui est répertorié
    comme option **Suggested** ou **Recommanded**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image34.jpeg)

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image35.jpeg)

10. Ouvrez le **fichier** .env

11. Remplacez **DB_CONNECTION STRING,** **AOAI_KEY** et
    **AOAI_Endpoint** que nous avons enregistrés dans le **notepad**
    plus tôt dans la tâche 2 de l'exercice 1.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image36.jpeg)

Maintenant, les variables d'environnement sont définies pour pointer
vers les ressources Azure que nous avons déjà créées.

### Tâche 2 : Exécuter le code

1.  De retour dans le **fichier Lab 0 ipynb**, **execute** la **first
    cell** en cliquant sur le bouton Lecture, pour installer la dernière
    bibliothèque cliente OpenAI.

![Un écran noir avec un fond noir Le contenu généré par l'IA peut être
incorrect.](./media/image37.jpeg)

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image38.jpeg)

2.  **Exécutez** la cellule suivante pour installer le **Python-dotenv**

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image39.jpeg)

3.  Appuyez sur **Ctrl+Maj+P**, tapez +++Reload Window+++ et
    sélectionnez l'option Developer:Reload Window qui est répertoriée.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image40.jpeg)

4.  Exécutez à nouveau à partir de la **first cell**.

5.  **Exécutez** la cellule suivante pour importer la bibliothèque
    OpenAI requise, os pour accéder aux variables d'environnement et
    dotenv pour charger les variables d'environnement à partir du
    fichier .env.

![Capture d'écran d'un programme informatique Le contenu généré par l'IA
peut être incorrect.](./media/image41.jpeg)

6.  **Exécutez** la cellule suivante pour créer le **Azure OpenAI
    client** afin d'appeler l'API de complétion de conversation Azure
    OpenAI :

![Un écran d'ordinateur avec du texte Le contenu généré par l'IA peut
être incorrect.](./media/image42.jpeg)

7.  **Exécutez** la cellule suivante pour appeler la méthode
    **.chat.completions.create()** sur le client afin d'effectuer un
    **chat completion**. Vous devriez recevoir une réponse par chat.

![Un écran d'ordinateur avec du texte dessus Le contenu généré par l'IA
peut être incorrect.](./media/image43.jpeg)

## Exercice 3 : Première application d'API Cosmos DB pour MongoDB

Cet exercice explique comment créer votre premier projet Cosmos DB. Nous
allons utiliser un notebook pour illustrer les opérations de base de
CRUD.

1.  Ouvrez le fichier **lab_1_first_application.ipynb** à partir du
    dossier **Labs**.

2.  Cliquez sur **Select kernel** et choisissez la version de **Python
    version**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image44.jpeg)

3.  **Exécutez** la première cellule pour faire installer **pymongo**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image45.jpeg)

4.  **Exécutez** la cellule suivante pour effectuer les **import**
    requis

![Capture d'écran d'un programme informatique Le contenu généré par l'IA
peut être incorrect.](./media/image46.jpeg)

5.  Exécutez la cellule suivante pour **Create a database.**

\[ ! Remarque\] **Remarque :** Cela utilisera la chaîne de connexion que
nous avons mise à jour dans le fichier .env

![Capture d'écran d'un programme informatique Le contenu généré par l'IA
peut être incorrect.](./media/image47.jpeg)

6.  **Exécutez** la cellule suivante pour créer une **collection**.

![Un écran noir avec du texte blanc Le contenu généré par l'IA peut être
incorrect.](./media/image48.jpeg)

7.  **Exécutez** la cellule suivante pour créer un **document**. L'une
    des méthodes de création d'un document consiste à utiliser la
    méthode insert_one. Cette méthode prend un seul document et l'insère
    dans la base de données.

![Capture d'écran d'un programme informatique Le contenu généré par l'IA
peut être incorrect.](./media/image49.jpeg)

8.  **Exécutez** la cellule suivante pour **retrieve a single document**
    de la base de données. La méthode **find_one** est utilisée ici à
    cet effet.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image50.jpeg)

9.  **Exécutez** la cellule suivante dans laquelle est utilisée la
    méthode **find_one_and_update** pour mettre à jour un seul document
    dans la base de données.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image51.jpeg)

10. **Exécutez** la cellule suivante dans laquelle est utilisée la
    méthode **delete_one** pour supprimer un seul document de la base de
    données.

![](./media/image52.jpeg)

11. La méthode **find** est utilisée pour rechercher plusieurs documents
    dans la base de données. **Exécutez** les **3 cellules suivantes**
    une par une pour le voir en action.

![Une capture d'écran d'ordinateur d'un programme Le contenu généré par
l'IA peut être incorrect.](./media/image53.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image54.jpeg)

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image55.jpeg)

![Une capture d'écran d'ordinateur d'un programme Le contenu généré par
l'IA peut être incorrect.](./media/image56.jpeg)

12. La cellule suivante **supprimera (delete)** la base de données et la
    collection créées dans cet exercice. Pour ce faire, utilisez la
    méthode **drop_database** sur l'objet de base de données

![Un écran d'ordinateur avec du texte Le contenu généré par l'IA peut
être incorrect.](./media/image57.jpeg)

## Exercice 4 : Charger des données dans Cosmos DB à l'aide de l'API MongoDB

L'exercice précédent a montré comment ajouter des données à une
collection individuellement. Cet exercice va montrer comment charger des
données à l'aide d'opérations en bloc dans plusieurs collections. Ces
données seront utilisées dans les ateliers suivants pour expliquer plus
en détail les fonctionnalités de l'API Azure Cosmos DB pour MongoDB sur
l'IA.

Ce bloc-notes montre comment charger des données dans Cosmos DB à partir
de fichiers JSON Cosmic Works dans la base de données à l'aide de l'API
MongoDB.

1.  Ouvrez le fichier **lab_2_load_data.ipynb** à partir du dossier
    **Labs**. Cliquez sur **Select Kernel** et sélectionnez la version
    de **Python**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image58.jpeg)

2.  Exécutez la première cellule pour installer les **requests**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image59.jpeg)

3.  **Exécutez** la cellule suivante pour effectuer les **importations**
    requises.

![Un écran d'ordinateur avec du texte vert Le contenu généré par l'IA
peut être incorrect.](./media/image60.jpeg)

4.  **Exécutez** la cellule suivante qui établit une **connexion
    (connection)** avec la **base de données (database)**.

![Un écran d'ordinateur avec du texte Le contenu généré par l'IA peut
être incorrect.](./media/image61.jpeg)

![Une capture d'écran d'ordinateur de texte Le contenu généré par l'IA
peut être incorrect.](./media/image62.jpeg)

5.  **Exécutez** la cellule suivante pour **charger (load)** les
    **produits (products)**.

![Une capture d'écran d'un écran d'ordinateur Le contenu généré par l'IA
peut être incorrect.](./media/image63.jpeg)

6.  **Exécutez** les cellules suivantes pour **charger (load)** les
    **données brutes** **des clients** et **des ventes (sales raw
    data)**. Dans ce référentiel, les données clients et ventes sont
    stockées dans le même fichier. Le champ type permet de différencier
    les deux types de documents.

![Capture d'écran d'un code informatique Le contenu généré par l'IA peut
être incorrect.](./media/image64.jpeg)

![](./media/image65.jpeg)

![Capture d'écran d'un code informatique Le contenu généré par l'IA peut
être incorrect.](./media/image66.jpeg)

7.  **Exécutez** la cellule suivante à **clean up**.

![Capture d'écran d'un programme informatique Le contenu généré par l'IA
peut être incorrect.](./media/image67.jpeg)

## Exercice 5 : Recherche vectorielle à l'aide d'Azure Cosmos DB basé sur vCore pour MongoDB

1.  Ouvrez le fichier **lab_3_mongodb_vector_search.ipynb** à partir du
    dossier **Labs.**

2.  Cliquez sur **Sélect Kernel** et sélectionnez **Python vesrion**.

![](./media/image68.jpeg)

3.  **Exécutez** la première cellule pour installer **tenacity**.

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image69.jpeg)

4.  **Exécutez** la cellule suivante pour effectuer les **importations
    (imports)** requises.

![Un écran d'ordinateur avec du texte Le contenu généré par l'IA peut
être incorrect.](./media/image70.jpeg)

5.  **Exécutez** la cellule suivante pour **charger (load)** les
    **settings** à partir du fichier .env.

![Un écran d'ordinateur avec du texte Le contenu généré par l'IA peut
être incorrect.](./media/image71.jpeg)

6.  Exécutez la cellule suivante pour établir la **connectivity** à la
    **base de données (database)**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image72.jpeg)

7.  **Exécutez** la cellule suivante pour établir la **connectivité
    Azure OpenAI (Azure OpenAI connectivity)**.

![Capture d'écran d'un code informatique Le contenu généré par l'IA peut
être incorrect.](./media/image73.jpeg)

8.  Le processus de création d'un champ d'intégration vectorielle sur
    chaque document ne doit être effectué qu'une seule fois. Toutefois,
    si un document change, le champ d'incorporation vectorielle devra
    être mis à jour avec un vecteur mis à jour. Cela se fait dans les
    deux cellules suivantes. **Exécutez** les deux cellules suivantes et
    observez les **embeddings** obtenus en sortie dans la seconde.

![Capture d'écran d'un programme informatique Le contenu généré par l'IA
peut être incorrect.](./media/image74.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image75.jpeg)

9.  **Exécutez** la cellule suivante à **vectoriser et mettez à jour
    tous les documents de la base de données Cosmic Works.**

![Capture d'écran d'ordinateur d'un code de programme Le contenu généré
par l'IA peut être incorrect.](./media/image76.jpeg)

10. **Exécutez** les **3** cellules suivantes pour ajouter des **champs
    vectoriels** (vector fields) aux **products, aux documents clients
    et commerciaux.**

**Remarque :** La première cellule prendra environ 5 minutes, la
deuxième environ 3 minutes et la troisième environ 20 minutes pour
terminer l'exécution.

! \[\](./media/image77.jpeg)

11. **Exécutez** la cellule suivante pour créer l'**index vectoriel des
    produits (products vector index)**.

![Une capture d'écran d'un écran d'ordinateur Le contenu généré par l'IA
peut être incorrect.](./media/image77.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image78.jpeg)

12. Maintenant que chaque document est associé à son incorporation de
    vecteurs et que les index vectoriels ont été créés sur chaque
    collection, nous pouvons désormais utiliser les fonctionnalités de
    recherche vectorielle d'Azure Cosmos DB pour MongoDB basé sur vCore.
    **Exécutez** les **3** cellules suivantes.

![](./media/image79.jpeg)

![](./media/image80.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image81.jpeg)

13. **Exécutez** les cellules suivantes pour observer l'utilisation des
    résultats de **recherche vectorielle (vector search results)** dans
    un modèle RAG avec Chat GPT-3.5

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image82.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image83.jpeg)

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image84.jpeg)

14. Observez le résultat des cellules suivantes.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image85.jpeg)

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image86.jpeg)

## Exercice 6 : Supprimer les ressources déployées

1.  Dans le portail Azure
    (+++[*https://portal.azure.com+++*](https://portal.azure.com+++/)),
    sélectionnez le groupe de ressources attribué.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image87.png)

2.  Sélectionnez toutes les ressources qui se trouvent en dessous,
    cliquez sur les **trois points dans** le menu et sélectionnez
    **Delete** pour supprimer toutes les ressources.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image88.png)

3.  Tapez +++delete+++ dans la zone de texte et cliquez sur le bouton
    Supprimer.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image89.png)

4.  Une fois les ressources supprimées, à partir de la page d'accueil du
    portail Azure, recherchez +++**Azure AI Services**+++ et
    sélectionnez-le.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image90.jpeg)

5.  Sélectionnez **Azure OpenAI** dans le volet gauche, puis Gérer
    **Manage deleted resources**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image91.jpeg)

6.  Sélectionnez la ressource qui y est répertoriée, puis cliquez sur
    **Purge**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image92.jpeg)

7.  Cliquez sur **Yes**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image93.jpeg)

**Résumé :**

Vous avez créé avec succès une solution avec Azure Cosmos DB pour la
recherche vectorielle MongoDB et l'extraction de documents avec les
services Azure OpenAI.
