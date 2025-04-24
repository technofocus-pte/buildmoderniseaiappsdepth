# Caso de Uso 12 – Integrar capacidades de AI generativa com o Azure Database for PostgreSQL Flexible Server para avaliar avaliações de determinados anúncios de AI

**Duração do laboratório --** 40 minutos

**Tipo de laboratório --** Conduzido por instrutor

**Introdução**

Neste laboratório, você aprenderá a integrar os serviços do Azure AI ao
PostgreSQL para aprimorar seu banco de dados com funcionalidades
avançadas de AI. Aproveitando o poder do Azure OpenAI e das extensões do
PostgreSQL, como pgvector e PostGIS, você habilitará análises de texto
sofisticadas, pesquisas de similaridade vetorial e consultas
geoespaciais diretamente no banco de dados. Este laboratório orienta
você no provisionamento dos recursos necessários do Azure, na
configuração do banco de dados e na execução de consultas complexas que
combinam insights orientados por AI com dados geoespaciais.

**Objetivos**

- Provisionar e configurar o banco de dados do Azure Database for
  PostgreSQL Flexible Server.

- Criar e gerenciar embeddings de vetores usando o serviço Azure OpenAI.

- Realizar pesquisas de similaridade de vetores para encontrar dados de
  texto semanticamente semelhantes.

- Utilizar a extensão PostGIS para análise de dados geoespaciais.

- Para integrar os serviços de Azure AI Language para análise de
  sentimentos e outras funções cognitivas.

- Otimizar e analisar o desempenho das consultas usando ferramentas de
  indexação e planejamento de consultas.

**Importante:** Se algum dos comandos não for **pasted** no
**CloudShell**, abra um bloco de notas, mantenha o cursor em um espaço
vazio do bloco de notas e clique no botão T do comando a ser colado. O
conteúdo será copiado para o bloco de notas e, em seguida, você poderá
copiar e colar do bloco de notas para o CloudShell.

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

\[! Alerta\] **Importante:** certifique-se de criar todos os seus
recursos neste grupo de recursos

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image1.png)

1.  A guia **Help** contém as informações de suporte. O valor do **ID**
    aqui é o **Lab instance ID** que será usado durante a execução do
    laboratório**.**

![](./media/image2.png)

## Exercício 1: Provisionar um Azure Database for PostgreSQL Flexible Server 

### Tarefa 0: registrar provedores de recursos

1.  Faça login no **Portal do Azure** -
    +++[https://portal.azure.com+++](https://portal.azure.com+++/) usando
    suas credenciais de login do Azure.

2.  Clique em **Subscriptions** e selecione **Resource Providers** em
    **Settings** no painel esquerdo.

3.  Procure por +++**Microsoft.DBforPostgreSQL**+++ e clique em Register
    para registrar esse provedor de recursos.

![](./media/image3.png)

### Tarefa 1: Provisionar um Azure Database for PostgreSQL Flexible Server 

1.  Abra um navegador da web e navegue até a página
    +++[https://portal.azure.com+++](https://portal.azure.com+++/)

2.  Selecione o ícone do **Cloud Shell** na barra de ferramentas do
    portal do Azure para abrir um novo painel do Cloud Shell na parte
    superior da janela do navegador.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.jpeg)

3.  Na primeira vez em que abrir o Cloud Shell, você poderá ser
    solicitado a escolher o tipo de shell que deseja usar (**Bash ou
    PowerShell**). Selecione **Bash**.

![](./media/image5.jpeg)

4.  Na caixa de diálogo **Getting started**), selecione  **Mount storage
    account** e selecione sua assinatura do Azure. Clique no botão
    **Apply**.

![](./media/image6.png)

5.  Na caixa de diálogo **Mount storage account**, selecione **we will
    create a storage account for you** e clique no botão **Next**.

![](./media/image7.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.jpeg)

6.  No prompt do cloud shell, execute os seguintes comandos para definir
    variáveis para a criação de recursos. As variáveis representam os
    nomes a serem atribuídos ao seu grupo de recursos e banco de dados e
    especificam a região do Azure na qual os recursos devem ser
    implementados.

7.  Substitua o nome do Resource group no comando abaixo pelo Resource
    group atribuído e execute o comando.

+++RG_NAME= \< Resource group Name \>+++

![](./media/image9.png)

8.  No nome do banco de dados, substitua o token {SUFFIX} pelo seu
     **Lab instance ID**, como suas iniciais, para garantir que o nome
    do servidor do banco de dados seja globalmente exclusivo.

+++DATABASE_NAME=<pgsql-flex-@lab.LabInstance.Id>+++

![](./media/image10.jpeg)

9.  Execute o comando abaixo para definir o valor da região.

+++REGION=@lab.CloudResourceGroup(ResourceGroup1).Location+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.jpeg)

10. Provisione uma instância de banco de dados do Azure Database for
    PostgreSQL dentro do grupo de recursos atribuído, executando o
    seguinte comando Azure CLI (esse comando levará 10 minutos para ser
    concluído)

