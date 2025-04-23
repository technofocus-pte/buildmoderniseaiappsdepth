# Caso de uso 06 - Implementação de aplicativo de chat em Azure Container Apps com PostgreSQL Flexible Server

**Objetivo:**

- Configurar o ambiente de desenvolvimento no Windows instalando o Azure
  CLI, o Node.js, atribuindo funções de assinatura do Azure, iniciando o
  Docker Desktop e habilitando o Visual Studio Code com a extensão Dev
  Containers.

- Implementar e testar o aplicativo de chat personalizado com PostgreSQL
  e OpenAI no Azure.

![A screenshot of a computer Description automatically
generated](./media/image1.jpeg)

Neste caso de uso, você configurará um ambiente de desenvolvimento
abrangente, implementará um aplicativo de chat integrado ao PostgreSQL e
verificará sua implementação no Azure. Isso envolve a instalação de
ferramentas essenciais, como Azure CLI, Docker e Visual Studio Code (já
fizemos isso para você no host env), a configuração de funções de
usuário no Azure, a implementação do aplicativo usando o Azure Developer
CLI e a interação com os recursos implementados para garantir seu
funcionamento.

**Principais tecnologias usadas:** Python, FastAPI, modelos do Azure
OpenAI, Azure Database for PostgreSQL e azure-container-apps,
ai-azd-templates.

**Duração estimada:** 45 minutos

**Tipo de laboratório:** Conduzido por instrutor

**Pré-requisitos:**

Conta do GitHub -- Espera-se que você tenha suas próprias credenciais de
login do GitHub. Se não tiver, crie uma aqui
- **https://github.com/signup?user_email=&source=form-home-signupobjectives**

## Exercício 1: Provisionar, implementar o aplicativo e testá-lo no navegador

### Tarefa 1: Copiar o nome do resource group existente

