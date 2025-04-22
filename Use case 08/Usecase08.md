# Caso de uso 08 – Criando e implementando o aplicativo de chat Contoso Real Estate para dar suporte aos clientes.

**Objetivo**

Este caso de uso demonstra algumas abordagens para criar experiências
semelhantes ao ChatGPT em seus próprios dados usando o padrão Retrieval
Augmented Generation. Ele usa o Azure OpenAI Service para acessar o
modelo ChatGPT (gpt-35-turbo) e o Azure AI Search para indexação e
recuperação de dados.

![Um diagrama de um processo de software Descrição gerada
automaticamente](./media/image1.jpeg)

Este caso de uso já vem com dados de exemplo, pronto para ser executado
de ponta a ponta. Neste aplicativo de exemplo, usamos uma empresa
fictícia chamada Contoso Real Estate, e a experiência permite que seus
clientes façam perguntas de suporte sobre o uso de seus produtos. Os
dados de amostra incluem um conjunto de documentos que descrevem os
termos de serviço, a política de privacidade e um guia de suporte.

O aplicativo é feito de vários componentes, incluindo:

- **Serviço de pesquisa**: o serviço de back-end que fornece os recursos
  de pesquisa e recuperação.

- **Serviço de indexador**: o serviço que indexa os dados e cria os
  índices de pesquisa.

- **Aplicativo da web**: o aplicativo da web de front-end que fornece a
  interface do usuário e orquestra a interação entre o usuário e os
  serviços de back-end.

![Um diagrama de um sistema de software Descrição gerada
automaticamente](./media/image2.jpeg)

- Interfaces de chat e perguntas e respostas

- Explora várias opções para ajudar os usuários a avaliar a
  confiabilidade das respostas, com citações, rastreamento do conteúdo
  de origem, etc.

- Mostra possíveis abordagens para a preparação de dados, a construção
  de prompts e orquestração de interação entre o modelo (ChatGPT) e o
  mecanismo de busca (Azure AI Search)

- Oferece configurações diretamente na interface do usuário (UX) para
  ajustar o comportamento e testar diferentes opções

- Possui monitoramento e rastreamento de desempenho opcionais com o
  Application Insights

**Principais tecnologias usadas** – Serviço OpenAI do Azure, modelo
ChatGPT (gpt-35-turbo) e Azure AI Search

**Duração estimada -** 40 minutos

## Exercício 1: Implementar o aplicativo e testá-lo no navegador

### Tarefa 1: Abrir o ambiente de desenvolvimento

1.  Abra seu navegador, vá até a barra de endereços e digite ou cole o
    seguinte
    URL: \`\`https://github.com/technofocus-pte/azure-search-openai-javascript\`\`
    e faça login com sua conta do Github.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image3.jpeg)

2.  Clique em **Fork**.

![Uma captura de tela de uma página da web Descrição gerada
automaticamente](./media/image4.jpeg)

3.  Insira o nome do repositório e clique em **Create fork**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image5.jpeg)

4.  Clique em **Code -\> Codespaces -\> +**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image6.jpeg)

5.  Aguarde a configuração do ambiente. Leva em torno de 5 a 10 minutos.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image7.jpeg)

### Tarefa 2: Provisionar os serviços necessários para criar e implementar o aplicativo de chat no Azure

1.  Execute o seguinte comando no Terminal. Copie o código e pressione
    Enter.

''azd auth login''

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image8.png)

2.  O navegador padrão é aberto para a inserção de um código. Digite o
    código copiado e clique em **Next**.

![](./media/image9.png)

3.  Entre com suas credenciais do Azure.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image10.png)

![Uma captura de tela de um erro do computador Descrição gerada
automaticamente](./media/image11.png)

4.  Volte para a guia Github Codespace. Execute o comando abaixo para
    inicializar o ambiente do projeto no diretório atual. Digite o nome
    do ambiente como ''**ragpgpy ''** e pressione Enter.

Observação: o nome do ambiente deve ser exclusivo

'' azd env novo''

![](./media/image12.png)

5.  Execute o comando abaixo para provisionar os serviços para o Azure,
    crie seu contêiner.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image13.png)

6.  Selecione os valores abaixo.

> ''Disposição azd''

- **Select an Azure Subscription to use**: selecione sua assinatura

- **Select an Azure location to use** : **East us2/west us2** (Às vezes,
  o Leste dos EUA pode não estar disponível, escolha o local na lista
  mencionada abaixo.)

- Selecione o grupo de recursos existente: Seu grupo de recursos
  existente (por exemplo: **ResourceGroup1)**

![](./media/image14.png)

9.  Aguarde até que o recurso seja provisionado completamente. Este
    processo levará em torno de 5 a 10 minutos para criar todos os
    recursos necessários.

> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image15.png)

### Tarefa 3: Implementar o aplicativo de chat e explorá-lo

1.  Execute o comando abaixo para implementar o aplicativo.

''azd deploy''

![](./media/image16.png)

2.  Aguarde a implementação. Leva menos de 5 minutos.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image17.png)

3.  Clique na URL do endpoint gerado.

![](./media/image18.png)

4.  Clique em **Open**.

![](./media/image19.png)

5.  O aplicativo é aberto em uma nova guia.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image20.png)

6.  Selecione o container **How to search and book rental?** e, em
    seguida, clique no botão de Enter ao lado da caixa de texto.

![](./media/image21.png)

### Tarefa 4: Limpar todos os recursos

1.  Volte para o **Azure portal -\> Resource group- \> Resource group
    name.**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image22.png)

2.  Selecione todos os recursos e clique em Excluir conforme mostrado na
    imagem abaixo. (**NÃO CLIQUE EM DELETE** resource group)

![](./media/image23.png)

3.  Digite \`\`**delete**\`\` na caixa de texto e clique em **Delete**.

![](./media/image24.png)

4.  Confirme a exclusão clicando em **Delete**.

![](./media/image25.png)

5.  Volte para a guia do portal do Github e atualize a página.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image26.png)

6.  Clique em Code, selecione a branch criada para este laboratório e
    clique em **Delete**.

![](./media/image27.png)

7.  Confirme a exclusão da branch clicando no botão **Delete**.

![](./media/image28.png)

### Resumo:

Este caso de uso pensou em você, implementando um aplicativo de chat
usando o padrão de Retrieval Augmented Generation em execução no Azure,
usando o Azure AI Search para recuperação de informações e os large
language models (LLMs) do Azure OpenAI e do LangChain para potencializar
as experiências de perguntas e respostas no estilo ChatGPT.
