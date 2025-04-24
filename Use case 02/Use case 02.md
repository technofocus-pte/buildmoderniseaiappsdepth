# Caso de uso 02 – Construya una lista de frutas en Quarkus web app con Azure App Service en Linux y PostgreSQL

**Duración estimada:** 40 minutos

**Tipo del laboratorio:** dirigido por el instructor

**Objetivo:**

Este caso de uso nos muestra cómo construir, configurar e implementar
una aplicación Quarkus segura en Azure App Service que esté conectado a
un PostgreSQL database (mediante el Azure Database for PostgreSQL).
Azure App Service es un servicio de web-hosting altamente escalable y
self-patching que puede implementar aplicaciones en Windows o Linux de
forma muy fácil. Cuando termine, tendrá una Quarkus app ejecutando en
Azure App Service en Linux.

**Requisitos previos:**

**Una cuenta de GitHub** – Se espera tener sus propias credenciales de
inicio de sesión de GitHub. Si no las tiene, por favor cree una cuenta
desde aquí.
- +++<https://github.com/signup?user_email=&source=form-home-signup+++>

## Ejercicio 0: Comprensión de la VM y las credenciales

En esta tarea, identifiquemos y entendamos las credenciales que vamos a
usar a lo largo de este laboratorio.

1.  La pestaña **Instructions** tiene la guía del laboratorio con las
    instrucciones que seguir a lo largo del laboratorio.

2.  La pestaña **Resources** tiene las credenciales que se necesita para
    la ejecución del laboratorio.

    - **URL** – el URL que nos lleva al Azure portal

    - **Subscription** – Esta es la ID de la suscripción que le han
      asignado

    - **Username** – El user id con el que debe iniciar sesión en Azure
      services.

    - **Password** – La contraseña para iniciar sesión en Azure.

Llamemos el username y password como las credenciales de login en Azure.
Vamos a usar estas credenciales siempre y cuando mencionamos Azure login
credentials.

- **Resource Group** – El grupo de recursos que le asignaron.

\[!Atención\] **Importante:** Asegúrese de crear todos sus recursos en
este Resource group.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  La pestaña **Help** tiene la información de Support. El valor de
    **ID** aquí es el **Lab instance ID** que se usará durante la
    ejecución del laboratorio.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## Ejericicio 1: Ejecute una muestra

Primero, configure una aplicación de muestra basada en datos como un
punto de partida. El repositorio de muestra que usamos aquí incluye una
configuración de container de desarrollador. El dev container tiene todo
lo que encesita para desarrollar una aplicación, incluidos el database,
caché, y todas las variables que necesita la aplicación de muestra. El
dev container puede funcionar en un GitHub codespace, lo que significa
que puede ejecutar la muestra en cualquier ordenador con un navegador
web.

1.  Desde un navegador, inicie sesión en su cuenta de
    GitHub +++\*\*<https://github.com/login**+++>.

2.  Abra este URL desde una nueva
    pestaña, +++\*\*<https://github.com/technofocus-pte/msdocs-quarkus-postgresql-sample-app**+++>.

3.  Seleccione **Fork -\> Create a new fork**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.jpeg)

4.  Haga clic en **Create fork** en la página Create a new fork.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

5.  En la forked page del repositorio, seleccione **Code** \> **Create
    codespace on main**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.jpeg)

**Ojo:** Si no aparece Create Codespace en la opción principal/main
option, haga clic en el símbolo + symbol junto a Codespaces.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

**Ojo:** la creación de codespace lleva alrededor de 10 minutos para
configurarse.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

6.  Ejecute +++mvn quarkus:dev+++ en el Terminal. Haga clic
    en **Allow** en el pop up.

![A screenshot of a browser AI-generated content may be
incorrect.](./media/image8.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

7.  Cuando ve la notificación **Your application running on port
    8080** **is available.**, seleccione **Open in Browser**. Deberá ver
    la aplicación de muestra en la pestaña del navegador.

Si ve una **notificación** con port **5005**, **sáltelo**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

8.  Para terminar el servidor de desarrollo de Quarkus,
    tecle **Ctrl+C** en el Codespace terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

## Ejercicio 2: Cree el App Service y PostgreSQL

Primero, cree los recursos de Azure. Los pasos incluidos en este
laboratorio crean un conjunto de recursos seguros-por-defecto que
incluyen App Service y Azure Database for PostgreSQL.

1.  Abra el Azure portal en +++<https://portal.azure.com/+++> e **inicie
    sesión** con los Azure login credentials desde la
    pestaña **Resources** de la VM.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

2.  Seleccione **Cancel** o el botón de close en la página Welcome.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.jpeg)

