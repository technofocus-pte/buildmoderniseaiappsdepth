# Caso de uso 11 – Criando um Copilot usando o Azure OpenAI, Azure Cosmos DB for NoSQL

Nesse caso de uso, você conectará um aplicativo Web Blazor ao Azure
Cosmos DB for NoSQL e ao Azure OpenAI usando kits de desenvolvimento de
software .NET. Seu código gerencia e consulta itens em um contêiner da
API para NoSQL. Além disso, o código envia prompts para o Azure OpenAI e
analisa as respostas.

**Duração do laboratório:** 45 minutos

**Tipo de laboratório**: Conduzido por instrutor

**Objetivo**

- Configurar o ambiente de desenvolvimento para Blazor, PostgreSQL e
  OpenAI.

- Criar um projeto Blazor e projetar uma interface de chat responsiva.

- Configurar o banco de dados PostgreSQL no Azure e conectá-lo ao
  aplicativo Blazor.

- Integrar o Azure OpenAI para funcionalidades aprimoradas de chat.

- Implementar o aplicativo Blazor e o banco de dados PostgreSQL no
  Azure.

- Testar o aplicativo para garantir uma interação perfeita entre os
  componentes.

- Monitorar e solucionar problemas do aplicativo implementado no Azure.

**Principais tecnologias usadas:** Azure Cosmos DB for NoSQL, Azure
OpenAI

## Exercício 0: Entender a VM e as credenciais

Nesta tarefa, identificaremos e entenderemos as credenciais que usaremos
em todo o laboratório.

1.  A guia **Instructions** contém o guia do laboratório com as
    instruções a serem seguidas em todo o laboratório.

2.  A guia **Resources** tem as credenciais necessárias para executar o
    laboratório.

    - **URL** – URL para o portal do Azure

    - **Subscription** – Este é o ID da assinatura atribuída a você

    - **Username** – a ID de usuário com a qual você precisa fazer login
      nos serviços do Azure.

    - **Password** – senha para o login do Azure.

Vamos chamar esse nome de usuário e senha como credenciais de login do
Azure. Usaremos essas credenciais sempre que mencionarmos as credenciais
de login do Azure.

- **Resource Group** – O **Resource Group** atribuído a você.

\[! Alerta\] **Important:** certifique-se de criar todos os seus
recursos neste grupo de recursos

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image1.png)

3.  A guia **Help** contém as informações de suporte. O valor do **ID**
    aqui é o **Lab instance ID** que será usado durante a execução do
    laboratório.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image2.png)

## Exercício 1: Implementar a infraestrutura e concluir a configuração inicial

Para concluir este projeto, você precisa de uma conta do Azure Cosmos DB
for NoSQL e uma conta do Azure OpenAI. Para simplificar esse processo,
implemente um modelo Bicep no Azure com essas duas contas.

### Tarefa 1: Implementar infraestrutura do modelo

1.  Abra o arquivo no caminho **C:\Labfiles\Build and Test a custom chat
    application Using Azure Cosmos DB and AzureOpenAI** e atualize a
    versão do Azure OpenAI na linha 96 para +++0125+++. **Save** o
    arquivo.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image3.png)

2.  Abra um novo navegador e insira a seguinte URL na barra de
    endereços: +++<https://portal.azure.com/+++> para abrir o Portal do
    Azure.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image4.jpeg)

3.  No portal do Azure, clique no botão **\[\>\_\] (Cloud Shell)**
    localizado no topo página, à direita da caixa de pesquisa. Um painel
    do Cloud Shell será aberto na parte inferior do portal. Na primeira
    vez que você abrir o Cloud Shell, poderá ser solicitado que escolha
    o tipo de shell que deseja usar (**Bash** ou **PowerShell**).
    Selecione **Bash**. Caso essa opção não seja exibida, prossiga para
    a próxima etapa.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image5.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image6.jpeg)

4.  Na caixa de diálogo Introdução **Getting Started**, selecione
    **Mount storage account**, selecione sua **subscription** e clique
    em **Apply**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image7.jpeg)

5.  Na caixa de diálogo **Mount storage account,** selecione **we will
    create a storage account for your** e clique em **Next**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image8.jpeg)

