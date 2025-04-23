# Cas d'usage 07 - Activation de la recherche sémantique dans Azure Database pour le serveur flexible PostgreSQL afin d'utiliser Azure OpenAI pour générer des plongements de vecteurs.

**Objectif** :

Dans ce cas d'utilisation, vous implémentez la recherche sémantique pour
générer et stocker des intégrations, installez les extensions
vectorielles et azure_ai dans un serveur flexible Azure Database pour
PostgreSQL, puis appliquez les extensions pour stocker les vecteurs
d'incorporation générés par Azure OpenAI

**Principales technologies utilisées** : Azure OpenAI, Azure Database
pour PostgreSQL, extension Azure AI

**Durée estimée** -- 45 minutes

**Type de Lab :** Dirigé par un instructeur

## Exercice 1 : Générer des plongements de vecteurs avec Azure OpenAI

Pour effectuer des recherches sémantiques, vous devez d'abord générer
des vecteurs d'incorporation à partir d'un modèle, les stocker dans une
base de données de vecteurs, puis interroger les intégrations. Vous
allez créer une base de données, la remplir avec des exemples de données
et exécuter des recherches sémantiques sur ces listes.

À la fin de cet exercice, vous disposerez d'une instance de serveur
flexible Azure Database pour PostgreSQL avec les extensions vectorielle
et azure_ai activées. Vous allez générer des intégrations pour le
tableau des annonces de l'ensemble de données Airbnb Open Data de
Seattle. Vous allez également exécuter des recherches sémantiques sur
ces listes en générant le vecteur d'intégration d'une requête et en
effectuant une recherche de distance cosinus vectoriel.

