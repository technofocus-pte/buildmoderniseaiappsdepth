Lab 01 -- Java-Dev - Deploy a Java web app to Azure App Service

**Estimated duration**:  20 min

**Lab Type:** Instructor led

**Objective**:

After you complete this module, you will be able to:

-   Create a PrimeFaces Java web application.

-   Configure your Java project to deploy to Azure App Service.

-   Deploy your Java web application to Azure App Service.

**What is Azure App Service?**

Azure provides Azure App Service as a platform as a service (PaaS) for
running Tomcat. It features a Windows and Linux environment, security,
load balancing, autoscaling, and DevOps integration. You can leave the
OS and Tomcat management to Azure and concentrate on building
applications.

./images/image1.png


Task 0 : Set up Environment Variables

1.  Search for [**Environment
    variables**](urn:gd:lg:a:send-vm-keys) from your Windows start menu
    and select Edit Environment variables.

./images/image2.png

2.  On System Properties window, Click on **Advanced -\> Environment
    variables**.

![Screenshot](./media/image3.png)

3.  Select **JAVA_HOME** and then click on **Edit** button .Update the
    value to [**C:\\Program
    Files\\Java\\jdk1.8.0_202**](urn:gd:lg:a:send-vm-keys) and then
    click on **OK**.

![Screenshot](./media/image4.png)

4.  Click on New and Add below MAVEN variable and value and then click
    on **OK**.

Variable Name : [**MAVEN_HOME**](urn:gd:lg:a:send-vm-keys)

Variable value : [**C:\\Software
folder\\apache-maven-3.9.4**](urn:gd:lg:a:send-vm-keys)

![Screenshot](./media/image5.png)

5.  Make sure **TOMCAT** variable set correctly.

Variable Name : [**TOMCAT_HOME**](urn:gd:lg:a:send-vm-keys)

Variable value : [**C:\\Program Files\\Apache Software
Foundation\\Tomcat 9.0**](urn:gd:lg:a:send-vm-keys)

![Screenshot](./media/image6.png)

6.  Select the **Path** and then click on **Edit**.

![Screenshot](./media/image7.png)

7.  Click on New and add below variable
    paths [**%JAVA_HOME%\\bin**](urn:gd:lg:a:send-vm-keys)

![Screenshot](./media/image8.png)

8.  Click on **New** again and add below path.

[**%MAVEN_HOME%\\bin**](urn:gd:lg:a:send-vm-keys)

![Screenshot](./media/image9.png)

9.  Click on New and add Chocolatey
    path [**C:\\ProgramData\\chocolatey\\bin**](urn:gd:lg:a:send-vm-keys)

![Screenshot](./media/image10.png)

10. Click OK on **edit Environment Variables** and Environment
    variables.

![Screenshot](./media/image11.png)

11. On System variables windows, click on **OK**.

![Screenshot](./media/image12.png)

Task 1 : Cloud slice resource group

1.  Open a browser and go to <https://portal.azure.com> and sign in with
    the cloud slice account specified in home page.

![](./media/image13.png)

2.  On the **Home** page, click on the **Resource groups** tile.

> ![](./media/image14.png)

3.  Make sure resource group already got created in your cloud slice
    account .You will be using the same resource and creating all your
    resources within this resource in this course.

> ![](./media/image15.png)

Task 1 : Get sample JSF applications

To deploy a Java web application, you can get a PrimeFaces JavaServer
Faces (JSF) web application repo in C:\\Labfiles

4.  Open **Gitbash**  as administrator from windows start and run [**az
    login**](urn:gd:lg:a:send-vm-keys) command.

**Note**: If see WARNING: A web browser has been opened at
https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize.
Please continue the login in the web browser. If no web browser is
available or if the web browser fails to open, use device code flow
with [**az login \--use-device-code**](urn:gd:lg:a:send-vm-keys).

![Screenshot](./media/image16.png)

2.  Default browser opens to sign in .Sign in with your Azure
    subscription account.

> ![Screenshot](./media/image17.png)
>
> ![Screenshot](./media/image18.png)

3.  Open **intellij IDEA** as administrator and open the above project
    from C:\\Labfiles. Select the **Start trail** radio button and then
    click on Start trail button.

![Screenshot](./media/image19.png)

![Screenshot](./media/image20.png)

4.  Click on **Allow access** button.

![Screenshot](./media/image21.png)

5.  Click on **Accept all.**

![Screenshot](./media/image22.png)

6.  Switch back to IntelliJ window and then click on Continue button.

![Screenshot](./media/image23.png)

7.  Click on **Open** and browse the above project
    from **C:\\LabFiles\\Deploy-PrimeFaces-JSF-Web-App-on-Tomcat-9.0-master\\Deploy-PrimeFaces-JSF-Web-App-on-Tomcat-9.0-master.**

> ![Screenshot](./media/image24.png)
>
> ![Screenshot](./media/image25.png)

8.  Click on **Trust the project.**

> ![Screenshot](./media/image26.png)

9.  Then you\'ll see the following files in the directory:

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
> ![Screenshot](./media/image27.png)

Exercise 2 : Maven Plugin for Azure App Service

Microsoft provides the Maven Plugin for Azure App Service to make it
easier for Java developers to deploy applications to Azure. By using
this plug-in, you can easily configure and deploy your application to
Azure. Execute the following command to use Maven Plugin for Azure App
Service.

Task 1 : Configure the Maven Plugin for Azure App Service

1.  Navigate back to the **Gitbash**  and navigate the project directory

cd
\"C:\\LabFiles\\Deploy-PrimeFaces-JSF-Web-App-on-Tomcat-9.0-master\\Deploy-PrimeFaces-JSF-Web-App-on-Tomcat-9.0-master\"

