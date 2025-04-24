# Caso de uso 11 – Construir un Copilot mediante Azure OpenAI, Azure Cosmos DB for NoSQL

En este caso de uso, conectará un Blazor web application a Azure Cosmos
DB for No SQL y Azure OpenAI mediante kits de desarrollo de software
.NET. Su código gestiona y pide artículos en un API para un NoSQL
container. Su código también manda prompts a Azure OpenAI y analiza la
respuesta.

**Duración del laboratorio:** 45 minutos

**Tipo del laboaratorio**: dirigido por el instructor

**Objetivo**

- Configurar el entorno de Blazor, PostgreSQL y OpenAI.

- Crear un proyecto Blazor y diseñe una interfaz de chat receptiva

- Configurar el PostgreSQL database en Azure y conectarlo a la
  aplicación Blazor.

- Integrar Azure OpenAI para obtener las funcionalidades de chat
  mejoradas.

- Implementar la aplicación Blazor y PostgreSQL database en Azure.

- Probar la aplicación para garantizar una interacción fluida entre los
  componentes.

- Para monitorrear y hacer troubleshooting de las aplicaciones
  implementadas en Azure.

**Tecnologías clave:** Azure Cosmos DB for NoSQL, Azure OpenAI

## Ejercicio 0: Comprensión de la VM y las credenciales

En esta tarea, identifiquemos y entendamos las credenciales que vamos a
usar a lo largo del laboratorio.

1.  La pestaña **Instructions** tiene la guía del laboratorio con las
    instrucciones que seguir a lo largo del laboratorio.

2.  La pestaña **Resources** tiene las credenciales que se necesita para
    la ejecución del laboratorio.

    - **URL** – el URL que nos lleva a Azure portal

    - **Subscription** – Esta es la ID de la suscripción que le
      asignaron

    - **Username** – El user id con el que debe iniciar sesión a Azure
      services.

    - **Password** – La contraseña para iniciar sesión en Azure.
      Llamemos el username y password como las credenciales de login de
      Azure. Vamos a usar estas credenciales siempre y cuando
      mencionamos Azure login credentials.

    - **Resource Group** – El grupo de recursos que le asignaron.

\[!Atención\] **Importante:** Asegúrese de crear todos sus recursos en
este Resource group

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  La pestaña **Help** tiene toda la información de Support. El valor
    de **ID** aquí es el **Lab instance ID** que se usará durante la
    ejecución del laboratorio.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## Ejercicio 1: Implemente la infraestructura y complete la configuración inicial

Para completar este proyecto, necesita una cuenta de Azure Cosmos DB for
NoSQL y una de Azure OpenAI. Para agilizar este proceso, implemente una
plantilla Bicep en Azure con ambas cuentas.

### Tarea 1: Implementar la infraestructura desde la plantilla

1.  Abra el archivo desde el path **C:\Labfiles\Build and Test a custom
    chat application Using Azure Cosmos DB and AzureOpenAI** y actualice
    la versión Azure OpenAI en la línea line 96 para ver
    +++0125+++. **Guarde** el archivo.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

2.  Abra un nuevo navegador e introduzca el siguiente URL en la barra de
    dirección: +++<https://portal.azure.com/+++> para abrir el Azure
    Portal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  En Azure portal, haga clic en el botón **\[\>\_\] (Cloud Shell)** en
    la parte superior de la página, a la derecha de la barra de
    búsqueda. Se abrirá un panel Cloud Shell en la parte inferior del
    portal. La primera vez que abra el Cloud Shell, puede que le pida
    elegir el tipo de shell para usar (**Bash** o **PowerShell**).
    Seleccione **Bash**. Si no ve esta opción, salte este paso.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  En el diálogo **Getting Started**, seleccione **Mount storage
    account**, seleccione su **suscripción** y haga clic en **Apply**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

5.  En el diálogo **Mount storage account**, seleccione **we will create
    a storage account for you** y haga clic en **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

![A close-up of a computer screen AI-generated content may be
incorrect.](./media/image9.jpeg)

6.  Asegure que el tipo de shell indicado en la parte superior izquierda
    del panel Cloud Shell esté cambiado a **Bash**. Si es
    poderoso **PowerShell**, cambie a **Bash** mediante el menú
    despegable.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.jpeg)