1.  Ouvrez un navigateur web et accédez à
    \`\`https:\\portal.azure.com/\`\` et connectez-vous avec vos
    informations d'identification Azure.

2.  Sélectionnez l'icône **Cloud Shell** dans la barre d'outils du
    portail Azure pour ouvrir un nouveau volet Cloud Shell en bas de la
    fenêtre de votre navigateur. Sélectionnez **Bash**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image1.jpeg)

3.  Sélectionnez **No storage account required**, sélectionnez votre
    abonnement, puis cliquez sur **Apply**.

![](./media/image2.png)

4.  À l'invite Cloud Shell, exécutez la commande ci-dessous pour cloner
    le projet

\`\`git clone https://github.com/technofocus-pte/postgresql-case\`\`

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image3.jpeg)

5.  Accédez au dossier du projet.

**\`\`cd postgresql-case\`\`**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image4.jpeg)

6.  Ensuite, vous exécutez trois commandes pour définir des variables
    afin de réduire le typage redondant lors de l'utilisation de
    commandes Azure CLI pour créer des ressources Azure. Les variables
    représentent le nom à attribuer à votre groupe de ressources
    (RG_NAME), la région Azure (REGION) dans laquelle les ressources
    seront déployées et un mot de passe généré de manière aléatoire pour
    la connexion de l'administrateur PostgreSQL (ADMIN_PASSWORD).

7.  Dans la première commande, la région attribuée à la variable
    correspondante est eastus ou westus, **\[but you can replace it with
    a location of your preference.\]{.mark}** Toutefois, si vous
    remplacez la valeur par défaut, vous devez sélectionner une autre
    \[région Azure qui prend en charge la synthèse
    abstractive\]{.underline} pour vous assurer que vous pouvez
    effectuer toutes les tâches dans les modules de ce parcours
    d'apprentissage.

''REGION=westus''

8.  La commande suivante attribue le nom du groupe de ressources
    existant à utiliser pour le groupe de ressources qui hébergera
    toutes les ressources utilisées dans cet exercice.

> \`\`RG_NAME=Your existing resource group name\`\`

![](./media/image5.png)

9.  La commande finale génère de manière aléatoire un mot de passe pour
    la connexion administrateur PostgreSQL. **Assurez-vous de le
    copier** dans un endroit sûr pour l'utiliser ultérieurement pour
    vous connecter à votre serveur flexible PostgreSQL.

a=()

for i in {a..z} {A..Z} {0..9};

do

a\[$RANDOM\]=$i

done

ADMIN_PASSWORD=$(IFS=; echo "${a\[\*\]::18}")

echo "Your randomly generated PostgreSQL admin user's password is:"

echo $ADMIN_PASSWORD

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.jpeg)

### Tâche 1 : Affecter un contributeur Cognitive Services

1.  Ouvrez un nouvel onglet et allez dans
    \`\`**https://portal.azure.com\`\`**. Connectez-vous à l'aide de vos
    identifiants Azure, puis cliquez sur la vignette **Subscription**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image7.jpeg)

2.  Cliquez sur subscription name.

![](./media/image8.png)

3.  Cliquez sur Contrôle d'accès (IAM) dans le menu de navigation de
    gauche. Cliquez sur **Add**, puis sélectionnez **Add role
    assignment.**

![](./media/image9.png)

4.  Recherchez \`\`Cognitive Services Contributor\`\` et
    sélectionnez-le, puis cliquez sur le bouton **Next**.

![Capture d'écran d'une attribution de service Description générée
automatiquement](./media/image10.jpeg)

5.  Sélectionnez **User, group or service principal,** puis cliquez sur
    le lien **select member**. Recherchez votre compte d'abonnement
    Azure et sélectionnez-le. Enfin, cliquez sur le bouton **Select**.

![](./media/image11.png)

6.  Cliquez sur le bouton **Review + assign**.

![](./media/image12.png)

7.  Cliquez à nouveau sur le bouton **Review + assign**.

> ![](./media/image13.png)

### Tâche 2 : Exécuter le script de déploiement Bicep pour provisionner les ressources Azure

1.  Revenez au 1er onglet du portail Azure avec Azure CLI pour exécuter
    un script de déploiement Bicep afin de provisionner des ressources
    Azure dans votre groupe de ressources : Le déploiement prend 3 à 5
    minutes

\`\`cd\`\`

\`\`az deployment group create --resource-group $RG_NAME --template-file
"postgresql-case/Allfiles/Labs/Shared/deploy.bicep" --parameters
restore=false adminLogin=pgAdmin
adminLoginPassword=$ADMIN_PASSWORD\`\`![Une capture d'écran d'un
ordinateur Description générée automatiquement](./media/image14.jpeg)

![Capture d'écran d'ordinateur d'un écran noir Description générée
automatiquement](./media/image15.jpeg)

2.  Le script de déploiement Bicep provisionne les services Azure requis
    pour effectuer cet exercice dans votre groupe de ressources. Les
    ressources déployées incluent un serveur flexible Azure Database
    pour PostgreSQL. Vous pouvez vérifier les ressources de votre groupe
    de ressources.

- **Azure OpenAI,**

- **Azure AI Language service.**

![](./media/image16.png)

3.  Cliquez sur Open AI resource

![](./media/image17.png)

4.  Cliquez sur **Keys and Endpoint** sous **Resource Management** dans
    le menu de navigation de gauche. Notez la clé 1 et le point de
    terminaison pour les utiliser dans la tâche 5

![](./media/image18.png)

5.  Le script Bicep effectue également certaines étapes de
    configuration, telles que l'ajout des extensions azure_ai et vector
    à la liste d'autorisation du serveur PostgreSQL (via le paramètre de
    serveur azure.extensions), la création d'une base de données nommée
    rentals sur le serveur et l'ajout d'un déploiement nommé embedding à
    l'aide du modèle **text-embedding-ada-002** à votre service Azure
    OpenAI.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image19.jpeg)

6.  Le déploiement prend généralement plusieurs minutes. Vous pouvez le
    surveiller à partir de Cloud Shell ou accéder à la page
    **Deployments** du groupe de ressources que vous avez créé ci-dessus
    et y observer la progression du déploiement.

7.  Fermez le volet Cloud Shell une fois le déploiement de vos
    ressources terminé.

### Tâche 3 : Se connecter à votre base de données à l'aide de psql dans Azure Cloud Shell

Dans cette tâche, vous vous connectez à la base de données rentals sur
votre serveur Azure Database pour PostgreSQL à l'aide de l'utilitaire de
ligne de commande psql à partir d'Azure Cloud Shell.

1.  Dans le portail Azure (https://portal.azure.com/), accédez à votre
    serveur flexible Azure Database for PostgreSQL nouvellement créé.

![](./media/image20.png)

1.  Dans la barre latérale, sélectionnez **Server Parameters**.
    Recherchez le **azure.extensions**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image21.png)

2.  Sélectionnez les extensions **Vector** et **AZURE_AI** si ce n'est
    pas déjà fait.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image22.png)

2.  Dans le menu des ressources, sous **Settings**, sélectionnez
    **Databases**, puis Se **Connect** pour la base de données des
    locations.

![](./media/image23.png)

3.  À l'invite « Mot de passe de l'utilisateur pgAdmin » dans Cloud
    Shell, saisissez le mot de passe généré de manière aléatoire pour la
    connexion **pgAdmin**.

Une fois connecté, l'invite psql de la base de données de locations
s'affiche.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image24.jpeg)

4.  Tout au long du reste de cet exercice, vous continuez à travailler
    dans Cloud Shell, il peut donc être utile de développer le volet
    dans la fenêtre de votre navigateur en sélectionnant le bouton
    **Maximize** en haut à droite du volet.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image25.jpeg)

### Tâche 4 : Configurer les extensions

Pour stocker et interroger des vecteurs, et pour générer des
intégrations, vous devez autoriser et activer deux extensions pour Azure
Database pour PostgreSQL Flexible Server : vector et azure_ai.

1.  Rebasculez l'onglet du portail Azure avec Azure CLI et exécutez la
    commande SQL suivante pour activer l'extension vectorielle. Pour des
    instructions détaillées

\`\`CREATE EXTENSION vector;\`\`

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image26.jpeg)

3.  Pour activer l'extension azure_ai, **update and run** la commande
    SQL suivante. Vous aurez besoin du point de terminaison et de la clé
    API pour la ressource Azure OpenAI.

> \`\`CREATE EXTENSION azure_ai;\`\`
>
> \`\`SELECT azure_ai.set_setting('azure_openai.endpoint',
> 'https://\<endpoint\>.openai.azure.com');\`\`

\`\`SELECT azure_ai.set_setting('azure_openai.subscription_key', '\<API
Key\>');\`\`![Une capture d'écran d'un programme informatique
Description générée automatiquement](./media/image27.jpeg)

### Tâche 5 : Remplir la base de données avec des exemples de données

Avant d'explorer l'extension azure_ai, ajoutez quelques tables à la base
de données de locations et remplissez-les avec des exemples de données
afin de disposer d'informations à utiliser lorsque vous examinez les
fonctionnalités de l'extension.

1.  Exécutez les commandes suivantes pour créer les tables d'annonces et
    d'avis afin de stocker les données d'annonces de biens locatifs et
    d'avis clients :

DROP TABLE IF EXISTS listings;

CREATE TABLE listings (

id int,

name varchar(100),

description text,

property_type varchar(25),

room_type varchar(30),

price numeric,

weekly_price numeric

);

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image28.jpeg)

DROP TABLE IF EXISTS reviews;

CREATE TABLE reviews (

id int,

listing_id int,

date date,

comments text

);

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image29.jpeg)

2.  Ensuite, utilisez la commande COPY pour charger des données à partir
    de fichiers CSV dans chaque table que vous avez créée ci-dessus.
    Commencez par exécuter la commande suivante pour remplir le tableau
    des listes :

\`\`\COPY reviews FROM
'postgresql-case/Allfiles/Labs/Shared/reviews.csv' CSV HEADER\`\`

La sortie de la commande doit être COPY 50, indiquant que 50 lignes ont
été écrites dans la table à partir du fichier CSV.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image30.jpeg)

3.  Enfin, exécutez la commande ci-dessous pour charger les avis des
    clients dans la table des avis :

''\COPY reviews FROM 'postgresql-case/Allfiles/Labs/Shared/reviews.csv'
CSV HEADER''

La sortie de la commande doit être COPY 354, indiquant que 354 lignes
ont été écrites dans la table à partir du fichier CSV.

![](./media/image31.jpeg)

4.  Pour réinitialiser vos exemples de données, vous pouvez exécuter des
    DROP TABLE listings et répéter ces étapes.

### Tâche 6 : Créer et stocker des vecteurs d'intégration

Maintenant que nous avons quelques exemples de données, il est temps de
générer et de stocker les vecteurs d'intégration. L'extension azure_ai
facilite l'appel de l'API d'intégration Azure OpenAI.

1.  Ajoutez la colonne vectorielle d'intégration.

Le modèle text-embedding-ada-002 est configuré pour retourner 1 536
dimensions, utilisez-le donc pour la taille de la colonne vectorielle.

\`\`ALTER TABLE listings ADD COLUMN listing_vector vector(1536);\`\`

![Capture d'écran d'ordinateur d'un écran noir Description générée
automatiquement](./media/image32.jpeg)

2.  Générez un vecteur d'incorporation pour la description de chaque
    liste en appelant Azure OpenAI via la fonction create_embeddings
    définie par l'utilisateur, qui est implémentée par l'extension
    azure_ai :

> UPDATE listings SET listing_vector =
> azure_openai.create_embeddings('embedding', description, max_attempts
> =\> 5, retry_delay_ms =\> 500) WHERE listing_vector IS NULL;

Notez que cela peut prendre plusieurs minutes, en fonction du quota
disponible.

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement](./media/image33.png)

### Tâche 7 : Effectuer une requête de recherche sémantique

Maintenant que vous disposez de données de liste augmentées de vecteurs
d'intégration, il est temps d'exécuter une requête de recherche
sémantique. Pour ce faire, récupérez le vecteur d'incorporation de la
chaîne de requête, puis effectuez une recherche cosinus pour trouver les
listes dont les descriptions sont les plus similaires sémantiquement à
la requête.

1.  Utilisez l'intégration dans une recherche en cosinus (\\=\>
    représente l'opération de distance en cosinus), en récupérant les 10
    listes les plus similaires à la requête.

SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;

Vous obtiendrez un résultat similaire à celui-ci. Les résultats peuvent
varier, car il n'est pas garanti que les vecteurs d'intégration soient
déterministes :

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image34.jpeg)

2.  Vous pouvez également projeter la colonne description pour pouvoir
    lire le texte des lignes correspondantes dont les descriptions
    étaient sémantiquement similaires. Par exemple, cette requête
    renvoie la meilleure correspondance :

SELECT id, description FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 1;

Ce qui imprime quelque chose comme :

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image35.jpeg)

Pour comprendre intuitivement la recherche sémantique, observez que la
description ne contient pas réellement les termes « brillant » ou «
naturel ». Mais il met en évidence « l'été » et « la lumière du
soleil », « les fenêtres » et une « fenêtre de plafond ».

### Tâche 8 : Vérifiez votre travail

Après avoir effectué les étapes ci-dessus, le tableau des annonces
contient des exemples de données provenant des données ouvertes Airbnb
de Seattle sur Kaggle. Les listes ont été complétées par des vecteurs
d'intégration pour exécuter des recherches sémantiques.

1.  Vérifiez que le tableau des offres comporte quatre colonnes : id,
    name, description, listing_vector.

'\`\`\d listings\`\`

Il devrait imprimer quelque chose comme :

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image36.jpeg)

2.  Vérifiez qu'au moins une ligne comporte une colonne listing_vector
    remplie.

\`\`SELECT COUNT(\*) \> 0 FROM listings WHERE listing_vector IS NOT
NULL;\`\`

Le résultat doit afficher un t, c'est-à-dire vrai. Une indication qu'il
existe au moins une ligne avec des intégrations de sa colonne de
description correspondante :

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image37.jpeg)

3.  Vérifiez que le vecteur d'incorporation a 1536 dimensions :

\`\`SELECT vector_dims(listing_vector) FROM listings WHERE
listing_vector IS NOT NULL LIMIT 1;\`\`

Docile:

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image38.jpeg)

4.  Vérifiez que les recherches sémantiques renvoient des résultats.

Utilisez l'intégration dans une recherche en cosinus, en récupérant les
10 listes les plus similaires à la requête.

\`\`SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;\`\`

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image39.jpeg)

5.  Revenez sur la même page pour passer à la tâche suivante.

## Exercice 2 - Création d'une fonction de recherche pour un système de recommandation

Encapsulons la logique d'intégration vectorielle et les appels d'API
dans une fonction. Dans cet exercice, vous allez installer les
extensions vector et azure_ai dans un serveur flexible Azure Database
pour PostgreSQL et explorer les fonctionnalités de l'extension pour
l'intégration d'[Azure
OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/overview)
dans votre base de données.

### Tâche 1 : Créer une fonction de recherche pour un système de recommandation

Construisons un système de recommandation à l'aide de la recherche
sémantique. Le système recommandera plusieurs annonces sur la base d'un
exemple d'annonce fourni. L'échantillon peut provenir de la liste que
l'utilisateur consulte ou de ses préférences. Nous allons implémenter le
système en tant que fonction PostgreSQL en exploitant l'extension
azure_openai.

À la fin de cet exercice, vous aurez défini une fonction
recommend_listing qui fournit au plus les listes numResults les plus
similaires à sampleListingId fournie. Vous pouvez utiliser ces données
pour générer de nouvelles opportunités, par exemple en rejoignant des
offres recommandées par rapport à des offres à prix réduit.

Déployer des ressources dans votre abonnement Azure

Cette étape vous guide tout au long de l'utilisation des commandes Azure
CLI à partir d'Azure Cloud Shell pour créer un groupe de ressources et
exécuter un script Bicep afin de déployer les services Azure nécessaires
à la réalisation de cet exercice dans votre abonnement Azure.

**Remarque :** Si vous effectuez plusieurs modules dans ce parcours
d'apprentissage, vous pouvez partager l'environnement Azure entre eux.
Dans ce cas, vous n'avez besoin d'effectuer cette étape de déploiement
de ressource qu'une seule fois.

### Tâche 2 : Créer la fonction de recommandation

1.  La fonction de recommandation prend un sampleListingId et renvoie
    les listes numResults les plus similaires. Pour ce faire, il crée un
    incorporation du nom et de la description de l'exemple de liste et
    exécute une recherche sémantique de ce vecteur de requête par
    rapport aux plongements de la liste.

> CREATE FUNCTION
>
> recommend_listing(sampleListingId int, numResults int)
>
> RETURNS TABLE(
>
> out_listingName text,
>
> out_listingDescription text,
>
> out_score real)
>
> AS $$
>
> DECLARE
>
> queryEmbedding vector(1536);
>
> sampleListingText text;
>
> BEGIN
>
> sampleListingText := (
>
> SELECT
>
> name || ' ' || description
>
> FROM
>
> listings WHERE id = sampleListingId
>
> );
>
> queryEmbedding := (
>
> azure_openai.create_embeddings('embedding', sampleListingText,
> max_attempts =\> 5, retry_delay_ms =\> 500)
>
> );
>
> RETURN QUERY
>
> SELECT
>
> name::text,
>
> description,
>
> -- cosine distance:
>
> (listings.listing_vector \<=\> queryEmbedding)::real AS score
>
> FROM
>
> listings
>
> ORDER BY score ASC LIMIT numResults;
>
> END $$
>
> LANGUAGE plpgsql;

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image40.jpeg)

### Tâche 3 : Interroger la fonction de recommandation

1.  Pour interroger la fonction de recommandation, transmettez-lui un ID
    d'inscription et le nombre de recommandations qu'elle doit faire.

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommendations
closest to a listing

Le résultat sera quelque chose comme :

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image41.jpeg)

2.  Pour voir le runtime de la fonction, assurez-vous que
    **track_functions** est activé dans la section **Server Parameters**
    sur le portail Azure (vous pouvez utiliser PL ou ALL) :

![](./media/image42.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image43.png)

### Tâche 4 : Vérifiez votre travail

1.  Assurez-vous que la fonction existe avec la bonne signature :

\`\`\df recommend_listing\`\`  
Vous devriez voir ce qui suit :

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image44.jpeg)

2.  Assurez-vous que vous pouvez l'interroger à l'aide de la requête
    suivante :

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommendations
closest to a listing

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image45.jpeg)

### Tâche 5 : Nettoyer

Une fois cet exercice terminé, supprimez les ressources Azure que vous
avez créées. La capacité configurée vous est facturée, et non la
quantité utilisée par la base de données. Suivez ces instructions pour
supprimer votre groupe de ressources et toutes les ressources que vous
avez créées pour cet atelier.

1.  Sur la page d'accueil, recherchez **Azure Open AI** et
    sélectionnez-le.

![](./media/image46.png)

2.  Sélectionnez la ressource Ouvrir l'IA, puis cliquez sur **Delete**.

![](./media/image47.png)

3.  Tapez **delete** dans la zone de texte, puis cliquez sur **\*\***
    **Delete.** Confirmez la suppression.

![](./media/image48.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image49.png)

4.  Cliquez sur **Manage deleted resources**, sélectionnez la ressource,
    puis cliquez sur le bouton **Purger** comme indiqué dans l'image
    ci-dessous.

![](./media/image50.png)

5.  Confirmez la purge en cliquant sur **Yes**.

![](./media/image51.png)

6.  Sur la page d'accueil, sélectionnez **Resource groups** sous
    Services Azure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image52.jpeg)

7.  Cliquez sur Nom du groupe de ressources.

![](./media/image53.png)

8.  Sur la page **Overview** de votre groupe de ressources, sélectionnez
    **all the resource**, puis cliquez sur **Delete. NE PAS supprimer le
    groupe de ressources (RESOURCE Group).**

> ![](./media/image54.png)

9.  Tapez **Delete** et cliquez sur Supprimer. Confirmez la suppression
    de la ressource en cliquant sur le bouton **Delete**.

![](./media/image55.png)

**Résumé** :

Vous avez appris à utiliser la recherche sémantique dans Azure Database
pour PostgreSQL Flexible Server afin d'interroger à l'aide
d'intégrations générées par Azure OpenAI. Pour ce faire, vous avez
effectué les opérations suivantes :

- Activation des extensions vectorielle et azure_ai.

- Création de colonnes vectorielles pour stocker les plongements.

- Génération et stockage des plongements.

- Interrogation de la base de données à l'aide d'un vecteur de requête.
