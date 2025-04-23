# Caso de uso 03 – Contenerizar una aplicación de reservas de vuelos e implementarla a Azure Kubernetes Service

**Objetivo**:

A finales de este módulo, podrá:

- Contenerizar una aplicación Java.

- Construir una imagen para la apliación Java.

- Ejecutar la imagen del container de forma local.

- Empujar la imagen de container a Azure Container Registry.

- Implementar la imagen del container a Azure Kubernetes Service

**Tecnologías clave** -- Java 11, Docker ,Maven

**Duración estimada**: 30 minutos

**Tipo del lab:** dirigido por el instructor

## Ejercicio 1: Configure su entorno de Azure

En este ejercicio, usará Azure CLI para crear recursos de Azure que se
necesita en las próximas unidades. Mediante Azure CLI, realice los
siguientes pasos

### Tarea 1: Autentique con Azure Resource Manager

1.  Abra Gitbash desde Desktop y ejecute el siguiente inicio de sesión
    de command en el Azure portal

**\`\`az login\`\`**

**Ojo**: Si ve WARNING: A web browser has been opened at
https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize.
Please continue the login in the web browser. If no web browser is
available or if the web browser fails to open, use device code flow
with az login --use-device-code

![A black background with yellow text Description automatically
generated](./media/image1.jpeg)

2.  Este command le llevará directamente al navegador predeterminado
    para iniciar sesión. Inicie sesión con su cuenta de suscripción de
    Azure.

![A screenshot of a computer Description automatically
generated](./media/image2.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image3.jpeg)

3.  Una vez autenticado, cambie a Gitbash

![A computer screen with white text Description automatically
generated](./media/image4.jpeg)

4.  Ahora habilitemos nuestro Azure subscription,. Ejecute el siguiente
    command

\`\`az account set --subscription "\< YOUR_SUBSCRIPTION_ID \>"\`\`

\`\`az account list --output table\`\`

![A computer screen with white text Description automatically
generated](./media/image5.jpeg)

5.  Define las variables locales. Para simplificar los commands que
    serán ejecutados más abajo, configure las siguientes variables del
    entorno

Ojo: ya hemos creado el resource group en la nube. Tiene que implementar
todos los recursos dentro del grupo de recursos existente. Puede
encontrarlo en Azurebportal o en la pestaña Resources en su VM

> export AZ_CONTAINER_REGISTRY="javaaksregist"$RANDOM
>
> export AZ_KUBERNETES_CLUSTER="javaakscluster"$RANDOM
>
> export AZ_LOCATION="westus"

export AZ_KUBERNETES_CLUSTER_DNS_PREFIX="javaakscontainer"

> export AZ_RESOURCE_GROUP= Your existing resource group name

**Ojo:** podrá reemplazarlo con la región de su elección, por ejemplo:
eastus. También va a quere reemplazarlo con un valor único ya que se usa
para generar un FQDN único (fully qualified domain name) para su Azure
Container Registry cuando se ha creado, por ejemplo:
someuniquevaluejavacontainerregistry.

![A screen shot of a computer Description automatically
generated](./media/image6.png)

8.  Azure Container Registry le permite construir, almacenar y gestionar
    las imágenes de container, donde se va a almacenar las imágenes
    container para la aplicación Java. Cree un Azure Container Registry
    con los siguientes commands.

\`\`az acr create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_CONTAINER_REGISTRY --sku Basic | jq\`\`

![A screenshot of a computer screen Description automatically
generated](./media/image7.jpeg)

9.  Configure Azure CLI para este Azure Container Registry recién creado

\`\`az configure --defaults acr=$AZ_CONTAINER_REGISTRY\`\`

![A black background with green and white text Description automatically
generated](./media/image8.jpeg)

10. Autentique a este nuevo Azure Container Registry

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`

![A computer screen with white text Description automatically
generated](./media/image9.jpeg)

11. Cree un Azure Kubernetes Cluster, va a necesitar un Azure Kubernetes
    Cluster para implementar la aplicación Java (container image).

