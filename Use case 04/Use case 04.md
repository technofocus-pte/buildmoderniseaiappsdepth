# Caso de uso 04 – Construya un TODO List ASP.NET app, impleméntelo a Azure App Service conectándose al SQL Database

**Duración estimada:** 40 minutos

**Tipo del laboratorio:** dirigido por el instructor

**Objetivo:**

Azure App Service proporciona un servicio de web-hosting altamente
escalable y self-patching. En este laboratorio, aprenderá cómo
implementar una ASP.NET app basada en datos en App Service y conectarla
al Azure SQL Database. Cuando termine, tendrá un ASP.NET app ejecutando
en Azure y conectado al SQL Database.

## Ejercicio 0: Comprensión de la VM y las credenciales 

En esta tarea, identifiquemos y entendamos las credenciales que vamos a
usar a lo largo de este laboratorio.

1.  La pestaña **Instructions** tiene la guía del laboratorio con las
    instrucciones que seguir a lo largo del laboratorio.

2.  La pestaña **Resources** tiene las credenciales que se necesita para
    la ejecución del laboratorio.

    - **URL** – el URL que nos lleva al Azure portal

    - **Subscription** – Esta es la ID de la suscripción que le han
      asignado

    - **Username** – El user id con el que debe iniciar sesión en los
      servicios de Azure.

    - **Password** – La contraseña para iniciar sesión en Azure.
      Llamemos el username y password como las credenciales de login en
      Azure. Vamos a usar estas credenciales siempre y cuando
      mencionamos Azure login credentials.

    - **Resource Group** – El grupo de recursos que le asignaron.

\[!Atención\] **Importante:** Asegúrese de crear todos sus recursos en
este Resource group.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  La pestaña **Help** tiene la información de Support. El valor
    de **ID** aquí es el **Lab instance ID** que se usará durante la
    ejecución del laboratorio.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

## Ejercicio 1: Implementación de una ASP.NET app a Azure con Azure SQL Database

### Tarea 1: Confugure el Visual Studio 2022 y ejecute la aplicación

1.  Desde la barra de Windows **Search**, tecle +++**Visual studio**+++
    y seleccione Visual Studio 2022. Si le pide que inicie sesión, siga
    los pasos 2 y 3 o continúe con el paso 4.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.jpeg)

2.  Haga clic en **Sign in** e inicie sesión con
    los **Username** y **password** en la sección **User
    Credentials** en la pestaña VM Resources.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

3.  Seleccione **Start Visual Studio**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.jpeg)

4.  Seleccione **Open a local folder**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.jpeg)

5.  Seleccione la carpeta **webappwithsqldb** en **C:\Labfiles** y haga
    clic en **Select Folder**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

6.  Una vez que se abre la carpeta, haga doble clic
    en **DotNetAppSqlDb.sln** desde el **Solution Explorer**.

**Ojo:** Si no se abre el Solution Explorer de forma automática, haga
clic en **View -\> Solution Explorer.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.jpeg)

7.  Haga clic en **Build** -\> **Build Solution**.

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image10.jpeg)

8.  Una vez que se complete el Build, seleccione **Debug -\>** **Start
    Debugging**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.jpeg)

9.  Esto hace que abra un navegador con la **Todos web app** ejecutando
    en ello.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.jpeg)

10. Agregue unos elementos en la aplicación al hacer clic en **Create
    New** como se muestra en las capturas de pantalla abajo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.jpeg)

![A screenshot of a application AI-generated content may be
incorrect.](./media/image15.jpeg)

11. Agregue unos elementos más a la lista.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.jpeg)

12. Desde el Visual Studio 2022, haga clic en **Debug -\>** **Stop
    Debugging**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.jpeg)

### Tarea 2: Publique la aplicación ASP.NET a Azure

1.  En el **Solution Explorer**, haga clic derecho en su proyecto
    **DotNetAppSqlDb** y seleccione **Publish**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.jpeg)

2.  Seleccione **Azure** y haga clic en **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.jpeg)

3.  Seleccione **Azure App Service (Windows)** en la pantalla **Which
    Azure service would you like to use to host your application?** Y
    haga clic en **Next**.

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image21.jpeg)

4.  En el diálogo Publish, haga clic en **Sign In** e inicie sesión a su
    suscipción de Azure, si todavía no lo ha hecho.

**Ojo:** Si ya ha iniciado sesión en su cuenta de Microsoft, asegúrese
de que la cuenta tenga su suscripción del Azure. Si la cuenta con la que
ha iniciado sesión no tiene su suspripción de Azure, vuelva a hacer clic
en ello para agregar la cuenta correcta.

5.  Haga clic en **Create new** para crear un nuevo App service.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.jpeg)

6.  Introduzca los siguientes datos.

[TABLE]

7.  Haga clic en **New** debajo de **Hosting Plan**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image23.png)

8.  Haga clic en **New** debajo de la opción **Hosting Plan** e
    introduzca los siguientes detalles y haga clic en **OK**.

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

