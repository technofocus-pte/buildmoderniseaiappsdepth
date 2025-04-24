# Caso de uso 09 – Construcción de una experiencia de Chat Bot Building mediante Azure Cosmos DB for Mongo DB y Azure OpenAI Service

**Objetivo:**

Este caso de uso creará una solución inteligente que combina la búsqueda
vectorial (vector search) y recuperación de documentos (document
retrieval) de Azure Cosmos DB for MongoDB basado en vCore con Azure
OpenAI services para construir una experiencia de chat bot.

![A diagram of a software application AI-generated content may be
incorrect.](./media/image1.jpeg)

**Tecnologías clave** -- Azure OpenAI Service, Azure Cosmos DB, modelo
de ChatGPT

**Duración estimada** -- 60 minutos

**Tipo de laboratorio** – dirigido por el instructor

**Importante:** Si alguno de los commands no se **pega** en
**PowerShell**, por favor abra un notepad, mantenga un cursor en
cualquier espacio vacío del notepad y luego haga clic en el botón T del
command para que se pegue. El contenido se copiará al notepad y entonces
lo puede copiar y pegar desde notepad en PowerShell.

## Ejercicio 0: Comprensión de la VM y las credenciales

En esta tarea, identifiquemos y entendamos las credenciales que vamos a
usar a lo largo del laboratorio.

1.  La pestaña **Instructions** tiene la guía del laboratorio con las
    instrucciones que seguir a lo largo del laboratorio.

2.  La pestaña **Resources** tiene las credenciales que se necesita para
    la ejecución del laboratorio.

    - **URL** – el URL que nos lleva al Azure portal

    - **Subscription** – Esta es la ID de suscripción que le han
      asignado

    - **Username** – El user id con el que debe iniciar sesión en Azure
      services.

    - **Password** – La contraseña para iniciar sesión en Azure.
      Llamemos el username y password como las credenciales de login en
      Azure. Vamos a usar estas credenciales siempre y cuando
      mencionamos Azure login credentials.

    - **Resource Group** – El grupo de recursos que le asignaron.

\[!Atención\] **Importante:** Asegúrese de crear todos sus recursos en
este Resource group

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  La pestaña **Help** tiene toda la información de Support. El valor
    de **ID** aquí es el **Lab instance ID** que se usará durante la
    ejecución del laboratorio.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

## Ejercicio 1: Aprovisionar los recursos de Azure

### Tarea 1: Cree los Azure resources con la ayuda de script

1.  Inicie sesión en Azure portal en +++\*\* utilizando las Azure login
    credentials desde la pestaña **Resources**.

2.  Desde el Azure portal, seleccione su suscripción. Desde el panel
    izquierdo, seleccione Resource Providers bajo Settings, seleccione
    +++**Microsoft.Alertsmanagement**+++ y haga clic en **Register**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  Desde la VM, busque +++**power shell**+++, haga clic derecho
    en **Windows PowerShell** y seleccione **Run as administrator**.
    Haga clic en **Yes** en el diálogo de confirmación.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  Ejecute el siguiente command para instalar Az in el PowerShell.

+++**Install-Module Az**+++

Seleccione **A** (Yes to all) cuando le indica.

**Ojo:** Esto llevará alrededor de 5 minutos para completar.

![A computer screen with white text AI-generated content may be
incorrect.](./media/image7.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

5.  Una vez hecho, ejecute el siguiente comando para importar el módulo
    de Az.

+++**Import-Module Az**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

6.  Ejecute el siguiente command para usar el inicio de sesión basado en
    navegador

+++Update-AzConfig -EnableLoginByWam $false+++

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image10.jpeg)

7.  Ejecute el siguiente command y seleccione su login de Azure si le
    indica, para iniciar sesión en Azure.

+++Connect-AzAccount+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

8.  Ejecute los siguientes commands para navegar hasta la
    carpeta **LabFiles**.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![A blue rectangular sign with white text AI-generated content may be
incorrect.](./media/image12.jpeg)

9.  Ejecute el siguiente command para instalar **Microsoft
    Bicep** mediante **winget**.

+++winget install -e --id Microsoft.Bicep+++

Tecle **Y** si le indica.

![A computer screen with white text AI-generated content may be
incorrect.](./media/image13.jpeg)

10. **Cierre** el PowerShell y **ábralo** de nuevo.

11. Ejecute el siguiente command y seleccione su login de Azure si le
    indica, para iniciar sesión en Azure.

+++Connect-AzAccount+++

12. Ejecute los siguientes commands para navegar hasta la
    carpeta **LabFiles**.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![A blue rectangular sign with white text AI-generated content may be
incorrect.](./media/image12.jpeg)

