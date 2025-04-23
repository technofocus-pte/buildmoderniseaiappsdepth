# Cas d'usage 05 - Déploiement d'une application web Python Restaurant pilotée par les données avec Azure Database pour PostgreSQL

**Objectif :**

Ce cas d'usage déploie une application web Python à l'aide de
l'infrastructure Flask et du service de base de données relationnelle
Azure Database pour PostgreSQL. L'application Flask est hébergée dans un
Azure App Service entièrement managé. Cette application est conçue pour
être exécutée localement, puis déployée sur Azure

![Schéma d'un plan de service Description générée
automatiquement](./media/image1.jpeg)

Vous allez déployer une application web Python pilotée par les données
(**Django** ou **Flask**) sur **Azure App Service** avec le service de
base de données relationnelle **Azure Database pour PostgreSQL**. Azure
App Service prend en charge Python dans un environnement de serveur
Linux.

**Principales technologies utilisées** : Java 17, Azure Database pour
PostgreSQL

**Durée estimée** -- 45 minutes

**Type de Lab :** Dirigé par un instructeur

**Pré-requis :**

Compte GitHub : vous devez disposer de vos propres identifiants de
connexion GitHub. Si vous n'en avez pas, créez-en un d'ici -
**https://github.com/signup?user_email=&source=form-home-signupobjectives**

Le **requirements.txt** dispose des packages suivants, tous utilisés par
une application Flask typique basée sur les données :

[TABLE]

### Tâche 1 : Enregistrer le prestataire de services

1.  Ouvrez un navigateur, accédez à <https://portal.azure.com> et
    connectez-vous avec votre compte Cloud Slice disponible dans
    l'onglet Ressource de votre machine virtuelle.

> ![](./media/image2.png)

2.  Sur la page d'accueil du portail Azure, cliquez sur la vignette
    **Resource groups**.

![](./media/image3.png)

3.  Copiez le nom du groupe de ressources et enregistrez-le dans le
    bloc-notes pour utiliser la tâche suivante afin de déployer les
    ressources requises dans ce groupe de ressources (resources group).

![](./media/image4.png)

4.  En haut de la navigation, cliquez sur Accueil.

![](./media/image5.png)

5.  Cliquez sur la tuile **Subscriptions.**

![](./media/image6.png)

6.  Cliquez sur le nom de l'abonnement (subscription).

![](./media/image7.png)

7.  Développez Paramètres dans le menu de navigation de gauche. Cliquez
    sur **Resource providers**, entrez Microsoft.AlertsManagement et
    sélectionnez-le, puis cliquez sur **Register**.

> ![](./media/image8.png)
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image9.png)

### Tâche 2 : Créer Github Codespace pour lancer le modèle Azure Developer CLI

Ce cas d'usage dispose d'une configuration de conteneur de
développement, ce qui facilite le développement d'applications
localement, leur déploiement sur Azure et leur surveillance. Nous
utilisons des modèles CLI de développement Azure pour déployer des
applications