az aks create --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER --attach-acr $AZ_CONTAINER_REGISTRY
--dns-name-prefix=$AZ_KUBERNETES_CLUSTER_DNS_PREFIX --generate-ssh-keys
| jq

![A computer screen with white text Description automatically
generated](./media/image10.jpeg)

**Ojo:** la creación de Azure Kubernetes Cluster puede tardar 10
minutos, una vez que ejecute el command, opcionalmente puede dejarla
continuar en la pestaña Azure CLI y seguir a próxima unidad.

### Tarea 2: Ejecute Docker

1.  En el menú de Start, haga clic en **DockerDesktop**

![A screenshot of a phone Description automatically
generated](./media/image11.jpeg)

2.  Asegúrese de que esté funcionando.

## Ejercicio 2: Contenerice la aplicación Java

En este ejercicio, va a contenerizar una aplicación Java.

### Tarea 1 : Construya una Java Application

Primero va a navegar el Flight Booking System para el repositorio
Airline Reservations y cd para la carpeta del proyecto de aplicación web
Airlines.

Opcionalmente, si tiene Java y Maven instalado, puede ejecutar los
siguientes command(s) en su CLI para entender la experiencia de
construir la aplicación sin Docker. Si no tiene Java y Maven, puede
saltar a la siguiente sección llamada "Construya un Docker file", en que
usará Docker para llamar a Java y Maven para ejecutar las construcciones
en su nombre.

1.  Ejecute el siguiente command en su CLI para navegar al proyecto.

