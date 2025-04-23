# Caso de uso 05- Implementar un Python Restaurant web app basado en datos con Azure Database for PostgreSQL

**Objetivo:**

Este caso de uso implementa un Python web app mediante el Flask
framework y el servicio de database relacional de Azure Database for
PostgreSQL. EL Flask app se aloja en un fully managed Azure App Service.
Esta aplicación se diseña para que sea ejecutada de forma local e
implementada en Azure

![A diagram of a service plan Description automatically
generated](./media/image1.jpeg)

Implementará un Python web app basada en datos (**Django** o **Flask**)
en **Azure App Service** con el servicio de database relacionado **Azure
Database for PostgreSQL**. Azure App Service admite Python en un Linux
server environment.

**Tecnologías clave** -- Java 17, Azure Database for PostgreSQL

**Duración estimada** -- 45 minutos

**Tipo del laboratorio:** dirigido por el instructor

**Prerequisitos:**

Cuenta de GitHub– se espera tener sus propias credenciales de GitHub. Si
no las tiene, puede crear unas aquí
- **https://github.com/signup?user_email=&source=form-home-signupobjectives**

El **requirements.txt** tiene los siguientes packages, todos usados por
una aplicación típica de Flask basada en datos:

[TABLE]

### Tarea 1: registre el Service provider

1.  Abra un navegador y vaya a <https://portal.azure.com> e inicie
    sesión con su cuenta cloud slice disponible en la pestaña Resource
    de su VM.

> ![](./media/image2.png)

2.  En la página de inicio de Azure portal, haga clic en **Resource
    groups**.

![](./media/image3.png)

3.  Copie el nombre del resource group y gúardelo en un notepad para
    utilizarlo en la siguiente tarea para implementar recusos necesarios
    en este resource group.

![](./media/image4.png)

4.  En la parte superior, haga clic en Home.

![](./media/image5.png)

5.  Haga clic en **Subscriptions**.

![](./media/image6.png)

6.  Haga clic en el subscription name.

![](./media/image7.png)

7.  Expanda Settings desde el menú de navegación izquierdo. Haga clic en
    **Resource providers**, introduzca Microsoft.AlertsManagement y
    selecciónelo y haga clic en **Register**.

> ![](./media/image8.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image9.png)

### Tarea 2: Cree Github Codespace para iniciar la plantilla Azure Developer CLI

Este caso de uso tiene una configuración de dev container, lo que
facilita el desarrollo de aplicaciones de forma local, la implementación
a Azure y su supervisión. Usamos las plantillas de Azure development CLI
para implementar las aplicaciones

