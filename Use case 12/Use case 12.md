# Cas d'utilisation 12 - Intégrer des fonctionnalités d'IA générative à Azure Database pour PostgreSQL Flexible Server afin d'évaluer les avis sur des listes d'IA données

**Durée du Lab :** 40 minutes

**Type de Lab --** Dirigé par un instructeur

**Introduction**

Dans cet Lab, vous allez apprendre à intégrer les services Azure AI à
PostgreSQL pour améliorer votre base de données avec des fonctionnalités
d'IA avancées. En tirant parti de la puissance d'Azure OpenAI et des
extensions PostgreSQL telles que pgvector et PostGIS, vous permettrez
des analyses de texte sophistiquées, des recherches de similarité
vectorielle et des requêtes géospatiales directement dans votre base de
données. Cet Lab vous guide dans l'approvisionnement des ressources
Azure nécessaires, la configuration de votre base de données et
l'exécution de requêtes complexes qui combinent des informations basées
sur l'IA avec des données géospatiales. data**.Objectives**

- Pour approvisionner et configurer Azure Database pour PostgreSQL
  Flexible Server.

- Pour créer et gérer des intégrations de vecteurs à l'aide du service
  Azure OpenAI.

- Effectuer des recherches de similarité vectorielle afin de trouver des
  données textuelles sémantiquement similaires.

- Utiliser l'extension PostGIS pour l'analyse de données géospatiales.

- Intégrer les services Azure AI Language pour l'analyse des sentiments
  et d'autres fonctions cognitives.

- Optimiser et analyser les performances des requêtes à l'aide d'outils
  d'indexation et de planification des requêtes.

**Important :** Si l'une des commandes n'est pas **collée** dans le
**CloudShell**, veuillez ouvrir un bloc-notes, maintenir le curseur dans
un espace vide du bloc-notes, puis cliquer sur le bouton T de la
commande à coller. Le contenu sera copié dans le bloc-notes, puis vous
pourrez le copier et le coller à partir du bloc-notes sur le CloudShell.

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

    - **Resource Group** : le **Resource Group** qui vous est attribué.

\[ ! Alerte\] **Important :** Assurez-vous de créer toutes vos
ressources sous ce groupe de ressources

![](./media/image1.png)

3.  L'onglet **Help** contient les informations d'assistance. La valeur
    **ID** ici est l'ID de **Lab instance** qui sera utilisé lors de
    l'exécution du Lab.

![](./media/image2.png)

## Exercice 1 : Approvisionner une base de données Azure pour un serveur flexible PostgreSQL

### Tâche 0 : Enregistrer les fournisseurs de ressources

1.  Connectez-vous à **Azure portal** -
    +++[https://portal.azure.com+++](https://portal.azure.com+++/) à
    l'aide de vos identifiants de connexion Azure.

2.  Cliquez sur **Subscriptions** et sélectionnez **Resource Providers**
    sous **Settings** dans le volet gauche.

3.  Recherchez +++**Microsoft.DBforPostgreSQL**+++ et cliquez sur
    **Register** pour enregistrer ce fournisseur de ressources (Resource
    Provider).

![](./media/image3.png)

### Tâche 1 : Approvisionner une base de données Azure pour un serveur flexible PostgreSQL

1.  Ouvrez un navigateur Web et accédez à l'écran
    +++[https://portal.azure.com+++](https://portal.azure.com+++/)

2.  Sélectionnez l'icône **Cloud Shell** dans la barre d'outils du
    portail Azure pour ouvrir un nouveau volet Cloud Shell en haut de la
    fenêtre de votre navigateur.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image4.jpeg)

3.  La première fois que vous ouvrez Cloud Shell, vous pouvez être
    invité à choisir le type de shell que vous souhaitez utiliser
    (**Bash** ou **PowerShell**). Sélectionnez **Bash**.

![](./media/image5.jpeg)

4.  Dans **Getting started** Prise en main, sélectionnez **Mount storage
    account,** puis sélectionnez votre abonnement Azure. Cliquez sur le
    bouton **Apply**.

![](./media/image6.png)

5.  Dans **Mount storage account** Monter le compte de stockage,
    sélectionnez **we will create a storage account for you** et cliquez
    sur le bouton **Next**.

![](./media/image7.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image8.jpeg)

6.  À l'invite Cloud Shell, exécutez les commandes suivantes pour
    définir des variables pour la création de ressources. Les variables
    représentent les noms à attribuer à votre groupe de ressources et à
    votre base de données et spécifient la région Azure dans laquelle
    les ressources doivent être déployées.

7.  Remplacez le nom du groupe de ressources dans la commande ci-dessous
    par le groupe de ressources affecté et exécutez la commande.

+++RG_NAME= \< Resource group Name \>+++

![](./media/image9.png)

8.  Dans le nom de la base de données, remplacez le jeton {SUFFIX} par
    votre **ID Lab instance ID**, tel que vos initiales, pour vous
    assurer que le nom du serveur de base de données est globalement
    unique.

+++DATABASE_NAME=<pgsql-flex-@lab.LabInstance.Id>+++

![](./media/image10.jpeg)

9.  Exécutez la commande ci-dessous pour définir la valeur de la région.

+++REGION=@lab.CloudResourceGroup(ResourceGroup1).Location+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image11.jpeg)

10. Approvisionnez une instance de base de données Azure Database for
    PostgreSQL au sein du groupe de ressources attribué en exécutant la
    commande Azure CLI suivante (Cette commande prendra 10 minutes)