\`\`cd
"C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines"\`\`

![A black background with text Description automatically
generated](./media/image12.jpeg)

2.  Ejecute el siguiente command en su CLI

\`\`mvn clean install\`\`

![A screenshot of a computer Description automatically
generated](./media/image13.jpeg)

**Ojo:** Se usó el mvn clean install command para ilustrar los retos
operacionales de no usar Docker multi-stage builds, lo que vamos a ver a
continuación. Este paso es opcional, y de todos modos puede seguir sin
ejecutar el Maven command.

3.  Maven debería haber construido exitosamente el Flight Booking System
    para Airline Reservations Web Application Archive artifact
    FlightBookingSystemSample-0.0.-SNAPSHOT.war, como se ve aquí

![A screenshot of a computer screen Description automatically
generated](./media/image14.jpeg)

## Tarea 2: Construya un Docker file

1.  Dentro de el root de su proyecto,
    containerize-and-deploy-Java-app-to-Azure/Project/Airlines, cree un
    file llamado **Dockerfile.**

\`\`vi Dockerfile\`\`

![](./media/image15.jpeg)

Agregue el siguiente contenido a Dockerfile y haga save and exit

\#

\# Build stage

\#

FROM maven:3.6.0-jdk-11-slim AS build

WORKDIR /build

COPY pom.xml .

COPY src ./src

COPY web ./web

RUN mvn clean package

\#

\# Package stage

\#

FROM tomcat:8.5.72-jre11-openjdk-slim

COPY tomcat-users.xml /usr/local/tomcat/conf

COPY --from=build /build/target/\*.war
/usr/local/tomcat/webapps/FlightBookingSystemSample.war

EXPOSE 8080

CMD \["catalina.sh", "run"\]

![A screenshot of a computer Description automatically
generated](./media/image16.jpeg)

**Ojo:** Opcionalmente, el Dockerfile_Solution en el root de su proyecto
contiene los contenidos necesarios. Como se ve, este Docker file Build
stage tiene seis instrucciones.

## Ejercicio 3 : Construya y ejecute un container image para la aplicación Java

En esta unidad, construirá y ejecutará el container image. Como se
menciona antes, un running instance de una imagen es un container.

### Tarea 1: Construya un container image

Ahora que ha construido exitosamente el Dockerfile, puede pedir al
Docker que construya un container image para usted.

**Ojo:** Asegúrese de que se ha configurado su Docker runtime para
construir Linux containers. Esto es importante porque se usa Dockerfile
como referencia container images (JDK/JRE) para la arquitectura de
Linux.

1.  **Docker build** es el command utilizado para construir container
    images. El **-t** argument se usa para especificar la etiqueta y
    el **.** es la ubicación para que el Docker encuentre el Dockerfile.
    Ejecute el siguiente command en su CLI.

**IMPORTANTE** : Este lab requiere jdk 11 . establezca java_home a jdk
11

\`\`docker build -t flightbookingsystemsample .\`\`

![A computer screen with text Description automatically
generated](./media/image17.jpeg)

2.  Docker build es algo parecido

![A screen shot of a computer screen Description automatically
generated](./media/image18.jpeg)

**Ojo:** Como ha visto antes, Docker ha ejecutado las instrucciones
desde las líneas que acaba de ver. Cada instrucción es un paso en un
orden sequencial. Vuelva a ejecutar el docker build command, note las
diferencias en los pasos, va a notar que ---\> usa cache en las capas
que no han cambiado. Si no está cambiando nada en la app (antes de
reejecutar el docker build command), entonces va a notar que todas las
capas con cache como binarios están intactas y se puede derivar desde
Docker cache). Esto es una lección importante a la hora de optimizar sus
container images y el coste de compute asosiado con el tiempo llevado en
construirlas.

3.  Docker también puede mostrar las imágenes disponibles que son
    residentes. Esto resulta útil para visualizar lo que está disponible
    para ejecutar. Ejecute el siguiente command en su CLI

docker image ls

Verá algo parecido:

![](./media/image19.jpeg)

### Tarea 2 : Ejecute el container image

1.  Ahora que ha construido un container image de forma exitosa, puede
    ejecutarlo.

2.  Docker run es el command utilizado para ejecutar un container image.
    Se usará el -p

:#### argument para reenviar el tráfico de localhost HTTP (el primer
port antes del colón) al conainer durante runtime (el segundo port
después del colón). Acúerdese del Dockerfile a lo cual el Tomcat app
server está escuchando en HTTP traffic en port 8080, por eso este es el
container port que se debe exponer. Por fin, se necesita
flightbookingsystemsample para ordenar al Docker cúal imagen se debe
ejecutar. Ejecute el siguiente command en su CLI:

\`\`docker run -p 8080:8080 flightbookingsystemsample\`\`

Verá algo parecido:

![A screen shot of a computer Description automatically
generated](./media/image20.jpeg)

![A screen shot of a computer screen Description automatically
generated](./media/image21.jpeg)

**Ojo:** si el command "docker run -p 8080:8080
flightbookingsystemsample" devuelve un error, use el port mencionado a
continuación

\`\`docker run -p 8081:8080 flightbookingsystemsample\`\`

3.  Abra un navegador y visite la página de inicio de Flight Booking
    System for Airline Reservations
    en http://localhost:8080/FlightBookingSystemSample

    - Verá lo siguiente:

![A plane flying in the sky Description automatically
generated](./media/image22.jpeg)

4.  Opcionalmente puede iniciar sesión como cualquier usuario de
    tomcat-users.xml como por ejemplo

Username :  **\`\`someuser@azure.com\`\`**

Password : \`\`password\`\`

![A screenshot of a computer Description automatically
generated](./media/image23.jpeg)

5.  Deje este git bash instance como está

## Ejercicio 4: Empuje el container image a Azure Container Registry

### Tarea 1: empuje un container image a Azure Container Registry

1.  En esta tarea, empujará un container image a Azure Container
    Registry. Azure Container Registry le permite construir, almacenar y
    gestionar las imágenes y artefactos del container en un registro
    privado para todos los tipos de las implementaciones del container.
    Use los Azure container registries con su container development y
    deployment pipelines existentes.

2.  Abra un nuevo instance de Gitbash y ejecute el az command para
    iniciar sesión en Azure portal.

\`\`az login\`\`

3.  Ejecute el siguiente command en su CLI

\`\`C:\Labfiles\containerize-and-deploy-Java-app-to-Azure-master\Project\Airlines\`\`

4.  Usaremos el mismo Authenticate con Azure Resource Manager que
    creamos antes en el Ejercicio 1 Tarea 1. Establezca las siguientes
    variables

[TABLE]

> **Ojo:** Si se ha pausado su sesión actual, o hace este paso en un
> tiepo diferente o desde un CLI diferente, tendrá que reiniciar sus
> variables del entorno y reautenticar con los siguientes CLI commands.

![A screen shot of a computer program Description automatically
generated](./media/image24.png)

### Tarea 2: Empuje un container image

En esta tarea, puede empujar su container image recién creado a Azure
Container Registry. Al hacerlo, su container image estará cerca de todos
sus Azure resources, como su Azure Kubernetes Cluster. Por fin,
configure el AKS para llamar a la imagen flightbookingsystemsample desde
Azure Container Registry.

1.  Para empujar el container image a Azure Container Registry, ejecute
    los siguientes tres commands en su CLI

2.  Inicie sesión en **Azure Container Registry** y ejecute el siguiente
    command

\`\`az acr login -n $AZ_CONTAINER_REGISTRY\`\`

![](./media/image25.jpeg)

3.  Primero etiquete el container image contruido antes con su Azure
    Container Registry:

\`\`docker tag flightbookingsystemsample
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`

