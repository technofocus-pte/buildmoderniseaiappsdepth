# Lab 01 – Java-Dev - Deploy a Java web app to Azure App Service


After you complete this module, you will be able to:

- Create a PrimeFaces Java web application.

- Configure your Java project to deploy to Azure App Service.

- Deploy your Java web application to Azure App Service.

**What is Azure App Service?**

Azure provides Azure App Service as a platform as a service (PaaS) for
running Tomcat. It features a Windows and Linux environment, security,
load balancing, autoscaling, and DevOps integration. You can leave the
OS and Tomcat management to Azure and concentrate on building
applications.

<img src="./media/image1.png" style="width:6.27153in;height:4.84583in"
alt="Screenshot" />

## Task 0 : Set up Environment Variables

1.  Search for [**Environment
    variables**](urn:gd:lg:a:send-vm-keys) from your Windows start menu
    and select Edit Environment variables.

<img src="./media/image2.png" style="width:6.5in;height:5.94653in"
alt="Screenshot" />

2.  On System Properties window, Click on **Advanced -\> Environment
    variables**.

<img src="./media/image3.png" style="width:6.08611in;height:6.80278in"
alt="Screenshot" />

3.  Select **JAVA_HOME** and then click on **Edit** button .Update the
    value to [**C:\Program
    Files\Java\jdk1.8.0_202**](urn:gd:lg:a:send-vm-keys) and then click
    on **OK**.

<img src="./media/image4.png" style="width:6.5in;height:5.06181in"
alt="Screenshot" />

4.  Click on New and Add below MAVEN variable and value and then click
    on **OK**.

Variable Name : [**MAVEN_HOME**](urn:gd:lg:a:send-vm-keys)

Variable value : [**C:\Software
folder\apache-maven-3.9.4**](urn:gd:lg:a:send-vm-keys)

<img src="./media/image5.png" style="width:6.5in;height:4.93889in"
alt="Screenshot" />

5.  Make sure **TOMCAT** variable set correctly.

Variable Name : [**TOMCAT_HOME**](urn:gd:lg:a:send-vm-keys)

Variable value : [**C:\Program Files\Apache Software Foundation\Tomcat
9.0**](urn:gd:lg:a:send-vm-keys)

<img src="./media/image6.png" style="width:6.5in;height:4.75625in"
alt="Screenshot" />

6.  Select the **Path** and then click on **Edit**.

<img src="./media/image7.png" style="width:6.5in;height:5.91458in"
alt="Screenshot" />

7.  Click on New and add below variable
    paths [**%JAVA_HOME%\bin**](urn:gd:lg:a:send-vm-keys)

<img src="./media/image8.png" style="width:6.5in;height:6.20486in"
alt="Screenshot" />

8.  Click on **New** again and add below path.

>```copy
>%MAVEN_HOME%\bin
>

<img src="./media/image9.png" style="width:6.5in;height:6.16528in"
alt="Screenshot" />

9.  Click on New and add Chocolatey
    path 
    
>```copy    
>C:\ProgramData\chocolatey\bin
>

<img src="./media/image10.png" style="width:6.5in;height:3.96736in"
alt="Screenshot" />

10. Click OK on **edit Environment Variables** and Environment
    variables.

<img src="./media/image11.png" style="width:6.5in;height:5.55972in"
alt="Screenshot" />

11. On System variables windows, click on **OK**.

<img src="./media/image12.png" style="width:6.19722in;height:7.08056in"
alt="Screenshot" />

## Task 1 : Cloud slice resource group

1.  Open a browser and go to <https://portal.azure.com> and sign in with
    the cloud slice account specified in home page.

<img src="./media/image13.png"
style="width:6.49236in;height:2.97708in" />

2.  On the **Home** page, click on the **Resource groups** tile.

> <img src="./media/image14.png" style="width:6.5in;height:4.12153in" />

3.  Make sure resource group already got created in your cloud slice
    account .You will be using the same resource and creating all your
    resources within this resource in this course.

> <img src="./media/image15.png"
> style="width:6.49236in;height:3.27292in" />

## Task 2 : Get sample JSF applications

To deploy a Java web application, you can get a PrimeFaces JavaServer
Faces (JSF) web application repo in C:\Labfiles

4.  Open **Gitbash**  as administrator from windows start and run [**az
    login**](urn:gd:lg:a:send-vm-keys) command.

>**Note**: If see WARNING: A web browser has been opened at
https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize.
Please continue the login in the web browser. If no web browser is
available or if the web browser fails to open, use device code flow
with [**az login --use-device-code**](urn:gd:lg:a:send-vm-keys).
>

<img src="./media/image16.png" style="width:6.27153in;height:0.77153in"
alt="Screenshot" />

2.  Default browser opens to sign in .Sign in with your Azure
    subscription account.

> <img src="./media/image17.png" style="width:6.27153in;height:2.0125in"
> alt="Screenshot" />
>
> <img src="./media/image18.png" style="width:6.27153in;height:4.29028in"
> alt="Screenshot" />

