# Caso de uso 06 – Implementar el chat app en Azure Container Apps with PostgreSQL Flexible Server

**Objetivo:**

- Para implementar el entorno del desarrollo al instalar Azure CLI,
  Node.js, asignar Azure subscription roles, iniciar Docker Desktop, y
  habilitar Visual Studio Code con las extensiones de Dev Containers.

- Para implementar y probar Custom Chat Application con PostgreSQL y
  OpenAI en Azure.

![A screenshot of a computer Description automatically
generated](./media/image1.jpeg)

En este caso de uso, configurará su entorno de desarrollo comprensivo,
implementará una aplicación de char integrada en PostgreSQL, y verifique
su implementación en Azure. Esto involucra la instalación de
herramientas esenciales como Azure CLI, Docker, y Visual Studio Code (ya
lo hemos hecho para usted en host env), la configuración de roles de
usuarios en Azure, implementación de la aplicación mediante Azure
Developer CLI, e interación con los recursos implementados para asegurar
funcionalidad.

**Tecnologías clave** -- Python, FastAPI, Azure OpenAI models, Azure
Database for PostgreSQL y azure-container-apps,ai-azd-templates.

**Duración estimada** -- 45 minutos

**Tipo del laboratorio:** dirigido por el instructor

**Prerequisitos:**

Cuenta GitHub – se espera tener sus propias credenciales de GitHub. Si
no las tiene, por favor cree unas desde aquí
- **https://github.com/signup?user_email=&source=form-home-signupobjectives**

## Ejercicio 1: Aprovisione, implemente la aplicación y prúebela desde el navegador

### Tarea 1: Copie el nombre del resource group existente

