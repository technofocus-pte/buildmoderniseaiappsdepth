# Caso de uso 07 – Habilitar semantic search en Azure Database for PostgreSQL flexible server para usar Azure OpenAI para generar los vector embeddings.

**Objetivo**:

En el caso de uso, implementa el semantic search para generar y
almacenar los embeddings, instale el vector y azure_ai extensions en un
Azure Database for PostgreSQL flexible server, y aplique las extensiones
para almacenar los embedding vectors generados por Azure OpenAI

**Tecnologías clave** -- Azure OpenAI, Azure Database for PostgreSQL,
Azure AI extension

**Duración estimada** -- 45 minutos

**Tipo del laboratorio:** dirigido por el instructor

## Ejercicio 1: Genere los vector embeddings con Azure OpenAI

Para realizar los semantic searches, debe generar los embedding vectors
desde un modelo, almacenarlos en un vector database, y hacer query a los
embeddings. Creará un database, poblarlo con datos de muestra, y
ejecutarlos con los semantic searches con los listings.

Por el fin de este ejercicio, tendrá un Azure Database for PostgreSQL
flexible server instance con el vector y azure_ai extensions
habilitados. Generará los embeddings para la tabla de listings de
Seattle Airbnb Open Data dataset. También ejecutará los semantic
searches contra esos listings al generar un embedding vector de query y
realizar un vector cosine distance search.

