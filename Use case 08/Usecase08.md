# Caso de uso 08 – Construir e implementar Contoso Real Estate chat app para ayudar a los clientes.

**Objetivo**

Este caso de uso demuestra unas maneras de crear experiencias similares
al ChatGPT sobre sus propios datos mediante el Retrieval Augmented
Generation pattern. Usa Azure OpenAI Service para acceder al ChatGPT
model (gpt-35-turbo), y Azure AI Search para la indexación de datos y
recuperación.

![A diagram of a software process Description automatically
generated](./media/image1.jpeg)

Este caso de uso incluye datos de muestra para que esté listo para
probar de principio a fin. En esta aplicación de muestra, usamos una
empresa imaginaria llamada Contoso Real Estate, y la experiencia les
permite a los clientes preguntar cosas de ayuda sobre el uso de sus
productos. Los datos de muestra incluyen un conjunto de documentos que
describen sus términos de servicio, políticas de privacidad y guía de
soporte.

La aplicación se crea con múltiples componentes, incluyendo:

- **Search service**: el servicio backend que proporciona capacidades de
  búsqueda y recuperación.

- **Indexer service**: el service que indexa los datos y crea search
  indexes.

- **Web app**: la aplicación web application en frontend que proporciona
  el interfaz del usuario y orquesta la interacción entre el usuario y
  servicios backend.

![A diagram of a software system Description automatically
generated](./media/image2.jpeg)

- Interfaces de chat y Q&A

- Explora varias opciones para ayudar a los usuarios evaluar la
  fiabilidad de las resupuestas con citationes, rastreo del contenido
  raíz, et cetera.

- Muestra los enfoques posibles para la preparación de datos,
  construcción de prompt y orquestación de la interación entre el modelo
  (ChatGPT) y recuperador (Azure AI Search)

- Las configuraciones integradas en el UX para alterar el comportamiento
  y experimentar con las opciones

- Rastreo de rendimiento y monitoreo opcionales con Application Insights

**Tecnologías clave** -- Azure OpenAI Service, ChatGPT model
(gpt-35-turbo), y Azure AI Search

**Duración estimada --** 40 minutos

## Ejercicio 1: implemente la aplicación y prúebelo desde el navegador

### Tarea 1: Abra el entorno de desarrollo 

1.  Abra su navegador, navegue a la barra de direcciones, tecle o pegue
    el siguiente
    URL: \`\`https://github.com/technofocus-pte/azure-search-openai-javascript\`\` e
    inicie sesión con su cuenta Github.

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

2.  Haga clic en **Fork**.

![A screenshot of a web page Description automatically
generated](./media/image4.jpeg)

3.  Introduzca el nombre del repositorio y haga clic en **Create fork**.

![A screenshot of a computer Description automatically
generated](./media/image5.jpeg)

4.  Haga clic en **Code -\> Codespaces -\> +**

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

5.  Espere a que se complete la configuración. Tarda unos 5-10 minutos.

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

### Tarea 2: Aprovisione los servicios requeridos para construir e implementar el chat app a Azure

1.  Ejecute el siguiente command en el Terminal. Copie el código y
    presione enter.

\`\`azd auth login\`\`

![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  Se abre el navegador predeterminado para introducir el código.
    Indroduzca el código copiado y haga clic en **Next**.

![](./media/image9.png)

3.  Inicie sesión con sus credenciales Azure.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

![A screenshot of a computer error Description automatically
generated](./media/image11.png)

6.  Cambie a la pestaña Github Codespace. Ejecute el siguiente command
    para iniciar el project environment en el directorio actual.
    Introduzca el Environment name como \`\`**ragpgpy \`\`** y presione
    Enter.

Ojo : env name tiene que ser único

\`\` azd env new\`\`

![](./media/image12.png)

7.  Ejecute el siguiente command para aprovisionar los servicios a
    Azure, construya su container.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

8.  Seleccione los valores siguientes.

> \`\`azd provision\`\`

- **Select an Azure Subscription to use** : seleccione su suscripción

- **Select an Azure location to use** : **East us2/west us2** (a veces,
  East US no está disponible, elija una ubicación desde el listado
  mencionado.)

- Select existing resource group : su resource group actual (por ejemplo
  :**ResourceGroup1 )**

![](./media/image14.png)

9.  Espere a que se complete el aprovisionamiento del recurso. Este
    proceso llevará 5-10 minutos para crear todos los recursos
    requeridos.

> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)

### Tarea 3: implemente el chat app y explórelo

10. Ejecute el siguiente command para implementar la aplicación.

\`\`azd deploy\`\`

![](./media/image16.png)

11. Espere para la implementación. Tarda unos 5 minutos.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

12. Haga clic en el endpoint url generado.

![](./media/image18.png)

13. Haga clic en **Open**.

![](./media/image19.png)

14. Abre una aplicación en una nueva pestaña.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

15. Seleccione **How to search and book rental?** Container y haga clic
    en el botón enter junto al text box.

![](./media/image21.png)

### Tarea 4: Limpie todos los recursos

1.  Vuelva a **Azure portal -\> Resource group- \> Resource group
    name.**

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  Seleccione todos los recursos y haga clic en Delete como se ve aquí.
    (**NO ELIMINE** el resource group)

![](./media/image23.png)

3.  Tecle \`\`**delete**\`\` en el cuadro de texto y haga clic en
    **Delete**.

![](./media/image24.png)

4.  Confirme la eliminación al hacer clic en **Delete**.

![](./media/image25.png)

5.  Cambie a Github portal tab y haga refresh en la página.

![A screenshot of a computer Description automatically
generated](./media/image26.png)

6.  Haga clic en Code, seleccione el branch creado para este laboratorio
    y haga clic en **Delete**.

![](./media/image27.png)

7.  Confirme la eliminación de branch al hacer clic en **Delete**.

![](./media/image28.png)

### Resumen:

Este caso de uso le enseñó la implementación de la aplicación Retrieval
Augmented Generation pattern funcionando en Azure, mediante Azure AI
Search para la recuperación y Azure OpenAI y LangChain large language
models (LLMs) para impulsar el estilo ChatGPT- y las experiencias Q&A