1.  Abra un navegador y vaya a \`\`**https:\\github.com\`\`** e inicie
    sesión con su cuenta de Github.

2.  Haga un fork de
    repositorio https://github.com/technofocus-pte/msdocs-flask-postgresql-sample-app a
    su cuenta al hacer clic en **Fork** como se ve en la imagen.

![A screenshot of a computer Description automatically
generated](./media/image10.jpeg)

3.  Introduzca un nombre único y haga clic en **Create repo**.

![A screenshot of a computer Description automatically
generated](./media/image11.jpeg)

4.  Desde el repositorio root de su fork,
    seleccione **Code** \> **Codespaces** \> **+**.

![A screenshot of a computer Description automatically
generated](./media/image12.jpeg)

5.  Espere a que complete la configuración del workspace.

![A screenshot of a computer Description automatically
generated](./media/image13.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

6.  En el terminal codespace, ejecute los siguientes commands:

> \# Install requirements

\`\`python3 -m pip install -r requirements.txt\`\`

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

7.  Ejecute el siguiente command para crear la variable del entorno

> \# Create .env with environment variables

\`\`cp .env.sample.devcontainer .env\`\`

![A screenshot of a computer program Description automatically
generated](./media/image16.jpeg)

8.  Ejecute el siguiente command para la migración de datos

> \# Run database migrations

\`\`python3 -m flask db upgrade\`\`

![A screenshot of a computer program Description automatically
generated](./media/image17.jpeg)

9.  Ejecute el siguiente command para:

> \# Start the development server

\`\`python3 -m flask run\`\`

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

10. Cuando ve el mensaje Your application running on port is available.,
    haga clic en **Open in Browser**.

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image19.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image20.jpeg)

11. Haga clic en el botón **Add new restaurant**.

![A white screen with black text Description automatically
generated](./media/image21.jpeg)

12. Introduzca estos detalles y haga clic en el botón **Submit**.

Name : \`\`**Contoso Rica\`\`**

Street Adress - \`\`**3A ,8th cross, Ferns street , Singapore\`\`**

Description - \`\`**This is a medium to high priced restaurant in the
city shopping center\`\`**

![A screenshot of a restaurant Description automatically
generated](./media/image22.jpeg)

13. Haga clic en **Add new review**.

![A screenshot of a computer Description automatically
generated](./media/image23.jpeg)

14. Introduzca su reseña y haga clic en el botón **Save changes**

**Your name : your name**

**Rating : your rating**

\`\`This is a medium to high priced restaurant in the city shopping
center. Service was a little bit confusing as we had at least 6 waiters
coming to ask us things. Food took some time to come. We had 2 menus:
one indian and one thai. The thai is 30% cheaper so we went for some
appetizers and thai red curry. Food took some time but it was worth it.
It was delicious and very well prepared. Overall, this is a good
eat.\`\`

![A screenshot of a computer Description automatically
generated](./media/image24.jpeg)

![A white card with black text Description automatically
generated](./media/image25.jpeg)

15. Agregue unas reseñas más y un nuevo restaurante con comentarios.

![A screenshot of a computer Description automatically
generated](./media/image26.jpeg)

### Tarea 3: Aprovisione el recurso necesario en Azure.

Este proyecto está diseñado para funcionar bien con el Azure Developer
CLI para facilitar el desarrollo de apliacciones de forma local,
implementarlas en Azure, y monitorearlas.

1.  Cambie a la pestaña del Github codespace, ejecute el siguiente
    command para iniciar un nuevo entorno azd:

\`\`azd init\`\`

![](./media/image27.jpeg)

2.  Le pedirá que proporcione un nombre del entorno (como
    **flask-app**XXXX (XXXX puede ser un nombre único)), lo cual vamos a
    usar más tarde en el nombre de los recursos implementados.

![](./media/image28.jpeg)

3.  Inicie sesión se se requiere \`\`**azd auth login\`\`**. Copie el
    código y presione enter.

![A screenshot of a computer Description automatically
generated](./media/image29.jpeg)

4.  Introduzca el código e inicie sesión con sus credenciales de Azure.

![A screenshot of a computer error Description automatically
generated](./media/image30.jpeg)

![](./media/image31.jpeg)

![A screenshot of a computer error Description automatically
generated](./media/image32.jpeg)

![A screenshot of a computer program Description automatically
generated](./media/image33.jpeg)

5.  Cambie a la pestaña Gtihub codespace y ejecute el siguiente command
    para aprovisionar e implementar todos los recursos. Le pedirá
    seleccionar su suscripción de Azure. Introduzca **1** para
    seleccionar su suscripición y presione Enter.

**\`\`azd provision\`\`**

![A computer screen shot of a computer code Description automatically
generated](./media/image34.png)

6.  Seleccione el location como **WestUS/eastus**. A continuación,
    aprovisionará los recursos en su cuenta e implementará el último
    código. Si recibe un error en la implementación, cambiar la
    ubicación podría ayudar (como a "westus"), dado que pueden estar
    limitaciones para algunos recursos.

![A screenshot of a computer program Description automatically
generated](./media/image35.png)

7.  Introduzca un nombre del resource group para su Azure portal
    (copiado en la tarea anterior) y presione enter.

![](./media/image36.png)

8.  La implementación lleva **20 - 30 minutos**. También puede verificar
    el estado de la implementación en el enlace generado o en **Azure
    portal-\> Resource group-\> Deployments**.

![](./media/image37.png)

![A screenshot of a computer Description automatically
generated](./media/image38.png)

![A screenshot of a computer Description automatically
generated](./media/image39.png)

![A screenshot of a computer Description automatically
generated](./media/image40.png)

### Tarea 4 : implemente la aplicación desde Github

1.  Ejecute el siguiente command para establecer la variable del entorno
    de resource.

\`\`azd env set AZURE_RESOURCE_GROUP {Name of existing resource group}
\`\`

Ojo : Reemplace {Nombre de su resource group existente} con el nombre de
su resource group disponible en la sección Resources en su VM.

![](./media/image41.png)

2.  Ejecute el command para implementar todos los recursos y espere a
    que se complete la implementación exitosamente.

\`\`azd deploy\`\`

![](./media/image42.png)

3.  Haga clic en Endpoint URL generada

![](./media/image43.png)

4.  Haga clic en **Open** para abrir el sitio web externo.

![](./media/image44.png)

5.  La aplicación se abre en una nueva pestaña.

![A screenshot of a computer Description automatically
generated](./media/image45.png)

### Tarea 5 : Transmita los diagnostic logs

Azure App Service captura todos los outputs de mensajes a la consola
para ayudarle en disgnosticar los problemas de su aplicación. La
aplicación incluye print() statements para demostrar esta capacidad como
se ve aquí.

@app.route('/', methods=\['GET'\])

def index():

print('Request for index page received')

restaurants = Restaurant.query.all()

return render_template('index.html', restaurants=restaurants)

1.  Cambie a **Azure portal- \> Resource group** y haga clic en **App
    service**.

![](./media/image46.png)

2.  En la página de App Service. Desde el menú izquierdo,
    seleccione **Monitoring -\>** **App Service logs.**

![](./media/image47.png)

3.  En **Application logging**, asegúrese de seleccionar **File
    System**. Selecciónelo si se requiere. En el menú superior,
    seleccione **Save**.

![](./media/image48.png)

4.  Desde el menú izquierdo, seleccione **Log stream**. Puede ver los
    registros para su aplicación, incluidos los registros de plataforma
    y registros desde dentro del container.

![](./media/image49.png)

### Tarea 6 : Limpie los recursos en Github.

1.  Cambie a Github, haga clic en **repo -\> Code -\> Codespaces.**
    Seleccione el branch correcto

![](./media/image50.png)

2.  Seleccione el branch y haga clic en **Delete**.

![](./media/image51.png)

3.  Haga clic en **Delete**.

![](./media/image52.png)

4.  Cambie a **Azure portal -\> Resource group.**

![A screenshot of a computer Description automatically
generated](./media/image53.png)

5.  Seleccione todos los recursos y haga clic en **Delete** (NO ELIMINE
    el resource group)

![](./media/image54.png)

6.  Introduzca \`\`**Delete**\`\` y haga clic en **Delete**.

![](./media/image55.png)

7.  Haga clic en **Delete** para confirmar la eliminación.

![](./media/image56.png)