> \`\`\`
>
> az postgres flexible-server create --name $DATABASE_NAME --location
> $REGION --resource-group $RG_NAME \\
>
> --admin-user s2admin --admin-password Seattle123Seattle123
> --database-name airbnb \\
>
> --public-access 0.0.0.0-255.255.255.255 --version 16 \\
>
> --sku-name Standard_D2s_v3 --storage-size 32 --yes
>
> \`\`\`

![](./media/image12.jpeg)

### Tarefa 2: Conecte-se ao banco de dados usando psql no Azure Cloud Shell

Nesta tarefa, você utiliza o utilitário de linha de comando psql,
diretamente no Azure Cloud Shell, para se conectar ao seu banco de
dados.

1.  Abra um navegador e vá para
    +++[https://portal.azure.com+++](https://portal.azure.com+++/) e
    entre com sua conta de assinatura do Azure.

2.  Na **Home** page, clique em **Resource Groups**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.jpeg)

3.  Clique no nome do grupo de recursos que lhe foi atribuído

![](./media/image14.png)

4.  In the resource group, select o recurso **PostgreSQL Flexible
    Server**.

![](./media/image15.png)

5.  No menu de navegação à esquerda, selecione **Connect** em
    **Settings**.

![](./media/image16.jpeg)

6.  Na página **Connect** do banco de dados no portal do Azure,
    selecione **airbnb** para o **Database name**, copie o bloco
    **Connection details **e cole-o no bloco de notas para usar as
    informações nas próximas tarefas.

![](./media/image17.jpeg)

7.  Na página inicial do Azure Database for PostgresSQL, clique em
    **Overview** no menu de navegação do lado esquerdo, copie o nome do
    servidor e cole-o no bloco de notas e, em seguida, **Save** o bloco
    de notas para usar as informações no próximo laboratório.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.jpeg)

8.  Na página inicial do Azure Database for PostgresSQL, selecione
    **Networking** em configurações e selecione **Allow public access
    from any Azure service within Azure to this server**. Clique no
    botão **Save**.

![](./media/image19.jpeg)

![](./media/image20.jpeg)

9.  Selecione o ícone do **Cloud Shell** na barra de ferramentas do
    portal do Azure para abrir um novo painel do Cloud Shell na parte
    superior da janela do navegador.

10. Cole as **Connection details **no Cloud Shell.

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image21.jpeg)

11. No prompt do Cloud Shell, substitua o token **{your_password}** pela
    senha que você atribuiu ao usuário **s2admin** ao criar seu banco de
    dados, a senha deve ser +++**Seattle123Seattle123**+++.

![](./media/image22.jpeg)

12. Conecte-se ao seu banco de dados usando o utilitário de linha de
    comando psql digitando o seguinte no prompt:

+++psql+++

![](./media/image23.jpeg)

Conectar-se ao banco de dados a partir do Cloud Shell requer que a opção
“Permitir acesso público de qualquer serviço do Azure dentro do Azure”
esteja marcada na página **Networking** do banco de dados**.** Se você
receber uma mensagem informando que não foi possível se conectar,
verifique se essa opção está marcada e tente novamente.

### Tarefa 3: Adicionar dados ao banco de dados

Usando o prompt de comando psql, você criará tabelas e as preencherá com
dados para uso no laboratório.

1.  Execute os seguintes comandos para criar tabelas temporárias para
    importar dados JSON de uma conta de blob storage pública.

> CREATE TABLE temp_calendar (data jsonb);
>
> CREATE TABLE temp_listings (data jsonb);
>
> CREATE TABLE temp_reviews (data jsonb);

![](./media/image24.jpeg)

2.  Usando o comando COPY, preencha cada tabela temporária com dados de
    arquivos JSON em uma conta de armazenamento pública.

+++\COPY temp_calendar (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/calendar.json'+++>

+++\COPY temp_listings (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/listings.json'+++>

+++\COPY temp_reviews (data) FROM PROGRAM
'curl <https://solliancepublicdata.blob.core.windows.net/ms-postgresql-labs/reviews.json'+++>

![](./media/image25.jpeg)

![](./media/image26.jpeg)

3.  Execute o seguinte comando para criar as tabelas que armazenarão os
    dados no formato utilizado por este laboratório:

> CREATE TABLE listings (
>
> listing_id int,
>
> name varchar(50),
>
> street varchar(50),
>
> city varchar(50),
>
> state varchar(50),
>
> country varchar(50),
>
> zipcode varchar(50),
>
> bathrooms int,
>
> bedrooms int,
>
> latitude decimal(10,5),
>
> longitude decimal(10,5),
>
> summary varchar(2000),
>
> description varchar(2000),
>
> host_id varchar(2000),
>
> host_url varchar(2000),
>
> listing_url varchar(2000),
>
> room_type varchar(2000),
>
> amenities jsonb,
>
> host_verifications jsonb,
>
> data jsonb
>
> );

![](./media/image27.jpeg)

> CREATE TABLE reviews (
>
> id int,
>
> listing_id int,
>
> reviewer_id int,
>
> reviewer_name varchar(50),
>
> date date,
>
> comments varchar(2000)
>
> );
>
> CREATE TABLE calendar (
>
> listing_id int,
>
> date date,
>
> price decimal(10,2),
>
> available boolean
>
> );

![](./media/image28.jpeg)

4.  Por fim, execute as instruções **INSERT INTO** abaixo para carregar
    os dados das tabelas temporárias para as tabelas principais,
    extraindo os dados do campo JSON para colunas individuais:

> INSERT INTO listings
>
> SELECT
>
> data\['id'\]::int,
>
> replace(data\['name'\]::varchar(50), '"', ''),
>
> replace(data\['street'\]::varchar(50), '"', ''),
>
> replace(data\['city'\]::varchar(50), '"', ''),
>
> replace(data\['state'\]::varchar(50), '"', ''),
>
> replace(data\['country'\]::varchar(50), '"', ''),
>
> replace(data\['zipcode'\]::varchar(50), '"', ''),
>
> data\['bathrooms'\]::int,
>
> data\['bedrooms'\]::int,
>
> data\['latitude'\]::decimal(10,5),
>
> data\['longitude'\]::decimal(10,5),
>
> replace(data\['description'\]::varchar(2000), '"', ''),
>
> replace(data\['summary'\]::varchar(2000), '"', ''),
>
> replace(data\['host_id'\]::varchar(50), '"', ''),
>
> replace(data\['host_url'\]::varchar(50), '"', ''),
>
> replace(data\['listing_url'\]::varchar(50), '"', ''),
>
> replace(data\['room_type'\]::varchar(50), '"', ''),
>
> data\['amenities'\]::jsonb,
>
> data\['host_verifications'\]::jsonb,
>
> data::jsonb
>
> FROM temp_listings;
>
> INSERT INTO reviews
>
> SELECT
>
> data\['id'\]::int,
>
> data\['listing_id'\]::int,
>
> data\['reviewer_id'\]::int,
>
> replace(data\['reviewer_name'\]::varchar(50), '"', ''),
>
> to_date(replace(data\['date'\]::varchar(50), '"', ''), 'YYYY-MM-DD'),
>
> replace(data\['comments'\]::varchar(2000), '"', '')
>
> FROM temp_reviews;
>
> INSERT INTO calendar
>
> SELECT
>
> data\['listing_id'\]::int,
>
> to_date(replace(data\['date'\]::varchar(50), '"', ''), 'YYYY-MM-DD'),
>
> data\['price'\]::decimal(10,2),
>
> replace(data\['available'\]::varchar(50), '"', '')::boolean
>
> FROM temp_calendar;

![](./media/image29.jpeg)

## Exercício 2: Adicionar as extensões Azure AI e Vector à lista de permissões**.**

Ao longo deste laboratório, você utilizará as extensões azure_ai e
pgvector para adicionar funcionalidades de AI generativa ao seu banco de
dados PostgreSQL.  
Neste exercício, você adiciona essas extensões à lista de permissões do
seu servidor, conforme descrito na documentação sobre como usar
extensões no PostgreSQL.

1.  Na página inicial, clique em **Resource Groups**.

![](./media/image30.jpeg)

2.  Clique no nome do resource group

![](./media/image14.png)

3.  No grupo de recursos, selecione Recurso do **PostgreSQL Flexible
    Server.**

![](./media/image15.png)

4.  No menu de navegação à esquerda do banco de dados, selecione
    **server parameters** em **Settings** e insira
    +++**azure.extensions**+++ na caixa de pesquisa. Expanda a lista
    suspensa **VALUE** e, em seguida, localize e marque a caixa ao lado
    de cada uma das seguintes extensões:

    - AZURE_AI

    - POSTGIS

    - VECTOR

![](./media/image31.jpeg)

![](./media/image32.jpeg)

![](./media/image33.jpeg)

5.  Selecione **Save** na barra de ferramentas, o que acionará uma
    impementação no banco de dados.

![](./media/image34.jpeg)

## Exercício 3: Criar um recurso do Azure OpenAI

A extensão azure_ai requer um serviço Azure OpenAI subjacente para criar
vetores de embedding. Neste exercício, você irá provisionar um recurso
do Azure OpenAI no portal do Azure e implantar um modelo de embedding
nesse serviço.

### Tarefa 1: Provisionar um serviço Azure OpenAI

Nesta tarefa, você criará um **novo serviço Azure OpenAI**.

1.  Na página inicial do portal do Azure, clique no **Azure portal
    menu** representado por três barras horizontais no lado esquerdo da
    barra de comandos do Microsoft Azure, como mostrado na imagem
    abaixo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.jpeg)

2.  Navegue e clique em **+ Create a resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.jpeg)

3.  Na página **Create a resource**, na barra de pesquisa **Search
    services and marketplace**, digite +++**Azure OpenAI**+++, e depois
    pressione o botão **Enter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.jpeg)

4.  Na página do **Marketplace**, navegue até a seção **Azure OpenAI**,
    clique na seta do menu suspenso do botão Create e, em seguida,
    selecione **Azure OpenAI** como mostrado na imagem. (Caso já tenha
    clicado na tela do **Azure OpenAI**, clique no botão **Create** na
    página do Azure OpenAI),

![A screenshot of a software page AI-generated content may be
incorrect.](./media/image38.png)

5.  Na aba Create Azure OpenAI **Basics**, insira as seguintes
    informações e clique no botão **Next**.

[TABLE]

6.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image39.png)

7.  Na guia **Network**, deixe todos os botões de rádio no estado padrão
    e clique no botão **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.jpeg)

8.  Na guia **Tags**, deixe todos os campos no estado padrão e clique no
    botão **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

9.  Na guia **Review+submit**, quando a validação for aprovada, clique
    no botão **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

10. Aguarde a conclusão da implementação. A implantação levará cerca de
    2 a 3 minutos.

\[!Note\] **Observação:** Se você vir uma mensagem informando que o
Serviço do Azure OpenAI está atualmente disponível para os clientes por
meio de um formulário de solicitação. A assinatura selecionada não foi
habilitada para o serviço e não tem uma cota para nenhum nível de preço;
você precisará clicar no link para solicitar acesso ao serviço Azure
OpenAI e preencher o formulário de solicitação.

### Tarefa 2: Recuperar a chave e o endpoint do serviço Azure OpenAI

1.  Na página **Overview** do recurso, selecione o botão **Go to
    resource**. Se solicitado, selecione as credenciais do laboratório:

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.jpeg)

2.  Na janela inicial do **Azure OpenAI**, navegue até a seção
    **Resource Management** e clique em **Keys and Endpoints**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.jpeg)

3.  Na página **Keys and Endpoints**, copie os valores de **KEY1, KEY
    2** e **Endpoint** e cole-os em um bloco de notas, conforme mostrado
    na imagem abaixo, e **save** o bloco de notas para usar as
    informações nas próximas tarefas

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.jpeg)

**Observação:** Você pode usar tanto a KEY1 quanto a KEY2. Ter sempre
duas chaves permite que você gire e regenere as chaves com segurança sem
causar interrupções no serviço.

### Tarefa 3: Implementar um modelo de embeddings

A extensão azure_ai permite a criação de embeddings vetoriais a partir
de texto. Para criar esses embeddings, é necessário ter um modelo
text-embedding-ada-002 (versão 2) implantado dentro do seu serviço Azure
OpenAI. Nesta tarefa, você usará o Azure OpenAI Studio para criar uma
implantação de modelo que você poderá utilizar.

1.  Na página do **Azure OpenAI**, clique em **Overview** no menu de
    navegação do lado esquerdo, role para baixo e clique no botão **Go
    to Azure OpenAI Studio**, conforme mostrado na imagem abaixo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

2.  Na página inicial do **Azure AI Foundry | Azure Open AI Service**,
    navegue até a seção Components e clique em **Deployments.**

3.  Na janela **Deployments**, selecione o menu suspenso **+Deploy
    model** o e selecione **Deploy base model.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

4.  Na caixa de diálogo **Select a model**, navegue e selecione
    cuidadosamente **text-embedding-ada-002** e, em seguida, clique no
    botão **Confirm**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

5.  **Deploy model**, defina o seguinte e selecione **Create** para
    implementar o modelo.

    - **Select a model**: Escolha **text-embedding-ada-002** na lista.

    - **Model version**: Certifique-se de que **2 (default)** esteja
      selecionado.

    - **Deployment name**: Digite +++**embeddings**+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

6.  Na janela **Deployment**, copie o **deployment name** e cole-os em
    um bloco de notas (como mostrado na imagem) e **save** o bloco de
    notas para usar as informações na próxima tarefa.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

## Exercício 4: Instalar e configurar a extensão azure_ai

Neste exercício, você instala a extensão azure_ai em seu banco de dados
e a configura para se conectar ao seu Azure OpenAI service.

### Tarefa 1: Conectar-se ao banco de dados usando psql no Azure Cloud Shell

Nesta tarefa, você usa o utilitário de linha de comando psql do Azure
Cloud Shell para se conectar ao banco de dados.

1.  Selecione o ícone do **Cloud Shell** na barra de ferramentas do
    portal do Azure para abrir um novo painel do Cloud Shell na parte
    superior da janela do navegador.

2.  Colar **Connection details** no Cloud Shell.

![A computer screen shot of a black screen AI-generated content may be
incorrect.](./media/image21.jpeg)

3.  No prompt do Cloud Shell, substitua o token **{your_password}** pela
    senha que você atribuiu ao usuário **s2admin** ao criar seu banco de
    dados, a senha deve ser **Seattle123Seattle123**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.jpeg)

4.  Conecte-se ao seu banco de dados usando o utilitário de linha de
    comando psql digitando o seguinte no prompt:

+++**psql**+++

![A black background with a black square AI-generated content may be
incorrect.](./media/image23.jpeg)

### Tarefa 2: Instalar a extensão azure_ai

A extensão azure_ai permite que você integre o Azure OpenAI e os Azure
Cognitive Services ao seu banco de dados. Para habilitar a extensão em
seu banco de dados, siga as etapas abaixo:

1.  Verifique se a extensão foi adicionada com êxito à lista de
    permissões executando o seguinte no prompt de comando psql:

+++SHOW azure.extensions;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.jpeg)

2.  Instale a extensão azure_ai usando o comando CREATE EXTENSION.

+++CREATE EXTENSION IF NOT EXISTS azure_ai;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.jpeg)

### Tarefa 3: Revisar os objetos contidos na extensão azure_ai

Revisar os objetos dentro da extensão azure_ai pode fornecer uma melhor
compreensão de seus recursos. Nesta tarefa, você inspeciona os vários
esquemas, UDFs (user-defined functions) e tipos compostos adicionados ao
banco de dados pela extensão.

1.  Você pode usar o metacomando \dx no prompt de comando **psql** para
    listar os objetos contidos na extensão.

\[!Note\] **Observação:** clique em qualquer tecla para continuar quando
o shell da nuvem solicitar **More...**

+++\dx+ azure_ai+++

![](./media/image56.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image57.jpeg)

A saída do metacomando mostra que a extensão azure_ai cria três
esquemas, várias UDFs (user-defined functions) e vários tipos compostos
no banco de dados. A tabela abaixo lista os esquemas adicionados pela
extensão e descreve cada um.

[TABLE]

2.  As funções e tipos estão todos associados a um dos esquemas. Para
    revisar as funções definidas no esquema azure_ai, use o meta-comando
    \df, especificando o esquema cujas funções devem ser exibidas. O
    comando \x auto, precedendo \df, permite que a exibição expandida
    seja aplicada automaticamente quando necessário, facilitando a
    visualização da saída do comando no Azure Cloud Shell.

+++\x auto+++ +++\df+ azure_ai.\*+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image58.jpeg)

A função azure_ai.set_setting() permite definir os valores de endpoint e
chave para os serviços Azure AI. Ela aceita uma **key** e o **value** a
ser atribuído. A função azure_ai.get_setting() fornece uma maneira de
recuperar os valores que você definiu com a função set_setting(). Ela
aceita a chave da configuração que você deseja visualizar. Para ambos os
métodos, a chave deve ser uma das seguintes:

### Task 4: Set the Azure OpenAI endpoint and key

Antes de usar as funções azure_openai, configure a extensão com o
endpoint e key do seu serviço Azure OpenAI.

1.  No comando abaixo, substitua os tokens **{endpoint}** e
    **{api-key}** pelos valores que você obteve do portal do Azure e, em
    seguida, execute os comandos no prompt do psql no painel do Cloud
    Shell para adicionar seus valores à tabela de configuração

2.  SELECT azure_ai.set_setting('azure_openai.endpoint','{endpoint}');

3.  SELECT azure_ai.set_setting('azure_openai.subscription_key',
    '{api-key}');

![A computer screen with white text AI-generated content may be
incorrect.](./media/image59.jpeg)

4.  Verifique as configurações gravadas na tabela de configuração usando
    as seguintes consultas:

5.  SELECT azure_ai.get_setting('azure_openai.endpoint');

6.  SELECT azure_ai.get_setting('azure_openai.subscription_key');

A extensão azure_ai agora está conectada à sua conta do Azure OpenAI e
pronta para gerar embeddings vetoriais..

![A computer screen with white text AI-generated content may be
incorrect.](./media/image60.jpeg)

## Exercício 5: Gerar embeddings vetoriais com o Azure OpenAI

O esquema azure_openai da extensão azure_ai permite que o Azure OpenAI
crie embeddings vetoriais para valores de texto. Usando esse esquema,
você pode gerar embeddings com o Azure OpenAI diretamente do banco de
dados para criar representações vetoriais de texto de entrada, que podem
ser usadas em buscas por similaridade vetorial, bem como consumidas por
modelos de aprendizado de máquina.

Embeddings são um conceito em aprendizado de máquina e natural language
processing (NLP) que envolve representar objetos, como palavras,
documentos ou entidades, como vetores em um espaço multidimensional.
Embeddings permitem que os modelos de aprendizado de máquina avaliem
quão estreitamente relacionadas as informações estão. Essa técnica
identifica de forma eficiente relações e semelhanças entre dados,
permitindo que algoritmos identifiquem padrões e façam previsões
precisas

### Tarefa 1: Habilitar suporte a vetores com a extensão pgvector

A extensão azure_ai permite gerar embeddings para texto de entrada. Para
que os vetores gerados sejam armazenados junto com o restante dos seus
dados no banco de dados, você precisa instalar a extensão pgvector,
seguindo as orientações na documentação sobre como habilitar o suporte a
vetores no seu banco de dados.

1.  Instale a extensão pgvector usando o comando CREATE EXTENSION..

+++CREATE EXTENSION IF NOT EXISTS vector; +++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image61.jpeg)

2.  Com o suporte a vetores adicionado ao seu banco de dados, adicione
    uma nova coluna à tabela de anúncios (listings) usando o tipo de
    dado vetor para armazenar as embeddings dentro da tabela. O modelo
    text-embedding-ada-002 produz vetores com 1536 dimensões, portanto,
    você deve especificar 1536 como o tamanho do vetor..

3.  ALTER TABLE listings

4.  ADD COLUMN description_vector vector(1536);

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image62.jpeg)

### Tarefa 2: Gerar e armazenar embeddings vetoriais

A tabela listings agora está pronta para armazenar embeddings. Usando a
função azure_openai.create_embeddings(), você pode criar vetores para o
campo description e inseri-los na recém-criada coluna description_vector
na tabela listings.

1.  Antes de usar a função create_embeddings(), execute o seguinte
    comando para inspecioná-la e revisar os argumentos necessários.

+++\df+ azure_openai.\* +++

![A computer screen with white text AI-generated content may be
incorrect.](./media/image63.jpeg)

A propriedade Argument data types na saída do comando \df+
azure_openai.\* revela a lista de argumentos que a função espera.

[TABLE]

2.  Usando o nome da implantação, execute a seguinte consulta para
    atualizar cada registro na tabela listings, inserindo os vetores de
    embeddings gerados para o campo description na coluna
    description_vector, utilizando a função
    azure_openai.create_embeddings(). Substitua {your-deployment-name}
    pelo valor do **Deployment** **name** que você copiou da página
    **Deployments** do Azure OpenAI Studio.

> DO $$
>
> DECLARE counter integer := (SELECT COUNT(\*) FROM listings WHERE
> description \<\> '' AND description_vector IS NULL);
>
> DECLARE r record;
>
> BEGIN
>
> RAISE NOTICE 'Total descriptions to embed: %', counter;
>
> WHILE counter \> 0 LOOP
>
> BEGIN
>
> FOR r IN
>
> SELECT listing_id FROM listings WHERE description \<\> '' AND
> description_vector IS NULL
>
> LOOP
>
> BEGIN
>
> UPDATE listings
>
> SET description_vector =
> azure_openai.create_embeddings('{your-deployment-name}', description)
>
> WHERE listing_id = r.listing_id;
>
> EXCEPTION
>
> WHEN OTHERS THEN
>
> RAISE NOTICE 'Waiting 1 second before trying again...';
>
> PERFORM pg_sleep(1);
>
> END;
>
> counter := (SELECT COUNT(\*) FROM listings WHERE description \<\> ''
> AND description_vector IS NULL);
>
> IF counter % 25 = 0 THEN
>
> RAISE NOTICE 'Remaining descriptions to embed: %', counter;
>
> END IF;
>
> END LOOP;
>
> END;
>
> END LOOP;
>
> END;
>
> $$;

![A computer screen with white text AI-generated content may be
incorrect.](./media/image64.jpeg)

A consulta acima utiliza um loop WHILE para recuperar registros da
tabela listings onde o campo description_vector está nulo e o campo
description não é uma string vazia. Em seguida, a consulta tenta
atualizar a coluna description_vector com uma representação vetorial do
campo description usando a função azure_openai.create_embeddings. O loop
é utilizado ao realizar essa atualização para evitar que as chamadas à
função de criação de embeddings excedam o limite de taxa de chamadas do
serviço Azure OpenAI. Se o limite de taxa de chamadas for excedido, você
verá avisos semelhantes aos seguintes na saída.

\[!Note\] **OBSERVAÇÃO:** Aguarde 1 segundo antes de tentar novamente...

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image65.jpeg)

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image66.jpeg)

3.  Você pode verificar se a coluna description_vector foi populada para
    todos os registros da tabela listings executando a seguinte
    consulta:

+++SELECT COUNT(\*) FROM listings WHERE description_vector IS NULL AND
description \<\> '';+++

O resultado da consulta deve ser uma contagem de 0.

![A black screen with white text AI-generated content may be
incorrect.](./media/image67.jpeg)

### Tarefa 3: Realizar uma busca por similaridade vetorial

Similaridade vetorial é um método usado para medir a semelhança entre
dois itens ao representá-los como vetores, que são sequências de
números. Vetores são frequentemente utilizados para realizar buscas com
modelos de linguagem (LLMs). A similaridade vetorial é comumente
calculada usando métricas de distância, como a distância euclidiana ou a
similaridade cosseno. A distância euclidiana mede a distância em linha
reta entre dois vetores em um espaço n-dimensional, enquanto a
similaridade cosseno mede o cosseno do ângulo entre dois vetores. Cada
embedding é um vetor de números de ponto flutuante, e a distância entre
dois embeddings no espaço vetorial está correlacionada com a
similaridade semântica entre os dois inputs no formato original..

1.  Antes de executar uma busca por similaridade vetorial, execute a
    consulta abaixo usando a cláusula ILIKE para observar os resultados
    de uma busca por registros com uma consulta em linguagem natural,
    mas sem usar vetores de similaridade:

+++SELECT listing_id, name, description FROM listings WHERE description
ILIKE '%Properties with a private room near Discovery Park%';+++

![A black background with white text AI-generated content may be
incorrect.](./media/image68.jpeg)

A consulta retorna zero resultados porque está tentando corresponder o
texto no campo de descrição com a consulta de linguagem natural
fornecida.

2.  Agora, execute uma consulta de **busca por similaridade cosseno** na
    tabela listings para realizar uma **busca por similaridade
    vetorial** com base nas descrições dos anúncios. Os **embeddings**
    são gerados para uma pergunta de entrada e, em seguida, convertidos
    para um array vetorial (::vector), permitindo a comparação com os
    vetores armazenados na tabela listings. Substitua
    {your-deployment-name} pelo **Deployment name** que você copiou da
    página de **Deployment** no **Azure OpenAI Studio**.

+++SELECT listing_id, name, description FROM listings ORDER BY
description_vector \<=\>
azure_openai.create_embeddings('{your-deployment-name}', 'Properties
with a private room near Discovery Park')::vector LIMIT 3;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image69.jpeg)

A consulta usa o \<=\> [vector
operator](https://github.com/pgvector/pgvector#vector-operators), que
representa o operador \cosine distance\\ usado para calcular a distância
entre dois vetores em um espaço multidimensional.

3.  Para executar a consulta novamente usando o comando EXPLAIN ANALYZE,
    você deve substituir **{your-deployment-name}** pelo **Deployment
    name** que você copiou da página de **Deployments** do Azure OpenAI
    Studio.

> EXPLAIN ANALYZE
>
> SELECT listing_id, name, description FROM listings
>
> ORDER BY description_vector \<=\>
> azure_openai.create_embeddings('{your-deployment-name}', 'Properties
> with a private room near Discovery Park')::vector
>
> LIMIT 3;

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image70.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image71.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image72.jpeg)

Na saída, observe o plano de consulta, que começará com algo semelhante
a:

Limit (cost=1098.54..1098.55 rows=3 width=261) (actual
time=10.505..10.507 rows=3 loops=1) -\> Sort (cost=1098.54..1104.10
rows=2224 width=261) (actual time=10.504..10.505 rows=3 loops=1)

…

Sort Method: top-N heapsort Memory: 27kB -\> Seq Scan on listings
(cost=0.00..1069.80 rows=2224 width=261) (actual time=0.005..9.997
rows=2224 loops=1) A consulta está utilizando uma varredura sequencial
para realizar a busca. O tempo de planejamento e execução será listado
no final dos resultados, e deve ser algo semelhante ao seguinte: Tempo
de planejamento: 62.020 ms Tempo de execução: 10.530 ms

4.  Para permitir buscas mais eficientes sobre o campo vetorial, crie um
    índice na tabela listings utilizando a métrica de distância cosseno
    e o método [HNSW](https://github.com/pgvector/pgvector#hnsw). O HNSW
    permite que o pgvector utilize os algoritmos mais modernos baseados
    em grafos para realizar consultas de vizinhos mais próximos de forma
    aproximada.

+++CREATE INDEX ON listings USING hnsw (description_vector
vector_cosine_ops);+++

![](./media/image73.jpeg)

5.  Para observar o impacto do índice HNSW na tabela, execute novamente
    a consulta utilizando a cláusula EXPLAIN ANALYZE para comparar o
    plano de execução e os tempos de execução da consulta. Substitua
    **{your-deployment-name}** pelo **Deployment name** que você copiou
    da página de **Deployments** do Azure OpenAI Studio.

> EXPLAIN ANALYZE
>
> SELECT listing_id, name, description FROM listings
>
> ORDER BY description_vector \<=\>
> azure_openai.create_embeddings('{your-deployment-name}', 'Properties
> with a private room near Discovery Park')::vector
>
> LIMIT 3;

![A computer screen with white text AI-generated content may be
incorrect.](./media/image74.jpeg)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image75.jpeg)

![A computer screen with white text AI-generated content may be
incorrect.](./media/image76.jpeg)

No resultado, observe que o plano de consulta agora inclui uma varredura
de índice mais eficiente:

Limit (cost=116.48..119.33 rows=3 width=261) (actual time=1.112..1.130
rows=3 loops=1) -\> Index Scan using listings_description_vector_idx on
listings (cost=116.48..2228.28 rows=2224 width=261) (actual
time=1.111..1.128 rows=3 loops=1)

Os tempos de execução da consulta devem refletir uma redução
significativa no tempo necessário para planejar e executar a consulta:

Tempo de planejamento: 56.802 ms

Tempo de execução: 1.167 ms

## 

As integrações dos serviços do Azure AI incluídas no esquema
azure_cognitive da extensão azure_ai fornecem um valioso conjunto de
recursos de linguagem de AI acessíveis diretamente do banco de dados. As
funcionalidades incluem análise de sentimentos, detecção de idioma,
extração de frases-chave, reconhecimento de entidades e resumo de texto.
Esses recursos são habilitados por meio do serviço do Azure AI Language.

Para revisar a lista completa de recursos do Azure AI acessíveis por
meio da extensão, consulte a documentação de Integração do Azure
Database for PostgreSQL Flexible Server com o Azure Cognitive Services.

### Tarefa 1: Provisionar o serviço Azure AI Language

É necessário um serviço do Azure AI Language para aproveitar as funções
cognitivas das extensões azure_ai. Neste exercício, você criará um
serviço do Azure AI Language.

1.  Na página inicial do portal do Azure, clique no **menu do portal do
    Azure**, representado por três barras horizontais no lado esquerdo
    da barra de comando do Microsoft Azure, conforme mostrado na imagem
    abaixo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image77.jpeg)

2.  Na página **Create a resource**, selecione **AI + Machine
    Learning** no menu à esquerda e, em seguida, selecione **Language
    service**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image78.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image79.jpeg)

3.  Na caixa de diálogo **Select additional features**,
    selecione **Continue to create your resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image80.jpeg)

4.  Na guia Create Language **Basics**, digite o seguinte::

[TABLE]

5.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image81.png)

6.  ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image82.jpeg)

7.  As configurações padrão serão utilizadas para as demais abas da
    configuração do serviço de linguagem, portanto, selecione o botão
    **Review + create**.

8.  Selecione o botão **Create** na guia **Review + create** para
    provisionar o serviço de idioma.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image83.png)

9.  Selecione **Go to resource group** na página deployment quando a
    implementação do serviço de idiomas estiver concluída.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image84.jpeg)

### Tarefa 2: Definir o endpoint e a chave do serviço Azure AI Language

Assim como nas funções azure_openai, para fazer chamadas bem-sucedidas
aos serviços da Azure AI usando a extensão azure_ai, é necessário
fornecer o ponto de extremidade (endpoint) e uma chave para o serviço do
Azure AI Language.

1.  Na página inicial do idioma, selecione o item **Keys and Endpoint**
    em **Resource Management** no menu de navegação à esquerda.

2.  Na página **Keys and Endpoints,** copie os valores de **KEY1, KEY
    2,** e **Endpoint** e, em seguida, **Save** essas informações em um
    bloco de notas para para usá-las nas próximas tarefas.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image85.jpeg)

3.  Copie seus valores de endpoint e chave de acesso e, no comando
    abaixo, substitua os tokens {endpoint} e {api-key} pelos valores que
    você recuperou do portal do Azure. Execute os comandos do prompt de
    comando psql no Cloud Shell para adicionar seus valores à tabela de
    configuração.

\[!Note\] **Observação:** Conecte-se ao prompt de comando psql antes de
executar os comandos abaixo.

SELECT azure_ai.set_setting('azure_cognitive.endpoint','{endpoint}');

SELECT azure_ai.set_setting('azure_cognitive.subscription_key',
'{api-key}');

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image86.jpeg)

### Tarefa 3: Analisar o sentimento das avaliações

Nesta tarefa, você usará a função azure_cognitive.analyze_sentiment para
analisar as avaliações dos anúncios do Airbnb.

1.  Para realizar a análise de sentimento usando o schema
    azure_cognitive na extensão azure_ai, você deve utilizar a função
    analyze_sentiment. Execute o comando abaixo para revisar essa
    função:

+++\df azure_cognitive.analyze_sentiment+++

![A computer screen with white text AI-generated content may be
incorrect.](./media/image87.jpeg)

A saída exibe o schema da função, o nome, o tipo de dado retornado e os
tipos de dados dos argumentos. Essas informações ajudam a entender como
utilizar a função.

2.  Também é essencial entender a estrutura do tipo de dado retornado
    pela função para que você possa manipular corretamente seu valor de
    retorno. Execute o seguinte comando para inspecionar o tipo
    sentiment_analysis_result:

+++\dT+ azure_cognitive.sentiment_analysis_result+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image88.jpeg)

3.  A saída do comando acima revela que o tipo sentiment_analysis_result
    é uma tupla. Para entender a estrutura dessa tupla, execute o
    seguinte comando para visualizar as colunas contidas no tipo
    composto sentiment_analysis_result:

+++\d+ azure_cognitive.sentiment_analysis_result+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image89.jpeg)

A saída desse comando deve ser semelhante ao seguinte: tipo composto.
"azure_cognitive.sentiment_analysis_result"

Column | Type | Collation | Nullable | Default | Storage | Description
----------------+------------------+-----------+----------+---------+----------+-------------

sentiment | text | | | | extended |

positive_score | double precision | | | | plain |

neutral_score | double precision | | | | plain |

negative_score | double precision | | | | plain |

O azure_cognitive.sentiment_analysis_result é um tipo de dado composto
que traz o resultado da análise de sentimentos feita sobre um texto. Ele
inclui o sentimento identificado (positivo, negativo, neutro ou misto) e
os escores correspondentes a cada um desses sentimentos. Os escores são
números reais entre 0 e 1. Exemplo: (neutral, 0.26, 0.64, 0.09) indica
sentimento neutro, com escore positivo de 0.26, neutro de 0.64 e
negativo de 0.09.

## Exercício 7: Execute uma consulta final para reunir tudo o que foi feito

Neste exercício, você irá se conectar ao seu banco de dados no
**pgAdmin** e executar uma consulta final que integra seu trabalho com
as extensões azure_ai, postgis e pgvector dos Laboratórios 3 e 4.

### Task 1: Install pgAdmin

1.  Abra um navegador e navegue até a
    página <https://www.pgadmin.org/download/pgadmin-4-windows/>

2.  Clique na versão mais recente do **pgAdmin**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image90.jpeg)

3.  Selecione **pgadmin4-8.9-x64.exe**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image91.jpeg)

4.  Execute e instale o arquivo baixado.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image92.jpeg)

5.  Na aba Select Setup Install Mode, selecione **Install for me
    only(recommended).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image93.jpeg)

6.  Clique no botão **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image94.jpeg)

7.  Selecione **I accept the agreement** e clique no botão **Next**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image95.jpeg)

8.  Selecione o caminho e clique no botão **Next**.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image96.jpeg)

9.  Na janela **Setup-pgAdmin 4**, clique no botão **Next.**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image97.jpeg)

10. Clique no botão **Install**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image98.jpeg)

11. Na janela **Setup-pgAdmin 4**, clique no botão **Finish**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image99.jpeg)

### Tarefa 2: Conectar-se ao banco de dados usando o pgAdmin

Nesta tarefa, você abrirá o **pgAdmin** e se conectará ao seu banco de
dados.

1.  Na caixa de pesquisa do Windows, digite **+++pgAdmin+++**, em
    seguida clique em **pgAdmin.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image100.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image101.jpeg)

2.  Registre seu servidor clicando com o botão direito em **Servers** no
    Object Explorer e selecionando **Register \> Server**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image102.jpeg)

3.  Na janela **Register - Server**, cole o nome do seu servidor Azure
    Database for PostgreSQL Flexible Server (que você salvou no
    Exercício 1 \> Tarefa 1) no campo **Name** da guia **General**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image103.jpeg)

4.  Em seguida, selecione a guia **Connection** e cole o **nome do seu
    servidor** no campo **Hostname/address**. Digite +++**s2admin**+++
    no campo **Username**, digite +++**Seattle123Seattle123**+++ no
    campo **Password**, e, opcionalmente, marque a opção **Save
    password**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image104.jpeg)

5.  Por fim, selecione a guia **Parameters** e defina o **SSL mode**
    como **require**. Clique em **Salve** para registrar seu servidor.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image105.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image106.jpeg)

6.  Depois de conectado ao seu servidor, expanda o nó **Databases** e
    selecione o banco de dados **airbnb**. Clique com o botão direito do
    mouse sobre o banco de dados **airbnb** e selecione **Query Tool**
    no menu de contexto

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image107.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image108.jpeg)

### Tarefa 3: Verificar se a extensão PostGIS está instalada no seu banco de dados

Para instalar a extensão PostGIS no seu banco de dados, você utilizará o
comando CREATE EXTENSION.

1.  Na janela de consultas que você abriu anteriormente, execute o
    comando CREATE EXTENSION com a cláusula IF NOT EXISTS para instalar
    a extensão PostGIS no seu banco de dados.

+++CREATE EXTENSION IF NOT EXISTS postgis;+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image109.jpeg)

Com a extensão PostGIS agora carregada, você está pronto para começar a
trabalhar com dados geoespaciais no banco de dados. A tabela listings
que você criou e preencheu acima contém as informações de latitude e
longitude de todas as propriedades listadas.

Para utilizar esses dados em análises geoespaciais, você deve alterar a
tabela listings para adicionar uma coluna do tipo geometry que aceite
dados do tipo ponto (point). Esses novos tipos de dados fazem parte da
extensão PostGIS.

2.  Para acomodar dados de ponto, adicione uma nova coluna de geometria
    à tabela que aceite dados de ponto. Copie e cole a seguinte consulta
    na janela de consulta aberta do pgAdmin:

3.  Listagens ALTER TABLE

+++ADD COLUMN listing_location geometry(point, 4326); +++

4.  Em seguida, atualize a tabela com os dados geoespaciais associados a
    cada listagem, adicionando os valores de longitude e latitude à
    coluna geometry.

5.  Listagens UPDATE

+++SET listing_location = ST_SetSRID(ST_Point(longitude, latitude),
4326);+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image110.jpeg)

### Tarefa 4: Execute uma consulta e visualize os resultados no mapa

1.  Copie e cole a seguinte consulta no editor de consultas aberto e
    execute-a para visualizar os dados armazenados na coluna
    listing_location.

+++SELECT listing_id, name, listing_location FROM listings LIMIT 50;+++

No painel Data Output panel, selecione o botão **View all
geometries** nesta coluna, localizado na **coluna listing_location** dos
resultados da consulta.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image111.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image112.jpeg)

2.  Agora, execute a seguinte consulta para realizar uma **busca de
    proximidade geoespacial**, retornando propriedades disponíveis para
    a semana de 13 de janeiro de 2016, com valor inferior a $75,00 por
    noite, e que estejam a uma curta distância do Discovery Park em
    Seattle.

> A consulta utiliza a função ST_DWithin, fornecida pela extensão
> PostGIS, para identificar listagens dentro de uma determinada
> distância do parque, cuja longitude é -122.410347 e latitude é
> 47.655598

3.  

> SELECT name, listing_location, summary
>
> FROM listings l
>
> INNER JOIN calendar c ON l.listing_id = c.listing_id
>
> WHERE ST_DWithin(
>
> listing_location,
>
> ST_GeomFromText('POINT(-122.410347 47.655598)', 4326),
>
> 0.025
>
> )
>
> AND c.date = '2016-01-13'
>
> AND c.available = 't'
>
> AND c.price \<= 75.00;

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image113.jpeg)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image114.jpeg)

**Resumo**

Neste laboratório, você integrou com sucesso Azure AI services ao
PostgreSQL para criar um ambiente de banco de dados com inteligência
artificial.  
Você começou provisionando os recursos do Azure e configurando seu banco
de dados PostgreSQL com as extensões necessárias.  
Em seguida, gerou vetores de embeddings para dados textuais e realizou
buscas por similaridade vetorial para encontrar registros semanticamente
semelhantes.  
Além disso, utilizou a extensão PostGIS para análise de dados
geoespaciais e o Azure AI Language para análise de sentimentos.  
Por fim, otimizou suas consultas utilizando indexação e analisou seu
desempenho, demonstrando a eficiência e a capacidade dessa solução
integrada para análises avançadas de dados

 