9.  Haga clic en **Create** en la pantalla App Service y espere a que se
    crean los recursos de Azure.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image25.png)

10. El diálogo **Publish** muestra los recursos que ha configurado. Haga
    clic en **Finish**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

11. Haga clic en **Close**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image27.jpeg)

12. Baje hasta la sección Server Dependencies y haga clic en el
    símbolo **+** para agregar la dependency.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.jpeg)

13. Seleccione **Azure SQL Database** en la página **Add dependency** y
    haga clic en **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.jpeg)

14. Haga clic en **Create New** junto a SQL databases, en el cuadro de
    diálogo **Connect to Azure SQL Database**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.jpeg)

15. En el cuadro de diálogo **Azure SQL Database Create new**, haga clic
    en **New** junto al Database server.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

16. Introduzca los siguientes detalles y haga clic en **OK**.

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

17. Haga clic en **Create** en el diálogo Create new.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

### Tarea 3: Configure la conexión del database

1.  Cuando el wizard termina de crear los recursos de bases de datos,
    haga clic en **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

2.  Introduzca los siguientes detalles en el diálogo **Connect to Azure
    SQL Database** y haga clic en **Finish**.

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.jpeg)

3.  Haga clic en **Finish** despúes de revisar el cuadro de **summary of
    changes**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

4.  Espere a que el configuration wizard termine y haga clic
    en **Close**. El Azure SQL Db ahora está **conectado** a su
    aplicación.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

5.  Desde la página Publish, haga clic en **Publish** en la esquina
    superior derecha.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

**Ojo:** Esto llevará alrededor de 5 minutos

6.  Una vez que se implementa su ASP.NET app en Azure, su navegador
    predeterminado se ejecuta con el URL a su aplicación
    implementada. **Agregue unos artículos *to-do***.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.jpeg)

### Tarea 4: Acceda el base de datos de forma local

Visual Studio le permite explorar y gestionar de forma fácil su nuevo
database en Azure en el **SQL Server Object Explorer**. El nuevo base de
datos ya ha abierto su firewall a la aplicación App Service que creó
usted. Pero para poder acceder a ello desde su ordenador local (por
ejemplo desde Visual Studio), debe abrir un firewall para sus
direcciones IP públicos de sus máquinas locales IP address. Si su
proveedor de servicios de internet cambia su dirección IP, tiene que
reconfigurar el firewall para acceder al Azure database de nuevo.

1.  Desde el Visual Studio 2022, el menú despegable de **View**,
    seleccione **SQL Server Object Explorer**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

2.  En la parte superior de **SQL Server Object Explorer**, haga clic en
    el botón **Add SQL Server**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.jpeg)

### Tarea 5: Configure la conexión del database

1.  En el diálogo **Connect**, expanda el **Azure** node. Todas sus
    instancias de SQL Database de Azure están enumerados aquí.

2.  Seleccione el database que creó anteriormente
    (**dotnetappsqldbdbserver98**). La conexión que creó anteriormente
    se introduce automáticamente en la parte inferior.

3.  Tecle la **contraseña** del administrador de database que usted creó
    antes(+++**PassWord98**+++) y haga clic en Connect.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.jpeg)

### Tarea 6: Permite las conexiones de los clientes desde su ordenador

Se abre el diálogo Create a new firewall rule. De forma predeterminada,
un servidor solo permite las conexiones a sus databases desde Azure
services, por ejemplo, su aplicación Azure. Para conectarse a su
database desde fuera del Azure, cree un firewall rule a nivel del
servidor. El firewall rule permite la dirección IP pública de su
ordenador local.

El diálogo ya tiene la dirección IP pública de su ordenador.

1.  Asegúrese de que se haya seleccionado *Add my client IP* y haga clic
    en OK.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.jpeg)

2.  Una vez que Visual Studio termina de crear la configuración de
    firewall para su SQL Database instance, su conexión aparece en **SQL
    Server Object Explorer**.

3.  Expanda su your **connection \> Databases \> \< YOUR DATABASE \> \>
    Tables**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

4.  Haga clic derecho en la tabla **Todoes** y seleccione **View Data**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

5.  Visualice el contenido de la tabla. Los datos agregados desde la app
    UI deberán estar ahí.

![](./media/image46.jpeg)

## Ejercicio 2: Actualice la aplicación con Code First Migrations

1.  Desde el **Solution Explorer**, abra **Models\Todo.cs** en el code
    editor. Agregue la siguiente propriedad a la clase **ToDo** como la
    última línea (despúes de la línes **public DateTime CreatedDate {
    get; set; }** ) y haga clic en **Save**.

+++**public bool Done { get; set; }**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.jpeg)

### Tarea 1: Ejecute Code First Migrations de forma local

Ejecute unos commands para actualizar su base de datos local.

1.  Desde el menú **Tools**, haga clic en **NuGet Package
    Manager** \> **Package Manager Console**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image48.jpeg)

2.  En la pantalla Package Manager Console, habilite Code First
    Migrations ejecutando este command.

+++**Enable-Migrations**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.jpeg)

3.  Agregue un migration ejecutando el siguiente command.