![Um close-up de uma tela de computador O conteúdo gerado por IA pode
estar incorreto.](./media/image9.jpeg)

6.  Certifique-se de que o tipo de shell indicado no canto superior
    esquerdo do painel do Cloud Shell esteja definido como **Bash**. Se
    estiver como **PowerShell**, altere para **Bash** usando o menu
    suspenso.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image10.jpeg)

7.  Assim que o terminal for iniciado, clique em **Manage files -\>
    Upload**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image11.jpeg)

8.  Selecione o arquivo **azuredeploy. JSON** no caminho
    **C:\Labfiles\Build and Test a custom chat application Using Azure
    Cosmos DB and AzureOpenAI**  e clique em **Open**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image12.jpeg)

Você deverá receber uma mensagem, indicando que o upload do arquivo foi
concluído com sucesso.

![Um fundo branco com texto preto O conteúdo gerado por IA pode estar
incorreto.](./media/image13.jpeg)

9.  Crie uma nova variável de shell chamada **resourceGroupName** com o
    nome do grupo de recursos do Azure que você criou
    (mslearn-cosmos-openai).

+++resourceGroupName="ResourceGroup1"+++(Obtenha o nome do grupo de
recursos na guia Resources)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image14.png)

10. Faça a implementação do arquivo de modelo **azuredeploy.json** no
    grupo de recursos usando o comando az group deployment create. Em
    seguida, execute o seguinte comando.

+++az deployment group create --resource-group $resourceGroupName --name
zero-touch-deployment --template-file azuredeploy.json +++

**Observação:** essa implementação pode levar aproximadamente de 5 a 10
minutos.

![Uma captura de tela de uma tela de computador O conteúdo gerado por IA
pode estar incorreto.](./media/image15.jpeg)

![Uma captura de tela de uma tela de computador O conteúdo gerado por IA
pode estar incorreto.](./media/image16.jpeg)

### Tarefa 2: Obter credenciais de conta do Azure Cosmos DB for NoSQL e do Azure OpenAI

A implementação acima criou contas do Azure Cosmos DB para NoSQL e do
Azure OpenAI, e armazenou suas credenciais na configuração do aplicativo
web do Azure App Service.  
Agora, você pode optar por usar o portal do Azure ou a Azure CLI para
recuperar as credenciais de cada serviço.

1.  Na página Inicial do portal do Azure, clique em **Resource groups.**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image17.jpeg)

2.  Selecione seu resource group.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image18.png)

3.  Na página **Resource Groups**, expanda o painel **Essentials** e
    observe o cabeçalho **Deployments**. O status da implementação deve
    estar como **Succeeded** neste momento.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image19.png)

4.  Agora, selecione a conta do **Azure Cosmos DB** para navegar até a
    página do recurso.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image20.png)

5.  Selecione a opção **Keys** na seção **Settings** do menu de
    navegação do recurso. Anote os valores dos campos **URI** e
    **PRIMARY KEY**. Você usará esses valores em etapas posteriores.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image21.jpeg)

6.  Retorne à página **Resource Groups**. Selecione a conta **Azure
    OpenAI**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image22.png)

7.  Na janela do **Azure Open AI**, navegue até a seção **Resource
    Management** e clique em **Keys and Endpoints.**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image23.jpeg)

8.  Na página **Keys and Endpoints,** copie os valores de **KEY1**
    (*você pode usar tanto o KEY1 quanto o KEY2)* e **Endpoint** e, em
    seguida, **Save** essas informações em um bloco de notas para para
    usá-las nas próximas tarefas.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image24.jpeg)

### Tarefa 3: Executar o Docker

1.  Na caixa de pesquisa do Windows, digite +++Docker+++ e clique em
    **Docker Desktop**.

![Uma captura de tela de uma área de trabalho O conteúdo gerado por IA
pode estar incorreto.](./media/image25.jpeg)

2.  Execute o Docker Desktop.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image26.jpeg)

## Exercício 2 – Configurar e criar o aplicativo inicial

1.  Na barra de pesquisa da VM, pesquise +++Visual Studio+++ e selecione
    **Visual Studio Code**.

2.  Clique em **File** -\> **Open Folder**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image27.jpeg)