3.  Open **intellij IDEA** as administrator and open the above project
    from C:\Labfiles. Select the **Start trail** radio button and then
    click on Start trail button.

<img src="./media/image19.png" style="width:6.5in;height:5.07986in"
alt="Screenshot" />

<img src="./media/image20.png" style="width:6.5in;height:5.2in"
alt="Screenshot" />

4.  Click on **Allow access** button.

<img src="./media/image21.png" style="width:6.5in;height:4.75208in"
alt="Screenshot" />

5.  Click on **Accept all.**

<img src="./media/image22.png" style="width:6.5in;height:4.43333in"
alt="Screenshot" />

6.  Switch back to IntelliJ window and then click on Continue button.

<img src="./media/image23.png" style="width:6.5in;height:4.87222in"
alt="Screenshot" />

7.  Click on **Open** and browse the above project
    from **C:\LabFiles\Deploy-PrimeFaces-JSF-Web-App-on-Tomcat-9.0-master\Deploy-PrimeFaces-JSF-Web-App-on-Tomcat-9.0-master.**

> <img src="./media/image24.png" style="width:6.27153in;height:4.77153in"
> alt="Screenshot" />
>
> <img src="./media/image25.png" style="width:6.25903in;height:4.93819in"
> alt="Screenshot" />

8.  Click on **Trust the project.**

> <img src="./media/image26.png" style="width:6.27153in;height:5.16667in"
> alt="Screenshot" />

9.  Then you'll see the following files in the directory:

> Deploy-PrimeFaces-JSF-Web-App-on-Tomcat-9.0
>
> ├── pom.xml
>
> └── src
>
> └── main
>
> ├── java
>
> │ └── com
>
> │ └── microsoft
>
> │ └── azure
>
> │ └── samples
>
> │ ├── controller
>
> │ │ └── TodoListController.java
>
> │ ├── dao
>
> │ │ ├── ItemManagement.java
>
> │ │ └── TodoItemManagementInMemory.java
>
> │ └── model
>
> │ └── TodoItem.java
>
> └── webapp
>
> ├── META-INF
>
> │ └── context.xml
>
> ├── WEB-INF
>
> │ ├── beans.xml
>
> │ ├── classes
>
> │ │ └── logging.properties
>
> │ ├── faces-config.xml
>
> │ └── web.xml
>
> └── index.xhtml
>

> <img src="./media/image27.png" style="width:6.27153in;height:3.67917in"
> alt="Screenshot" />

## Exercise 1 : Maven Plugin for Azure App Service

Microsoft provides the Maven Plugin for Azure App Service to make it
easier for Java developers to deploy applications to Azure. By using
this plug-in, you can easily configure and deploy your application to
Azure. Execute the following command to use Maven Plugin for Azure App
Service.

### Task 1 : Configure the Maven Plugin for Azure App Service

1.  Navigate back to the **Gitbash**  and navigate the project directory

>```copy
>cd "C:\LabFiles\Deploy-PrimeFaces-JSF-Web-App-on-Tomcat-9.0-master\Deploy-PrimeFaces-JSF-Web-App-on-Tomcat-9.0-master"
>

<img src="./media/image28.png" style="width:6.27153in;height:0.95069in"
alt="Screenshot" />

2.  To configure the Maven Plugin for Azure App Service, execute the
    following **command:**

>```copy
> mvn com.microsoft.azure:azure-webapp-maven-plugin:2.5.0:config
>

> <img src="./media/image29.png" style="width:6.27153in;height:4.77153in"
> alt="Screenshot" />

3.  After the command, some questions will appear at the prompt, so
    enter and select the appropriate items and set them. Enter the
    following options:

| **Item**                       | **Input value**                |
|--------------------------------|--------------------------------|
| Subscription                   | Choose your Azure Subscription |
| Define value for OS            | 2: Linux                       |
| Define value for Java version  | Java 8                         |
| Define value for runtime stack | 4: TOMCAT 9.0                  |
| Define value for pricing tier  | P1v2                           |
| Confirm (Y/N)                  | Y                              |

> <img src="./media/image30.png" style="width:6.27153in;height:4.69167in"
> alt="Screenshot" />

4.  After you execute the command, the results will appear:

> <img src="./media/image31.png" style="width:6.27153in;height:5.59236in"
> alt="Screenshot" />

5.  Check your **Intelliji**. You'll see a new section in the  section
    in your **pom.xml** file.

> <img src="./media/image32.png" style="width:6.27153in;height:4.07431in"
> alt="Screenshot" />

6.  Change below values in pom.xml and save it

