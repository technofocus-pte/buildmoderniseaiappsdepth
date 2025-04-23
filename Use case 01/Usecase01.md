# Caso de uso 01 - Incorpore aplicativos Quarkus Todo em Container Apps e integre-os a um banco de dados relacional.

**Objetivo:**

Este caso de uso mostra como desenvolver, configurar e implementar um
aplicativo Quarkus seguro de lista de tarefas (To-do) no Container Apps,
conectado a um Azure Database for PostgreSQL.  
Ao final, você terá um aplicativo Quarkus em execução no Azure App
Service no Linux.  
Principais tecnologias utilizadas: Java 17, Azure Database for
PostgreSQL.

**Principais tecnologias usadas** – Java 17, Azure Database for
PostgreSQL

**Duração estimada** - 45 minutos

**Tipo de laboratório:** Conduzido por instrutor

### Tarefa 0: Configurar variáveis de ambiente

1.  Pesquise Variável de ambiente no menu Iniciar do Windows e selecione
    Edit System Environment variable.

![](./media/image1.jpeg)

2.  Clique no botão **Environment Variable**.

![](./media/image2.jpeg)

3.  Selecione **JAVA_HOME** em **User variable for Admin** e clique em
    **Edit**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image3.jpeg)

4.  Insira o valor da variável como **C:\Program Files\Java\jdk-17** e
    clique em **Ok**.

![](./media/image4.jpeg)

5.  Navegue até a pasta **C:\Software** e clique com o botão direito do
    mouse na pasta **apache-maven-3.9.4-bin.zip** e selecione **Extract
    All**.

![](./media/image5.jpeg)

6.  **Extract** na mesma pasta.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image6.jpeg)

7.  Volte para a janela Edit Environment variable, selecione
    **MAVEN_HOME** e clique em **Edit**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image7.jpeg)