3.  Selecione **cosmosdb-chatgpt** em **C:\LabFiles** e clique em
    **Select Folder**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image28.jpeg)

4.  Clique na **Yes, I trust the authors** na caixa de diálogo **Do you
    trust the authors dialog**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image29.jpeg)

5.  No editor do **Visual Studio Code**, clique em **Terminal**, abra um
    **New Terminal**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image30.jpeg)

6.  Em um aplicativo .NET, é comum usar os provedores de configuração
    para injetar novas configurações em seu aplicativo. Para este
    projeto, utilize o arquivo **appsettings.Development.json** para
    fornecer os valores mais atualizados do endpoint e da chave do Azure
    OpenAI.

7.  Abra o arquivo **appsettings.Development.JSON**. Substitua os textos
    de exemplo que estão no lugar do uri e da key pelos valores reais do
    **Azure Cosmos DB** e **Azure OpenAI**  que você salvou no bloco de
    notas.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image31.jpeg)

8.  **Crie** o projeto .NET executando o comando abaixo.

+++dotnet build+++

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image32.jpeg)

## Exercício 3: Entendendo o código

### Tarefa 1: Adicionar membros necessários e uma instância do cliente

1.  Abra o arquivo **Services/OpenAiService.cs**. Esse arquivo
    implementa as variáveis de classe necessárias para usar o cliente
    Azure OpenAI. Ele também define alguns prompts estáticos e cria uma
    nova instância da classe OpenAIClient.

2.  Esse bloco de código cria uma nova variável do tipo string chamada
    \_systemPromptText com um bloco estático de texto que será enviado
    ao assistente de AI antes de cada prompt.

> private readonly string \_systemPrompt = @"
>
> You are an AI assistant that helps people find information.
>
> Provide concise answers that are polite and professional." +
> Environment.NewLine;

3.  Esse bloco de código cria outra variável string chamada
    \_summarizePrompt com um texto estático, que será enviado ao
    assistente de AI com instruções sobre como resumir uma conversa.

> private readonly string \_summarizePrompt = @"
>
> Summarize this prompt in one or two words to use as a label in a
> button on a web page.
>
> Do not use any punctuation." + Environment.NewLine;

4.  Esse bloco de código cria uma nova instância da classe OpenAIClient
    usando o ponto de extremidade para criar um Uri e a chave para criar
    um AzureKeyCredential.

> Uri uri = new(endpoint);
>
> Credencial AzureKeyCredential = new(key);
>
> \_client = novo(
>
> ponto de extremidade: uri,
>
> keyCredential: credencial
>
> );

**Tarefa 2: Fazer uma pergunta ao modelo de AI**

Primeiro, implemente uma conversa de pergunta e resposta enviando um
prompt do sistema, uma pergunta e uma ID de sessão para que o modelo de
AI possa fornecer uma resposta no contexto da conversa atual.
Certifique-se de medir o número de tokens necessários para analisar o
prompt e retornar uma resposta (ou conclusão neste contexto).

1.  Esse bloco de código cria uma nova variável chamada options do tipo
    ChatCompletionsOptions. Adiciona as duas variáveis de mensagem à
    lista Mensagens e define o valor de Usuário para o parâmetro do
    construtor sessionId.

> ChatCompletionsOptions options = new()
>
> {
>
> DeploymentName = "chatmodel",
>
> Mensagens = {
>
> novo ChatRequestSystemMessage(\_systemPrompt),
>
> new ChatRequestUserMessage(userPrompt)
>
> },
>
> Usuário = sessionId,
>
> MaxTokens = 4000,
>
> Temperatura = 0,3f,
>
> NucleusSamplingFactor = 0,5f,
>
> FrequencyPenalty = 0,
>
> PresencePenalty = 0
>
> };

2.  O método GetChatCompletionsAsync da variável de cliente Azure OpenAI
    (\_client) é invocado de forma assíncrona. O resultado é armazenado
    em uma variável chamada conclusões do tipo ChatCompletions.

> Response\<ChatCompletions\> completionsResponse = await_client.
> GetChatCompletionsAsync(opções);
>
> ChatCompletions conclusões = completionsResponse.Value;