1.  Abra seu navegador e acesse o portal do Azure
    \`\`https:\\portal.azure.com\`\`.  Faça login com sua conta Azure
    slice (Credenciais Azure) disponível na seção Instruções/Recursos do
    seu ambiente host.

![A screenshot of a computer Description automatically
generated](./media/image2.jpeg)

2.  Na página inicial, clique na caixa de diálogo **Resource groups**.

![A screenshot of a computer Description automatically
generated](./media/image3.png)

3.  Certifique-se de que já tenha um grupo de recursos criado para você
    trabalhar. Nunca exclua esse grupo de recursos. Em vez disso, você
    pode excluir recursos dentro do grupo de recursos, mas não o grupo
    de recursos em si.

4.  Clique no nome resource group

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  Copie o nome do resource group e salve-o no Bloco de Notas para
    usá-lo na implementação de todos os recursos nesse resource group.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

### Tarefa 2: Executar o Docker

1.  Na área de trabalho, clique duas vezes em **Docker Desktop**.

![A screenshot of a computer Description automatically
generated](./media/image6.jpeg)

2.  Execute o Docker Desktop.

![A screenshot of a computer Description automatically
generated](./media/image7.jpeg)

### Tarefa 3: Registrar o provedor de serviços

1.  Volte para a guia Portal do Azure e clique na caixa de diálogo
    **Subscription**.

![](./media/image8.png)

2.  Clique em subscription name.

![](./media/image9.png)

3.  Clique em **Settings - \> Resource provider** no menu de navegação à
    esquerda.

![](./media/image10.png)

4.  Digite \`\`**Microsoft.AlertsManagement**\`\` e pressione Enter.
    Selecione-o e, em seguida, clique em **Register**.

![](./media/image11.png)

![A screenshot of a computer Description automatically
generated](./media/image12.png)

### Tarefa 4: Abrir o ambiente de desenvolvimento

1.  Abra seu navegador, vá até a barra de endereços e digite ou cole o
    seguinte URL:
    https://github.com/technofocus-pte/rag-postgres-openai-python.git.
    Uma nova guia será aberta pedindo para abrir no Visual Studio Code.
    Selecione **Open Visual Studio Code.**

![](./media/image13.jpeg)

2.  Clique em **Fork** para criar uma cópia do repositório. Dê um nome
    único ao repositório e clique no botão **Create repo**.

![A screenshot of a computer Description automatically
generated](./media/image14.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image15.jpeg)

3.  Clique em **Code -\> Codespaces -\> Codespaces+**

![A screenshot of a computer Description automatically
generated](./media/image16.jpeg)

4.  Aguarde o ambiente Codespaces ser configurado. Pode levar alguns
    minutos para concluir a configuração.

![A screenshot of a computer Description automatically
generated](./media/image17.jpeg)

![A screenshot of a computer Description automatically
generated](./media/image18.jpeg)

### Tarefa 5: Provisionar serviços e implementar o aplicativo no Azure

1.  Execute o seguinte comando no Terminal. Ele gera o código para
    copiar. Copie o código e pressione Enter.

\`\`azd auth login\`\`

![](./media/image19.png)

2.  O navegador padrão será aberto para você inserir o código gerado
    para verificação. Insira o código e clique em **Next**.

![](./media/image20.png)

3.  Faça login com suas credenciais do Azure.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

4.  Para criar um ambiente para os recursos do Azure, execute o seguinte
    comando do Azure Developer CLI. Ele pedirá que você insira um nome
    para o ambiente. Insira qualquer nome de sua escolha e pressione
    Enter (ex: **ragpgpy**)

**Observação:** Ao criar um ambiente, certifique-se de que o nome seja
composto apenas por letras minúsculas.

\`\`azd env new\`\`

![A screenshot of a computer Description automatically
generated](./media/image22.png)

5.  Execute o seguinte comando da Azure Developer CLI para provisionar
    os recursos do Azure e implementar o código**.**

\`\`azd provision \`\`

![A screenshot of a computer Description automatically
generated](./media/image23.png)

6.  Quando solicitado, selecione uma **subscription** para criar os
    recursos e escolha a região mais próxima de sua localização; neste
    laboratório, escolhemos a região **East US2**.

![](./media/image24.png)

7.  Será solicitado que você “**Digite um valor para o parâmetro de
    infraestrutura**” **'existingResourceGroupName':** digite o resource
    group copiado na Tarefa 1 (por exemplo: **ResourceGroup1 used for
    the development slice).** Você pode copiar o nome do resource group
    name na seção de **Resources**, conforme mostrado na imagem abaixo.

> ![](./media/image25.png)

8.  Quando solicitado, **enter a value for the 'openAILocation'
    infrastructure parameter** e selecione a região mais próxima de sua
    localização; neste laboratório, escolhemos a região **North Central
    US.**

![](./media/image26.png)

9.  O provisionamento dos recursos levará cerca de 5 a 10 minutos.
    Clique em **Yes** se for solicitado.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

10. Aguarde até que o modelo provisione todos os recursos com sucesso.

![A screenshot of a computer Description automatically
generated](./media/image28.png)

11. Execute o comando abaixo para definir o resource group

\`\`azd env set AZURE_RESOURCE_GROUP {your resource group name}\`\`

![](./media/image29.png)

12. Execute o comando abaixo para implementar o aplicativo no Azure.

\`\`azd deploy\`\`

![](./media/image30.png)

13. Aguarde a conclusão da implementação. A implementação leva
    aproximadamente 5 minutos.

![A screenshot of a computer Description automatically
generated](./media/image31.png)

14. Clique no link do endpoint do aplicativo da web implementado.

![](./media/image32.png)

15. Clique em **Open**. Uma nova guia será aberta com o aplicativo.

![](./media/image33.png)

16. O aplicativo está aberto.

![A screenshot of a chat Description automatically
generated](./media/image34.png)

### Tarefa 6: Use o aplicativo de chat para obter respostas de arquivos

1.  Na página do aplicativo da web **RAG on database
    |OpenAI+PoastgreSQL**, clique no botão **Best shoe for hiking?** e
    observe o resultado.

![](./media/image35.png)

![A screenshot of a computer Description automatically
generated](./media/image36.png)

2.  Clique em **clear chat.**

![](./media/image37.png)

3.  Na página do aplicativo da web **RAG on database
    |OpenAI+PoastgreSQL**, clique no botão **Climbing gear cheaper than
    \\30** e observe o resultado.

![](./media/image38.png)

![A screenshot of a computer Description automatically
generated](./media/image39.png)

4.  Clique em **clear chat.**

### Tarefa 7: Verificar os recursos implementados

1.  Na página inicial do portal do Azure, clique em **Resource Groups**.

![](./media/image40.png)

2.  Clique no nome do seu resource group.

![](./media/image41.png)

3.  Verifique se o recurso abaixo foi implementado com sucesso

    - Container App

    - Application Insights

    - Container Apps Environment

    - Log Analytics workspace

    - Azure OpenAI

    - Azure Database for PostgreSQL flexible server

    - Container registry

![](./media/image42.png)

4.  Clique no nome do recurso **Azure OpenAI**.

![](./media/image43.png)

5.  Em **Overview**, no menu de navegação à esquerda, clique em **Go to
    Azure AI Foundry portal** e selecione para abrir uma nova guia.

![](./media/image44.png)

6.  Clique em **Shared resources -\>** **Deployments** no menu de
    navegação à esquerda e verifique se
    o **gpt-35-turbo**, **text-embedding-ada-002** foi implementado com
    sucesso.

![](./media/image45.png)

### Tarefa 8: Limpar todos os recursos

Para limpar todos os recursos criados por essa amostra:

1.  Volte para **Azure portal -\> Resource group- \> Resource group
    name.**

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2.  Selecione todos os recursos e, em seguida, clique em Delete,
    conforme mostrado na imagem abaixo. (**NÃO CLIQUE EM DELETE**
    resource group)

![](./media/image47.png)

3.  Digite \`\`**delete**\`\` na Caixa de texto e clique em **Delete**.

![](./media/image48.png)

4.  Confirme a exclusão clicando em **Delete**.

![](./media/image49.png)

5.  Volte para a guia do portal do Github e atualize a página.

![A screenshot of a computer Description automatically
generated](./media/image50.png)

6.  Clique em Code, selecione o branch criado para este laboratório e
    clique em **Delete**.

![A screenshot of a computer Description automatically
generated](./media/image51.png)

1.  Confirme a exclusão do branch clicando no botão **Delete**.

![A screenshot of a computer Description automatically
generated](./media/image52.png)