> \`\`\`
>
> az postgres flexible-server create --name $DATABASE_NAME --location
> $REGION --resource-group $RG_NAME \\
>
> --admin-user s2admin --admin-password Seattle123Seattle123
> --database-name airbnb \\
>
> --public-access 0.0.0.0-255.255.255.255 --version 16 \\
>
> --sku-name Standard_D2s_v3 --storage-size 32 --yes
>
> \`\`\`

![](./media/image12.jpeg)

### Tâche 2 : Se connecter à la base de données à l'aide de psql dans Azure Cloud Shell

Dans cette tâche, vous allez utiliser l'utilitaire de ligne de commande
psql d'Azure Cloud Shell pour vous connecter à votre base de données.

1.  Ouvrez un navigateur, accédez à
    +++[https://portal.azure.com+++](https://portal.azure.com+++/) et
    connectez-vous avec votre compte d'abonnement Azure.

2.  Sur la page d'**accueil**, cliquez sur **Resource Groups**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image13.jpeg)

3.  Cliquez sur le nom du groupe de ressources qui vous a été attribué
    (**your assigned resource group**.)

![](./media/image14.png)

4.  Dans le groupe de ressources, sélectionnez la ressource **PostgreSQL
    Flexible Server**

![](./media/image15.png)

5.  Dans le menu de navigation de gauche, sélectionnez **Connect** sous
    **Settings**.

![](./media/image16.jpeg)

6.  À partir de la page **Connect** de la base de données dans le
    portail Azure, sélectionnez **airbnb** pour le nom de la **Database
    name**, puis copiez le bloc **Connection details** et collez-le dans
    le bloc-notes pour utiliser les informations dans les tâches à
    venir.

![](./media/image17.jpeg)

7.  Dans la page d'accueil d'Azure Database pour PostgresSQL, cliquez
    sur **Overview** dans le menu de navigation de gauche, copiez le nom
    du serveur et collez-le dans le bloc-notes, puis **Save** le
    bloc-notes pour utiliser les informations dans le Lab à venir.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image18.jpeg)

8.  Dans la page d'accueil d'Azure Database pour PostgreSQL,
    sélectionnez **Networking** sous Paramètres, puis **Allow public
    access from any Azure service within Azure to this server**. Cliquez
    sur le bouton **Save**.

![](./media/image19.jpeg)

![](./media/image20.jpeg)

9.  Sélectionnez l'icône **Cloud Shell** dans la barre d'outils du
    portail Azure pour ouvrir un nouveau volet Cloud Shell en haut de la
    fenêtre de votre navigateur.

10. Collez **Connection details** dans Cloud Shell.

![Capture d'écran d'ordinateur d'un écran noir Le contenu généré par
l'IA peut être incorrect.](./media/image21.jpeg)

11. À l'invite Cloud Shell, remplacez le jeton **{your_password}** par
    le mot de passe que vous avez attribué à l'**utilisateur s2admin**
    lors de la création de votre base de données, le mot de passe doit
    être +++**Seattle123Seattle123**+++.

![](./media/image22.jpeg)

12. Connectez-vous à votre base de données à l'aide de l'utilitaire de
    ligne de commande psql en entrant ce qui suit à l'invite :

+++psql+++

![](./media/image23.jpeg)

La connexion à la base de données à partir de Cloud Shell nécessite que
la case Autoriser l'accès public à partir de n'importe quel service
Azure au sein d'Azure vers le serveur soit cochée sur la page
**Networking** de la base de données. Si vous recevez un message
indiquant que vous ne parvenez pas à vous connecter, vérifiez que cette
option est cochée et réessayez.

### Tâche 3 : Ajouter des données à la base de données

À l'aide de l'invite de commande psql, vous allez créer des tables et
les remplir avec des données à utiliser dans le Lab.

1.  Exécutez les commandes suivantes pour créer des tables temporaires
    pour l'importation de données JSON à partir d'un compte de stockage
    d'objets blob public.

> CREATE TABLE temp_calendar (data jsonb) ;
>
> CREATE TABLE temp_listings (data jsonb) ;
>
> CREATE TABLE temp_reviews (data jsonb) ;

![](./media/image24.jpeg)

2.  À l'aide de la commande COPY, remplissez chaque table temporaire
    avec des données provenant de fichiers JSON dans un compte de
    stockage public.

+++\COPY temp_calendar (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/calendar.json'+++>

+++\COPY temp_listings (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/listings.json'+++>

+++\COPY temp_reviews (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/reviews.json'+++>

![](./media/image25.jpeg)

![](./media/image26.jpeg)

3.  Exécutez la commande suivante pour créer les tables de stockage des
    données dans la forme utilisée par cet Lab :

> CREATE TABLE listings (
>
> listing_id int,
>
> name varchar(50),
>
> street varchar(50),
>
> city varchar(50),
>
> state varchar(50),
>
> country varchar(50),
>
> zipcode varchar(50),
>
> bathrooms int,
>
> bedrooms int,
>
> latitude decimal(10,5),
>
> longitude decimal(10,5),
>
> summary varchar(2000),
>
> description varchar(2000),
>
> host_id varchar(2000),
>
> host_url varchar(2000),
>
> listing_url varchar(2000),
>
> room_type varchar(2000),
>
> amenities jsonb,
>
> host_verifications jsonb,
>
> data jsonb
>
> );

![](./media/image27.jpeg)

> CREATE TABLE reviews (
>
> id int,
>
> listing_id int,
>
> reviewer_id int,
>
> reviewer_name varchar(50),
>
> date date,
>
> comments varchar(2000)
>
> );
>
> CREATE TABLE calendar (
>
> listing_id int,
>
> date date,
>
> price decimal(10,2),
>
> available boolean
>
> );

![](./media/image28.jpeg)

4.  Enfin, exécutez les instructions **INSERT INTO suivantes** pour
    charger les données des tables temporaires dans les tables
    principales, en extrayant les données du champ de données JSON dans
    des colonnes individuelles :

> INSERT INTO listings
>
> SELECT
>
> data\['id'\]::int,
>
> replace(data\['name'\]::varchar(50), '"', ''),
>
> replace(data\['street'\]::varchar(50), '"', ''),
>
> replace(data\['city'\]::varchar(50), '"', ''),
>
> replace(data\['state'\]::varchar(50), '"', ''),
>
> replace(data\['country'\]::varchar(50), '"', ''),
>
> replace(data\['zipcode'\]::varchar(50), '"', ''),
>
> data\['bathrooms'\]::int,
>
> data\['bedrooms'\]::int,
>
> data\['latitude'\]::decimal(10,5),
>
> data\['longitude'\]::decimal(10,5),
>
> replace(data\['description'\]::varchar(2000), '"', ''),
>
> replace(data\['summary'\]::varchar(2000), '"', ''),
>
> replace(data\['host_id'\]::varchar(50), '"', ''),
>
> replace(data\['host_url'\]::varchar(50), '"', ''),
>
> replace(data\['listing_url'\]::varchar(50), '"', ''),
>
> replace(data\['room_type'\]::varchar(50), '"', ''),
>
> data\['amenities'\]::jsonb,
>
> data\['host_verifications'\]::jsonb,
>
> data::jsonb
>
> FROM temp_listings;
>
> INSERT INTO reviews
>
> SELECT
>
> data\['id'\]::int,
>
> data\['listing_id'\]::int,
>
> data\['reviewer_id'\]::int,
>
> replace(data\['reviewer_name'\]::varchar(50), '"', ''),
>
> to_date(replace(data\['date'\]::varchar(50), '"', ''), 'YYYY-MM-DD'),
>
> replace(data\['comments'\]::varchar(2000), '"', '')
>
> FROM temp_reviews;
>
> INSERT INTO calendar
>
> SELECT
>
> data\['listing_id'\]::int,
>
> to_date(replace(data\['date'\]::varchar(50), '"', ''), 'YYYY-MM-DD'),
>
> data\['price'\]::decimal(10,2),
>
> replace(data\['available'\]::varchar(50), '"', '')::boolean
>
> FROM temp_calendar;

![](./media/image29.jpeg)

## Exercice 2 : Ajouter des extensions Azure AI et Vector à la liste d'autorisation

Tout au long de cet Lab, vous utiliserez les extensions azure_ai et
pgvector pour ajouter des fonctionnalités d'IA générative à votre base
de données PostgreSQL. Dans cet exercice, vous allez ajouter ces
extensions à la *liste d'autorisation de votre serveur*, comme décrit
dans la section Utilisation des extensions PostgreSQL.

1.  Sur la page d'accueil, cliquez sur **Resource Groups**.

![](./media/image30.jpeg)

2.  Cliquez sur le nom de votre groupe de ressources (Resource Groups).

![](./media/image14.png)

3.  Dans le groupe de ressources, sélectionnez la ressource **PostgreSQL
    Flexible Server**

![](./media/image15.png)

4.  Dans le menu de navigation de gauche de la base de données,
    sélectionnez **Server parameters** sous **Settings**, puis entrez
    +++**azure.extensions**+++ dans la zone de recherche. Développez la
    liste déroulante VALUE, puis recherchez et cochez la case en regard
    de chacune des extensions suivantes :

    - AZURE_AI

    - POSTGIS

    - VECTEUR

![](./media/image31.jpeg)

![](./media/image32.jpeg)

![](./media/image33.jpeg)

5.  Sélectionnez **Save** dans la barre d'outils, ce qui déclenchera un
    déploiement sur la base de données.

![](./media/image34.jpeg)

## Exercice 3 : Créer une ressource Azure OpenAI

L'extension azure_ai nécessite un service Azure OpenAI sous-jacent pour
créer des intégrations vectorielles. Dans cet exercice, vous allez
provisionner une ressource Azure OpenAI dans le portail Azure et
déployer un modèle d'incorporation dans ce service.

### Tâche 1 : Approvisionner un service Azure OpenAI

Dans cette tâche, vous allez créer un service Azure OpenAI.

1.  À partir de la page d'accueil du portail Azure, cliquez sur le
    **Azure portal menu** représenté par trois barres horizontales sur
    le côté gauche de la barre de commandes Microsoft Azure, comme
    illustré dans l'image ci-dessous.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image35.jpeg)

2.  Naviguez et cliquez sur **+ Create a resource**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image36.jpeg)

3.  Sur la page **Create a resource**, dans la barre de recherche
    **Search services and marketplace**, tapez +++**Azure OpenAI**+++,
    puis appuyez sur le bouton **Enter**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image37.jpeg)

4.  Sur la page **Marketplace**, accédez à la section **Azure OpenAI**,
    cliquez sur la liste déroulante du bouton Créer, puis sélectionnez
    **Azure OpenAI** comme indiqué dans l'image. (Au cas où vous auriez
    déjà cliqué sur l'icône **Azure OpenAI**, puis cliquez sur le bouton
    **Create** sur la **Azure OpenAI page**).

![Une capture d'écran d'une page de logiciel Le contenu généré par l'IA
peut être incorrect.](./media/image38.png)

5.  Sous l'onglet Create Azure OpenAI **Basics**, entrez les
    informations suivantes et cliquez sur le bouton **Next**.

[TABLE]

6.  ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA
    peut être incorrect.](./media/image39.png)

7.  Dans l'onglet **Network**, laissez toutes les cases d'option dans
    l'état par défaut et cliquez sur le bouton **Next**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image40.jpeg)

8.  Dans l'onglet **Tags**, laissez tous les champs dans l'état par
    défaut et cliquez sur le bouton **Next**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image41.png)

9.  Dans l'onglet **Review+submit**, une fois la validation réussie,
    cliquez sur le bouton **Create**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image42.png)

10. Attendez la fin du déploiement. Le déploiement prendra environ 2 à 3
    minutes.

\[ ! Remarque **Remarque :** Si vous voyez un message indiquant que le
service Azure OpenAI est actuellement disponible pour les clients via un
formulaire de demande. L'abonnement sélectionné n'a pas été activé pour
le service et n'a pas de quota pour les niveaux tarifaires ; vous devrez
cliquer sur le lien pour demander l'accès au service Azure OpenAI et
remplir le formulaire de demande.

### Tâche 2 : Récupérer la clé et le point de terminaison du service Azure OpenAI

1.  Sur la page **Overview** de la ressource, sélectionnez le bouton
    **Go to resource**. Si vous y êtes invité, sélectionnez les
    informations d'identification du Lab :

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image43.jpeg)

2.  Dans votre fenêtre **Azure OpenAI home**, accédez à la section
    **Resource Management**, puis cliquez sur **Keys and Endpoints**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image44.jpeg)

3.  Dans la page **Keys and Endpoints**, copiez les valeurs **KEY1, KEY
    2** et **Endpoint** et collez-les dans un bloc-notes comme illustré
    dans l'image ci-dessous, puis **Save** le bloc-notes pour utiliser
    les informations dans les tâches à venir.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image45.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image46.jpeg)

**Remarque :** Vous pouvez utiliser KEY1 ou KEY2. Le fait d'avoir
toujours deux clés vous permet de faire pivoter et de régénérer les clés
en toute sécurité sans provoquer d'interruption de service.

### Tâche 3 : Déployer un modèle d'incorporation

L'extension azure_ai permet la création d'embeddings vectoriels à partir
de texte. Pour créer ces intégrations, vous devez déployer un modèle
text-embedding-ada-002 (version 2) au sein de votre service Azure
OpenAI. Dans cette tâche, vous allez utiliser Azure OpenAI Studio pour
créer un déploiement de modèle que vous pouvez utiliser.

1.  Dans la page **Azure OpenAI**, cliquez sur **Overview** dans le menu
    de navigation de gauche, faites défiler vers le bas et cliquez sur
    le bouton **Go to Azure OpenAI Studio**, comme illustré dans l'image
    ci-dessous.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image47.png)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image48.png)

2.  Sur Azure **AI Foundry | Ouvrez la** page d'accueil d'Azure AI
    Service, accédez à la section Composants et cliquez sur
    **Déploiements**.

3.  Dans la fenêtre **Deployments**, déroulez-le **+ Deploy model** et
    sélectionnez **Deploy base model**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image49.png)

4.  Dans la boîte de dialogue **Select a model**, naviguez et
    sélectionnez soigneusement **text-embedding-ada-002**, puis cliquez
    sur le bouton **Confirm**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image50.png)

5.  Dans la boîte de dialogue **Deploy model**, définissez les éléments
    suivants et sélectionnez **Create** pour déployer le modèle.

    - **Sélectionnez un modèle** : choisissez **text-embedding-ada-002**
      dans la liste.

    - **Version du modèle** : Assurez-vous que **2 (par défaut)** est
      sélectionné.

    - **Nom du déploiement** : Entrez +++**embeddings**+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image51.png)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image52.png)

6.  Dans la fenêtre **Deployments**, copiez le **Deployment name** et
    collez-le dans un bloc-notes (comme illustré dans l'image), puis
    **Save** le bloc-notes pour utiliser les informations dans la tâche
    à venir.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image53.png)

## Exercice 4 : Installer et configurer l'extension azure_ai

Dans cet exercice, vous allez installer l'extension azure_ai dans votre
base de données et la configurer pour qu'elle se connecte à votre
service Azure OpenAI.

### Tâche 1 : Se connecter à la base de données à l'aide de psql dans Azure Cloud Shell

Dans cette tâche, vous allez utiliser l'utilitaire de ligne de commande
psql d'Azure Cloud Shell pour vous connecter à votre base de données.

1.  Sélectionnez l'icône **Cloud Shell** dans la barre d'outils du
    portail Azure pour ouvrir un nouveau volet Cloud Shell en haut de la
    fenêtre de votre navigateur.

2.  Collez **Connection details** dans Cloud Shell.

![Capture d'écran d'ordinateur d'un écran noir Le contenu généré par
l'IA peut être incorrect.](./media/image21.jpeg)

3.  À l'invite Cloud Shell, remplacez le jeton **{your_password}** par
    le mot de passe que vous avez attribué à l'utilisateur **s2admin**
    lors de la création de votre base de données, le mot de passe doit
    être **Seattle123Seattle123**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image22.jpeg)

4.  Connectez-vous à votre base de données à l'aide de l'utilitaire de
    ligne de commande psql en entrant ce qui suit à l'invite :

+++**psql**+++

![Un fond noir avec un carré noir Le contenu généré par l'IA peut être
incorrect.](./media/image23.jpeg)

### Tâche 2 : Installer l'extension azure_ai

L'extension azure_ai vous permet d'intégrer Azure OpenAI et Azure
Cognitive Services dans votre base de données. Pour activer l'extension
dans votre base de données, procédez comme suit :

1.  Vérifiez que l'extension a bien été ajoutée à la liste
    d'autorisation en exécutant ce qui suit à partir de l'invite de
    commande psql :

+++ SHOW azure.extensions;+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image54.jpeg)

2.  Installez l'extension azure_ai à l'aide de la commande CREATE
    EXTENSION.

+++ CREATE EXTENSION IF NOT EXISTS azure_ai;+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image55.jpeg)

### Tâche 3 : Examiner les objets contenus dans l'extension azure_ai

L'examen des objets de l'extension azure_ai peut permettre de mieux
comprendre ses capacités. Dans cette tâche, vous allez inspecter les
différents schémas, fonctions définies par l'utilisateur (UDF) et types
composites ajoutés à la base de données par l'extension.

1.  Vous pouvez utiliser la méta-commande \dx à partir de l'invite de
    commande **psql** pour répertorier les objets contenus dans
    l'extension.

\[ ! Remarque\] **Remarque :** Cliquez sur n'importe quelle touche pour
continuer lorsque le shell du cloud vous invite avec **More...**

+++\dx+ azure_ai+++

![](./media/image56.jpeg)

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image57.jpeg)

Le résultat de la métacommande montre que l'extension azure_ai crée
trois schémas, plusieurs fonctions définies par l'utilisateur (UDF) et
plusieurs types composites dans la base de données. Le tableau
ci-dessous répertorie les schémas ajoutés par l'extension et les décrit.

[TABLE]

2.  Les fonctions et les types sont tous associés à l'un des schémas.
    Pour passer en revue les fonctions définies dans le schéma azure_ai,
    utilisez la méta-commande \df, en spécifiant le schéma dont les
    fonctions doivent être affichées. La commande \x auto qui précède
    \df permet d'appliquer automatiquement l'affichage étendu si
    nécessaire pour faciliter l'affichage de la sortie de la commande
    dans Azure Cloud Shell.

+++\x auto+++ +++\df+ azure_ai.\*+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image58.jpeg)

La fonction azure_ai.set_setting() vous permet de définir le point de
terminaison et les valeurs de clé pour les services Azure AI. Il accepte
une **clé (Key)** et la **valeur (value)** à lui attribuer. La fonction
azure_ai.get_setting() permet de récupérer les valeurs que vous avez
définies avec la fonction set_setting(). Il accepte la **clé (Key)** du
paramètre que vous souhaitez afficher. Pour les deux méthodes, la clé
doit être l'une des suivantes :

### Tâche 4 : Définir le point de terminaison et la clé Azure OpenAI

Avant d'utiliser les fonctions azure_openai, configurez l'extension à
votre point de terminaison et à votre clé de service Azure OpenAI.

1.  Dans la commande ci-dessous, remplacez
    **{endpoint}** et **{api-key}** par les valeurs que vous avez
    récupérées à partir du portail Azure, puis exécutez les commandes à
    partir de l'invite de commande psql dans le volet Cloud Shell pour
    ajouter vos valeurs à la table de configuration.

2.  SELECT azure_ai.set_setting('azure_openai.endpoint','{endpoint}') ;

3.  SELECT azure_ai.set_setting('azure_openai.subscription_key',
    '{api-key}') ;

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image59.jpeg)

4.  Vérifiez les paramètres écrits dans la table de configuration à
    l'aide des requêtes suivantes :

5.  SELECT azure_ai.get_setting('azure_openai.endpoint');

6.  SELECT azure_ai.get_setting('azure_openai.subscription_key');

L'extension azure_ai est maintenant connectée à votre compte Azure
OpenAI et prête à générer des intégrations de vecteurs.

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image60.jpeg)

## Exercice 5 : Générer des intégrations de vecteurs avec Azure OpenAI

Le schéma azure_openai de l'extension azure_ai permet à Azure OpenAI de
créer des intégrations vectorielles pour les valeurs de texte. À l'aide
de ce schéma, vous pouvez générer des intégrations avec Azure OpenAI
directement à partir de la base de données pour créer des
représentations vectorielles du texte d'entrée, qui peuvent ensuite être
utilisées dans des recherches de similarité vectorielle, ainsi que
consommées par des modèles d'apprentissage automatique.

Les intégrations sont un concept d'apprentissage automatique et de
traitement du langage naturel (NLP) qui consiste à représenter des
objets, tels que des mots, des documents ou des entités, sous forme de
vecteurs dans un espace multidimensionnel. Les intégrations permettent
aux modèles d'apprentissage automatique d'évaluer dans quelle mesure les
informations sont étroitement liées. Cette technique identifie
efficacement les relations et les similitudes entre les données, ce qui
permet aux algorithmes d'identifier des modèles et de faire des
prédictions précises.

### Tâche 1 : Activer la prise en charge des vecteurs avec l'extension pgvector

L'extension azure_ai vous permet de générer des plongements pour le
texte d'entrée. Pour permettre aux vecteurs générés d'être stockés avec
le reste de vos données dans la base de données, vous devez installer
l'extension pgvector en suivant les instructions de la section Activer
la prise en charge des vecteurs dans la documentation de votre base de
données.

1.  Installez l'extension pgvector à l'aide de la commande CREATE
    EXTENSION.

+++ CREATE EXTENSION IF NOT EXISTS vector; +++

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image61.jpeg)

2.  Une fois le vecteur pris en charge ajouté à votre base de données,
    ajoutez une nouvelle colonne à la table des listes à l'aide du type
    de données vectoriel pour stocker les plongements dans la table. Le
    modèle text-embedding-ada-002 produit des vecteurs avec des
    dimensions de 1536, vous devez donc spécifier 1536 comme taille de
    vecteur.

3.  ALTER TABLE Listings

4.  ADD COLUMN description_vector vector(1536) ;

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image62.jpeg)

### Tâche 2 : Générer et stocker des plongements de vecteurs

Le tableau des listes est maintenant prêt à stocker les intégrations. À
l'aide de la fonction azure_openai.create_embeddings(), vous créez des
vecteurs pour le champ de description et les insérez dans la colonne
description_vector nouvellement créée dans la table des listings.

1.  Avant d'utiliser la fonction create_embeddings(), exécutez la
    commande suivante pour l'inspecter et vérifier les arguments requis
    :

+++\df+ azure_openai.\* +++

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image63.jpeg)

La propriété Types de données Argument dans la sortie de la commande
\df+ azure_openai.\* révèle la liste des arguments attendus par la
fonction.

[TABLE]

2.  À l'aide du nom du déploiement, exécutez la requête suivante pour
    mettre à jour chaque enregistrement dans la table des listes, en
    insérant les plongements de vecteurs générés pour le champ de
    description dans la colonne description_vector à l'aide de la
    fonction azure_openai.create_embeddings(). Remplacez
    {votre-nom-de-déploiement} par la valeur **Deployment name** que
    vous avez copiée à partir de la page **Deployments** d'Azure OpenAI
    Studio . Notez que cette requête prend environ cinq minutes.

> DO $$
>
> DECLARE counter integer := (SELECT COUNT(\*) FROM listings WHERE
> description \<\> '' AND description_vector IS NULL);
>
> DECLARE r record;
>
> BEGIN
>
> RAISE NOTICE 'Total descriptions to embed: %', counter;
>
> WHILE counter \> 0 LOOP
>
> BEGIN
>
> FOR r IN
>
> SELECT listing_id FROM listings WHERE description \<\> '' AND
> description_vector IS NULL
>
> LOOP
>
> BEGIN
>
> UPDATE listings
>
> SET description_vector =
> azure_openai.create_embeddings('{your-deployment-name}', description)
>
> WHERE listing_id = r.listing_id;
>
> EXCEPTION
>
> WHEN OTHERS THEN
>
> RAISE NOTICE 'Waiting 1 second before trying again...';
>
> PERFORM pg_sleep(1);
>
> END;
>
> counter := (SELECT COUNT(\*) FROM listings WHERE description \<\> ''
> AND description_vector IS NULL);
>
> IF counter % 25 = 0 THEN
>
> RAISE NOTICE 'Remaining descriptions to embed: %', counter;
>
> END IF;
>
> END LOOP;
>
> END;
>
> END LOOP;
>
> END;
>
> $$;

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image64.jpeg)

La requête ci-dessus utilise une boucle WHILE pour récupérer les
enregistrements de la table listings où le champ description_vector est
nul et où le champ description n’est pas une chaîne vide. La requête
tente ensuite de mettre à jour la colonne description_vector avec une
représentation vectorielle de la colonne description en utilisant la
fonction azure_openai.create_embeddings. La boucle est utilisée lors de
cette mise à jour afin d’éviter que les appels à la fonction
create_embeddings ne dépassent la limite de fréquence d’appel du service
Azure OpenAI. Si la limite de fréquence est dépassée, vous verrez des
avertissements similaires à ceux-ci dans la sortie :

\[ ! Remarque\] **REMARQUE** : Attendez 1 seconde avant de réessayer...

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image65.jpeg)

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image66.jpeg)

3.  Vous pouvez vérifier que la colonne description_vector a été
    renseignée pour tous les enregistrements d'annonces en exécutant la
    requête suivante :

+++SELECT COUNT(\*) FROM listings WHERE description_vector IS NULL AND
description \<\> '';+++

Le résultat de la requête doit être un nombre de 0.

![Un écran noir avec du texte blanc Le contenu généré par l'IA peut être
incorrect.](./media/image67.jpeg)

### Tâche 3 : Effectuer une recherche de similarité vectorielle

La similarité vectorielle est une méthode utilisée pour mesurer la
similarité entre deux éléments en les représentant sous forme de
vecteurs, c’est-à-dire une série de nombres. Les vecteurs sont souvent
utilisés pour effectuer des recherches à l’aide de LLMs. La similarité
vectorielle est généralement calculée à l’aide de métriques de distance,
telles que la **distance euclidienne** ou la **similarité cosinus**. La
**distance euclidienne** mesure la distance en ligne droite entre deux
vecteurs dans un espace n-dimensionnel, tandis que la **similarité
cosinus** mesure le cosinus de l’angle entre deux vecteurs. Chaque
**embedding** est un vecteur de nombres à virgule flottante, donc la
distance entre deux embeddings dans l’espace vectoriel est corrélée à la
similarité sémantique entre deux entrées dans leur format d’origine.

1.  Avant d’exécuter une recherche basée sur la similarité vectorielle,
    exécutez la requête ci-dessous en utilisant la clause ILIKE pour
    observer les résultats d’une recherche basée sur une requête en
    langage naturel, sans utiliser la similarité vectorielle :

> +++SELECT listing_id, name, description FROM listings WHERE
> description ILIKE '%Properties with a private room near Discovery
> Park%';+++

![Un fond noir avec du texte blanc Le contenu généré par l'IA peut être
incorrect.](./media/image68.jpeg)

La requête ne renvoie aucun résultat, car elle tente de faire
correspondre le texte du champ de description avec la requête en langage
naturel fournie.

2.  Exécutez maintenant une requête de recherche de similarité cosinus
    sur la table *listings* afin d’effectuer une recherche de similarité
    vectorielle sur les descriptions des annonces. Les *embeddings* sont
    générés à partir d’une question saisie, puis convertis en tableau
    vectoriel (::vector), ce qui permet leur comparaison avec les
    vecteurs stockés dans la table *listings*. Remplacez
    *{your-deployment-name}* par la valeur du nom de déploiement que
    vous avez copiée depuis la page *Deployments* dans *Azure OpenAI
    Studio*.

+++SELECT listing_id, name, description FROM listings ORDER BY
description_vector \<=\>
azure_openai.create_embeddings('{your-deployment-name}', 'Properties
with a private room near Discovery Park')::vector LIMIT 3;+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image69.jpeg)

La requête utilise l’opérateur vectoriel \<=\>, qui représente
l’opérateur de **cosine distance** utilisé pour calculer la distance
entre deux vecteurs dans un espace multidimensionnel.

3.  Exécutez de nouveau la même requête en utilisant la clause **EXPLAIN
    ANALYZE** afin d’afficher le plan d’exécution et les temps de
    traitement de la requête. Remplacez **{your-deployment-name}** par
    la valeur du nom de déploiement que vous avez copiée depuis la page
    **Deployments** dans **Azure OpenAI Studio**.

> EXPLAIN ANALYZE
>
> SELECT listing_id, name, description FROM listings
>
> ORDER BY description_vector \<=\>
> azure_openai.create_embeddings('{your-deployment-name}', 'Properties
> with a private room near Discovery Park')::vector
>
> LIMIT 3;

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image70.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image71.jpeg)

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image72.jpeg)

Dans la sortie, notez le plan de requête, qui commencera par quelque
chose de similaire à :

Limit (coût=1098.54..1098.55 lignes=3 largeur=261) (temps
réel=10.505..10.507 lignes=3 boucles=1) -\> Trier (coût=1098.54..1104.10
lignes=2224 largeur=261) (temps réel=10.504..10.505 lignes=3 boucles=1)

…

Méthode de tri : top-N heapsort Mémoire : 27kB -\> Seq Scan sur les
listes (cost=0.00..1069.80 rows=2224 width=261) (temps réel=0.005..9.997
rows=2224 loops=1) La requête utilise un tri par balayage séquentiel
pour effectuer la recherche. Les temps de planification et d'exécution
seront indiqués à la fin des résultats et doivent ressembler à ce qui
suit : Temps de planification : 62.020 ms Temps d'exécution : 10.530 ms

4.  Pour permettre une recherche plus efficace sur le champ vectoriel,
    créez un index sur listings en utilisant la distance cosine et HNSW
    (abréviation de Hierarchical Navigable Small World). HNSW permet à
    pgvector d'exploiter les derniers algorithmes basés sur des graphes
    pour approximer les requêtes de plus proches voisins.

+++CREATE INDEX ON listings USING hnsw (description_vector
vector_cosine_ops);+++![](./media/image73.jpeg)

5.  Pour observer l’impact de l’index *hnsw* sur la table, exécutez à
    nouveau la requête avec la clause EXPLAIN ANALYZE afin de comparer
    les temps de planification et d’exécution. Remplacez
    {your-deployment-name} par la valeur *Deployment name* que vous avez
    copiée depuis la page *Deployments* dans *Azure OpenAI Studio*.

EXPLAIN ANALYZE

SELECT listing_id, name, description FROM listings

ORDER BY description_vector \<=\>
azure_openai.create_embeddings('{your-deployment-name}', 'Properties
with a private room near Discovery Park')::vector

LIMIT 3;![Un écran d'ordinateur avec du texte blanc Le contenu généré
par l'IA peut être incorrect.](./media/image74.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image75.jpeg)

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image76.jpeg)

Dans les résultats, remarquez que le plan de requête inclut désormais
une exploration d’index plus efficace :

Limit (cost=116.48..119.33 rows=3 width=261) (actual time=1.112..1.130
rows=3 loops=1)

-\> Index Scan using listings_description_vector_idx on listings
(cost=116.48..2228.28 rows=2224 width=261) (actual time=1.111..1.128
rows=3 loops=1)

Les temps d'exécution de la requête doivent refléter une réduction
significative du temps nécessaire à la planification et à l'exécution de
la requête :

Temps de planification : 56.802 ms

Temps d'exécution : 1.167 ms

## Exercice 6 : Intégrer Azure AI Services

Les intégrations de services Azure AI incluses dans le schéma
azure_cognitive de l'extension azure_ai fournissent un ensemble complet
de fonctionnalités du langage AI accessibles directement à partir de la
base de données. Les fonctionnalités incluent l'analyse des sentiments,
la détection de la langue, l'extraction de phrases clés, la
reconnaissance d'entités et la synthèse de texte. Ces fonctionnalités
sont activées via le service Azure AI Language.

Pour consulter la liste complète des fonctionnalités Azure AI
accessibles via l'extension, consultez la documentation Intégrer Azure
Database pour PostgreSQL Flexible Server à Azure Cognitive Services.

### Tâche 1 : Approvisionner un service Azure AI Language

Un service Azure AI Languageservice est requis pour tirer parti des
fonctions cognitives des extensions azure_ai. Dans cet exercice, vous
allez créer un service Azure AI Language.

1.  À partir de la page d'accueil du portail Azure, cliquez sur **Azure
    portal menu** représenté par trois barres horizontales sur le côté
    gauche de la barre de commandes Microsoft Azure, comme illustré dans
    l'image ci-dessous.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image77.jpeg)

2.  Sur la page **Create a resource**, sélectionnez **AI + Machine
    Learning** dans le menu de gauche, puis sélectionnez **Language
    service**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image78.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image79.jpeg)

3.  Dans la boîte de dialogue **Select additional features,**
    sélectionnez **Continue to create your resource**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image80.jpeg)

4.  Dans l'onglet Create Language **Basics**, entrez ce qui suit :

[TABLE]

5.  ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA
    peut être incorrect.](./media/image81.png)

6.  ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA
    peut être incorrect.](./media/image82.jpeg)

7.  Les paramètres par défaut seront utilisés pour les onglets restants
    de la configuration du service de langue, sélectionnez donc le
    bouton **Review + create**.

8.  Sélectionnez le bouton **Create** dans l'onglet **Review + create**
    pour provisionner le service de langue.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image83.png)

9.  Sélectionnez **Go to resource group** sur la page de déploiement
    lorsque le déploiement du service de langage est terminé.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image84.jpeg)

### Tâche 2 : Définir le point de terminaison et la clé du service Azure AI Language

Comme pour les fonctions azure_openai, pour effectuer des appels sur les
services Azure AI à l'aide de l'extension azure_ai, vous devez fournir
le point de terminaison et une clé pour votre service Azure AI Language.

1.  Sur la page d'accueil Langue, sélectionnez l'élément **Keys and
    Endpoint** sous **Resource Management** dans le menu de navigation
    de gauche.

2.  Dans la page **Keys and Endpoints**, copiez les valeurs **KEY1, KEY
    2** et **Endpoint** et collez-les dans un bloc-notes comme illustré
    dans l'image ci-dessous, puis **Save** le bloc-notes pour utiliser
    les informations dans les tâches à venir.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image85.jpeg)

3.  Copiez les valeurs de votre point de terminaison et de votre clé
    d'accès, puis dans la commande ci-dessous, remplacez les jetons
    {endpoint} et {api-key} par les valeurs que vous avez récupérées à
    partir du portail Azure. Exécutez les commandes à partir de l'invite
    de commande psql dans Cloud Shell pour ajouter vos valeurs à la
    table de configuration.

\[ ! Remarque **:** Connectez-vous à l'invite de commande psql avant
d'exécuter les commandes ci-dessous.

SELECT azure_ai.set_setting('azure_cognitive.endpoint','{endpoint}') ;

SELECT azure_ai.set_setting('azure_cognitive.subscription_key',
'{api-key}') ;

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image86.jpeg)

### Tâche 3 : Analyser le sentiment des avis

Dans cette tâche, vous allez utiliser la fonction
azure_cognitive.analyze_sentiment pour évaluer les avis sur les annonces
Airbnb.

1.  Pour effectuer une analyse des sentiments à l'aide du schéma
    azure_cognitive dans l'extension azure_ai, vous utilisez la fonction
    analyze_sentiment. Exécutez la commande ci-dessous pour examiner
    cette fonction :

+++\df azure_cognitive.analyze_sentiment+++

![Un écran d'ordinateur avec du texte blanc Le contenu généré par l'IA
peut être incorrect.](./media/image87.jpeg)

La sortie affiche le schéma, le nom, le type de données de résultat et
les types de données d'argument de la fonction. Ces informations aident
à comprendre comment utiliser la fonction.

2.  Il est également essentiel de comprendre la structure du type de
    données de résultat généré par la fonction afin de pouvoir gérer
    correctement sa valeur de retour. Exécutez la commande suivante pour
    inspecter le type de sentiment_analysis_result :

+++\dT+ azure_cognitive.sentiment_analysis_result+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image88.jpeg)

3.  Le résultat de la commande ci-dessus révèle que le type
    sentiment_analysis_result est un tuple. Pour comprendre la structure
    de ce tuple, exécutez la commande suivante pour examiner les
    colonnes contenues dans le type composite sentiment_analysis_result
    :

+++\d+ azure_cognitive.sentiment_analysis_result+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image89.jpeg)

Le résultat de cette commande doit ressembler à ce qui suit : Type
composite « azure_cognitive.sentiment_analysis_result »

Chronique | Type | Collation | Nullable | Par défaut | Stockage | La
description
----------------+------------------+-----------+----------+---------+----------+-------------

Sentiment | texte | | | | Prolongé |

positive_score | double précision | | | | uni |

neutral_score | double précision | | | | uni |

negative_score | double précision | | | | uni |

L’azure_cognitive.sentiment_analysis_result est un type composite
contenant les prédictions de sentiment du texte d'entrée. Il comprend le
sentiment, qui peut être positif, négatif, neutre ou mitigé, et les
scores pour les aspects positifs, neutres et négatifs trouvés dans le
texte. Les scores sont représentés sous forme de nombres réels entre 0
et 1. Par exemple, dans (neutre,0,26,0,64,0,09), le sentiment est neutre
avec un score positif de 0,26, neutre de 0,64 et négatif à 0,09.

## Exercice 7 : Exécuter une requête finale pour lier le tout

Dans cet exercice, vous vous connectez à votre base de données dans
**pgAdmin** et exécutez une requête finale qui relie votre travail avec
les extensions azure_ai, postgis et pgvector dans les Labs 3 et 4.

### Tâche 1: Installer pgAdmin

1.  Ouvrez un navigateur Web et accédez à
    l'<https://www.pgadmin.org/download/pgadmin-4-windows/>

2.  Cliquez sur la dernière version de **pgAdmin**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image90.jpeg)

3.  Sélectionnez **pgadmin4-8.9-x64.exe**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image91.jpeg)

4.  Exécuter et installer le fichier téléchargé

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image92.jpeg)

5.  Dans l'onglet Sélectionner le mode d'installation du programme
    d'installation, sélectionnez **Install for me only(recommended)**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image93.jpeg)

6.  Cliquez sur le bouton **Next**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image94.jpeg)

7.  Sélectionnez **I accept the agreement** et cliquez sur le bouton
    **Next**

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image95.jpeg)

8.  Sélectionnez le chemin d'accès et cliquez sur le bouton **Next**

![Une capture d'écran d'une erreur informatique Le contenu généré par
l'IA peut être incorrect.](./media/image96.jpeg)

9.  Dans la fenêtre **Setup-pgAdmin 4**, cliquez sur le bouton **Next**

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image97.jpeg)

10. Cliquez sur le bouton **Install**

![Une capture d'écran d'un programme informatique Le contenu généré par
l'IA peut être incorrect.](./media/image98.jpeg)

11. Dans la fenêtre **Setup-pgAdmin 4**, cliquez sur le bouton
    **Finish**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image99.jpeg)

### Tâche 2 : Se connecter à la base de données à l'aide de pgAdmin

Dans cette tâche, vous allez ouvrir pgAdmin et vous connecter à votre
base de données.

1.  Dans votre champ de recherche Windows, saisissez +++**pgAdmin**+++,
    puis cliquez sur **pgAdmin**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image100.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image101.jpeg)

2.  Enregistrez votre serveur en cliquant avec le bouton droit sur
    **Servers** dans l'Explorateur d'objets et en sélectionnant
    **Register \> Server**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image102.jpeg)

3.  Dans la boîte de dialogue **Register - Server**, collez le nom de
    votre serveur Azure Database for PostgreSQL Flexible Server (que
    vous avez enregistré dans l'exercice 1\> tâche 1) dans le champ
    **Name** de l'onglet **General**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image103.jpeg)

4.  Ensuite, sélectionnez l'onglet **Connection** et collez le nom de
    votre serveur dans le champ **Hostname/address**. Entrez
    +++**s2admin**+++ dans le champ Nom **d'utilisateur**, entrez
    +++**Seattle123Seattle123**+++ dans la zone **Password**, et
    éventuellement, sélectionnez **Save password**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image104.jpeg)

5.  Enfin, sélectionnez l'onglet **Parameters** et définissez le **SSL
    mode** à **requis**. Sélectionnez **Save** pour enregistrer votre
    serveur.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image105.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image106.jpeg)

6.  Une fois connecté à votre serveur, développez le **Databases** de
    données et sélectionnez la base de données **airbnb**. Faites un
    clic droit sur la base **de données airbnb** et sélectionnez **Query
    Tool** dans le menu contextuel.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image107.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image108.jpeg)

### Tâche 3 : Vérifiez que l'extension PostGIS est installée dans votre base de données

Pour installer l'extension postgis dans votre base de données, vous
allez utiliser la commande CREATE EXTENSION.

1.  Dans la fenêtre de requête que vous avez ouverte ci-dessus, exécutez
    la commande CREATE EXTENSION avec la clause IF NOT EXISTS pour
    installer l'extension postgis dans votre base de données.

+++CREATE EXTENSION IF NOT EXISTS postgis;+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image109.jpeg)

Avec l'extension PostGIS maintenant chargée, vous êtes prêt à commencer
à travailler avec des données géospatiales dans la base de données. Le
tableau des annonces que vous avez créé et rempli ci-dessus contient la
latitude et la longitude de toutes les propriétés répertoriées. Pour
utiliser ces données à des fins d'analyse géospatiale, vous devez
modifier la table des listes afin d'ajouter une colonne de géométrie qui
accepte le type de données de point. Ces nouveaux types de données sont
inclus dans l'extension postgis.

2.  Pour prendre en charge les données ponctuelles, ajoutez une nouvelle
    colonne de géométrie à la table qui accepte les données ponctuelles.
    Copiez et collez la requête suivante dans la fenêtre de requête
    pgAdmin ouverte :

3.  ALTER TABLE listings

+++ADD COLUMN listing_location geometry(point, 4326); +++

4.  Ensuite, mettez à jour la table avec les données géospatiales
    associées à chaque liste en ajoutant les valeurs de longitude et de
    latitude dans la colonne de géométrie.

5.  UPDATE listings

+++SET listing_location = ST_SetSRID(ST_Point(longitude, latitude),
4326) ;+++

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image110.jpeg)

### Tâche 4 : Exécuter une requête et afficher les résultats sur une carte

1.  Copiez et collez la requête suivante dans l'éditeur de requête
    ouvert, puis exécutez-la pour afficher les données stockées dans la
    colonne **listing_location**.

+++SELECT listing_id, name, listing_location FROM listings LIMIT 50;+++

Dans le panneau Sortie de données, sélectionnez le bouton **View all
geometries** de cette colonne affichée dans la colonne
**listing_location** des résultats de la requête.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image111.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image112.jpeg)

2.  Maintenant, exécutez la requête suivante pour effectuer une
    **requête de proximité géospatiale (geospatial proximity query)**,
    renvoyant les propriétés disponibles pour la semaine du 13 janvier
    2016, dont le prix est inférieur à 75,00 $ par nuit et qui se
    trouvent à une courte distance de Discovery Park à Seattle. La
    requête utilise la fonction ST_DWithin fournie par l'extension
    PostGIS pour identifier les inscriptions situées à une distance
    donnée du parc, qui a une longitude de -122,410347 et une latitude
    de 47,655598.

> SELECT name, listing_location, summary
>
> FROM listings l
>
> INNER JOIN calendar c ON l.listing_id = c.listing_id
>
> WHERE ST_DWithin(
>
> listing_location,
>
> ST_GeomFromText('POINT(-122.410347 47.655598)', 4326),
>
> 0.025
>
> )
>
> AND c.date = '2016-01-13'
>
> AND c.available = 't'
>
> AND c.price \<= 75.00;![Une capture d'écran d'un ordinateur Le contenu
> généré par l'IA peut être incorrect.](./media/image113.jpeg)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image114.jpeg)

**Résumé**

Dans cet Lab, vous avez réussi à intégrer les services Azure AI à
PostgreSQL pour créer un puissant environnement de base de données basé
sur l'IA. Vous avez commencé par approvisionner les ressources Azure et
configurer votre base de données PostgreSQL avec les extensions
nécessaires. Vous avez ensuite généré des plongements vectoriels pour
les données textuelles et effectué des recherches de similarité
vectorielle pour trouver des enregistrements sémantiquement similaires.
En outre, vous avez utilisé l'extension PostGIS pour l'analyse des
données géospatiales et le service Azure AI Language pour l'analyse des
sentiments. Enfin, vous avez optimisé vos requêtes à l'aide de
l'indexation et analysé leurs performances, démontrant ainsi
l'efficacité et la capacité de cette solution intégrée d'analyse avancée
des données.

 