7.  Una vez que empieza el terminal, haga clic en **Manage files -\>
    Upload**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

8.  Seleccione el archivo **azuredeploy.JSON** desde el
    path **C:\Labfiles\Build and Test a custom chat application Using
    Azure Cosmos DB and AzureOpenAI** y seleccione **Open**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

Deberá recibir un mensaje de éxito para la carga del archivo.

![A white background with black text AI-generated content may be
incorrect.](./media/image13.jpeg)

9.  Cree una nueva variable shell llamado **resourceGroupName** con el
    nombre del Azure resource group que usted crea.
    (mslearn-cosmos-openai).

+++resourceGroupName="ResourceGroup1"+++(Obtenga el nombre de Resource
Group desde la pestaña Resources)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

10. Implemente el archivo de la plantilla **azuredeploy.json** en el
    grupo de recursos mediante el az group deployment create. A
    continuación, ejecute el siguiente command.

+++az deployment group create --resource-group $resourceGroupName --name
zero-touch-deployment --template-file azuredeploy.json+++

**Ojo:** Esta implementación puede llevar alrededor de 5-10 minutos.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image15.jpeg)

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image16.jpeg)

### Tarea 2: Obtenga las credenciales de cuentas de Azure Cosmos DB for NoSQL y Azure OpenAI

Esta implementación ha implementado las cuentas de Azure Cosmos DB for
NoSQL y Azure OpenAI y ha guardado sus credenciales en la configuración
de Azure App Service web app. Ahora, tiene la opción de elegir Azure
portal o Azure CLI para recuperar las credenciales para cada servicio.

1.  Desde la página de inicio de Azure portal, haga clic en **Resource
    groups.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

2.  Seleccione su grupo de recursos.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

3.  En la página **Resource Groups**, expanda el panel **Essentials** y
    observe el encabezado **Deployments**. El estado de la
    implementación debe estar **Succeeded** a esta altura.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

4.  Ahora, seleccione la cuenta **Azure Cosmos DB** para navegar hasta
    la página del recurso.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

5.  Seleccione la opción **Keys** en la sección **Settings** del menú de
    navegación de resources. Registre los valores de los
    campos **URI** y **PRIMARY KEY**. Puede usar esos valores más tarde.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.jpeg)

6.  Vuelve a la página **Resource Groups**. Seleccione la cuenta **Azure
    OpenAI**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

7.  En su pantalla de **Azure Open AI**, navegue hasta la sección
    **Resource Management**, y haga clic en **Keys and Endpoints**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.jpeg)

8.  En la página **Keys and Endpoints**, copie el **KEY1,** (*puede usar
    o KEY1 o KEY2)* y **Endpoint** y, a continuación, **Guarde** el
    notepad para utilizar esta información en las tareas siguientes.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

### Tarea 3: Ejecute el Docker

1.  En la barra de búsqueda de su Windows, tecle +++Docker+++ , y luego
    haga clic en **Docker Desktop**.

![A screenshot of a desktop AI-generated content may be
incorrect.](./media/image25.jpeg)

2.  Ejecute el Docker Desktop.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.jpeg)

## Ejercicio 2 – Configure y construya la aplicación starter

1.  Desde la barra de búsqueda VM, busque +++Visual Studio+++ y
    seleccione **Visual Studio Code**.

2.  Haga clic en **File** -\> **Open Folder**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

3.  Seleccione **cosmosdb-chatgpt** desde **C:\LabFiles** y haga clic
    en **Select Folder**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

4.  Haga clic en la opción **Yes, I trust the authors** en el **Do you
    trust the authors dialog**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.jpeg)

5.  En el editor de **Visual Studio Code**, haga clic en **Terminal**,
    abra un **New Terminal**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

6.  En una aplicación .NET, es normal usar los proveedores de
    configuración para inyectar nuevas configuraciones en una
    aplicación. Para esta aplicación, use el
    archivo **appsettings.Development.json** para proporcionar los
    valores más recientes para el endpoint y key de Azure OpenAI.

7.  Abra el archivo **appsettings.Development.JSON**. Reemplace los
    marcadores temporales de los valores uri y key de los recursos
    de **Azure Cosmos DB** y **Azure OpenAI** en el archivo con los
    valores que guardamos antes.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