![](./media/image26.jpeg)

4.  Luego, empuje el container image a Azure Container Registry

\`\`docker push
$AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample\`\`

![A screen shot of a computer Description automatically
generated](./media/image27.jpeg)

![A screen shot of a computer Description automatically
generated](./media/image28.jpeg)

5.  Ahora visualice el Azure Container Registry image meta-data de la
    imagen recién empujada. Ejecute el siguiente command en su CLI

\`\`az acr repository show -n $AZ_CONTAINER_REGISTRY --image
flightbookingsystemsample:latest\`\`

Verá algo parecido:

![A computer screen with white text Description automatically
generated](./media/image29.jpeg)

6.  Container image ahora se reside dentro de Azure Container Registry y
    está listo para implementaciones por Azure Services como Azure
    Kubernetes Service.

## Ejercicio 5: Implemente el container image a Azure Kubernetes Service

En este ejercicio, implementará un container image a Azure Kubernetes
Service.

### Tarea 1: Implemente un container image

1.  Implementará este **flightbookingsystemsample** container image a su
    Azure Kubernetes Cluster.

2.  Dentro de el root de su
    proyecto, **Flight-Booking-System-JavaServlets_App/Project/Airlines**,
    Cree un archivo llamado deployment.yml. ejecute el siguiente command
    en su CLI:

\`\`vi deployment.yml\`\`

![](./media/image30.jpeg)

3.  Agregue el siguiente contenido a deployment.yml y haga save and
    exit:

**Ojo:** Tendrá que actualizar el valor de la variable del entorno de
AZ_CONTAINER_REGISTRY, del Ejercicio 1 Tarea1 ( AZ_CONTAINER_REGISTRY=
javaaksregist )

apiVersion: apps/v1

kind: Deployment

metadata:

name: flightbookingsystemsample

spec:

replicas: 1

selector:

matchLabels:

app: flightbookingsystemsample

template:

metadata:

labels:

app: flightbookingsystemsample

spec:

containers:

\- name: flightbookingsystemsample

image:
\<AZ_CONTAINER_REGISTRY\>.azurecr.io/flightbookingsystemsample:latest

resources:

requests:

cpu: "1"

memory: "1Gi"

limits:

cpu: "2"

memory: "2Gi"

ports:

\- containerPort: 8080

---

apiVersion: v1

kind: Service

metadata:

name: flightbookingsystemsample

spec:

type: LoadBalancer

ports:

\- port: 8080

targetPort: 8080

selector:

app: flightbookingsystemsample

![A screenshot of a computer program Description automatically
generated](./media/image31.jpeg)

4.  Presione Esc and: y
    tecle \`\`**[wq](urn:gd:lg%F0%9F%85%B0%EF%B8%8Fsend-vm-keys)\`\`** y
    presione enter para guardar el archivo.

**Ojo:** Opcionalmente, el deployment_solution.yml en el root de su
proyecto tiene los contenidos necesarios, se lo puede resultar fácil
renombrar/actualizar el contenido del archivo.

5.  En deployment.yml notará que deployment.yml contiene un Deployment y
    un Service. El deployment se usa para administrar un conjunto de
    pods y el servicio para permitir acceso a través de redes a los
    pods. Notará que los pods están configurados para llamar una sola
    imagen, el \< AZ_CONTAINER_REGISTRY
    \>.azurecr.io/flightbookingsystemsample:latest desde Azure Container
    Registry. También notará que el servicio está configurado para
    permitir el tráfico entrante de pods de HTTP en port 8080, de una
    manera similar que como ha ejecutado el container image de forma
    local con el -p port argument.

6.  Debería haber completado la creación de Azure Kubernetes Cluster.

7.  Ahora configure su Azure CLI para acceder a Azure Kubernetes Cluster
    a través de kubectl command. Instale kubectl de forma local mediante
    az aks install-cli command. Ejecute el siguiente command en su CLI

\`\`az aks install-cli\`\`

![](./media/image32.jpeg)

8.  Configure kubectl para conectar a su Kubernetes cluster mediante az
    aks get-credentials command. Ejecute el siguiente command en su CLI

\`\`az aks get-credentials --resource-group $AZ_RESOURCE_GROUP --name
$AZ_KUBERNETES_CLUSTER\`\`

- Verá algo parecido:

![](./media/image33.jpeg)

9.  Ahora pida a Azure Kubernetes Service que aplique los cambios de
    deployment.yml a su cluster. Ejecute el siguiente command en su CLI

\`\`kubectl apply -f deployment.yml\`\`

- Verá algo parecido:

![](./media/image34.jpeg)

10. Ahora use **kubectl** para monitorear el estado de la
    implementación. Ejecute el siguiente command en su CLI

\`\`kubectl get all\`\`

- Verá algo parecido:

![A computer screen with text and numbers Description automatically
generated](./media/image35.jpeg)

**Ojo:** Tendrá que sustituir el ip address a continuación,
20.81.13.151, y con ello el EXTERNAL-IP y note el nombre del POD, vamos
a necesitarlo en los próximos pasos.

11. Si el estado de su **POD** es **Running** entonces la app debe ser
    accesible.

12. Puede ver los registros de apps dentro de cada pod. Ejecute el
    siguiente command en su CLI

\`\`kubectl logs pod/flightbookingsystemsample-\`\`

![A screen shot of a computer Description automatically
generated](./media/image36.jpeg)

![A computer screen with white text Description automatically
generated](./media/image37.jpeg)

13. Ahora use el **EXTERNAL-IP** esde su output de kubectl get services
    flightbookingsystemsample para acceder a running app dentro de Azure
    Kubernetes Service.

**Ojo:** You'Tendrá que sustituir el ip address a continuación,
20.81.13.151, y con ello el EXTERNAL-IP desde el command que acaba de
ejecutar.

14. Abra un navegador y visite la página de inicio de Flight Booking
    System Sample en **http://YOUR
    IPCON:8080/FlightBookingSystemSample** (actualice con su external IP
    address )

    - Verá algo parecido:

![A plane flying in the sky Description automatically
generated](./media/image38.jpeg)

**Ojo:** puede iniciar sesión como cualquier usuario de tomcat-users.xml
como por ejemplo someuser@azure.com: password

### Tarea 2 : Limpie los recursos

1.  Cambie a Azure portal. Haga clic en **Resource groups**.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

2.  Haga clic en el nombre de resource group.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

3.  Seleccione todos los recursos y haga clic en **Delete** (NO ELIMINE
    – Resource group)

4.  Introduzca \`\`delete\`\` y haga clic en **Delete**.

![A screenshot of a computer Description automatically
generated](./media/image41.png)

5.  Confirme la eliminación de recursos.

![A screenshot of a computer Description automatically
generated](./media/image42.png)

**Resumen:** ¡Felicidades! Hemos incluido en contenedores e implementado
una aplicación Java en Azure Kubernetes Service. Como parte del
laboratorio, hemos creado una aplicación Java en contenedores, hemos
insertado container image en Azure Container Registry y, a continuación,
la hemos implementado en Azure Kubernetes Service.