+++**Add-Migration AddProperty**+++

![](./media/image50.jpeg)

4.  Actualice el database local ejecutando el siguiente command.

+++**Update-Database**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.jpeg)

5.  Tecle **Ctrl+F5** para ejecutar el app o haga clic en **Debug -\>
    Start without Debugging**. Pruebe los enlaces de edit, details, y
    create.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image52.jpeg)

6.  Se abre la página de application y todavía se ve lo mismo porque su
    application logic no está usando esta nueva propriedad.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.jpeg)

### Tarea 2: Use la nueva property

Haga algunos cambios en su código para usar la Done property.

1.  Desde Visual Studio, abra **Controllers\TodosController.cs**.
    encuentre el método **Create()** en la línea 52 y agregue
    +++**Done**+++ a la lista de propriedades en el Bind attribute.
    Cuando termine, su Create() method signature se verá así como el
    siguiente código:

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

2.  Abra **Views\Todos\Create.cshtml**. agregue el siguiente código
    después del \< div class="form-group" \> for **CreatedDate**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image55.jpeg)

\<div class="form-group"\>

@Html.LabelFor(model =\> model.Done, htmlAttributes: new { @class =
"control-label col-md-2" })

\<div class="col-md-10"\>

\<div class="checkbox"\>

@Html.EditorFor(model =\> model.Done)

@Html.ValidationMessageFor(model =\> model.Done, "", new { @class =
"text-danger" })

\</div\>

\</div\>

\`\`\`

3.  Abra **Views\Todos\Index.cshtml**. Agregue el siguiente código en el
    elemento **th** vacío, después del elemento **th** para el
    **CreatedDate**.

<+++@Html.DisplayNameFor>(model =\> model.Done)+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image56.jpeg)

4.  Agregue este código justo arriba de los html.ActionLink() helper
    methods.

> \<td\>
>
> @Html.DisplayFor(modelItem =\> item.Done)

\`\`\`

\![\](./media/image53.jpeg)

5.  Tecle **Ctrl+F5** para ejecutar la app.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.jpeg)

### Tarea 3: Habilite las Code First Migrations en Azure

1.  Haga clic derecho en el proyecto y seleccione **Publish**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

2.  Haga clic en **More actions** \> **Edit** para abrir las
    configuraciones de publish.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.jpeg)

3.  En el menú despegable **MyDatabaseContext**, seleccione la conexión
    de database para su Azure SQL Database.

4.  Seleccione **Execute Code First Migrations** (que se ejecuta al
    inicio de la aplicación), y, a continuación, haga clic en **Save**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image60.jpeg)

5.  En la página Publish, haga clic en **Publish**.

![A black rectangular object with white text AI-generated content may be
incorrect.](./media/image61.jpeg)

6.  La aplicación actualizada ahora está disponible en Azure.

7.  Intente agregar los artículos to-do de nuevo y seleccione **Done**,
    y deberán aparecer en su página de inicio como un artículo
    completado.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image62.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image63.jpeg)

## Ejercicio 3: emisión de registros de aplicación

1.  En la página publish, baje hasta la sección **Hosting**. En la
    esquina superior derecha, haga clic en **...** \> **View Streaming
    Logs**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image64.jpeg)

2.  Los registros ahora se emiten en la pantalla de salida/Output.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image65.jpeg)

3.  Todavía no se ve ningún trace messages porque, cuando selecciona
    View Streaming Logs por primera vez, su Azure app establece el nivel
    de trace a Error, lo que solo registra los eventos de error.

\[!Nota\] **Ojo:** Reinicie el logging stream desde el Visual Studio si
todavía no lo ve.

### Tarea 1: Cambie los trace levels

1.  Vaya a la página de publish. En la sección de Hosting, haga clic
    en **… \> Open in Azure portal**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image66.jpeg)

2.  En el Azure portal – app page, seleccione **App Service Logs** desde
    el panel izquierdo debajo de la sección **Monitoring**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image67.jpeg)

3.  En **Application Logging** (File System), seleccione **Verbose** en
    Level. Haga clic en **Save**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image68.jpeg)

4.  Desde su navegador, acceda la web app en Azure y haga unas
    actividades.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image69.jpeg)

5.  Los trace messages ahora se emiten a la pantalla Output en Visual
    Studio.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image70.jpeg)

6.  Para terminar el log-streaming service, haga clic en **Stop
    monitoring** button en la pantalla Output.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image71.jpeg)

7.  Cierre el Visual Studio.

## Ejercicio 4: Limpie los recursos

1.  Desde el Azure portal, abra su grupo de recursos.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image72.png)

2.  Seleccione todos los recursos y haga clic en Delete.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image73.png)

3.  Tecle +++delete+++ en el cuadro de texto y seleccione Delete.
    Seleccione **Delete** de nuevo en el cuadro de diálogo de
    confirmación.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image74.png)

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image75.png)

**Resumen**

En este laboratorio, ha aprendido a implementar una ASP.NET app basada
en datos en App Service y conectarla a Azure SQL Database.