![Screenshot](./media/image28.png)

2.  To configure the Maven Plugin for Azure App Service, execute the
    following **command:**

> mvn com.microsoft.azure:azure-webapp-maven-plugin:2.5.0:config
>
> ![Screenshot](./media/image29.png)

3.  After the command, some questions will appear at the prompt, so
    enter and select the appropriate items and set them. Enter the
    following options:

  -----------------------------------------------------------------------
  **Item**                           **Input value**
  ---------------------------------- ------------------------------------
  Subscription                       Choose your Azure Subscription

  Define value for OS                2: Linux

  Define value for Java version      Java 8

  Define value for runtime stack     4: TOMCAT 9.0

  Define value for pricing tier      P1v2

  Confirm (Y/N)                      Y
  -----------------------------------------------------------------------

> ![Screenshot](./media/image30.png)

4.  After you execute the command, the results will appear:

> ![Screenshot](./media/image31.png)

5.  Check your **Intelliji**. You\'ll see a new section in the  section
    in your **pom.xml** file.

> ![Screenshot](./media/image32.png)

6.  Change below values in pom.xml and save it

-   **maven.compiler.source to 1.8 ( line #12)**

-   maven.compiler.target to **1.8 (line #13 )**

-   resourceGroup -- your Cloud slice resource group name (line #69)

-   region -- eastus (line #72)

> ![](./media/image33.png)
>
> ![](./media/image34.png)

Task 3 : Compile and deploy to Azure App Service

1.  Now that the settings for deploying to Azure App Service are
    complete, compile the source code again . Go back to the Gitbash and
    run below command

> mvn clean package
>
> ![Screenshot](./media/image35.png)

2.  Once compiled, use the Maven Plugin for Azure Web Apps command to
    deploy your application. Execute the following command:

> mvn azure-webapp:deploy

**IMPORTANT**: Raise a support ticket if you see below error -
subscription is not allowed to create or update Azure Web App
-[**https://learn.microsoft.com/en-gb/azure/azure-portal/supportability/how-to-create-azure-support-request**](urn:gd:lg:a:send-vm-keys) ![Screenshot](./media/image36.png)

When the deployment is completed, the following message will be output.

![A screenshot of a computer program Description automatically
generated](./media/image37.png)

3.  The public URL of the deployed application is displayed
    in Successfully deployed the artifact to. Access your URL with a
    browser.

> ![](./media/image38.png)
>
> [**https://azure-javaweb-app-XXXXXXXXXX.azurewebsites.net**](urn:gd:lg:a:send-vm-keys)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image39.png)

Task 4 : Confirm the log stream from the command line

1.  To access the log stream, update the below command with your appname
    and execute the following Gitbash CLI command. (Replace
    XXXXXXXXXXXXXX with your code)

> az webapp log tail -g azure-javaweb-app-XXXXXXXXXXXXXX-rg -n
> azure-javaweb-app-XXXXXXXXXXXXXX
>
> eg : az webapp log tail -g azure-javaweb-app-1691402277330-rg -n
> azure-javaweb-app-1691402277330
>
> Then you can see the following result:
>
> ![Screenshot](./media/image40.png)
>
> ![Screenshot](./media/image41.png)

Exercise 3 : Auto scale a web app by using custom metrics

Task 1 : Configure autoscale

Configure the autoscale settings for your App Service plan.

1.  Open a new tab in browser and go
    to [**https://portal.azure.com**](urn:gd:lg:a:send-vm-keys) and sign
    in with your Office 365 admin tenant credentials.

2.  Search and select [**autoscale**](urn:gd:lg:a:send-vm-keys) in the
    search bar or select **Autoscale** under **Settings** in the menu
    bar on the left.

> ![Screenshot](./media/image42.png)

3.  Select your **App Service plan**. You can only configure production
    plans.

> ![](./media/image43.png)

Task 2 : Set up a scale-out rule

Set up a scale-out rule so that Azure spins up another instance of the
web app when your web app is handling more than 70 sessions per
instance.

1.  Select **Custom autoscale**.In the **Rules** section of the default
    scale condition, select **Add a rule**.

> ![](./media/image44.png)
>
> ![](./media/image45.png)

2.  From the **Metric source** dropdown, select **Other resource**.

3.  From **Resource type**, select **App Service plans**.

4.  From the **Resource** dropdown, select your web app.

5.  Select a **Metric namespace** to base your scaling on. For example,
    use **CPU Percentage**.

> ![](./media/image46.png){width="6.5in" height="2.5076388888888888in"}

6.  Select the **Enable metric divide by instance count** checkbox so
    that the number of sessions per instance is measured.

7.  From the **Operator** dropdown, select **Greater than**.

8.  Enter the **Metric threshold to trigger the scale action**. For
    example, use **70**.

9.  Under **Action**, set **Operation** to **Increase count by**.
    Set **Instance count** to **1**.

10. Select **Add**.

> ![](./media/image47.png){width="6.492361111111111in"
> height="3.9166666666666665in"}

Limit the number of instances

1.  Set the maximum number of instances that can be spun up in
    the **Maximum** field of the **Instance limits** section. For
    example, use **4**.

2.  Select **Save**.

> ![Screenshot](./media/image48.png){width="6.247222222222222in"
> height="4.5in"}

Task 3 : Clean up resources:

**Do not delete resource group and the app as the same application will
be used in
Lab 13 - Configuring your App Service or Azure Functions app to use Entra ID login
to test authentication and authorization features.**

**Summary:**

In this labguide, we learnt how to create and package a Java web
application, how to use the Maven Plugin for Azure Web Apps, and how to
deploy your application to Azure App Service. These steps are applicable
not only for JSF applications but also most Java web applications.