3.  Por fim, o bloco de código abaixo retorna uma tupla como resultado
    do método GetChatCompletionAsync com o conteúdo da conclusão como
    uma cadeia de caracteres, o número de tokens associados ao prompt e
    o número de tokens para a resposta.

> return (
>
> completionText: completions.Choices\[0\].Message.Content,
>
> completionTokens: completions.Usage.CompletionTokens
>
> );

**Tarefa 3: Solicitar ao modelo de AI que resuma uma conversa**

Agora, envie para o modelo de AI um prompt de sistema diferente, sua
conversa atual e o ID da sessão, para que o modelo de AI possa resumir a
conversa em poucas palavras.

1.  O código abaixo cria uma variável ChatCompletionsOptions chamada
    options, com as duas variáveis de mensagem na lista Messages, User
    configurado para o parâmetro do construtor sessionId, MaxTokens
    configurado para 200 e as propriedades restantes.

> ChatCompletionsOptions options = new()
>
> {
>
> DeploymentName = "chatmodel",
>
> Messages = {
>
> new ChatRequestSystemMessage(\_systemPrompt),
>
> new ChatRequestUserMessage(conversationText)
>
> },
>
> User = sessionId,
>
> MaxTokens = 200,
>
> Temperature = 0.0f,
>
> NucleusSamplingFactor = 1.0f,
>
> FrequencyPenalty = 0,
>
> PresencePenalty = 0
>
> };

2.  O código abaixo chama o método assíncrono
    \_client.GetChatCompletionsAsync, passando o nome do modelo
    (\_modelName) e a variável options como parâmetros, e armazena o
    resultado na variável completions do tipo ChatCompletions. O
    conteúdo da resposta é retornado como resultado do método
    SummarizeAsync.

> Response\<ChatCompletions\> completionsResponse = await
> \_client.GetChatCompletionsAsync(options);
>
> ChatCompletions completions = completionsResponse.Value;
>
> string completionText = completions.Choices\[0\].Message.Content;
>
> return completionText;

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image33.jpeg)

**Tarefa 4 – Conectar ao Azure Cosmos DB for NoSQL**

A classe CosmosDbService contém uma implementação inicial de um serviço
semelhante à classe OpenAiService que você trabalhou anteriormente neste
módulo. No entanto, esta classe utiliza o SDK .NET para Azure Cosmos DB,
que funciona de maneira ligeiramente diferente.

Esta seção explica a implementação das variáveis da classe e do cliente
necessários para acessar o Azure Cosmos DB para NoSQL utilizando o
cliente.

1.  Abra o arquivo **Services/CosmosDbService.cs**.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image34.jpeg)

2.  O código abaixo cria uma variável chamada options do tipo
    CosmosSerializationOptions e define a propriedade
    PropertyNamingPolicy da variável como
    CosmosPropertyNamingPolicy.CamelCase.

> CosmosSerializationOptions options = new()
>
> {
>
> PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
>
> };

**Observação:** definir essa propriedade garantirá que o JSON produzido
pelo SDK seja serializado e desserializado em maiúsculas e minúsculas,
independentemente de como a propriedade correspondente está formatada na
classe .NET.

3.  O código abaixo cria uma nova instância do tipo CosmosClient chamada
    client usando a classe CosmosClientBuilder, juntamente com endpoint,
    key, e as opções de serialização que você especificou anteriormente.

> CosmosClient client = new CosmosClientBuilder(endpoint, key)
>
> .WithSerializerOptions(options)
>
> .Build();

4.  O código abaixo cria uma nova variável anulável do tipo Database
    chamada database, chamando o método GetDatabase da variável client.

**Database? database = client?.GetDatabase(databaseName);**

5.  O código abaixo atribui a variável container recebida no construtor
    à variável de classe \_container, somente se não for nula. Caso
    contrário, lança uma exceção ArgumentException.

> \_container = container ??
>
> throw new ArgumentException("Unable to connect to existing Azure
> Cosmos DB container or database.");

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image35.jpeg)

**Tarefa 5 – Implementar o serviço Azure Cosmos DB for NoSQL**