8.  **Construya** el proyecto .NET al ejecutar el siguiente command.

+++dotnet build+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image32.jpeg)

## Ejercicio 3: Comprensión del código

### Tarea 1: Agregue los miemros requeridos y un client instance

1.  Abra el archivo **Services/OpenAiService.cs**. este archivo
    implementa los class variables necesarios para usar el cliente de
    Azure OpenAI. Implementa unos static prompts y crea un nuevo
    instance de la clase OpenAIClient.

2.  Este código crea un new string variable llamado_systemPromptText con
    un static block de texto para mandárselo al asistente IA antes de
    cada prompt.

> private readonly string \_systemPrompt = @"
>
> You are an AI assistant that helps people find information.
>
> Provide concise answers that are polite and professional." +
> Environment.NewLine;

3.  Este code block crea otro new string variable llamado
    \_summarizePrompt con un static block de texto para mandárselo un
    asistente IA con instrucciones de cómo resumir una conversación.

> private readonly string \_summarizePrompt = @"
>
> Summarize this prompt in one or two words to use as a label in a
> button on a web page.
>
> Do not use any punctuation." + Environment.NewLine;

4.  Este code block crea un nuevo instance de la clase OpenAIClient
    mediante el endpoint para construir un Uri y el key para crear un
    AzureKeyCredential.

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

**Tarea 2: pregunte algo a un modelo IA**

Primero, implemente una conversación de preguntas y respuestas al mandar
un system prompt, una pregunta, y ID de la sesión para que el modelo IA
puede proporcionar una respuesta en el contexto de la conversación
actual. Asegure de medir el número de tokens que se necesita para
analizar el prompt y devolver una respuesta (o proporcionar una
finalización en este contexto).

1.  Este code block crea una nueva variable llamado options del tipo
    ChatCompletionsOptions. Agrega dos message variables a la lista de
    Messages y establece el valor del usuario al sessionId constructor
    parameter.

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

2.  El método GetChatCompletionsAsync del Azure OpenAI client variable
    (\_client) se invoca de forma asincrónica. El resultado se guarda en
    la variable llamada completions del tipo ChatCompletions.

> Response\<ChatCompletions\> completionsResponse =
> await_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;

3.  Finalmente, el block of code en la parte inferior, devuelve un tuple
    como un resultado del método GetChatCompletionAsync con el contenido
    del completion como un string, el número de tokens asociados con el
    prompt, y el número de tokens para la respuesta.

> return (
>
> completionText: completions.Choices\[0\].Message.Content,
>
> completionTokens: completions.Usage.CompletionTokens
>
> );

**Tarea 3: Pregunte al modelo IA para resumir una conversación**

Ahora, mande un diferente system prompt al modelo IA, su conversación
actual, y ID de sesión para que el modelo IA pueda resumir la
conversación en unas palabras.

2.  El código siguiente crea una variable ChatCompletionsOptions llamado
    opciones con dos message variables en la lista Messages, User
    establecidos a sessionId constructor parameter, MaxTokens
    establecido a 200, y las propiedades restantes.

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

3.  El siguiente código invoca el \_client.GetChatCompletionsAsync de
    forma asincrónica con el nombre del modelo (\_modelName) y el
    options variable como el parámetro y guarda el resultado en una
    variable llamada completions del tipo ChatCompletions. Devuelve el
    contenido del completion en la forma del string como el resultado
    del método SummarizeAsync.

> Response\<ChatCompletions\> completionsResponse = await
> \_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;
>
> string completionText = completions.Choices\[0\].Message.Content;
>
> return completionText;

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image33.jpeg)

**Tarea 4 – Conecte a Azure Cosmos DB for NoSQL**

El CosmosDbService class contiene un stub implementation del servicio
similar a la clase OpenAiService en la que trabajó en este módulo. Por
contrario, esta clase usa el .NET SDK para Azure Cosmos DB, que funciona
de una manera diferente.

Esta sección explica la implementación de class variables y el cliente
requerido para acceder al Azure Cosmos DB for NoSQL mediante el cliente.

1.  Abra el archivo **Services/CosmosDbService.cs**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image34.jpeg)

2.  El siguiente código crea una variable llamada options del tipo
    CosmosSerializationOptions y establece el PropertyNamingPolicy
    property de la variable a CosmosPropertyNamingPolicy.CamelCase.

