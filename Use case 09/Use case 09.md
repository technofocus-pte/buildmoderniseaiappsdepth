# Caso de uso 09 – Criando uma experiência de chat bot usando o Azure Cosmos DB for MongoDB e o Azure OpenAI Service

**Objetivo:**

Esse caso de uso criará uma solução inteligente que combina a pesquisa
vetorial e a recuperação de documentos do Azure Cosmos DB for MongoDB
baseada em vCore com os serviços do Azure OpenAI para criar uma
experiência de chat bot.

![Um diagrama de um aplicativo de software O conteúdo gerado por IA pode
estar incorreto.](./media/image1.jpeg)

**Principais tecnologias usadas** – Azure OpenAI Service, Azure Cosmos
DB, modelo ChatGPT

**Duração estimada** - 60 minutos

**Tipo de laboratório** - conduzido por instrutor

**Importante:** Se algum dos comandos não for **colado** no
**PowerShell**, abra um bloco de notas, posicione o cursor em um espaço
vazio e clique no botão **T** do comando que deseja colar. O conteúdo
será copiado para o Bloco de Notas e, a partir dele, você poderá copiar
e colar no PowerShell.

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
estar incorreto.](./media/image2.png)

3.  A guia **Help** contém as informações de suporte. O valor do **ID**
    aqui é o **Lab instance ID** que será usado durante a execução do
    laboratório.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image3.png)

## Exercício 1: Provisionar recursos do Azure

### Tarefa 1: Criar recursos do Azure usando script

1.  Faça login no portal do Azure em +++\*\* e faça login usando suas
    credenciais de login do Azure na guia **Resources**.

2.  No portal do Azure, selecione sua assinatura. No painel esquerdo,
    selecione Resource providers em Settings, selecione
    +++**Microsoft.Alertsmanagement**+++ e clique em **Register**.

![Uma captura de tela de uma tela de computador O conteúdo gerado por IA
pode estar incorreto.](./media/image4.jpeg)

3.  Na VM, procure por **+++power shell+++**, clique com o botão direito
    do mouse em **Windows PowerShell** e selecione **Run as
    administrator**. Clique em **Yes** na caixa de diálogo de
    confirmação.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image5.jpeg)

![Uma captura de tela de um erro do computador O conteúdo gerado por IA
pode estar incorreto.](./media/image6.jpeg)

4.  Execute o comando abaixo para instalar o Az no PowerShell.

+++**Install-Module Az**+++

Selecione **A** (Yes to all) quando solicitado.

**Observação:** isso levará até 5 minutos para ser concluído.

![Uma tela de computador com texto branco O conteúdo gerado por IA pode
estar incorreto.](./media/image7.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image8.jpeg)

5.  Uma vez feito isso, execute o comando abaixo para importar o módulo
    Az.

+++**Import-Module Az**+++

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image9.jpeg)

6.  Execute o comando abaixo para utilizar a autenticação via navegador

+++Update-AzConfig -EnableLoginByWam $false+++

![Uma captura de tela de uma tela de computador O conteúdo gerado por IA
pode estar incorreto.](./media/image10.jpeg)

7.  Execute o comando abaixo e, se solicitado, selecione sua conta do
    Azure para fazer login.

+++Connect-AzAccount+++

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image11.jpeg)

8.  Execute os comandos abaixo para navegar até a pasta **LabFiles**.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![Um sinal retangular azul com texto branco O conteúdo gerado por IA
pode estar incorreto.](./media/image12.jpeg)

9.  Execute o comando abaixo para instalar o **Microsoft Bicep** usando
    **winget**.

+++winget install -e --id Microsoft.Bicep+++

Digite **Y**, se solicitado.

![Uma tela de computador com texto branco O conteúdo gerado por IA pode
estar incorreto.](./media/image13.jpeg)

10. **Feche** o PowerShell e **abra-o** novamente.

11. Execute o comando abaixo e, se solicitado, selecione sua conta do
    Azure para fazer login.

+++Connect-AzAccount+++

12. Execute os comandos abaixo para navegar até a pasta **LabFiles**.

+++cd\\++

+++cd LabFiles\\Build a Chat bot'\Labs\deploy+++

![Um sinal retangular azul com texto branco O conteúdo gerado por IA
pode estar incorreto.](./media/image12.jpeg)

13. Execute o comando abaixo para definir a SubscriptionId.

+++Set-AzContext -SubscriptionId @lab.CloudSubscription.Id+++

![Uma captura de tela de computador de uma tela azul O conteúdo gerado
por IA pode estar incorreto.](./media/image14.jpeg)

14. Abra o arquivo **azuredeploy.bicep** no caminho **C:\LabFiles\Build
    a Chat bot\Labs\deploy** e substitua as letras **dgxxxxxxx** na
    linha 35 por <+++dg@lab.LabInstance.Id>+++. Na linha **74**,
    atualize a versão como **+++0125+++.**

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image15.png)