O serviço Azure Cosmos DB (CosmosDbService) gerencia consultas, criação,
exclusão e atualização de sessões e mensagens na sua aplicação de
assistente de AI. Para gerenciar todas essas operações, o serviço
precisa implementar diversos métodos para cada operação potencial,
utilizando vários recursos do SDK do .NET.

Há vários requisitos principais a serem abordados neste exercício:

- Implementar operações para criar uma sessão ou mensagem

- Implementar consultas para recuperar várias sessões ou mensagens

- Implementar uma operação para atualizar uma única sessão ou atualizar
  várias mensagens em lote

- Implementar uma operação para consultar e excluir várias sessões e
  mensagens relacionadas

> O Azure Cosmos DB for NoSQL armazena dados no formato JSON, permitindo
> que armazenemos vários tipos de dados em um único contêiner. Esta
> aplicação armazena tanto uma "sessão" de chat com o assistente de AI
> quanto as "mensagens" individuais dentro de cada sessão. Com a API
> para NoSQL, a aplicação pode armazenar ambos os tipos de dados no
> mesmo contêiner e depois diferenciá-los por meio de um simples campo
> de tipo.

.

1.  Abra o arquivo **Services/CosmosDbService.cs**.

2.  O código abaixo cria uma nova variável chamada partitionKey do tipo
    PartitionKey, utilizando a propriedade SessionId da sessão atual
    como parâmetro.

**PartitionKey partitionKey = new(session. SessionId);**

3.  O código abaixo invoca o método CreateItemAsync do container,
    passando o parâmetro session e a variável partitionKey. O resultado
    é retornado como resposta do método InsertSessionAsync.

> return await \_container.CreateItemAsync\<Session\>(
>
> item: session,
>
> partitionKey: partitionKey
>
> );

4.  O código abaixo cria uma variável PartitionKey usando
    session.SessionId como valor da chave de partição. Em seguida, cria
    uma nova variável chamada newMessage, com a propriedade Timestamp
    atualizada para o timestamp UTC atual. Invoca o método
    CreateItemAsync, passando tanto a nova mensagem quanto a variável da
    chave de partição. O resultado é retornado como a resposta de
    InsertMessageAsync.

> PartitionKey partitionKey = new(message.SessionId);
>
> Message newMessage = message with { TimeStamp = DateTime.UtcNow };
>
> return await \_container.CreateItemAsync\<Message\>(
>
> item: newMessage,
>
> partitionKey: partitionKey
>
> );

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image36.jpeg)

**Tarefa 6: Recuperar várias sessões ou mensagens**

Existem dois principais casos de uso onde a aplicação precisa recuperar
múltiplos itens do nosso container. O primeiro caso é quando a aplicação
recupera todas as sessões do usuário atual, filtrando os itens onde type
= Session. O segundo caso é quando a aplicação recupera todas as
mensagens de uma sessão, realizando um filtro similar onde type =
Session e sessionId = \<valor\>. Ambos os filtros são implementados aqui
utilizando o SDK .NET e um feed iterator.

.

1.  O código abaixo cria uma nova variável chamada query do tipo
    QueryDefinition. Ele utiliza o método fluente WithParameter para
    atribuir o nome da classe Session como valor para o parâmetro. Em
    seguida, invoca o método genérico GetItemQueryIterator\<\> na
    variável \_container, passando o tipo genérico Session e a variável
    query como parâmetro. O resultado é armazenado em uma variável do
    tipo FeedIterator chamada response.