8.  Insira o valor da variável como
    \`C:\Software\apache-maven-3.9.4-bin\apache-maven-3.9.4 e clique em
    **OK.**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image8.jpeg)

9.  Na janela **Environmental Variable**s, clique em **Ok** e novamente
    em **OK.**

![](./media/image9.jpeg)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image10.jpeg)

10. Atualize **JAVA_HOME** de acordo com os requisitos do laboratório
    antes de executá-lo.

## Exercício 1: Gerar o aplicativo Quarkus usando o Maven

Existem várias maneiras de gerar uma estrutura de projeto do Quarkus.
Você pode usar a interface da Web do Quarkus, um plug-in IDE ou o
plug-in Quarkus Maven. Vamos usar o plug-in Maven para gerar a estrutura
do projeto.

Você gera seu aplicativo com várias dependências:

- A dependência resteasy para expor um endpoint REST

- A dependência jackson para serializar e desserializar JSON

- A dependência hibernate para interagir com o banco de dados

- A dependência do postgresql para se conectar ao banco de dados
  PostgreSQL

- A dependência do docker para criar uma imagem do Docker

Você não precisa especificar dependências do Azure porque executará sua
aplicação localmente primeiro e, em seguida, implementará uma versão
conteinerizada dela no Azure Container Apps.

### Tarefa 1 : Gerar o aplicativo Quarkus

1.  Abra o **Git Bash** no menu Iniciar da janela e execute o comando
    abaixo

> mvn -U io.quarkus:quarkus-maven-plugin:3.7.3:create \\
>
> -DplatformVersion=3.7.3 \\
>
> -DprojectGroupId=com.example.demo \\
>
> -DprojectArtifactId=todo \\
>
> -DclassName="com.example.demo.TodoResource" \\
>
> -Dpath="/api/todos" \\
>
> -DjavaVersion=17 \\

-Dextensions="resteasy-jackson, hibernate-orm-panache, jdbc-postgresql,
docker"

![](./media/image11.jpeg)

2.  Este comando cria um novo projeto Quarkus. Ele gera uma estrutura de
    diretórios do Maven (src/main/java para o código-fonte e
    src/test/java para os testes).  
    Também cria algumas classes Java, alguns testes e arquivos
    Dockerfile.  
    Além disso, gera um arquivo pom.xml com todas as dependências
    necessárias (Hibernate, RESTEasy, Jackson, PostgreSQL e Docker):

3.  Clique em Pesquisar e digite \`\`IntelliJ IDE\`\`e selecione
    **IntelliJ IDE**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image12.jpeg)

4.  Marque a caixa de seleção de confirmação e clique no botão
    **Continue**.

![Uma captura de tela de uma tela de computador Descrição gerada
automaticamente](./media/image13.jpeg)

5.  Feche a janela Data sharing.

![](./media/image14.jpeg)

6.  Selecione o botão de opção **Start trial** e clique no botão **Start
    trial**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image15.jpeg)

7.  Clique no botão **Allow access**.

![](./media/image16.jpeg)

8.  Feche o navegador, volte para a janela de licença do IntelliJ e
    clique no botão **Continue**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image17.jpeg)

9.  Clique em **Open** pasta.

![](./media/image18.jpeg)

10. Navegue até **C:\Users\Admin\todo** e selecione a pasta do projeto
    **todo** e clique em **OK.**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image19.jpeg)

11. Clique no botão **Trust project**.

![](./media/image20.jpeg)

12. Abra o arquivo **pom.xml** e você deverá ver o seguinte formato em
    XML:

> \<dependencies\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-hibernate-orm-panache\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-resteasy-jackson\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-jdbc-postgresql\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-container-image-docker\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-arc\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-hibernate-orm\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-resteasy\</artifactId\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.quarkus\</groupId\>
>
> \<artifactId\>quarkus-junit5\</artifactId\>
>
> \<scope\>test\</scope\>
>
> \</dependency\>
>
> \<dependency\>
>
> \<groupId\>io.rest-assured\</groupId\>
>
> \<artifactId\>rest-assured\</artifactId\>
>
> \<scope\>test\</scope\>
>
> \</dependency\>

\</dependencies\>

**Observação:** Todas as dependências no arquivo pom.xml estão definidas
no BOM (bill of materials) do Quarkus: io.quarkus.platform:quarkus-bom.

![](./media/image21.jpeg)

### Tarefa 2: Codificar o aplicativo

1.  Vá para **src/main/java/com.example.demo** e clique com o botão
    direito do mouse em **MyEntity.Java -\> Refactor -\> Rename**.

![](./media/image22.jpeg)

2.  Renomeie a classe gerada ***MyEntity.java*** para ''Todo.java''
    (localizada na mesma pasta que o *arquivo TodoResource.java*)

![](./media/image23.jpeg)

3.  Substitua o código existente pelo código Java a seguir. Ele utiliza
    a **Java Persistence API** (jakarta.persistence.\* package) para
    armazenar e recuperar dados do seu servidor PostgreSQL. Também
    utiliza o \[Hibernate ORM with Panache\]{.underline} (herdando de
    io.quarkus.hibernate.orm.panache.PanacheEntity) para simplificar a
    camada de persistência.

4.  Você usará uma entidade JPA (@Entity) para mapear diretamente o
    objeto Java Todo para a tabela Todo no PostgreSQL. O endpoint REST
    TodoResource cria uma nova instância da entidade Todo e a persiste.
    Essa classe é um modelo de domínio mapeado para a tabela Todo, que
    será criada automaticamente pelo JPA.

5.  Estender a classe PanacheEntity fornece uma série de métodos
    genéricos de CRUD (create, read, update, and delete) para o seu
    tipo. Assim, é possível salvar e deletar objetos Todo com apenas uma
    linha de código Java.

6.  Configure o JDK no IntelliJ, caso ainda não esteja configurado.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image24.jpeg)

1.  Substitua o código existente pelo seguinte código Java para a
    entidade Todo:

> package com.example.demo; 
>
>  
>
> import io.quarkus.hibernate.orm.panache.PanacheEntity; 
>
>  
>
> import jakarta.persistence.Entity; 
>
> import java.time.Instant; 
>
>  
>
> @Entity 
>
> public class Todo extends PanacheEntity { 
>
>  
>
>     public String description; 
>
>  
>
>     public String details; 
>
>  
>
>     public boolean done; 
>
>  
>
>     public Instant createdAt = Instant.now(); 
>
>  
>
>     @Override 
>
>     public String toString() { 
>
>         return "Todo{" + 
>
>                 "id=" + id + '\\' + 
>
>                 ", description='" + description + '\\' + 
>
>                 ", details='" + details + '\\' + 
>
>                 ", done=" + done + 
>
>                 ", createdAt=" + createdAt + 
>
>                 '}'; 
>
>     } 
>
> }

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image25.jpeg)

8.  Para gerenciar essa classe, atualize a classe **TodoResource** para
    que ela possa publicar interfaces REST para armazenar e recuperar
    dados usando HTTP.  
    Abra a classe **TodoResource** e substitua o código pelo seguinte:

> package com.example.demo; 
>
> import jakarta.inject.Inject; 
>
> import jakarta.transaction.Transactional; 
>
> import jakarta.ws.rs.Consumes; 
>
> import jakarta.ws.rs.GET; 
>
> import jakarta.ws.rs.POST; 
>
> import jakarta.ws.rs.Path; 
>
> import jakarta.ws.rs.Produces; 
>
> import static jakarta.ws.rs.core.MediaType.APPLICATION_JSON; 
>
> import jakarta.ws.rs.core.Response; 
>
> import jakarta.ws.rs.core.UriBuilder; 
>
> import jakarta.ws.rs.core.UriInfo; 
>
> import org.jboss.logging.Logger; 
>
> import java.util.List; 
>
> @Path("/api/todos") 
>
> @Consumes(APPLICATION_JSON) 
>
> @Produces(APPLICATION_JSON) 
>
> public class TodoResource { 
>
>     @Inject 
>
>     Logger logger; 
>
>  
>
>     @Inject 
>
>     UriInfo uriInfo; 
>
>     @POST 
>
>     @Transactional 
>
>     public Response createTodo(Todo todo) { 
>
>         logger.info("Creating todo: " + todo); 
>
>         Todo.persist(todo); 
>
>         UriBuilder uriBuilder =
> uriInfo.getAbsolutePathBuilder().path(todo.id.toString()); 
>
>         return
> Response.created(uriBuilder.build()).entity(todo).build(); 
>
>     } 
>
>     @GET 
>
>     public List\<Todo\> getTodos() { 
>
>         logger.info("Getting all todos"); 
>
>         return Todo.listAll(); 
>
>     } 
>
> } 

### ![A screenshot of a computer program Description automatically generated](./media/image26.jpeg)

### **Tarefa 3: Executar o aplicativo**

Quando você executa o aplicativo em modo de desenvolvimento, o Docker
Desktop precisa estar em execução. Isso ocorre porque o Quarkus detecta
que você precisa de um banco de dados PostgreSQL (por causa da
dependência do PostgreSQL quarkus-jdbc-postgresql declarada no *arquivo
pom.xml*), baixa a imagem do PostgreSQL Docker Desktop e inicia um
contêiner com o banco de dados. Em seguida, ele cria automaticamente a
tabela Todo no banco de dados.

1.  Clique duas vezes em **Docker Desktop** e minimize a janela.
    Certifique-se de que está em execução. (não é necessário fazer
    login)

![Uma tela de computador com um fundo branco Descrição gerada
automaticamente](./media/image27.jpeg)

2.  Volte para o Gitbash e execute o aplicativo to-do usando este
    comando:

cd todo

./mvnw quarkus:dev

![](./media/image28.jpeg)

3.  O aplicativo Quarkus deve iniciar e se conectar ao seu banco de
    dados. Você deve ver a seguinte saída:

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image29.jpeg)

![Uma tela de computador com texto e imagens Descrição gerada
automaticamente](./media/image30.jpeg)

![](./media/image31.jpeg)

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image32.jpeg)

4.  Clique em **Allow access**.

![](./media/image31.jpeg)

5.  Para testar o aplicativo, você pode usar cURL.

Em uma nova instância separada do Gitbash, crie um novo item de tarefa
no banco de dados com o comando a seguir. Você deve ver o log no console
do Quarkus:

curl --header "Tipo de conteúdo: application/json" \\

--request POST \\

--data '{"description":"Faça o Quarkus MS Learn","details":"Faça o MS
Learn sobre a implementação do Quarkus nos Aplicativos de Contêiner do
Azure","done": "true"}' \\

http://127.0.0.1:8080/api/todos

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image33.jpeg)

6.  Este comando deve retornar o item criado (com um identificador):

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image33.jpeg)

7.  Crie uma segunda tarefa usando o seguinte comando cURL:

> curl --header "Content-Type: application/json" \\
>
>     --request POST \\
>
>     --data '{"description":"Take Azure Container Apps MS
> Learn","details":"Take the ACA Learn module","done": "false"}' \\

http://127.0.0.1:8080/api/todos

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image34.jpeg)

8.  Em seguida, recupere os dados usando uma nova solicitação cURL:

curl http://127.0.0.1:8080/api/todos 

Este comando retorna a lista de itens de tarefas, incluindo os itens que
você criou:

![](./media/image35.jpeg)

### Tarefa 4 : Testar o aplicativo

Para testar a aplicação, você pode usar a classe **TodoResourceTest**
existente. Ela precisa testar o endpoint REST. Para testar o endpoint,
ela utiliza o \[RESTAssured\]{.underline}.

1.  Volte para o IntelliJ e abra a classe **TodoResourceTest** em
    **src/test/java/com/example/demo**. Substitua o código na **classe
    TodoResourceTest** pelo seguinte código:

> package com.example.demo; 
>
>  
>
> import io.quarkus.test.junit.QuarkusTest; 
>
> import static io.restassured.RestAssured.given; 
>
> import static jakarta.ws.rs.core.HttpHeaders.CONTENT_TYPE; 
>
> import static jakarta.ws.rs.core.MediaType.APPLICATION_JSON; 
>
> import org.junit.jupiter.api.Test; 
>
>  
>
> @QuarkusTest 
>
> class TodoResourceTest { 
>
>  
>
>     @Test 
>
>     void shouldGetAllTodos() { 
>
>         given() 
>
>                 .when().get("/api/todos") 
>
>                 .then() 
>
>                 .statusCode(200); 
>
>     } 
>
>  
>
>     @Test 
>
>     void shouldCreateATodo() { 
>
>         Todo todo = new Todo(); 
>
>         todo.description = "Take Quarkus MS Learn"; 
>
>         todo.details = "Take the MS Learn on deploying Quarkus to
> Azure Container Apps"; 
>
>         todo.done = true; 
>
>  
>
>         given().body(todo) 
>
>                 .header(CONTENT_TYPE, APPLICATION_JSON) 
>
>                 .when().post("/api/todos") 
>
>                 .then() 
>
>                 .statusCode(201); 
>
>     } 
>
> } 

![Uma captura de tela de computador de um programa Descrição gerada
automaticamente](./media/image36.jpeg)

2.  Ao testar o aplicativo, o Docker Desktop precisa estar em execução
    pois o Quarkus detecta que precisa do banco de dados PostgreSQL para
    teste.

3.  Volte para **Gitbash** e pressione Ctrl + C para encerrar a execução
    atual.  
    Em seguida, execute os comandos abaixo para testar o aplicatvo:

./mvnw teste limpo

![Uma tela de computador com texto e imagens Descrição gerada
automaticamente](./media/image37.jpeg)

Você deve ver uma saída semelhante a esta:

![Uma tela de computador com texto e números Descrição gerada
automaticamente](./media/image38.jpeg)

## Exercício 2 – Configurar Aplicativos de Contêiner do Azure

Neste exercício, você criará um grupo de recursos do Azure que conterá
os recursos para o aplicativo. Em seguida, configurará o banco de dados
PostgreSQL usando o Azure CLI. Por fim, configurará o aplicativo Quarkus
para acessar o banco de dados PostgreSQL remoto. Use um terminal de sua
escolha para executar os comandos.

## Tarefa 1 : Preparar o ambiente de trabalho

Você precisa configurar algumas variáveis de ambiente. Aqui estão
algumas observações sobre as variáveis que você criará:

[TABLE]

**Observação:** Você pode nomear seus recursos do Azure da maneira que
preferir. Este artigo fornece abreviações de exemplo para muitos
recursos do Azure (por exemplo, rg para resource groups e ca para
container apps).

1.  Use os comandos a seguir para configurar as variáveis. Certifique-se
    de modificar os valores conforme descrito na tabela anterior. Essas
    variáveis de ambiente são usadas em todo o restante deste módulo.

**Observação:** o PostgreSQL é compatível apenas com **o Westus** .
Tente primeiro no local do oeste e se você tiver algum problema, tente
no local perto de você

export AZ_PROJECT_Quarkus="azure-deploy-quarkus-"$RANDOM

exportação AZ_CONTAINERAPP="ca${AZ_PROJECT_Quarkus}"

export AZ_CONTAINERAPP_ENV="cae${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_DB_NAME="postgres${AZ_PROJECT_Quarkus}"

export AZ_POSTGRES_USERNAME="azuser123"

exportar AZ_POSTGRES_PASSWORD="P@55w.rd12345"

export AZ_POSTGRES_SERVER_NAME="psql${AZ_PROJECT_Quarkus}"

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image39.png)

2.  Volte para o Gitbash e execute o comando abaixo para definir a
    variável do grupo de recursos. Copie o nome do grupo de recursos.

> export AZ_RESOURCE_GROUP="Seu grupo de recursos existente"
>
> export AZ_LOCATION="Localização perto de você"

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image40.png)

![Uma captura de tela de computador de texto Descrição gerada
automaticamente](./media/image41.png)

3.  Execute o comando ''az login'' Ele abrirá o navegador padrão para
    realizar o login.  
    Faça login com a conta da sua assinatura do Azure.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image42.png)

### Tarefa 2: Criar uma instância do Azure Database for PostgreSQL

1.  Agora você criará um servidor PostgreSQL gerenciado. Execute o
    seguinte comando para criar uma pequena instância do Banco de Dados
    do Azure para PostgreSQL:

az postgres flexible-server create --resource-group "$AZ_RESOURCE_GROUP"
--location "$AZ_LOCATION" --name "$AZ_POSTGRES_SERVER_NAME"
--database-name "$AZ_POSTGRES_DB_NAME" --admin-user
"$AZ_POSTGRES_USERNAME" --admin-password "$AZ_POSTGRES_PASSWORD"
--public-access "Todos" --tier "Burstable" --sku-name "Standard_B1ms"
--storage-size 32 --version "16"

![Uma captura de tela de um código de computador Descrição gerada
automaticamente](./media/image43.jpeg)

2.  Esse comando cria um pequeno servidor PostgreSQL que usa as
    variáveis que você configurou anteriormente.

![Uma captura de tela de uma tela de computador Descrição gerada
automaticamente](./media/image44.jpeg)

### Tarefa 3: Configurar o Quarkus para acessar o banco de dados PostgreSQL

1.  Agora você conectará o aplicativo Quarkus ao banco de dados
    PostgreSQL. Para fazer isso, primeiro você precisa obter a cadeia de
    conexão para o banco de dados:

2.  Execute o comando abaixo para obter a cadeia de conexão para o banco
    de dados.

3.  exportar POSTGRES_CONNECTION_STRING=$(

> az postgres flexible-server show-connection-string --server-name
> "$AZ_POSTGRES_SERVER_NAME" --database-name "$AZ_POSTGRES_DB_NAME"
> --admin-user "$AZ_POSTGRES_USERNAME" --admin-password
> "$AZ_POSTGRES_PASSWORD" --query "connectionStrings.jdbc" --output tsv

)

export
POSTGRES_CONNECTION_STRING_SSL="$POSTGRES_CONNECTION_STRING&ssl=true&sslmode=require"

echo "POSTGRES_CONNECTION_STRING_SSL=$POSTGRES_STRING_DE_CONEXÃO_SSL"

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image45.jpeg)

4.  Observe a cadeia de conexão retornada.

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image46.jpeg)

### Tarefa 4: Configurar o aplicativo Quarkus para se conectar ao banco de dados PostgreSQL

1.  Volte para o Intellij IDE. Atualize o arquivo
    **application.properties** na pasta **src/main/resources** do
    projeto para configurar a cadeia de conexão com o banco de dados
    PostgreSQL.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image47.jpeg)

2.  Defina a **propriedade quarkus.datasource.jdbc.url** como o valor
    \\POSTGRES_CONNECTION_STRING_SSL de saída anterior. A parte
    **&ssl=true&sslmode=require** da cadeia de conexão força o driver a
    usar SSL, um requisito para o Azure Database for PostgreSQL.

quarkus.hibernate-orm.database.generation=atualizar

quarkus.datasource.jdbc.url=\<o valor POSTGRES_CONNECTION_STRING_SSL\>

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image48.jpeg)

### Tarefa 5: Execute o aplicativo Quarkus localmente para testar a conexão remota do banco de dados

1.  Volte para o Gitbash e execute o comando abaixo para executar o
    aplicativo localmente:

./mvnw clean quarkus:dev

![Uma tela de computador com texto e imagens Descrição gerada
automaticamente](./media/image49.jpeg)

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image50.jpeg)

![](./media/image51.jpeg)

2.  Quando o Quarkus estiver em execução, crie algumas tarefas usando os
    seguintes comandos cURL em uma janela de terminal separada:

> curl --header "Tipo de conteúdo: application/json" \\
>
> --request POST \\
>
> --data '{"description":"Faça o Quarkus MS Learn","details":"Faça o MS
> Learn sobre a implementação do Quarkus nos Aplicativos de Contêiner do
> Azure","done": "true"}' \\

''http://127.0.0.1:8080/api/todos''

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image52.jpeg)

curl --header "Content-Type: application/json" \\

    --request POST \\

    --data '{"description":"Take Azure Container Apps MS
Learn","details":"Take the ACA Learn module","done": "false"}' \\

'' http://127.0.0.1:8080/api/todos''

![](./media/image53.jpeg)

3.  Em seguida, verifique se os itens de tarefas (to-dos) estão no banco
    de dados acessando o endpoint GET que está definido no aplicativo de
    tarefas:

''Curl http://127.0.0.1:8080/api/todos''

Você deve ver a seguinte saída:

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image54.jpeg)

Se você vir essa saída, você executou com êxito o aplicativo Quarkus e
se conectou ao banco de dados PostgreSQL remoto.

## Exercício 3: Implementar um aplicativo Quarkus nos Azure Container Apps

Neste exercício, você cria o ambiente dos Azure Container Apps usando a
CLI do Azure.

### Tarefa 1: Configurar o Dockerfile para o aplicativo Quarkus

1.  Os Aplicativos de Contêiner são usados para implementar aplicativos
    em contêineres. Portanto, primeiro você precisa conteinerizar o
    aplicativo Quarkus em uma imagem do Docker. Esse processo é fácil
    porque o plug-in Quarkus Maven já gerou alguns Dockerfiles em
    **src/main/docker**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image55.jpeg)

2.  Volte para o Gitbash e pressione Ctrl+C . Execute o comando abaixo
    para renomear um desses **Dockerfiles, *Dockerfile.jvm*,** para
    ***Dockerfile*** e movê-lo para a pasta raiz:

''mv src/main/docker/Dockerfile.jvm ./Dockerfile''

![Uma tela preta com texto branco Descrição gerada
automaticamente](./media/image56.jpeg)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image55.jpeg)

3.  Substitua o conteúdo após o comentário longo no **Dockerfile** pelo
    seguinte, ou seja, na linha \# 80

> FROM registry.access.redhat.com/ubi8/openjdk-17:1.18 
>
>  
>
> ENV LANGUAGE='en_US:en' 
>
>  
>
>  
>
> \# We make four distinct layers so if there are application changes
> the library layers can be re-used 
>
> COPY --chown=185 target/quarkus-app/lib/ /deployments/lib/ 
>
> COPY --chown=185 target/quarkus-app/\*.jar /deployments/ 
>
> COPY --chown=185 target/quarkus-app/app/ /deployments/app/ 
>
> COPY --chown=185 target/quarkus-app/quarkus/ /deployments/quarkus/ 
>
>  
>
> EXPOSE 8080 
>
> USER 185 
>
> ENV JAVA_OPTS_APPEND="-Dquarkus.http.host=0.0.0.0
> -Djava.util.logging.manager=org.jboss.logmanager.LogManager" 
>
> ENV JAVA_APP_JAR="/deployments/quarkus-run.jar" 
>
>  
>
> ENTRYPOINT \[ "/opt/jboss/container/java/run/run-java.sh" \] 

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image57.jpeg)

4.  Este Dockerfile espera que o aplicativo Quarkus seja empacotado como
    um ***arquivo* quarkus-run.jar**. Esse nome é o nome padrão para o
    aplicativo Quarkus quando ele é empacotado como um arquivo JAR. Você
    precisa ter certeza de que o aplicativo Quarkus está empacotado como
    um arquivo JAR. Para fazer isso, execute o seguinte comando Maven:

\`\`./mvnw package\`\`

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image58.jpeg)

![](./media/image59.jpeg)

5.  Esse comando empacota o aplicativo Quarkus em um arquivo JAR e gera
    um arquivo ***quarkus-run.jar*** na pasta ***target/quarkus-app***.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image60.jpeg)

### Tarefa 2: Criar o ambiente de Container Apps e implementar o contêiner

1.  Agora que o Dockerfile está no local certo, você pode criar o
    ambiente de Container Apps e implementar o contêiner usando um único
    comando Azure CLI. Execute o seguinte comando na raiz do projeto:

az containerapp up --name "$AZ_CONTAINERAPP" --environment
"$AZ_CONTAINERAPP_ENV" --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --ingress external --target-port 8080 --source .

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image61.jpeg)

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image61.jpeg)

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image62.jpeg)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image63.jpeg)

2.  Este comando realiza várias ações:

    - Cria um ambiente do Azure Container Apps, caso ele ainda não
      exista

    - Cria um registro do Azure se ele não existir

    - Cria um workspace do Log Analytics se ele não existir

    - Cria a imagem do Docker e a envia por push para o registro do
      Azure

    - Implementa a imagem do Docker no ambiente de Aplicativos de
      Contêiner

O comando az containerapp up leva algum tempo para ser executado. Você
deve ver uma saída semelhante à seguinte:

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image64.jpeg)

### Tarefa 3: Validar a implementação

Você pode validar se a implementação foi bem-sucedida de várias
maneiras. A maneira mais fácil é pesquisar seu grupo de recursos no
portal do Azure. Você deve ver recursos semelhantes aos seguintes:

1.  Abra um navegador e vá para ''https:\\portal.azure.com'' e entre com
    sua conta de assinatura do Azure. Clique na Caixa Resource groups.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image65.jpeg)

2.  Clique no nome do resource group.

![](./media/image66.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image67.png)

3.  Você também pode verificar a implementação executando o comando a
    seguir. Ele lista todos os recursos criados pelo comando az
    containerapp up.

az resource list --location "$AZ_LOCATION" --resource-group
"$AZ_RESOURCE_GROUP" --output table

Você deve ver uma saída semelhante a esta:

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image68.png)

### Tarefa 4: Execute o aplicativo Quarkus implementado

1.  Agora você pode executar o aplicativo Quarkus implementado.
    Primeiro, você precisa obter a URL do aplicativo.

2.  Volte para o Gitbash e execute o comando abaixo para obter a URL do
    aplicativo.

> export AZ_APP_URL=$( 
>
>     az containerapp show \\
>
>         --name "$AZ_CONTAINERAPP" \\
>
>         --resource-group "$AZ_RESOURCE_GROUP" \\
>
>         --query "properties.configuration.ingress.fqdn" \\
>
>         --output tsv \\ ) 
>
> \`\`echo "AZ_APP_URL=$AZ_APP_URL"\`

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image69.jpeg)

3.  Seu aplicativo está pronto em
    https://\\app-name\>.azurecontainerapps.io/. Observe o protocolo
    https. Esse protocolo é usado porque o aplicativo é implementado com
    um certificado TLS. Para testar o aplicativo, você pode usar cURL:

> curl --header "Content-Type: application/json" \\
>
>     --request POST \\
>
>     --data '{"description":"Configuration","details":"Congratulations,
> you have set up your Quarkus application correctly!","done": "true"}'
> \\

'' https://$AZ_APP_URL/api/todos''

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image70.jpeg)

4.  Recupere os dados usando uma nova solicitação cURL:

''curl https://$AZ_APP_URL/api/todos''

5.  Este comando retorna a lista de todos os itens to-do do banco de
    dados:

![Uma tela de computador com texto branco Descrição gerada
automaticamente](./media/image71.jpeg)

6.  Volte para o portal do Azure e clique no nome do container app.

![](./media/image72.png)

7.  Clique no link da URL do aplicativo. Ela abre o aplicativo na guia
    do navegador.

![](./media/image73.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image74.png)

8.  Execute este comando, você pode transmitir os logs do seu contêiner
    ao criar novas tarefas:

az containerapp logs show --name "$AZ_CONTAINERAPP" --resource-group
"$AZ_RESOURCE_GROUP" -–follow

![Uma captura de tela de uma tela de computador Descrição gerada
automaticamente](./media/image75.png)

9.  Execute mais comandos cURL. Você deverá ver os registros percorrendo
    o terminal.

''curl https://$AZ_APP_URL/api/todos''

![Uma captura de tela de uma tela de computador Descrição gerada
automaticamente](./media/image76.png)

## Exercício 4: Excluir recursos no grupo de recursos

### Tarefa 1: Excluir recursos.

1.  Volte para o portal do Azure. Clique em **Resources groups**.

![](./media/image77.png)

2.  Clique no nome do resource group.

![](./media/image78.png)

3.  Selecione todos os recursos e clique em **Delete** (NÃO SELECIONE
    DELETE – Resource group)

![](./media/image79.png)

4.  Digite ''delete'' e clique em **Delete**.

![](./media/image80.png)

5.  Confirme a exclusão de recursos.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image81.png)

**Resumo**

Você aprendeu a usar o Maven para inicializar o aplicativo e um ambiente
de desenvolvimento integrado (IDE) para editar o código. Você aprendeu a
usar o Docker para iniciar um banco de dados PostgreSQL local para poder
executar e testar o aplicativo localmente. Você executou com sucesso o
aplicativo Quarkus e se conectou ao banco de dados PostgreSQL remoto.

### 