![](./media/image16.png)

15. Execute o comando abaixo para implementar os recursos no Azure, como
    o workspace do Azure Cosmos DB, o Azure OpenAI.

New-AzResourceGroupDeployment -ResourceGroupName
@lab.CloudResourceGroup(ResourceGroup1).Name -TemplateFile
.\azuredeploy.bicep -TemplateParameterFile .\azuredeploy.parameters.json
-c \`\`\`

\>\[!Note\] \*\*Observação:\*\* A implementação levará cerca de 10 a 15
minutos.

Se houver um problema com a implementação e ela falhar, recomenda-se
alterar o nome definido no Passo 14 para outro valor e tentar novamente
o processo.

\>\[! Note\] \*\*Observação:\*\* Digite Y quando solicitado.

![Uma captura de tela de computador de uma tela azul O conteúdo gerado
por IA pode estar incorreto.](./media/image17.jpeg)

![](./media/image18.jpeg)

\>\[!Note\]\*\*Observação:\*\* Se não houver atualização no PowerShell
após 15 a 20 minutos, verifique em Resource Group -\> Deployments in the
Azure portal ou pressione \*\*Enter\*\* na janela \*\*PowerShell\*\*.

### Tarefa 2: Verificar os recursos criados no Azure

1.  Faça login no **portal do Azure** em
    +++<https://portal.azure.com/+++> usando suas **credenciais de login
    do Azure**. Selecione **Resource groups**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image19.jpeg)

2.  Na lista Resource groups, selecione **assigned Resource Group**.

![Uma captura de tela de uma página da web O conteúdo gerado por IA pode
estar incorreto.](./media/image20.png)

3.  Observe que um conjunto de recursos foi criado, incluindo o **Azure
    OpenAI**, o **App Service** e o **Azure Cosmos DB for MongoDB**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image21.png)

4.  Clique no recurso **Azure OpenAI**.

![](./media/image22.png)

5.  Selecione **Keys and Endpoint** em **Resource Management**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image23.jpeg)

6.  Copie e salve a **Key 1** e **Endpoint** em um bloco de notas para
    referência posterior.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image24.jpeg)

7.  De volta à página resource group, selecione o recurso do **Azure
    Cosmos DB for Mongo DB** .

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image25.png)

8.  Clique em **Connection strings** em **Settings**. Copie o valor de
    Self (sempre este cluster).

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image26.jpeg)

9.  Copie a connection string e cole-a em um bloco de notas. Substitua o
    \< **password** \> por **+++myMongoDB98+++** na cadeia de conexão
    copiada e salve-o no bloco de notas.

## Exercício 2: Explorar e usar modelos do Azure OpenAI a partir do código

### Tarefa 1: Configurar o ambiente

1.  Na barra de pesquisa do Windows da VM do laboratório, pesquise
    +++Visual Studio Code+++ e abra o **Visual Studio Code**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image27.jpeg)

2.  Clique em **Open Folder**. (Se não aparecer, selecione **File -\>
    Open Folder**)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image28.jpeg)

3.  Navegue até **C:\Labfiles**, clique em **Build a Chat bot** e
    selecione **Select Folder.**

![Uma captura de tela de um bot de bate-papo O conteúdo gerado por IA
pode estar incorreto.](./media/image29.jpeg)

4.  Clique em **Yes, I trust the Authors** no pop-up.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image30.jpeg)

5.  No Visual Studio Code, abra **lab_0_explore_and_use_models.ipynb**
    na pasta **Labs.**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image31.jpeg)

6.  Clique em **Select Kernel.**

7.  Selecione **Install** no pop-up **Do you want to install the
    recommended extensions for Python**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image32.jpeg)

8.  Clique em **Allow access**, se solicitado.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image33.jpeg)

9.  Clique em **Selecionar Kernel**. Selecione **Python Environments**
    e, em seguida, escolha **Python 3.12.3** ou superior que aparecer
    como opção **Sugerida** ou **Recomendada**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image34.jpeg)

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image35.jpeg)

10. Abra o arquivo **.env**

11. Substitua **DB_CONNECTION STRING,** **AOAI_KEY** e **AOAI_Endpoint**
    por aqueles que você salvou no **bloco de notas** anteriormente na
    Tarefa 2 do Exercício 1.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image36.jpeg)

Agora, as variáveis de ambiente foram definidas para referenciar os
recursos do Azure que já criamos.

### Tarefa 2: Executar o código

1.  De volta ao arquivo **Lab 0 ipynb**, **execute** a **primeira
    célula** clicando no botão Reproduzir, para instalar a biblioteca de
    cliente OpenAI mais recente.

![Uma tela preta com fundo preto O conteúdo gerado por IA pode estar
incorreto.](./media/image37.jpeg)

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image38.jpeg)

2.  **Execute** a próxima célula para instalar o **Python-dotenv**

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image39.jpeg)

3.  Pressione **Ctrl+Shift+P**, digite +++Reload Window+++ e selecione a
    opção Developer:Reload Window que aparecer na lista.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image40.jpeg)

4.  Execute novamente a partir da **primeira célula**.

5.  Depois, **execute** a próxima célula que importa a biblioteca do
    OpenAI, além do os (pra acessar variáveis de ambiente) e o dotenv
    (que carrega o conteúdo do arquivo .env).

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image41.jpeg)

6.  **Execute** a célula seguinte para instanciar o cliente do **Azure
    OpenAI**, utilizado para interagir com a API de Chat Completion do
    serviço Azure OpenAI:

![Uma tela de computador com texto O conteúdo gerado por IA pode estar
incorreto.](./media/image42.jpeg)

7.  **Execute** a próxima célula para chamar o método
    **.chat.completions.create()** no cliente para executar uma **chat
    completion**. Você deverá obter uma resposta de chat.

![Uma tela de computador com texto nela O conteúdo gerado por IA pode
estar incorreto.](./media/image43.jpeg)

## Exercício 3: Primeiro aplicativo com a API do Cosmos DB para MongoDB

Este exercício abordará como criar seu primeiro projeto do Cosmos DB.
Usaremos um notebook para demonstrar as operações básicas do CRUD.

1.  Abra o arquivo **lab_1_first_application.ipynb** na pasta **Labs.**

2.  Clique em **Select kernel** e escolha a **Python version**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image44.jpeg)

3.  **Execute** a primeira célula para fazer instalar o **pymongo**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image45.jpeg)

4.  **Execute** a próxima célula para fazer as **imports** necessárias

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image46.jpeg)

5.  Execute a próxima célula para **Create a database.**

\[!Note\] **Observação:** essa operação usará a connection string
definida previamente no arquivo .env

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image47.jpeg)

6.  **Execute** a próxima célula para criar uma **collection**.

![Uma tela preta com texto branco O conteúdo gerado por IA pode estar
incorreto.](./media/image48.jpeg)

7.  **Execute** a próxima célula para criar um **documento**. Uma das
    maneiras de criar um documento é utilizando o método insert_one.
    Esse método permite adicionar um único documento ao banco de dados.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image49.jpeg)

8.  **Execute** a próxima célula para** retrieve a single document **do
    banco de dados. O método **find_one** é utilizado para essa
    finalidade.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image50.jpeg)

9.  **Execute** a próxima célula na qual o método
    **find_one_and_update** é usado para atualizar um único documento no
    banco de dados.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image51.jpeg)

10. **Execute** a próxima célula na qual o método **delete_one** é usado
    para excluir um único documento do banco de dados.

![](./media/image52.jpeg)

11. O método **find** é usado para consultar vários documentos no banco
    de dados. **Execute** as ** next 3 cells**, uma a uma, para vê-lo em
    ação.

![Uma captura de tela de computador de um programa O conteúdo gerado por
IA pode estar incorreto.](./media/image53.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image54.jpeg)

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image55.jpeg)

![Uma captura de tela de computador de um programa O conteúdo gerado por
IA pode estar incorreto.](./media/image56.jpeg)

12. A célula a seguir **delete** o banco de dados e a coleção criados
    neste exercício. Isso é feito usando o método **drop_database** no
    objeto de banco de dados

![Uma tela de computador com texto O conteúdo gerado por IA pode estar
incorreto.](./media/image57.jpeg)

## Exercício 4: Carregar dados no Cosmos DB usando a API do MongoDB

O exercício anterior demonstrou como adicionar dados individualmente a
uma coleção.  
Este exercício demonstrará como carregar dados em massa em várias
coleções. Esses dados serão utilizados nos próximos laboratórios para
explorar mais a fundo as capacidades da API do Azure Cosmos DB para
MongoDB no contexto de AI.

Este notebook mostra como carregar dados no Cosmos DB a partir de
arquivos JSON do Cosmic Works, utilizando a API do MongoDB.

1.  Abra o arquivo **lab_2_load_data.ipynb** na pasta **Labs**. Clique
    em **Select Kernel** e selecione a **versão do Python**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image58.jpeg)

2.  Execute a primeira célula para instalar **requests**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image59.jpeg)

3.  **Execute** a próxima célula para fazer as **importações**
    necessárias.

![Uma tela de computador com texto verde O conteúdo gerado por IA pode
estar incorreto.](./media/image60.jpeg)

4.  **Execute** a próxima célula que estabelece uma **conection** com a
    **database**.

![Uma tela de computador com texto O conteúdo gerado por IA pode estar
incorreto.](./media/image61.jpeg)

![Uma captura de tela de computador de texto O conteúdo gerado por IA
pode estar incorreto.](./media/image62.jpeg)

5.  **Execute** a próxima célula para **load** os **productos**.

![Uma captura de tela de uma tela de computador O conteúdo gerado por IA
pode estar incorreto.](./media/image63.jpeg)

6.  **Execute** as próximas células para **load** os **sales raw data**.
    Nesse repositório, os dados de clientes e vendas são armazenados no
    mesmo arquivo. O campo type é usado para diferenciar os dois tipos
    de documentos.

![Uma captura de tela de um código de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image64.jpeg)

![](./media/image65.jpeg)

![Uma captura de tela de um código de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image66.jpeg)

7.  **Execute** a próxima célula para **clean up**.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image67.jpeg)

## Exercício 5: Pesquisa vetorial usando o Azure Cosmos DB para MongoDB baseado em vCore

1.  Abra o arquivo **lab_3_mongodb_vector_search.ipynb** da pasta
    **Labs.**

2.  Clique em **Select Kernel** e selecione a **Python version**.

![](./media/image68.jpeg)

3.  **Execute** a primeira célula para instalar a **tenacity**.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image69.jpeg)

4.  **Execute** a próxima célula para executar as **imports**
    necessárias.

![Uma tela de computador com texto O conteúdo gerado por IA pode estar
incorreto.](./media/image70.jpeg)

5.  **Execute** a próxima célula para **load** as **settings** do
    arquivo .env.

![Uma tela de computador com texto O conteúdo gerado por IA pode estar
incorreto.](./media/image71.jpeg)

6.  Execute a próxima célula para establish **connectivity** to
    the **database**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image72.jpeg)

7.  **Execute** a próxima célula para Establish **Azure OpenAI
    connectivity**.

![Uma captura de tela de um código de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image73.jpeg)

8.  O processo de criação de um campo de vetor de embedding em cada
    documento só precisa ser feito uma vez. No entanto, se um documento
    for alterado, o campo de vetor de embedding precisará ser atualizado
    com um vetor atualizado. Isso é feito nas próximas duas células.
    **Execute** as duas próximas células e observe os vetores de
    **embedding** obtidos como saída na segunda.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image74.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image75.jpeg)

9.  **Execute** a próxima célula para **Vectorize and update all
    documents in the Cosmic Works database. **

![Uma captura de tela de computador de um código de programa O conteúdo
gerado por IA pode estar incorreto.](./media/image76.jpeg)

10. **Execute** as próximas **3** células para adicionar **vector
    fields** aos **products, customer and sales documents.**

**Observação:** A primeira célula levará cerca de 5 minutos, a segunda
em torno de 3 minutos e a terceira em torno de 20 minutos para completar
a execução.

! \[\](./media/image77.jpeg)

11. **Execute** a próxima célula para criar **products vector index**.

![Uma captura de tela de uma tela de computador O conteúdo gerado por IA
pode estar incorreto.](./media/image77.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image78.jpeg)

12. Agora Agora que cada documento tem seu vetor de embedding associado
    e que os índices vetoriais foram criados em cada coleção, podemos
    usar os recursos de busca vetorial do Azure Cosmos DB for MongoDB
    baseado em vCore. **Execute** as próximas **3** células.

![](./media/image79.jpeg)

![](./media/image80.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image81.jpeg)

13. **Execute** as próximas células para observar como os **vector
    search results** são usados em um padrão RAG com o Chat GPT-3.5

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image82.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image83.jpeg)

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image84.jpeg)

14. Observe a saída das células a seguir.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image85.jpeg)

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image86.jpeg)

## Exercício 6: Excluir os recursos implementados

1.  No portal do
    Azure(+++[https://portal.azure.com+++](https://portal.azure.com+++/)),
    selecione o resource group atribuído.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image87.png)

2.  Selecione todos os recursos abaixo dele, clique nos **three dots**
    no menu e selecione **Delete** para excluir todos os recursos.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image88.png)

3.  Digite +++delete+++ na caixa de texto e clique no botão Delete.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image89.png)

4.  Depois que os recursos forem excluídos, na página inicial do portal
    do Azure, pesquise **+++Azure AI Services+++** e selecione-o.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image90.jpeg)

5.  Selecione **Azure OpenAI** no painel esquerdo e, em seguida,
    selecione **Manage deleted resources**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image91.jpeg)

6.  Selecione o recurso listado lá e clique em **Purge**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image92.jpeg)

7.  Clique em **Yes**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image93.jpeg)

**Resumo:**

Você criou com sucesso uma solução com o Azure Cosmos DB para MongoDB
para busca vetorial e recuperação de documentos utilizando os serviços
do Azure OpenAI.