> QueryDefinition query = new QueryDefinition("SELECT DISTINCT \* FROM c
> WHERE c.type = @type")
>
> .WithParameter("@type", nameof(Session));
>
> FeedIterator\<Session\> response =
> \_container.GetItemQueryIterator\<Session\>(query);

2.  O código abaixo dentro do loop while obtém de forma assíncrona a
    próxima página de resultados invocando o método ReadNextAsync na
    variável response e, em seguida, adiciona esses resultados à
    variável de lista chamada output. Fora do loop while, a variável
    output é retornada com uma lista de sessões como resultado do método
    o método GetSessionsAsync.

> FeedResponse\<Session\> results = await response.ReadNextAsync();
>
> output.AddRange(results);
>
> return output;

3.  O código abaixo utiliza o método fluente WithParameter para atribuir
    o parâmetro @sessionId ao identificador da sessão passado como
    argumento, e o parâmetro @type ao nome da classe Message.

> QueryDefinition query = new QueryDefinition("SELECT \* FROM c WHERE
> c.sessionId = @sessionId AND c.type = @type")
>
> .WithParameter("@sessionId", sessionId)
>
> .WithParameter("@type", nameof(Message));

4.  Crie um FeedIterator\< Message \>usando a variável de consulta e o
    método GetItemQueryIterator\<\>.

FeedIterator response = \_container.GetItemQueryIterator(query);

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image37.jpeg)

## Exercício 4: Executar o aplicativo

Agora seu aplicativo possui uma implementação completa utilizando o
Azure OpenAI e o Azure Cosmos DB. É possível realizar testes de ponta a
ponta depurando a solução.

1.  No **Visual Studio Code Terminal**, compile o projeto utilizando o
    seguinte comando.

**+++dotnet build+++**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image38.jpeg)

2.  Inicie o aplicativo com a recarga automática de código habilitada,
    usando o comando dotnet watch.

+++**dotnet watch run --non-interactive**+++

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image39.jpeg)

3.  O Visual Studio Code abrirá o navegador simples integrado com o
    aplicativo web em execução. No aplicativo web, crie uma nova sessão
    de chat clicando em **+ Create New Chat** e faça uma pergunta ao
    assistente de AI. Em seguida, feche o aplicativo web em execução.

![Uma captura de tela de um bate-papo O conteúdo gerado por IA pode
estar incorreto.](./media/image40.jpeg)

4.  Cole o texto a seguir na caixa de texto e clique no ícone **Send**.

+++How many wins does it take to promote to the Premier League?+++

![Uma captura de tela de um bate-papo O conteúdo gerado por IA pode
estar incorreto.](./media/image41.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image42.jpeg)

5.  Cole o texto a seguir na caixa de texto e clique no ícone **Send**.

+++What is Azure OpenAI?+++

![Uma captura de tela de um bate-papo O conteúdo gerado por IA pode
estar incorreto.](./media/image43.jpeg)

![Uma captura de tela de um bate-papo O conteúdo gerado por IA pode
estar incorreto.](./media/image44.jpeg)

6.  Feche o terminal.

## Exercício 5: Limpar o grupo de recursos

1.  Abra um novo navegador e digite a seguinte URL na barra de
    endereços: +++<https://portal.azure.com/+++> para abrir o Portal do
    Azure.

2.  Na página Resource group, selecione o **assigned Resource group**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image45.png)

3.  Selecione todos os **resources** e, em seguida, selecione
    **Delete**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image46.png)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image47.png)

4.  Digite **+++delete+++** na caixa de texto e clique em **Delete**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image48.png)

![Uma captura de tela de um erro do computador O conteúdo gerado por IA
pode estar incorreto.](./media/image49.png)

5.  Uma mensagem de confirmação vai aparecer avisando que os recursos
    foram excluídos com sucesso.

6.  Depois que os recursos forem excluídos, na página inicial do portal
    do Azure, pesquise por **Azure AI Services** e selecione-os.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image50.jpeg)

7.  Selecione **Azure OpenAI** no painel esquerdo e, em seguida,
    selecione **Manage deleted resources**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image51.jpeg)

8.  Selecione o recurso que aparece na lista e clique em **Purge**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image52.jpeg)

9.  Clique em **Yes**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image53.jpeg)

**Resumo**

Este laboratório ofereceu um guia completo para a construção,
implementação e validação de um aplicativo de chat personalizado
utilizando Blazor, PostgreSQL e Azure OpenAI. Durante a atividade, foi
realizada a configuração do ambiente de desenvolvimento, o
desenvolvimento de uma interface de chat com Blazor, a configuração e
conexão de um banco de dados PostgreSQL hospedado no Azure, além da
integração com os serviços do Azure OpenAI para adicionar
funcionalidades avançadas. Por fim, o aplicativo foi implementado e
testado na nuvem. Esta experiência prática proporcionou conhecimentos
essenciais para o desenvolvimento e gerenciamento de aplicações web
modernas com o uso de tecnologias inovadoras e serviços em nuvem.
