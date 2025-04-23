# Caso de uso10: Implementar el chat application para responder a las preguntas del usuario y rastrear el historial del chat en las conversaciones

**Objetivo:**

Este caso de uso le lleva a través de los pasos para conectar una
aplicación Blazor actual a una cuenta Azure Cosmos DB for NoSQL y una
cuenta Azure OpenAI. Su aplicación manda prompts al modelo en Azure
OpenAI y hace parches de las respuestas. Su apliación también almacena
varias sesiones de las conversaciones y sus mensajes correspondientes
como artíuclos adjuntados en un sólo container dentro de Azure Cosmos DB
for NoSQL.

En resumen, la aplicación:

- **Connecta** al modelo Azure OpenAI mediante .NET SDK

- **Manda** prompts al modelo y hace parche del completion response

- **Conecta** a Azure Cosmos DB for NoSQL mediante .NET SDK

- **Gestion** de los artículos con operaciones individuales, queries y
  transactional batches

Esta apliación chat de muestra responde a las preguntas del usuario y
rastrea el historial de chat en las conversaciones.

![](./media/image1.jpeg)

**Tecnologías clave** --, Csharp, nosql ,asp-net,blazor,azure-cosmos-db,

**Duración estimada** -- 45 minutos

**Tipo del laboratorio:** dirigido por el instructor

