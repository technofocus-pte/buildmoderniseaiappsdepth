# 

# Caso de uso 10: Implementação do aplicativo de chat para responder às perguntas do usuário e rastrear o histórico de conversas

**Objetivo:**

Este caso de uso orienta você pelas etapas para conectar um aplicativo
Blazor existente a uma conta do Azure Cosmos DB for NoSQL e a uma conta
do Azure OpenAI. Seu aplicativo envia prompts para o modelo hospedado no
Azure OpenAI e analisa as respostas. Além disso, ele armazena várias
sessões de conversa e suas respectivas mensagens como itens organizados
em um único contêiner dentro do Azure Cosmos DB for NoSQL.

Em resumo, o aplicativo irá:

- **Conectar-se** ao modelo do Azure OpenAI usando o SDK .NET

- **Enviar** prompts ao modelo e analisar a resposta de conclusão

- **Conectar-se** ao Azure Cosmos DB for NoSQL usando o SDK .NET

- **Gerenciar** os itens com operações individuais, consultas e lotes
  transacionais

Esse aplicativo de chat é capaz de responder perguntas dos usuários e
rastrear o histórico das conversas.

![](./media/image1.jpeg)

**Principais tecnologias usadas** --, Csharp, nosql, asp-net, blazor,
azure-cosmos-db,

**Duração estimada** - 45 minutos

**Tipo de laboratório:** Conduzido por instrutor

**Pré-requisitos:**

Conta do GitHub – Espera-se que você tenha suas próprias credenciais de
login do GitHub. Se você não tiver, crie uma a partir daqui -''
**https://github.com/signup?user_email=&source=form-home-signupobjectives''**

### Tarefa 1: Executar o Docker

1.  Na caixa de pesquisa do Windows, digite **Docker** e clique em
    **Docker Desktop**.

![](./media/image2.jpeg)

### Tarefa 2: Registrar provedor de serviços

1.  Abra um navegador, vá para <https://portal.azure.com> e entre com
    suas credenciais do Azure disponíveis na guia **Resource** de sua
    VM.

> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image3.png)

2.  Na página inicial do portal do Azure, clique na caixa de diálogo
    **Resource groups**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image4.png)

3.  Copie o nome do resource group e salve-o no Bloco de Notas para
    usá-lo na implementação de todos os recursos nesse resource group.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image5.png)

4.  Volte para a página inicial e clique na caixa de diálogo
    **Subscription**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image6.png)

5.  Clique em subscription name.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image7.png)

6.  Clique em **Settings - \> Resource provider** no menu de navegação à
    esquerda.

![](./media/image8.png)