1.  Ouvrez un navigateur et allez dans \`\`**https:\\github.com\`\`** et
    connectez-vous avec votre compte Github.

2.  Forkez ce dépôt
    https://github.com/technofocus-pte/msdocs-flask-postgresql-sample-app à
    votre compte en cliquant sur **Fork** comme indiqué dans l'image
    ci-dessous.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image10.jpeg)

3.  Entrez le nom unique, puis cliquez sur **Create repo**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image11.jpeg)

4.  À partir de la racine du référentiel de votre duplication,
    sélectionnez **Code** \> **Codespaces** \> **+**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image12.jpeg)

5.  Attendez que l'espace de travail soit configuré.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image13.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image14.jpeg)

6.  Dans le terminal codespace, exécutez les commandes suivantes :

> \# Install requirements

\`\`python3 -m pip install -r requirements.txt\`\`

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image15.jpeg)

7.  Exécuter la commande below pour créer une variable d'environnement

> \# Create .env avec des variables d'environnement

\`\`cp .env.sample.devcontainer .env\`\`

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image16.jpeg)

8.  Exécuter la commande below pour la migration des données

> \# Run database migrations

\`\`python3 -m flask db upgrade\`\`

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image17.jpeg)

9.  Exécutez la commande below pour

> \# Start the development server

\`\`python3 -m flask run\`\`

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image18.jpeg)

10. Lorsque le message Votre application exécutée sur le port est
    disponible s'affiche, cliquez sur **Open in Browser**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image18.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image19.jpeg)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image20.jpeg)

11. Cliquez sur le bouton **Add new restaurant**.

![Un écran blanc avec du texte noir Description générée
automatiquement](./media/image21.jpeg)

12. Entrez les détails ci-dessous et cliquez sur le bouton **Submit**.

Name : \`\`**Contoso Rica\`\`**

Street Adress - \`\`**3A ,8th cross, Ferns street , Singapore\`\`**

Description - \`\`**This is a medium to high priced restaurant in the
city shopping center\`\`**

![Une capture d'écran d'un restaurant Description générée
automatiquement](./media/image22.jpeg)

13. Cliquez sur le bouton **Add new** review.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image23.jpeg)

14. Entrez votre avis, puis cliquez sur le bouton. **Save changes**

**Your name : your name**

**Rating : your rating**

''C'est un restaurant à prix moyen à élevé dans le centre commercial de
la ville. Le service était un peu déroutant car nous avions au moins 6
serveurs qui venaient nous demander des choses. La nourriture a mis un
certain temps à venir. Nous avions 2 menus : un indien et un
thaïlandais. Le thaï est 30 % moins cher, nous avons donc opté pour des
amuse-gueules et du curry rouge thaïlandais. La nourriture a pris un
certain temps mais cela en valait la peine. C'était délicieux et très
bien préparé. Dans l'ensemble, c'est un bon repas.’’

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image24.jpeg)

![Une carte blanche avec du texte noir Description générée
automatiquement](./media/image25.jpeg)

15. Ajoutez quelques avis supplémentaires et un nouveau restaurant avec
    des commentaires.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image26.jpeg)

### Tâche 3 : Approvisionner la ressource requise dans Azure.

Ce projet est conçu pour fonctionner correctement avec l'interface de
ligne de commande Azure Developer, ce qui facilite le développement
d'applications localement, leur déploiement sur Azure et leur
surveillance.

1.  Revenez à l'onglet de l'espace de code Github, exécutez la commande
    ci-dessous pour initialiser un nouvel environnement azd :

\`\`azd init\`\`![](./media/image27.jpeg)

2.  Il vous demandera de fournir un nom d'environnement (par exemple
    **flask-app**XXXX (XXXX peut être un numéro unique)), qui sera
    ensuite utilisé dans le nom des ressources déployées.

![](./media/image28.jpeg)

3.  Connectez-vous si nécessaire \`\`**azd auth login\`\`**.copiez le
    code et appuyez sur Entrée.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image29.jpeg)

4.  Entrez le code, puis connectez-vous avec vos informations
    d'identification Azure.

![Une capture d'écran d'une erreur informatique Description générée
automatiquement](./media/image30.jpeg)

![](./media/image31.jpeg)

![Une capture d'écran d'une erreur informatique Description générée
automatiquement](./media/image32.jpeg)

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image33.jpeg)

5.  Revenez à l'onglet Codespace Gtihub et exécutez la commande
    ci-dessous pour provisionner et déployer toutes les ressources. Il
    vous invite à sélectionner votre abonnement Azure. Entrez **1** pour
    sélectionner votre abonnement et appuyez sur Entrée.

**\`\`azd provision\`\`**![Capture d'écran d'un code informatique
Description générée automatiquement](./media/image34.png)

6.  Sélectionnez l'emplacement **WestUS/eastus**. Ensuite, il
    provisionnera les ressources de votre compte et déploiera le dernier
    code. Si vous obtenez une erreur lors du déploiement, il peut être
    utile de modifier l'emplacement (par exemple, "westus"), car il peut
    y avoir des contraintes de disponibilité pour certaines ressources.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image35.png)

7.  Entrez le nom du groupe de ressources à partir de votre portail
    Azure (copié dans la tâche précédente) et appuyez sur Entrée.

![](./media/image36.png)

8.  Le déploiement prend **20 à 30 minutes**. Vous pouvez également
    vérifier l'état du déploiement à partir du lien généré ou **Azure
    portal-\> Resource group-\> Deployments**.

![](./media/image37.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image38.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image39.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image40.png)

### Tâche 4 : Déployer l'application depuis Github

1.  Exécutez la commande below pour définir la variable d'environnement
    du groupe de ressources.

\`\`azd env set AZURE_RESOURCE_GROUP {Name of existing resource group}
\`\`

Remarque : Remplacez {Nom du groupe de ressources existant} par le nom
de votre groupe de ressources disponible dans la section Ressources de
votre machine virtuelle.

![](./media/image41.png)

2.  Exécutez la commande below pour déployer toutes les ressources et
    attendez que le déploiement se termine correctement.

\`\`azd deploy\`\`![](./media/image42.png)

3.  Cliquez sur l'URL du point de terminaison généré

![](./media/image43.png)

4.  Cliquez sur **Open** pour ouvrir le site Web externe.

![](./media/image44.png)

5.  L'application s'ouvre dans un nouvel onglet.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image45.png)

### Tâche 5 : Diffuser les journaux de diagnostic

Azure App Service capture tous les messages générés dans la console pour
vous aider à diagnostiquer les problèmes liés à votre application.
L'application inclut des instructions print() pour démontrer cette
capacité, comme indiqué ci-dessous.

@app.route('/', methods=\['GET'\])

def index():

print('Request for index page received')

restaurants = Restaurant.query.all()

return render_template('index.html', restaurants=restaurants)

1.  Revenez à **Azure portal- \> Resource group**, puis cliquez sur
    **App service**.

![](./media/image46.png)

2.  Dans la page App Service. Dans le menu de gauche, sélectionnez
    **Monitoring -\>** **App Service logs.**

![](./media/image47.png)

3.  Sous **Application logging**, assurez-vous que **File System** est
    sélectionnée. Sélectionnez-le si nécessaire. Dans le menu supérieur,
    sélectionnez **Save**.

![](./media/image48.png)

4.  Dans le menu de gauche, sélectionnez **Log stream**. Vous voyez les
    journaux de votre application, y compris les journaux de plate-forme
    et les journaux à l'intérieur du conteneur.

![](./media/image49.png)

### Tâche 6 : Nettoyer les ressources dans Github.

1.  Revenez à Github, cliquez sur **repo -\> Code -\> Codespaces.**
    Sélectionnez la bonne branche

![](./media/image50.png)

2.  Sélectionnez la branche et cliquez sur **Delete**.

![](./media/image51.png)

3.  Cliquez sur **Delete**.

![](./media/image52.png)

4.  Revenez au **Azure portal -\> Resource group.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image53.png)

5.  Sélectionnez toutes les ressources puis cliquez sur **Delete** (NE
    supprimez PAS le groupe de ressources (resource group))

![](./media/image54.png)

6.  Entrez \`\`**Delete**\`\` puis cliquez sur **Delete**.

![](./media/image55.png)

7.  Cliquez sur **Delete** pour confirmer la suppression.

![](./media/image56.png)