> CosmosSerializationOptions options = new()
>
> {
>
> PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
>
> };

**Ojo:** Establecer esta propiedad asegurará que el JSON producido por
el SDK será serialized y deserialized en el *camel case* a pesar de la
manera en la que el property correspondiente se constreñe en la clase
.NET.

3.  El siguiente código crea un nuevo instance del tipo CosmosClient
    llamado client mediante la clase CosmosClientBuilder, endpoint, key,
    y opciones de serialization que especificó.

> CosmosClient client = new CosmosClientBuilder(endpoint, key)
>
> .WithSerializerOptions(options)
>
> .Build();

4.  El siguiente código crea un new nullable variable del tipo Database
    llamado database al llamar el método GetDatabase del client
    variable.

**Database? database = client?.GetDatabase(databaseName);**

5.  El siguiente código asigna el container variable del constructor al
    \_container variable de la clase sólo si no es null. Si es null,
    haga throw en un ArgumentException.

> \_container = container ??
>
> throw new ArgumentException("Unable to connect to existing Azure
> Cosmos DB container or database.");

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.jpeg)

**Tarea 5 – Implemente el servicio Azure Cosmos DB for NoSQL**

El servicio Azure Cosmos DB (CosmosDbService) gestiona el querying,
creating, deleting, y updating de las sesiones y mensajes en sy
aplicación del asistente de la IA. Para gestionar todas estas
operaciones, el servicio tiene que implementar múliples métodos para
cada operación potencial mediante varias características del .NET SDK.

Hay varios requisitos clave para enfrentarse a esto en este ejercicio:

- Implementar las operaciones para crear una sesión o un mensaje

- Implementar los queries para recuperar múltiples sesiones o mensajes

- Implementar una operación para actualizar una sóla sesión o múltiples
  mensajes por batch update

- Implementar una operación para hacer query y eliminar varias sesiones
  o mensajes relacionadas

Azure Cosmos DB for NoSQL guarda los datos en el formato JSON, lo que
nos permite guardar muchos tipos de datos en un solo container. Esta
aplicación guarda tanto un chat "session" con el AI assistant como
individual "messages" con cada sesión. Con el API for NoSQL, la
aplicación puede guardar ambos tipos de datos en el mismo container y
luego diferenciar entre ellos mediante un sencillo type field.

1.  Abra el archivo **Services/CosmosDbService.cs**.

2.  El siguiente código crea una nueva variable llamada partitionKey del
    tipo PartitionKey utilizando SessionId property de la sesión actual
    como el parámetro.

**PartitionKey partitionKey = new(session.SessionId);**

3.  El siguiente código invoca el método CreateItemAsync del container
    pasando por el session parameter y partitionKey variable. Devuelve
    la respuesta como el resultado del método InsertSessionAsync.

> return await \_container.CreateItemAsync\<Session\>(
>
> item: session,
>
> partitionKey: partitionKey
>
> );

4.  El siguiente código crea un PartitionKey variable mediante
    session.SessionId como el valor del partition key. Crea un nuevo
    message variable llamado newMessage con el Timestamp property
    actualizado al UTC timestamp actual. Invoque el CreateItemAsync
    pasando por el new message y partition key variables. Devuelve la
    respuesta como el resultado de InsertMessageAsync.

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

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image36.jpeg)

**Tarea 6: Recupere varias sesiones o mensajes**

Hay dos casos de uso principales en los que la aplicación necesita
recuperar varios elementos de nuestro contenedor. En primer lugar, la
aplicación recupera todas las sesiones del usuario actual filtrando los
elementos a aquellos en los que type = Session. En segundo lugar, la
aplicación recupera todos los mensajes de una sesión realizando un
filtro similar donde type = Session & sessionId = . Ambas consultas se
implementan aquí mediante el .NET SDK y un feed iterator.

1.  El siguiente código crea una nueva variable denominada consulta de
    tipo QueryDefinition. Utiliza el método fluido WithParameter para
    asignar el nombre de la clase Session como valor del parámetro. A
    continuación, invoca el método genérico GetItemQueryIterator\<\> en
    la variable \_container pasando el tipo genérico Session y la
    variable de consulta como parámetro. Guarde el resultado en una
    variable de tipo FeedIterator llamada response.