3.  Introduzca +++**web app database**+++ en la barra de búsqueda en la
    parte superior del Azure portal. Seleccione el artículo
    etiquetado **Web App + Database** en el encabezado **Marketplace**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.jpeg)

4.  En **Create Web App + Database**, introduzca los siguientes datos y
    seleccione **Review + create**

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)
>
> ![A screenshot of a web application AI-generated content may be
> incorrect.](./media/image17.jpeg)

5.  Una vez que la validación sea exitosa, haga clic en **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

**Ojo:** La creación de la aplicación lleva aalrededor de 15 minutos.

6.  Una vez que termine la implementación, haga clic en **Go to
    resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

7.  Se le lleva directamente a la **página** **App Service.** Haga clic
    en **Home** en la esquina superior izquierdo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.jpeg)

8.  Haga clic en el menú Portal y, desde ahí, seleccione **Resource
    Groups**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.jpeg)

9.  Seleccione el Resource group que le asignaron y asegure que se han
    creado los siguientes recursos como resultado de la implementación
    que acabamos de ejecutar.

> \- App Service plan
>
> \- App Service
>
> \- Virtual network
>
> \- Azure Database for PostgreSQL flexible server
>
> \- Private DNS zone

![A screenshot of a group AI-generated content may be
incorrect.](./media/image22.png)

## Ejercicio 3: Verifique las configuraciones de conexión

El wizard de creación ya le ha generado las variables de conectividad
como la configuración de la aplicación. En este paso, aprenderá dónde se
encuentra la app settings y cómo se puede crear unas propias
configuraciones.

1.  Haga clic en **App Service** desde la lista de recursos en el
    Resource group.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image23.jpeg)

2.  En la página de App Service, desde el menú izquierdo,
    seleccione **Environment variables** en **Settings**.

3.  En la pestaña **App settings** de la página **Environment
    variables**, asegure que aparece
    el **AZURE_POSTGRESQL_CONNECTIONSTRING**. Es inyectado en runtime
    como una variable del entorno/environment variable.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.jpeg)

4.  Seleccione **+ Add**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.jpeg)

5.  Asigne un nombre a la configuración +++**PORT**+++ y establezca su
    valor a +++**8080**+++, lo que es el default port de la aplicación
    Quarkus. Seleccione **Apply**.

![A screenshot of a login AI-generated content may be
incorrect.](./media/image26.jpeg)

6.  Seleccione **Apply**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.jpeg)

7.  Seleccione **Confirm**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

8.  Recibirá una notificación diciendo que se han actualizado las
    configuraciones de la aplicación.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image29.jpeg)

## Ejercicio 4: Implemente el código de muestra

En este paso, configurará el GitHub deployment mediante GitHub Actions.
No es solo una de muchas maneras de implementar en App Service, pero
también una gran manera de tener una integración continúa en su proceso
de desarrollo. De forma predeterminada, cada git push a su repositorio
de GitHub arrancará la acción de build and deploy.

1.  En la página de App Service, desde el menú izquierdo,
    seleccione **Deployment Center** en **Deployment**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

2.  En Source, seleccione **GitHub**. De forma predeterminada, se
    selecciona GitHub Actions como el proveedor de build.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.jpeg)

3.  Haga clic en **Authorize** e inicie sesión en su cuenta de GitHub y
    siga el prompt para autorizar el Azure.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image32.jpeg)

4.  Introduzca los siguientes detalles, deje el resto al valor
    predeterminado y haga clic en **Save**.

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image33.jpeg)

5.  Una vez que haga clic en **Save**, App Service rinde un archivo de
    workflow en el repositorio de GitHub elegido, en el directorio
    .github/workflows.

6.  Volviendo a GitHub codespace de su sample fork, ejecute +++**git
    pull origin main**+++. Esto consigue un archivo de workflow recién
    comprometido en su codespace.

\[!Nota\] **Ojo:** Si encuentra casos de prueba que siguen ejecutando en
el terminal, puede teclar Ctrl+C y, a continuación, ejecute el command
anterior.

![A screenshot of a computer code AI-generated content may be
incorrect.](./media/image34.jpeg)

7.  Abra **src/main/resources/application.properties** en el explorador.
    Quarkus utiliza este archivo para cargar las prorpiedades de Java.