7.  Digite \`\`**Microsoft.AlertsManagement**\`\` e pressione Enter.
    Selecione-o e, em seguida, clique em **Register**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image9.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image10.png)

### Tarefa 3: Provisionar serviços e aplicativos para o Azure

1.  Abra um navegador e vá para ''https:\\github.com'' e faça login com
    sua conta do Github. Procure o repositório abaixo.

![](./media/image11.jpeg)

2.  Procure o repositório abaixo e clique em **Fork**.

> ''https://github.com/technofocus-pte/chat-csharp-cosmos-db-nosql-openai''

![](./media/image12.jpeg)

3.  Digite o nome do repositório e clique em **Create repository**.

![](./media/image13.jpeg)

4.  Clique em **Code -\> Code space -\> Open Code space.**

![](./media/image14.jpeg)

5.  Aguarde até que o contêiner Dev seja configurado, isso levará em
    torno de 3 a 5 minutos.

![](./media/image15.jpeg)

6.  Execute o comando abaixo para fazer login no AZD. Copie o código
    gerado e pressione Enter.

> ''**azd auth login''**

![](./media/image16.jpeg)

7.  Cole o código gerado e entre com suas credenciais do Azure.

![](./media/image17.jpeg)

![](./media/image18.jpeg)

8.  Execute o comando abaixo para inicializar o projeto no diretório
    atual. Digite o nome do ambiente como ''**cosmoschatapp''** e
    pressione Enter.

''azd init ''

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image19.png)

9.  Execute o comando abaixo para implementar os serviços no Azure, crie
    seu contêiner. Selecione os valores abaixo.

> ''Disposição azd''
>
> **Select an Azure Subscription to use**: selecione sua assinatura
>
> **Select an Azure location to use** : **East us/west us** (Às vezes, o
> Leste dos EUA pode não estar disponível, escolha um local diferente e
> implemente.)
>
> **Enter a value for the 'existingResourceGroupName' infrastructure
> parameter:** **ResourceGroup1**

![](./media/image20.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image21.png)

10. Aguarde até que o recurso seja provisionado completamente. Este
    processo levará em torno de 5 a 10 minutos para criar todos os
    recursos necessários.

![](./media/image22.png)

### Tarefa 4: Implementar o aplicativo no Azure

1.  Volte para o portal do Azure e clique na caixa Resource groups na
    página inicial.

![](./media/image23.png)

2.  Clique no nome do resource group.

![](./media/image24.png)

3.  Você deve ver os recursos abaixo

- **Container**

- **Container Registry**

- **Azure Cosmos Db account**

- **AureOpenAI**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image25.png)

4.  Clique no nome **Container registry**.

![](./media/image26.png)

5.  Expanda a opção **Setting** no menu de navegação à esquerda, clique
    em **Access keys.** Marque a **Admin user check box.** Copie o
    **Login server**, **user name** e **password** para um bloco de
    notas para usá-los no momento de implementar o aplicativo.

![](./media/image27.png)

6.  Duplique a guia para abrir o portal Azure em uma nova guia.

![](./media/image28.png)

7.  Clique no nome do resource group no menu de navegação superior.

![](./media/image29.png)

8.  Clique em Name e, em seguida Container App.

![](./media/image30.png)

9.  Clique no botão **Authorize** em Github-Sign in para autenticar com
    sua conta do GitHub. Autorize sua conta do Github.

10. Selecione os valores abaixo

> **Organization: sua organização do Github**
>
> **Repository:** chat-csharp-cosmos-db-nosql-openai
>
> **Branch:** main

![](./media/image31.png)

11. Role a tela até **Registry settings** e insira os valores abaixo. Em
    seguida, clique no botão **Start continuous deployment.**

- Repository source: **Docker Hub ou outros registros.**

- Login server URL: o servidor de login que você copiou do Container
  Registry (etapa 5)

- Username: seu nome de usuário copiado do Container Registry (etapa 5

- Password: senha copiada do Container Registry (etapa 5)

![](./media/image32.png)

12. Clique no link do workflow file. Ele abrirá uma nova guia no Github.

![](./media/image33.png)

13. Clique na guia **Actions**.

![](./media/image34.png)

14. Aguarde a conclusão da implementação.

![](./media/image35.png)

15. Não feche nenhuma guia.

### Tarefa 5: Acessar o aplicativo de chat

1.  Volte para o portal do Azure e clique em **Overview** no menu de
    navegação à esquerda e, em seguida, clique na **Application Url**.
    Isso abrirá uma nova aba para carregar o aplicativo.

![](./media/image36.png)

2.  Clique no botão **Create New Chat**.

![](./media/image37.png)

3.  Digite o prompt abaixo.

\`\`What is the seating capacity for Lumen in Seattle?\`\`

![](./media/image38.jpeg)

4.  Digite o prompt abaixo. Explore o aplicativo com diferentes prompts.

\`\`is that bigger than Dogger stadium??\`\`

![](./media/image39.jpeg)

### Tarefa 6 : Limpar todos os recursos

Para limpar todos os recursos criados por este exemplo:

1.  Volte para a guia do portal do Github e atualize a página.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image40.png)

2.  Clique em Code, selecione o branch criado para este laboratório e
    clique em **Delete**.

![](./media/image41.png)

3.  Confirme a exclusão do branch clicando no botão **Delete**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image42.png)

5.  Volte para o **Azure portal -\> Resource group- \> Resource group
    name.**

![](./media/image43.png)

6.  Selecione todos os recursos e clique em Delete conforme mostrado na
    imagem abaixo. (**NÃO CLIQUE em DELETE** resource group)

![](./media/image44.png)

7.  Digite \`\`**delete**\`\`na caixa de texto e clique em **Delete**.

> ![](./media/image45.png)

8.  Confirme a exclusão clicando em **Delete**.

![](./media/image46.png)

**Resumo:**

Você implementou classes de serviço usando os pacotes
Microsoft.Azure.Cosmos e Azure.AI.OpenAI disponíveis no NuGet. Você
enviou prompts para a interface conversacional do Azure OpenAI,
juntamente com prefixos contextuais, e analisou as propriedades usage e
body da resposta. Você também usou o Azure Cosmos DB for NoSQL para
armazenar as sessões de conversação e as mensagens, organizando tudo
dentro de um único container.