13. Ejecute el siguiente command para configurar el Subscription ID.

+++Set-AzContext -SubscriptionId @lab.CloudSubscription.Id+++

![A computer screen shot of a blue screen AI-generated content may be
incorrect.](./media/image14.jpeg)

14. Abra el archivo **azuredeploy.bicep** en el path **C:\LabFiles\Build
    a Chat bot\Labs\deploy**, y reemplace las letras **dgxxxxxxx** en la
    línea 35 con <+++dg@lab.LabInstance.Id>+++. En la línea **74**,
    actualice la versión como +++**0125**+++.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

![](./media/image16.png)

15. Ejecute el siguiente command para implementar los recursos como el
    workspace de Azure Cosmos DB, Azure OpenAI en Azure.

New-AzResourceGroupDeployment -ResourceGroupName
@lab.CloudResourceGroup(ResourceGroup1).Name -TemplateFile
.\azuredeploy.bicep -TemplateParameterFile .\azuredeploy.parameters.json
-c \`\`\`

\>\[!Nota\] \*\*Ojo:\*\* La implementación llevará alrededor de 10-15
minutos.

Si sale algún problema con la implementación y resulta fallido, intente
actualizar el nombre en el paso 14 a algo diferente e intente la
implementación de nuevo.

\>\[!Nota\] \*\*Ojo:\*\* Tecle Y cuando le indica.

![A computer screen shot of a blue screen AI-generated content may be
incorrect.](./media/image17.jpeg)

![](./media/image18.jpeg)

\>\[!Nota\] \*\*Ojo:\*\* Si no hay ninguna actualización en PowerShell
después de 15-20 minutos, averigue Resource Group -\> Deployments en el
Azure Portal o haga clic en \*\*Enter\*\* en la pantalla
\*\*PowerShell\*\*.

### Tarea 2: Averigue los recursos creados en Azure

1.  Inicie sesión en **Azure portal** en
    +++<https://portal.azure.com/+++> utilizando sus **Azure login
    credentials**. Seleccione **Resource groups**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

2.  Desde la lista de Resource groups, seleccione el **Resource Group
    asignado**.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image20.png)

3.  Note que se ha creado un conjunto de recursos incluidos **Azure
    OpenAI resource**, **App Service**, **Azure Cosmos DB for MongoDB**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

4.  Haga clic en el recurso de **Azure OpenAI**.

![](./media/image22.png)

5.  Seleccione **Keys and Endpoint** en **Resource Management**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.jpeg)

6.  Copie y guarde el **Key 1** y **Endpoint** en un notepad para poder
    referirse a ello más tarde.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

7.  Volviendo a la página de resource group, seleccione el
    recurso **Azure Cosmos DB for Mongo DB**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

8.  Haga clic en **Connection strings** en **Settings**. Copie el valor
    de **Self (always this cluster)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.jpeg)

9.  Copie la connection string y péguelo en un notepad. Reemplace el
    \< **password** \> con +++**myMongoDB98**+++ en el connection string
    copiado y gúardelo en el notepad.

## Ejercicio 2: Explore y use los modelos Azure OpenAI desde el código

### Tarea 1: Configuración del Entorno

1.  Desde la barra de búsqueda de windows del laboratorio VM, busque
    +++Visual studio code+++ y abra **Visual Studio Code**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

2.  Haga clic en **Open Folder**. (si no aparece automáticamente,
    seleccione **File -\> Open Folder**)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

3.  Navegue a **C:\Labfiles**, haga clic en la carpeta **Build a Chat
    bot** y seleccione **Select Folder.**

![A screenshot of a chat bot AI-generated content may be
incorrect.](./media/image29.jpeg)

4.  Haga clic en **Yes, I trust the Authors** en el pop up.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

5.  Desde Visual Studio Code,
    abra **lab_0_explore_and_use_models.ipynb** desde la
    carpeta **Labs**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

6.  Haga clic en **Select Kernel.**

7.  Seleccione **Install** en el pop up **Do you want to install the
    recommended extensions for Python**?

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image32.jpeg)

8.  Haga clic en **Allow access** si le indica.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.jpeg)

9.  Haga clic en **Select Kernel**. Seleccione **Python Environments** y
    luego **Python 3.12.3** o posterior que se enumera como la
    opción **Suggested** o **Recommended**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.jpeg)

10. Abra el archivo **.env**

11. Reemplace **DB_CONNECTION
    STRING**, **AOAI_KEY** y **AOAI_Endpoint** que hemos guardado antes
    en el **notepad** en la Tarea 2 del Ejercicio 1.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

Ahora, las variables del entorno están aptos para los recursos de Azure
que ya hemos creado.

### Tarea 2: Ejecute el código

1.  Volviendo al archivo **Lab 0 ipynb**, **ejecute** la **primera
    celda** al hacer clic en el botón Play, para instalar la última
    biblioteca del cliente de OpenAI.

![A black screen with a black background AI-generated content may be
incorrect.](./media/image37.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image38.jpeg)

2.  **Ejecute** la siguiente celda para instalar el **Python-dotenv**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image39.jpeg)

3.  Tecle **Ctrl+Shift+P**, tecle +++Reload Window+++ y seleccione la
    opción Developer:Reload Window que se enumera.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

4.  Ejecute desde la **primera celda** de nuevo.

5.  **Ejecute** la siguiente celda para importar la bibliioteca
    necesaria de OpenAI; os para acceder a las variables del entorno y
    dotenv para cargar las variables del entorno desde el archivo .env.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image41.jpeg)

6.  **Ejecute** la siguiente celda para crear el **Azure OpenAI
    client** para llamar al Azure OpenAI Chat completion API:

![A computer screen with text AI-generated content may be
incorrect.](./media/image42.jpeg)

7.  **Ejecute** la siguiente celda para llamar el
    método **.chat.completions.create()** sobre el cliente para llevar a
    cabo un **chat completion**. Deberá recibir una respuesta del chat.

![A computer screen with text on it AI-generated content may be
incorrect.](./media/image43.jpeg)

## Ejercicio 3: Primera aplicación de Cosmos DB for MongoDB API

Este ejercicio le enseñará cómo crear su primer proyecto de Cosmos DB.
Usaremos un notebook para demostrar las operaciones básicas de CRUD.

1.  Abra el archivo **lab_1_first_application.ipynb** desde la
    carpeta **Labs**.

2.  Haga clic en **Select kernel** y elija la **versión** **Python**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

3.  **Ejecute** la primera celda para instalar **pymongo**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

4.  **Ejecute** la siguiente celda para hacer los **imports** requeridos

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image46.jpeg)

5.  Ejecute la siguiente celda para **Crear un database.**

\[!Nota\] **Ojo:** Esto usará la connection string que actualizamos en
el archivo .env

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image47.jpeg)

6.  **Ejecute** la siguiente celda para crear una **collection**.

![A black screen with white text AI-generated content may be
incorrect.](./media/image48.jpeg)

7.  **Ejecute** la siguiente celda para crear un **document**. Uno de
    los métodos de crear un documento es utilizar el método insert_one.
    Este método lleva un sólo documento y lo inserta en la base de
    datos.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image49.jpeg)

8.  **Ejecute** la siguiente celda para **recuperar un sólo documento**
    desde la base de datos. Aquí se usa el método **find_one** para este
    propósito.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.jpeg)

9.  **Ejecute** la siguiente celda en la que se usa el
    método **find_one_and_update** para actualizar un solo documento en
    la base de datos.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

10. **Ejecute** la siguiente celda en la que se usa el método
    **delete_one** para eliminar un solo documento desde la database.

![](./media/image52.jpeg)

11. El método **find** se usa para hacer query para múltiples documentos
    en la base de datos. **Ejecute** las **siguientes 3 celdas** uno por
    uno para verlo en acción.

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image53.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image55.jpeg)

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image56.jpeg)

12. La siguiente celda **eliminará** la database y collection creadas en
    este ejercicio. Esto se hace mediante el método
    **drop_database** sobre el objeto de database

![A computer screen with text AI-generated content may be
incorrect.](./media/image57.jpeg)

## Ejercicio 4: cargue los datos en Cosmos DB mediante la MongoDB API

El ejercicio anterior nos demostró cómo se añade datos a una colección
individualmente. Este ejericio desmostrará cómo de cargan datos en
múltiples colleciones mediante operaciones en masa. Estos datos serán
utilizados en próximos laboratorios para explicar mejor las capacidades
de Azure Cosmos DB API for MongoDB sobre AI.

Este notebook demuestra cómo cargar datos en Cosmos DB desde los
archivos Cosmic Works JSON en la database mediante la MongoDB API.

1.  Abra el archivo **lab_2_load_data.ipynb** desde la carpeta **Labs**.
    Haga clic en **Select Kernel** y seleccione la
    **versión** **Python**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

2.  Execute the first cell to install **requests**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

3.  **Ejecute** la siguiente celda para hacer los **imports**
    requeridos.

![A computer screen with green text AI-generated content may be
incorrect.](./media/image60.jpeg)

4.  **Ejecute** la siguiente celda que establece una conexión con
    la **database**.

![A computer screen with text AI-generated content may be
incorrect.](./media/image61.jpeg)

![A computer screen shot of text AI-generated content may be
incorrect.](./media/image62.jpeg)

5.  **Ejecute** la siguiente celda para **cargar** los **products**.

![A screen shot of a computer screen AI-generated content may be
incorrect.](./media/image63.jpeg)

6.  **Ejecute** las siguientes celdas para **cargar** **los datos
    brutos** de **clientes** y **ventas**. En este repositorio, los
    datos de clientes y ventas se guardan en la misma carpeta. Se usa el
    type field para diferenciar entre dos tipos de documentos.

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image64.jpeg)

![](./media/image65.jpeg)

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image66.jpeg)

7.  **Ejecute** la siguiente celda para hacer **clean up**.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image67.jpeg)

## Ejercicio 5: Vector Search mediante vCore-based Azure Cosmos DB for MongoDB

1.  Abra el archivo **lab_3_mongodb_vector_search.ipynb** desde la
    carpeta **Labs**.

2.  Haga clic en **Select Kernel** y seleccione la
    **versión** **Python**.

![](./media/image68.jpeg)

3.  **Ejecute** la primera celda para instalar **tenacity**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image69.jpeg)

4.  **Ejecute** la siguiente celda para llevar a cabo los **imports**
    requeridos.

![A computer screen with text AI-generated content may be
incorrect.](./media/image70.jpeg)

5.  **Ejecute** la siguiente celda para **cargar**
    los **settings** desde el archivo .env.

![A computer screen with text AI-generated content may be
incorrect.](./media/image71.jpeg)

6.  Ejecute la siguiente celda para establecer
    la **connectivity** al **database**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image72.jpeg)

7.  **Ejecute** la siguiente celda para establecer **Azure OpenAI
    connectivity**.

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image73.jpeg)

8.  El proceso de la creación de un vector embedding field en cada
    documento solo se hace una vez. Sin embargo, si un documento cambia,
    se tiene que actualizar vector embedding field con un vector
    actualizado. Esto se hace en las siguentes 2 celdas. **Ejecute** las
    dos celdas y observe las **embeddings** obtenidas como salidas en la
    segunda celda.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image74.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image75.jpeg)

9.  **Ejecute** la siguiente celda para **Vectorizar y actualizar todos
    los documentos en la Cosmic Works database.**

![A computer screen shot of a program code AI-generated content may be
incorrect.](./media/image76.jpeg)

10. **Ejecute** las siguientes **3** celdas para añadir **vector
    fields** a **los documentos de productos, clientes y ventas.**

**Ojo:** la primera celda llevará alrededor de 5 minutos, la segunda 3
minutos, y la tercera llevará unos 20 minutos hacia la ejecución
completa.

\![\](./media/image77.jpeg)

11. **Ejecute** la siguiente celda para crear **products vector index**.

![A screen shot of a computer screen AI-generated content may be
incorrect.](./media/image77.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image78.jpeg)

12. Ahora que cada documento tiene su propio vector embedding asociado y
    se han creado las vector indexes sobre cada colección, podemos usar
    las capacidades de búsqueda vectorial del vCore-based Azure Cosmos
    DB for MongoDB. **Ejecute** las siguientes **3** celdas.

![](./media/image79.jpeg)

![](./media/image80.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image81.jpeg)

13. **Ejecute** las siguientes celdas para observar el uso de **vector
    search results** en un RAG pattern con Chat GPT-3.5

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image82.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image83.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image84.jpeg)

14. Observe la salida de las siguientes celdas.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image85.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image86.jpeg)

## Ejercicio 6: Elimine los recursos implementados

1.  Desde el Azure
    portal(+++[https://portal.azure.com+++](https://portal.azure.com+++/)),
    seleccione el resource group asignado.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image87.png)

2.  Seleccione todos los recursos en ello. Haga clic en los **tres
    puntitos** en el menú y seleccione **Delete** para eliminar todos
    los recursos.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image88.png)

3.  Tecle +++delete+++ en el cuadro de texto y haga clic en el botón
    Delete.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image89.png)

4.  Una vez eliminado, desde la página de inicio de Azure portal, busque
    +++**Azure AI Services**+++ y selecciónelo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.jpeg)

5.  Seleccione **Azure OpenAI** desde el panel izquierdo y a continuacón
    seleccione **Manage deleted resources**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image91.jpeg)

6.  Seleccione el recurso enumerado ahí y haga clic en **Purge**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image92.jpeg)

7.  Haga clic en **Yes**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image93.jpeg)

**Resumen:**

Ha creado una solución para Azure Cosmos DB for MongoDB vector search y
document retrieval con Azure OpenAI services con éxito.