8.  Encuentre el código (líneas 10-11). Este código establece la
    variable de la producción **%prod.quarkus.datasource.jdbc.url** a la
    configuración de la aplicación proporcionado por el creation wizard
    para usted. El **quarkus.package.type** está preparado para
    construir un Uber-Jar, que usted necesita ejecutar en su App
    Service.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.jpeg)

9.  Abra **.github/workflows/main_quarkuwebapp\[lab instance
    id\].yml** en el explorador. El archivo fue creado por el App
    Service create wizard.

10. En el paso Build with Maven, cambie el comando Maven a +++**mvn
    clean install -DskipTests**+++.

**-DskipTests** salta las pruebas en su proyecto Quarkus, para evitar el
fallo precipitado de GitHub workflow.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

11. Seleccione la extensión **Source Control**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

12. En el cuadro de texto, tecle un commit message como +++**Configure
    DB and deployment workflow**+++. Seleccione **Commit**, y, a
    continuación, confirme al seleccionar **Yes**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.jpeg)

13. Seleccione **Sync changes 1**, y luego confirme al
    seleccionar **OK**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

14. Volviendo a la página Deployment Center en el portal de Azure,
    Seleccione **Logs**. Un nuevo deployment run ya habría iniciado
    desde sus cambios comprometidos.

15. En el log item para el deployment run, seleccione la
    entrada **Build/Deploy Logs** con el último timestamp.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.jpeg)

16. Se le lleva al repositorio de GitHub y ve que se ejecuta la GitHub
    action. El archivo workflow define dos pasos difererentes, build y
    deploy. Espere a que el GitHub run muestre un estado de Complete.
    Lleva alrededor de 5 minutos.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image42.jpeg)

## Ejercicio 5: Navegue a la app

1.  Desde el Azure
    portal(+++[https://portal.azure.com+++](https://portal.azure.com+++/)),
    abra el grupo de recursos **ResourceGroup1** y seleccione el
    recurso **App Service**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

2.  Desde el menú izquierdo, seleccione **Overview** y seleccione el URL
    de su aplicación junto a **Default domain**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

3.  Pegue el URL copiado en un nuevo navegador para abrir la aplicación.

![A screenshot of a fruit list AI-generated content may be
incorrect.](./media/image45.jpeg)

4.  Agregue unas frutas a la lista. Ahora, está ejecutando una
    aplicación web en Azure App Service, con una conectividad segura a
    Azure Database for PostgreSQL.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.jpeg)

## Ejercicio 6: Agilice los diagnostic logs

Azure App Service captura todos los mensajes de salida a la consola para
ayudarle a diagnosticar los problemas de su aplicación. La aplicación de
muestra incluye las instrucciones de registro JBoss estándares para
demostrar esta capacidad, como se menciona a continuación:

1.  Desde la página de Azure portal App Service, desde el menú
    izquierdo, seleccione **App Service logs** en **Monitoring**.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image48.jpeg)

2.  En **Application logging**, seleccione **File System**. En el menú
    de arriba, seleccione **Save**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.jpeg)

3.  Desde el menú izquierdo, seleccione **Log stream**. Ve los logs para
    su aplicación, incluidos los registros de la plataforma y registros
    dentro del container.

![A computer screen shot of a computer screen AI-generated content may
be incorrect.](./media/image50.jpeg)

## Ejercicio 7: limpie los recursos

1.  Desde la página de inicio de Azure portal Home, seleccione Resource
    groups.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

2.  Seleccione el **NetworkWatcherRG** y haga clic en **Delete resource
    group**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

3.  Tecle +++NetworkWatcherRG+++ en el cuadro de texto y haga clic
    en **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image54.png)

4.  A continuación, desde la página Resource, seleccione el Resource
    group asignado a usted.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

5.  Seleccione todos los **resources**, y luego seleccione **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

6.  Tecle +++**delete**+++ en el cuadro de texto y haga clic
    en **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image58.png)

7.  Una notificación de éxito de la eliminación de recursos confirma la
    eliminación.

8.  Volviendo a GitHub Workspace, haga clic en el menú despegable junto
    a **Code**, seleccione los tres puntos junto a codespace name y haga
    clic en **Delete**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

**Resumen:**

Hemos aprendido cómo implementar una aplicación Quarkus segura en Azure
App Service, conectarla a PostgreSQL database para añadir nombres de
frutas desde el UI de la aplicación.