1.  Abra su navegador, abra Azure
    portal \`\`https:\\portal.azure.com\`\`.  inicie sesión en su Azure
    slice account (Azure Credentials***)*** disponible en la sección
    instructions/Resources de su entorno host.

![A screenshot of a computer Description automatically
generated](./media/image2.jpeg)

2.  En Home page, haga clic en **Resource groups**.

![A screenshot of a computer Description automatically
generated](./media/image3.png)

3.  Asegúrese de tener un resource group creado para usted. No elimine
    este resource group nunca. En cambio, puede eliminar los recursos
    dentro del resource group, pero no el resource group mismo.

4.  Haga clic en el nombre de resource group

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  Copie el nombre de resource group y gúardelo en su Notepad para usar
    para implementar todos los recursos en este resource group

![A screenshot of a computer Description automatically
generated](./media/image5.png)

### Tarea 2: Ejecute el Docker

1.  En el Desktop, haga doble clic en **Docker Desktop**.

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

2.  Ejecute el Docker Desktop.

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

### Tarea 3: registre el proveedor de Service 

1.  Cambie a la pestaña Azure portal, haga clic en **Subscription**.

![](./media/image8.png)

2.  Haga clic en nombre de suscripción.

![](./media/image9.png)

3.  Haga clic en **Settings - \> Resource provider** desde el menú de
    navegación izquierdo.

![](./media/image10.png)

4.  Tecle \`\`**Microsoft.AlertsManagement**\`\` y presione enter.
    Seléccionelo y haga clic en **Register**.

![](./media/image11.png)

![A screenshot of a computer Description automatically
generated](./media/image12.png)

### Tarea 4: Abra el entorno del desarrollo

1.  Abra su navegador, navegue a la barra de direcciones, tecle o pegue
    el siguiente
    URL: \`\`https://github.com/technofocus-pte/rag-postgres-openai-python.git\`\` se
    abre una pestaña y le pide que lo abra en Visual studio code.
    Seleccione **Open Visual Studio Code.**

![](./media/image13.jpeg)

2.  Haga clic en **fork** para hacer fork en el repo. Dé un nombre único
    al repo y haga clic en el botón **Create repo**.

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

3.  Haga clic en **Code -\> Codespaces -\> Codespaces+**

![A screenshot of a computer Description automatically
generated](./media/image16.jpeg)

4.  Espere a que complete la configuración de Codespaces environment.
    Tarda unos minutos para un setup completo

![A screenshot of a computer Description automatically
generated](./media/image17.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

### Tarea 5: Aprovisione los servicios e implemente la apliación a Azure

1.  Ejecute el siguiente command en el Terminal. Genera el código para
    copiar. Copie el código y presione Enter.

\`\`azd auth login\`\`

![](./media/image19.png)

2.  El navegador predeterminado abre el código generado para verificar.
    Introduzca el código y haga clic en **Next**.

![](./media/image20.png)

3.  Inicie sesión con sus credenciales Azure.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

4.  Para crear un entorno para sus recursos Azure, ejecute el siguiente
    Azure Developer CLI command. Le pregunta introducir el nombre del
    entorno. Introduzca cualquier nombre de su elección y presione enter
    (por ejemplo: **ragpgpy**)

**Ojo:** cuando crea un entorno, asegure que el nombre consiste de
letras minúsculas.

\`\`azd env new\`\`

![A screenshot of a computer Description automatically
generated](./media/image22.png)

5.  Ejecute el siguiente Azure Developer CLI command para aprovisionar
    los recursos Azure e implementar el código.

\`\`azd provision \`\`

![A screenshot of a computer Description automatically
generated](./media/image23.png)

6.  Cuando se le pide, seleccione una **subscription** para crear los
    recursos y seleccione la región más cerca de su ubicación; en este
    laboratorio, hemos elegido la región **East US2**.

![](./media/image24.png)

7.  Le pedirá esto “**Enter a value for the 'existingResourceGroupName'
    infrastructure parameter:**” introduzca el resource group copiado en
    Tarea 1 (por ejemplo: **ResourceGroup1 used for the development
    slice).** puede copiar el nombre resource group desde la sección
    **Resources** como se ve aquí

> ![](./media/image25.png)

8.  Cuando se le solicite, **enter a value for the 'openAILocation'
    infrastructure parameter** seleccione la región más cerca de su
    ubicación; en este laboratorio, hemos elegido la región **North
    Central US**

![](./media/image26.png)

9.  Aprovisionar el recurso tardará unos 5 - 10 minutos. Haga clic en
    **Yes** si le pide.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

10. Espere a que la plantilla para aprovisionar todos los recursos
    exitosamente.

![A screenshot of a computer Description automatically
generated](./media/image28.png)

11. Ejecute el siguiente command para establecer el siguiente resource
    group

\`\`azd env set AZURE_RESOURCE_GROUP {your resource group name}\`\`

![](./media/image29.png)

12. Ejecute el siguiente command para implementar la aplicación en
    Azure.

\`\`azd deploy\`\`

![](./media/image30.png)

13. Espere a que se complete la implementación. La implementación lleva
    \<5

![A screenshot of a computer Description automatically
generated](./media/image31.png)

14. Haga clic en el enlace de endpoint de aplicación web implementada.

![](./media/image32.png)

15. Haga clic en **Open**. Abre una nueva pestaña dentro de la
    aplicación

![](./media/image33.png)

16. Se abre la aplicación.

![A screenshot of a chat Description automatically
generated](./media/image34.png)

### Tarea 6: Use la aplicación chat para obtener respuestas basadas en los archivos

1.  En la página de web app the **RAG on database
    |OpenAI+PostgreSQL**, haga clic en **best shoe for hiking?** y
    observe el output

![](./media/image35.png)

![A screenshot of a computer Description automatically
generated](./media/image36.png)

2.  Haga clic en **clear chat.**

![](./media/image37.png)

3.  En la página de web app **RAG on database |OpenAI+PoastgreSQL**,
    haga clic en **Climbing gear cheaper than \\30** y observe el output

![](./media/image38.png)

![A screenshot of a computer Description automatically
generated](./media/image39.png)

4.  Haga clic en **clear chat.**

### Tarea 7: Verifique los recursos implementados en el Azure portal

1.  En la página de inicio de Azure portal, haga clic en **Resource
    Groups**.

![](./media/image40.png)

2.  Haga clic en el nombre de resource group

![](./media/image41.png)

3.  Asegure que se implementa los recursos siguientes de forma exitosa

    - Container App

    - Application Insights

    - Container Apps Environment

    - Log Analytics workspace

    - Azure OpenAI

    - Azure Database for PostgreSQL flexible server

    - Container registry

![](./media/image42.png)

4.  Haga clic en el nombre de recursos **Azure OpenAI**.

![](./media/image43.png)

5.  En **Overview** en el menú de navegación izquierdo, haga clic
    en **Go to Azure AI Foundry portal** y selecciónelo para abrir una
    nueva pestaña.

![](./media/image44.png)

6.  Haga clic en **Shared resources -\>** **Deployments** desde el menú
    de navegación izquierdo y asegure
    que **gpt-35-turbo**, **text-embedding-ada-002** se implementa
    exitosamente

![](./media/image45.png)

### Tarea 8 : Limpie los recursos 

Para limpiar todos los recursos creados en esta muestra:

1.  Cambie a **Azure portal -\> Resource group- \> Resource group
    name.**

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2.  Seleccione todos los recursos y a continuación haga clic en Delete
    como se ve en la imagen. (**NO ELIMINE** el resource group)

![](./media/image47.png)

3.  Tecle \`\`**delete**\`\` en la casilla de texto y haga clic en
    **Delete**.

![](./media/image48.png)

4.  Confirme la eliminación al hacer clic en **Delete**.

![](./media/image49.png)

5.  Cambie a la pestaña Github portal y actualice la página.

![A screenshot of a computer Description automatically
generated](./media/image50.png)

1.  Haga clic en **Code**, seleccione el branch creado para este
    laboratorio y haga clic en **Delete**.

![A screenshot of a computer Description automatically
generated](./media/image51.png)

2.  Confirme la eliminación de branch al hacer clic en el botón
    **Delete**.

![A screenshot of a computer Description automatically
generated](./media/image52.png)