**Prerequisitos:** Cuenta de GitHub – se espera tener su propia cuenta
de GitHub. Si no la tiene, por favor cree una aquí
-\`\` **https://github.com/signup?user_email=&source=form-home-signupobjectives\`\`**

### Tarea 1: Ejecute el Docker

1.  En el cuadro de search de su Windows, tecle **Docker**, y haga clic
    en **Docker Desktop**.

![](./media/image2.jpeg)

### Tarea 2 : Registre el Service provider

1.  Abra un navegador y vaya a <https://portal.azure.com> e inicie
    sesión con sus credenciales de Azure disponibles en la pestaña
    **Resource** de su VM.

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

2.  En Home page del Azure portal, haga clic en **Resource groups**.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

3.  Copie el nombre del resource group y guárdelo en un notepad para la
    próxima tarea para implementar lso recursos requeridos en este
    resource group.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

4.  Navegue a Home page, haga clic en **Subscription**.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

5.  Haga clic en el nombre de subscription.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

6.  Haga clic en **Settings - \> Resource provider** desde el menú de
    navegación izquierdo.

![](./media/image8.png)

7.  Tecle \`\`**Microsoft.AlertsManagement**\`\` y presione enter.
    Selecciónelo y haga clic en **Register**.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

![A screenshot of a computer Description automatically
generated](./media/image10.png)

### Tarea 3: Apovisione los servicios y aplicación a Azure

1.  Abra un navegador y vaya a \`\`https:\\github.com\`\` e inicie
    sesión con su cuenta Github.

![](./media/image11.jpeg)

2.  Busque el siguiente repo y haga clic en **Fork**.

> \`\`https://github.com/technofocus-pte/chat-csharp-cosmos-db-nosql-openai\`\`

![](./media/image12.jpeg)

3.  Inroduzca el nombre del repository y haga clic en **Create
    repository**.

![](./media/image13.jpeg)

4.  Haga clic en **Code -\> Code space -\> Open Code space.**

![](./media/image14.jpeg)

5.  Espere a que se complete la configuración de Dev container. Lleva
    unos 3-5 minutos

![](./media/image15.jpeg)

6.  Ejecute el siguiente command para iniciar sesión en AZD. Copie el
    código generado y presione Enter. 

> \`\`**azd auth login\`\`**

![](./media/image16.jpeg)

7.  Pegue el siguiente código e inicie sesión con su código generado y
    acceda con sus credenciales Azure.

![](./media/image17.jpeg)

![](./media/image18.jpeg)

8.  Ejecute el siguiente command para iniciar el proyecto en el
    directory actual. Introduzca el Environment name
    como \`\`**cosmoschatapp\`\`** y presione Enter.

\`\`azd init \`\`

![A screenshot of a computer Description automatically
generated](./media/image19.png)

9.  Ejecute el siguiente command para implementar los servicios a Azure,
    construya su container. Seleccione los siguientes valores below
    values.

> \`\`azd provision\`\`
>
> **Select an Azure Subscription to use** : seleccione su suscripción
>
> **Select an Azure location to use** : **East us/west us** (a veces,
> East US no está disponible, elija otro e implemente.)
>
> **Enter a value for the 'existingResourceGroupName' infrastructure
> parameter:** **ResourceGroup1**

![](./media/image20.png)

![A screenshot of a computer Description automatically
generated](./media/image21.png)

10. Espere a que se complete el aprovisionamiento del recurso. Este
    proceso llevará 5-10 minutos para crear todos recursos requeridos.

![](./media/image22.png)

### Tarea 4: Implemente la aplicación a Azure

1.  Cambie a Azure portal y haga clic en Resource groups en Home page.

![](./media/image23.png)

2.  Haga clic en el nombre resource group.

![](./media/image24.png)

3.  Debe ver los siguientes recursos

- **Container**

- **Container Registry**

- **Azure Cosmos Db account**

- **AureOpenAI**

![A screenshot of a computer Description automatically
generated](./media/image25.png)

4.  Haga clic en el nombre de **Container registry**.

![](./media/image26.png)

5.  Expanda **Setting** desde left navigation menu, haga clic en
    **Access keys.** Seleccione **Admin user check box.** Copie el
    **Login server**, **user name** y **password** a un notepad para
    usarlos en la implementación de la aplicación.

![](./media/image27.png)

6.  Duplique la pestaña para abrir Azure portal en una nueva pestaña.

![](./media/image28.png)

7.  Haga clic en el nombre de resource group desde el menú de navegación
    superior.

![](./media/image29.png)

8.  Haga clic en nombre Container App.

![](./media/image30.png)

9.  Haga clic en el botón **Authorize** en **Github-Sign in to
    authenticate with your GitHub account**. Autorice su cuenta Github.

10. Seleccione los siguientes valores

> **Organization : su Github organization**
>
> **Repository:** chat-csharp-cosmos-db-nosql-openai
>
> **Branch :** main

![](./media/image31.png)

11. Baje a **Registry settings** y introduzca los siguientes valores y
    haga clic en el botón **Start continuous deployment**.

- Repository source : **Docker Hub or other registries.**

- Login server URL : su Login server copiado desde Container registry
  (paso#5)

- Username : su username desde container registry (paso \#5)

- Password : su password desde container registry (paso \# 5)

![](./media/image32.png)

12. Haga clic en el enlace Workflow file. Abre una pestaña con Github.

![](./media/image33.png)

13. Haga clic en la pestaña **Actions**.

![](./media/image34.png)

14. Espere a que se complete la implementación.

![](./media/image35.png)

15. No cierre las pestañas.

### Tarea 5: Acceda al chat app

1.  Cambie a Azure portal y haga clic en **Overview** desde la
    navegación izquierdo y haga clic en **Application Url**. Abre un
    nuevo para cargar la aplicación.

![](./media/image36.png)

2.  Haga clic en **Create New Chat**.

![](./media/image37.png)

3.  Introduzca el siguiente prompt.

\`\`What is the seating capacity for Lumen in Seattle?\`\`

![](./media/image38.jpeg)

4.  Introduzca el siguiente prompt. Explore la aplicación con diferentes
    prompts.

\`\`is that bigger than Dogger stadium??\`\`

![](./media/image39.jpeg)

### Tarea 6: Limpie todos los recursos

Para limpiar los recursos creados por esta muestra:

1.  Cambie a Github portal y actualice la página.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

2.  Haga clic en Code, seleccione el branch creados para este
    laboratorio y haga clic en **Delete**.

![](./media/image41.png)

3.  Confirme la eliminación del branch al hacer clic en **Delete**.

![A screenshot of a computer Description automatically
generated](./media/image42.png)

5.  Vuelva a **Azure portal -\> Resource group- \> Resource group
    name.**

![](./media/image43.png)

6.  Seleccione todos los recursos y haga clic en Delete como se ve aquí.
    (**NO ELIMINE** el resource group)

![](./media/image44.png)

7.  Tecle \`\`**delete**\`\` en el cuadro de texto y haga clic en
    **Delete**.

> ![](./media/image45.png)

8.  Confirme la eliminación al hacer clic en **Delete**.

![](./media/image46.png)

**Resumen:**

Tiene implementado los service classes mediante Microsoft.Azure.Cosmos y
Azure.AI.OpenAI packages en NuGet. Ha mandado prompts a Azure OpenAI
conversational interface junto con los contextual prefixes y ha parchado
las propiedades de uso y body de la respuesta. También ha usado Azure
Cosmos DB for NoSQL para almacenar las sesiones de las conversaciones y
mensajes dentro de un solo container.