> QueryDefinition query = new QueryDefinition("SELECT DISTINCT \* FROM c
> WHERE c.type = @type")
>
> .WithParameter("@type", nameof(Session));
>
> FeedIterator\<Session\> response =
> \_container.GetItemQueryIterator\<Session\>(query);

2.  El siguiente código dentro del while loop, obtiene de forma
    asincrónica la siguiente página de resultados invocando
    ReadNextAsync en la variable de respuesta y, a continuación, agregue
    esos resultados a la variable de lista denominada output. Fuera del
    bucle while, la variable de salida se devuelve con una lista de
    sesiones como resultado del método GetSessionsAsync.

> FeedResponse\<Session\> results = await response.ReadNextAsync();
>
> output.AddRange(results);
>
> return output;

3.  En el código siguiente se usa el método fluido WithParameter para
    asignar el parámetro @sessionId al session identifier pasado como
    parámetro y el parámetro @type al nombre de la clase Message.

> QueryDefinition query = new QueryDefinition("SELECT \* FROM c WHERE
> c.sessionId = @sessionId AND c.type = @type")
>
> .WithParameter("@sessionId", sessionId)
>
> .WithParameter("@type", nameof(Message));

4.  Cree un FeedIterator\< Message \> Usando la variable de consulta y
    GetItemQueryIterator\<\> method.

FeedIterator response = \_container.GetItemQueryIterator(query);

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image37.jpeg)

## Ejercicio 4: Ejecute la aplicación

Ahora la aplicación tiene una implementación completa de Azure OpenAI y
Azure Cosmos DB. Puede probar la aplicación de un extremo a otro
depurando la solución.

1.  Desde el **Visual Studio Code Terminal**, construya el proyecto
    usando el siguiente command.

+++**dotnet build**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.jpeg)

2.  Inicie la aplicación con las recargas habilitadas mediante dotnet
    watch.

+++**dotnet watch run --non-interactive**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

3.  Visual Studio Code inicia el explorador simple en la herramienta con
    la aplicación web en ejecución. En la aplicación web, cree una nueva
    sesión de chat haciendo clic en **+ Create New Chat** y haga una
    pregunta al asistente de IA. A continuación, cierre la aplicación
    web en ejecución.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image40.jpeg)

4.  Pegue el siguiente texto en el cuadro de texto y haga clic en
    **Send**.

+++How many wins does it take to promote to the Premier League?+++

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image41.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

5.  Pegue el siguiente texto en el cuadro de texto y haga clic en
    **Send**.

+++What is Azure OpenAI?+++

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image43.jpeg)

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image44.jpeg)

6.  Cierre el terminal.

## Ejercicio 5: Limpie el grupo de recursos

1.  Abra un nuevo navegador e introduzca el siguiente URL en la barra de
    dirección +++<https://portal.azure.com/+++> para abrir el Azure
    Portal.

2.  Desde la página Resource group, seleccione su **Resource group
    asignado**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  Seleccione todos los **resources**, y luego seleccione **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

4.  Tecle +++**delete**+++ en el cuadro de texto y haga clic
    en **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image49.png)

5.  Una notificación de éxito en los recursos eliminados confirma la
    eliminación.

6.  Una vez eliminados los recursos, en la página principal de Azure
    Portal, busque **Azure AI Services** y selecciónelo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.jpeg)

7.  Seleccione **Azure OpenAI** en el panel izquierdo y, a continuación,
    seleccione **Manage deleted resources**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

8.  Seleccione el recurso que aparece allí y luego haga clic
    en **Purge**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.jpeg)

9.  Haga clic en **Yes**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

**Resumen**

Este laboratorio proporcionó una guía completa para crear, implementar y
probar una aplicación de chat personalizada mediante Blazor, PostgreSQL
y Azure OpenAI. En este laboratorio, ha aprendido a configurar el
entorno de desarrollo necesario, ha creado y diseñado una interfaz de
chat basada en Blazor, ha configurado y conectado una base de datos
PostgreSQL en Azure, ha integrado Azure OpenAI para mejorar las
funcionalidades y, por último, ha implementado y probado la aplicación
en Azure. Esta experiencia práctica lo equipó con las habilidades para
desarrollar y administrar aplicaciones web modernas utilizando
tecnologías de vanguardia y servicios en la nube