1.  Abra un anvegadpr web y navegue a \`\`https:\\portal.azure.com/\`\`e
    inicie sesión con sus credenciales Azure.

2.  Seleccione el ícono **Cloud Shell** en el toolbar de Azure portal
    para abrir un nuevo panel Cloud Shell en la parte inferior de la
    ventana del navegador. Seleccione **Bash**.

![A screenshot of a computer Description automatically
generated](./media/image1.jpeg)

3.  Seleccione el botón **No storage account required** radio,
    seleccione la suscripción y haga clic en **Apply**.

![](./media/image2.png)

4.  En el prompt de Cloud Shell, ejecute el siguiente command para
    duplicar el proyecto

\`\`git clone https://github.com/technofocus-pte/postgresql-case\`\`

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

5.  Navegue a la carpeta del proyecto.

**\`\`cd postgresql-case\`\`**

![A screenshot of a computer Description automatically
generated](./media/image4.jpeg)

6.  A continuación, ejecutará tres commands para definir las variables
    para reducir la tecla redundante a la hora de usar Azure CLI
    commands para crear los recursos Azure. Las variables representan el
    nombre para asignar a su resource group (RG_NAME), la región Azure
    (REGION) en la cual se implementarán los recursos y una contraseña
    generada de forma aleatoria para el inicio de sesión de PostgreSQL
    administrator (ADMIN_PASSWORD).

7.  En el primer command, la región asignada a la variable
    correspondiente es eastus o westus, **\[pero la puede reemplazar con
    una ubicación de su elección.\]{.mark}.** Sin embargo, si reemplaza
    la predeterminada, debe seleccionar otra \[Azure region que admite
    el resumen abstractivo\]{.underline} para asegurar que puede
    completar todas las tareas en este módulo, en este learning path.

\`\`REGION=westus\`\`

8.  El siguiente command asigna el nombre de resource group existente
    para ser utilizado para su resource group que alojará todos los
    recursos utilizados en este ejercicio.

> \`\`RG_NAME=Your existing resource group name\`\`

![](./media/image5.png)

9.  El command final genera aleatoriamente una contraseña para su login
    de PostgreSQL admin. **Asegure que lo copie** a un lugar seguro para
    usarlo más tarde para conectarse a su PostgreSQL flexible server.

> a=()
>
> for i in {a..z} {A..Z} {0..9};
>
> do
>
> a\[$RANDOM\]=$i
>
> done
>
> ADMIN_PASSWORD=$(IFS=; echo "${a\[\*\]::18}")
>
> echo "Your randomly generated PostgreSQL admin user's password is:"

echo $ADMIN_PASSWORD

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

### Tarea 1: Asigne Cognitive Services Contributor

1.  Abra una nueva pestaña y vaya
    a \`\`**https://portal.azure.com\`\`**. Inicie sesión en sus
    credenciales Azure y luego haga clic en **Subscription**.

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

2.  Haga clic en subscription name .

![](./media/image8.png)

3.  Haga clic en Access control (IAM) desde el menú de navegación. Haga
    clic en **Add** y seleccione **Add role assignment.**

![](./media/image9.png)

4.  Busque \`\`Cognitive Services Contributor\`\` y seleccione y haga
    clic en el botón **Next**.

![A screenshot of a service assignment Description automatically
generated](./media/image10.jpeg)

5.  Seleccione **User, group or service principal** y haga clic en el
    enlace **select member**. Busque su cuenta de Azure subscription y
    selecciónelo. Por fin, haga clic en el botón **Select**.

![](./media/image11.png)

6.  Haga clic en el botón **Review + assign**.

![](./media/image12.png)

7.  Haga clic en el botón **Review + assign** de nuevo.

> ![](./media/image13.png)

### Tarea 2: Ejecute Bicep deployment script para aprovisionar Azure resources

1.  Cambie a la primera pestaña Azure portal con Azure CLI para ejecutar
    a Bicep deployment script para aprovisionar Azure resources en su
    resource group: la implementación lleva 3 – 5 minutos

\`\`cd\`\`

\`\`az deployment group create --resource-group $RG_NAME --template-file
"postgresql-case/Allfiles/Labs/Shared/deploy.bicep" --parameters
restore=false adminLogin=pgAdmin adminLoginPassword=$ADMIN_PASSWORD\`\`

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

![A computer screen shot of a black screen Description automatically
generated](./media/image15.jpeg)

2.  El Bicep deployment script aprovisiona los servicios Azure
    requeridos para completar este ejercicio en su resource group. Los
    recursos implementados incluyen un Azure Database for PostgreSQL -
    Flexible Server. Puede revisar los recursos en su resource group.

- **Azure OpenAI,**

- **Azure AI Language service.**

![](./media/image16.png)

3.  Haga clic en Open AI resource

![](./media/image17.png)

4.  Haga clic en **Keys and Endpoint** en **Resource Management** desde
    el menú de navegación izquierdo. Haga una nota de Key 1 y endpoint
    para usarla en tarea 5

![](./media/image18.png)

5.  El Bicep script también realiza unos pasos de configuración, como
    añadir azure_ai y vector extensions al *allowlist de* PostgreSQL
    server (a través de azure.extensions server parameter), creando un
    database llamado rentals en el servidor, y añadiendo una
    implementación llamada embedding mediante el modelo
    **text-embedding-ada-002** al Azure OpenAI service.

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

6.  La implementación normalmente tarda unos minutos en completar. Puede
    monitorearla desde Cloud Shell o navegarla a la página
    **Deployments** para el resource group que creó antes y observe el
    progreso de la implementación ahí.

7.  Cierre el panel de Cloud Shell una vez más cuando la implementación
    del recurso se complete.

### Tarea 3 : Conéctese a su database mediante psql en el Azure Cloud Shell

En esta tarea, se conecta a rentals database en su Azure Database for
PostgreSQL server mediante el psql command-line utility desde Azure
Cloud Shell.

1.  En el Azure portal (https://portal.azure.com/), navegue al nuevo
    Azure Database for PostgreSQL - Flexible Server.

![](./media/image20.png)

1.  En la barra lateral, seleccione **Server Parameters**.Search para el
    parámetro **azure.extensions**.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

2.  Seleccione las extensiones **Vector** y **AZURE_AI** si todavía no
    están seleccionadas.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  En el menú de recursos, en **Settings**,
    seleccione **Databases,** seleccione **Connect** para el rentals
    database.

![](./media/image23.png)

3.  En el prompt "Password for user pgAdmin" en Cloud Shell, introduzca
    de forma aleatoria una contraseña para el inicio de sesión
    de **pgAdmin**.

Una vez dentro, se muestra el psql prompt para rentals database.

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

4.  A lo largo del ejercicio restante, continuamos trabajando en Cloud
    Shell, para que pueda resultar útil expandir el panel dentro de su
    ventana del navegador al seleccionar el botón **Maximize** en la
    esquina superior derecha en el panel.

![A screenshot of a computer Description automatically
generated](./media/image25.jpeg)

### Tarea 4: Configure las extensiones

Para almacenar y hacer query a los vectors, y para generar los
embeddings, necesita el allow-list y también habilitar dos extensiones
para Azure Database for PostgreSQL Flexible Server: vector y azure_ai.

1.  Cambie a Azure portal tab con Azure cli y ejecute el siguiente SQL
    command para habilitar el vector extension. Para instrucciones más
    detalladas:

\`\`CREATE EXTENSION vector;\`\`

![A screenshot of a computer Description automatically
generated](./media/image26.jpeg)

3.  Para habilitar azure_ai extension, **actualice y ejecute** el
    siguiente SQL command. Necesita el endpoint y API key para el
    recurso Azure OpenAI.

> \`\`CREATE EXTENSION azure_ai;\`\`
>
> \`\`SELECT azure_ai.set_setting('azure_openai.endpoint',
> 'https://\<endpoint\>.openai.azure.com');\`\`

\`\`SELECT azure_ai.set_setting('azure_openai.subscription_key', '\<API
Key\>');\`\`

![A screenshot of a computer program Description automatically
generated](./media/image27.jpeg)

### Tarea 5 : Poble el database con datos de muestra

Antes de explorar la extensión azure_ai, agregue un par de tablas a
rentals database y poblarlas con datos de muestra, para que tendrá
información para trabajar a la hora de revisar la funcionalidad de la
extensión.

1.  Ejecute los siguientes commands para crear los listings y revisar
    tablas para almacenar los listings de propiedades de alquiler y
    datos de reseñas de clientes:

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

![A screenshot of a computer Description automatically
generated](./media/image28.jpeg)

DROP TABLE IF EXISTS reviews;

CREATE TABLE reviews (

id int,

listing_id int,

date date,

comments text

);

![A screenshot of a computer Description automatically
generated](./media/image29.jpeg)

2.  A continuación, use el COPY command para cargar datos desde archivos
    CSV en cada tabla que creó antes. Empiece con ejecutar el siguiente
    command para poblar la tabla de listado:

\`\`\COPY listings FROM
'postgresql-case/Allfiles/Labs/Shared/listings.csv' CSV HEADER\`\`

El command output debe ser COPY 50, indicando que se escribieron 50
filas en la tabla desde el archivo CSV.

![A screenshot of a computer program Description automatically
generated](./media/image30.jpeg)

3.  Finalmente, ejecute el siguiente command para cargar las reseñas de
    cliente en reviews table:

\`\`\COPY reviews FROM
'postgresql-case/Allfiles/Labs/Shared/reviews.csv' CSV HEADER\`\`

El command output debe ser COPY 354, indicando que se escribieron 354
filas en la tabla desde el archivo CSV.

![](./media/image31.jpeg)

4.  Para reestablecer sus datos de muestra, puede ejecutar los DROP
    TABLE listings, y repetir estos pasos.

### Tarea 6 : Cree y almecene los embedding vectors

Ahora que tenemos unos datos de muestra, es la hora de generar y
almacenar los embedding vectors. La extensión azure_ai hace fácil la
llamada the Azure OpenAI embedding API.

1.  Agregue la columna embedding vector.

El modelo text-embedding-ada-002 está configurado para devolver 1,536
dimensiones, por eso úselo para vector column size.

\`\`ALTER TABLE listings ADD COLUMN listing_vector vector(1536);\`\`

![A computer screen shot of a black screen Description automatically
generated](./media/image32.jpeg)

2.  Genere un embedding vector para la descripción de cada listado al
    llamar Azure OpenAI a través de la función create_embeddings
    user-defined, lo cual está implementado por la extensión azure_ai:

> UPDATE listings SET listing_vector =
> azure_openai.create_embeddings('embedding', description, max_attempts
> =\> 5, retry_delay_ms =\> 500) WHERE listing_vector IS NULL;

Note que esto puede tardar unos minutos, en función de la cuota
disponible.

![A screenshot of a computer screen Description automatically
generated](./media/image33.png)

### Tarea 7: Realizar un semantic search query

Ahora que tiene los datos de listings aumentado con embedding vectors,
es la hora de ejecutar un semantic search query. Para hacerlo, obtenga
el query string embedding vector, y realice un cosine search para
encontrar los listings cuyas descripciones son más similares
semánticamente al query.

1.  Use el embedding en un cosine search (\\=\> representa cosine
    distance operation), buscando 10 listings más similares al query.

SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;

Obtendrá un resultado similar a esto. Los resultados pueden variar, como
no se garantiza que los embedding vectors sean determinísticos:

![A screenshot of a computer Description automatically
generated](./media/image34.jpeg)

2.  También puede proyectar la columna de descripción para poder leer el
    texto de las filas coincidentes cuyas descripciones eran similar
    semánticamente. Por ejemplo, este query devuelve la mejor
    coincidencia:

SELECT id, description FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 1;

Lo que imprime algo así:

![A screenshot of a computer Description automatically
generated](./media/image35.jpeg)

Para entender semantic search de forma intuitiva, observe que la
descripción no contiene los términos "bright" o "natural." Pero sí que
destaca "summer" y "sunlight," "windows," y un "ceiling window."

### Tarea 8: Verifique su trabajo

Después de realizar los pasos, la tabla de listings contiene los datos
de muestrade Seattle Airbnb Open Data en Kaggle. Los listings fueron
aumentados con embedding vectors para ejecutar semantic searches.

1.  Confirme la tabla de listings tiene cuatro columnas: id, name,
    description, y listing_vector.

\`\`\d listings\`\`

Debe imprime algo así:

![A screenshot of a computer Description automatically
generated](./media/image36.jpeg)

2.  Confirme que al menos una fila tiene listing_vector column poblada.

\`\`SELECT COUNT(\*) \> 0 FROM listings WHERE listing_vector IS NOT
NULL;\`\`

El resultado debe mostrar un t, significa true. Una indicación que hay
al menos una fila con embeddings de su columna de descripción
correspondiente:

![A screen shot of a computer Description automatically
generated](./media/image37.jpeg)

3.  Confirme el embedding vector tiene 1536 dimensiones:

\`\`SELECT vector_dims(listing_vector) FROM listings WHERE
listing_vector IS NOT NULL LIMIT 1;\`\`

Produciendo:

![A screen shot of a computer Description automatically
generated](./media/image38.jpeg)

4.  Confirme que semantic searches devuelven resultados.

Use el embedding en un cosine search, buscando los 10 listings más
similares al query.

\`\`SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;\`\`

![A screenshot of a computer program Description automatically
generated](./media/image39.jpeg)

5.  Quédese en la misma página para seguir con la próxima tarea.

## Ejercicio 2 - Cree un search function para un sistema de recomendación

Llegamos al final de vector embedding logic y API calls en una función.
En este ejercicio, va a instalar el vector y extensiones azure_ai en un
Azure Database for PostgreSQL flexible server y explorar las capacidades
de extensión para integrar [Azure
OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/overview) en
su database.

### Tarea 1: Cree un search function para el sistema de recomendación

Construimos un sistema de recomendaciones mediante semantic search. El
sistema recomendará varios listings basados en un listing de muestra. La
muestra puede pertenecer a la lista a la que se refiere el usuario o
según su preferencia. Impementamos el sistema como una función
PostgreSQL aprovechando la extensión azure_openai.

Para los finales de este ejercicio, habrá definido una función
recommend_listing que proporciona at most numResults listings most
similar to the supplied sampleListingId. Puede usar estos datos para
impulsar nuevas oportunidades, como juntar los listings recomendados con
los listings con descuentos.

Implemente los recursos en su Azure subscription

Este paso le guía para usar los Azure CLI commands del Azure Cloud Shell
para crear un resource group y ejecute un Bicep script para implementar
los Azure services necesarios para completar este ejercicio en su Azure
subscription.

**Ojo:** Si está en múltiples módulos de este learning path, puede
compartir el entorno de Azure entre ellos. En este caso, necesita
completar esta implementación del recursosolo una vez.

### Tarea 2 : Cree la función recomendada

1.  La recommendation function toma un sample ListingId y devuelve otros
    listings más similar numResults. Para hacerlo, crea un embedding del
    nombre y la descripción del listing de muestra y realiza un semantic
    search de aquel query vector contra los listing embeddings.

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

![A screenshot of a computer Description automatically
generated](./media/image40.jpeg)

### Tarea 3 : Hacer query a la recommendation function

1.  Para hacer query a la recommendation function, páselo a un listing
    ID y un número de recomendaciones que debe realizar.

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommendations
closest to a listing

El resultado se verá así:

![A screenshot of a computer Description automatically
generated](./media/image41.jpeg)

2.  Para ver el function runtime, asegure que está
    habilitado **track_functions** en la sección **Server
    Parameters** en Azure Portal (puede usar PL o ALL):

![](./media/image42.png)

![A screenshot of a computer Description automatically
generated](./media/image43.png)

### Tarea 4 : Verifique su trabajo

1.  Asegure que la función existe con la firma correcta:

\`\`\df recommend_listing\`\`

Debe ver lo siguiente:

![A screenshot of a computer Description automatically
generated](./media/image44.jpeg)

2.  Asegure que puede hacer query mediante el siguiente query:

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommendations
closest to a listing

![A screenshot of a computer Description automatically
generated](./media/image45.jpeg)

### Tarea 5: Limpieza

Una vez que haya completado este ejercicio, elimine los Azure resources
creados. Se le cobra por el configured capacity, y no por el uso de
database. Siga estas instrucciones para eliminar su resource group y
todos los recursos que creó para este laboratorio.

1.  En Home page, busque **Azure Open AI** y elíjalo.

![](./media/image46.png)

2.  Seleccione el Open AI resource y haga clic en **Delete** .

![](./media/image47.png)

3.  Tecle **delete **en el text box y haga clic en
    **\*\*Delete.** Confirme la eliminación.

![](./media/image48.png)

![A screenshot of a computer Description automatically
generated](./media/image49.png)

4.  Haga clic en **Manage deleted resources**, seleccione el recurso y
    haga clic en **Purge** como se ve aquí.

![](./media/image50.png)

5.  Confirme el purge al hacer clic en **Yes**.

![](./media/image51.png)

6.  En el home page, seleccione **Resource groups** en Azure services.

![A screenshot of a computer Description automatically
generated](./media/image52.jpeg)

7.  Haga clic en Resource group name.

![](./media/image53.png)

8.  En la página **Overview** de su resource group, seleccione **all the
    resource** y luego en **Delete**. **NO ELIMINE el RESOURCE Group.**

> ![](./media/image54.png)

9.  Tecle **Delete** y haga clic en Delete. Confirme la eliminación de
    recursos al hacer clic en el botón **Delete**.

![](./media/image55.png)

**Resumen**:

Ha aprendido cómo usar semantic search en Azure Database for PostgreSQL
Flexible Server para hacer query mediante embeddings generados por Azure
OpenAI. Ha logrado este search al:

- Habilitar el vector y las extensiones azure_ai.

- Crear vector columns para almacenar embeddings.

- Generar y almacenar los embeddings.

- Hacer query al database mediante un query vector.