- **maven.compiler.source to 1.8 ( line \#12)**

- maven.compiler.target to **1.8 (line \#13 )**

- resourceGroup – your Cloud slice resource group name (line \#69)

- region – eastus (line \#72)

> <img src="./media/image33.png"
> style="width:6.49375in;height:3.49375in" />
>
> <img src="./media/image34.png"
> style="width:6.49375in;height:3.13472in" />

### Task 2 : Compile and deploy to Azure App Service

1.  Now that the settings for deploying to Azure App Service are
    complete, compile the source code again . Go back to the Gitbash and
    run below command

>```copy
> mvn clean package
>

> <img src="./media/image35.png" style="width:6.27153in;height:4.27153in"
> alt="Screenshot" />

2.  Once compiled, use the Maven Plugin for Azure Web Apps command to
    deploy your application. Execute the following command:

>```copy
> mvn azure-webapp:deploy
>

**IMPORTANT**: Raise a support ticket if you see below error -
subscription is not allowed to create or update Azure Web App
-[**https://learn.microsoft.com/en-gb/azure/azure-portal/supportability/how-to-create-azure-support-request**](urn:gd:lg:a:send-vm-keys) <img src="./media/image36.png" style="width:6.5in;height:0.67639in"
alt="Screenshot" />

When the deployment is completed, the following message will be output.

<img src="./media/image37.png" style="width:6.5in;height:3.28264in"
alt="A screenshot of a computer program Description automatically generated" />

3.  The public URL of the deployed application is displayed
    in Successfully deployed the artifact to. Access your URL with a
    browser.

> <img src="./media/image38.png"
> style="width:6.4875in;height:3.32708in" />

 [**https://azure-javaweb-app-XXXXXXXXXX.azurewebsites.net**](urn:gd:lg:a:send-vm-keys)

> <img src="./media/image39.png" style="width:6.5in;height:4.49722in"
> alt="A screenshot of a computer Description automatically generated" />

### Task 3 : Confirm the log stream from the command line

1.  To access the log stream, update the below command with your appname
    and execute the following Gitbash CLI command. (Replace
    XXXXXXXXXXXXXX with your code)

>```copy
> az webapp log tail -g azure-javaweb-app-XXXXXXXXXXXXXX-rg -n
> azure-javaweb-app-XXXXXXXXXXXXXX
>

> eg : az webapp log tail -g azure-javaweb-app-1691402277330-rg -n
> azure-javaweb-app-1691402277330
>

 Then you can see the following result:

> <img src="./media/image40.png" style="width:6.27153in;height:2.93819in"
> alt="Screenshot" />
>
> <img src="./media/image41.png" style="width:6.27153in;height:3.80278in"
> alt="Screenshot" />

## Exercise 3 : Auto scale a web app by using custom metrics

### Task 1 : Configure autoscale

**Configure the autoscale settings for your App Service plan.**

1.  Open a new tab in browser and go
    to [**https://portal.azure.com**](urn:gd:lg:a:send-vm-keys) and sign
    in with your Office 365 admin tenant credentials.

2.  Search and select [**autoscale**](urn:gd:lg:a:send-vm-keys) in the
    search bar or select **Autoscale** under **Settings** in the menu
    bar on the left.

> <img src="./media/image42.png" style="width:6.25903in;height:3.65417in"
> alt="Screenshot" />

3.  Select your **App Service plan**. You can only configure production
    plans.

> <img src="./media/image43.png" style="width:6.5in;height:3.66667in" />

### Task 2 : Set up a scale-out rule

**Set up a scale-out rule so that Azure spins up another instance of the web app when your web app is handling more than 70 sessions per instance.**

1.  Select **Custom autoscale**.In the **Rules** section of the default
    scale condition, select **Add a rule**.

> <img src="./media/image44.png" style="width:6.5in;height:4.37847in" />
>
> <img src="./media/image45.png"
> style="width:6.49236in;height:4.19722in" />

2.  From the **Metric source** dropdown, select **Other resource**.

3.  From **Resource type**, select **App Service plans**.

4.  From the **Resource** dropdown, select your web app.

5.  Select a **Metric namespace** to base your scaling on. For example,
    use **CPU Percentage**.

> <img src="./media/image46.png" style="width:6.5in;height:2.50764in" />

6.  Select the **Enable metric divide by instance count** checkbox so
    that the number of sessions per instance is measured.

7.  From the **Operator** dropdown, select **Greater than**.

8.  Enter the **Metric threshold to trigger the scale action**. For
    example, use **70**.

9.  Under **Action**, set **Operation** to **Increase count by**.
    Set **Instance count** to **1**.

10. Select **Add**.

> <img src="./media/image47.png"
> style="width:6.49236in;height:3.91667in" />

### Task 3 : Limit the number of instances

1.  Set the maximum number of instances that can be spun up in
    the **Maximum** field of the **Instance limits** section. For
    example, use **4**.

2.  Select **Save**.

> <img src="./media/image48.png" style="width:6.24722in;height:4.5in"
> alt="Screenshot" />

### Task 4 : Clean up resources:

**Do not delete resource group and the app as the same application will
be used in
Lab 13 - Configuring your App Service or Azure Functions app to use Entra ID login
to test authentication and authorization features.**

**Summary:**

In this labguide, we learnt how to create and package a Java web
application, how to use the Maven Plugin for Azure Web Apps, and how to
deploy your application to Azure App Service. These steps are applicable
not only for JSF applications but also most Java web applications.
