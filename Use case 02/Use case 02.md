# Caso de uso 02 – Criar um aplicativo Web Quarkus da Lista de Frutas com o Azure App Service no Linux e no PostgreSQL

**Duração estimada:** 40 minutos

**Tipo de laboratório:** Conduzido por instrutor

**Objetivo:**

Este caso de uso mostra como criar, configurar e implementar um
aplicativo Quarkus seguro no Azure App Service conectado a um banco de
dados PostgreSQL (usando o Azure Database for PostgreSQL). O Azure App
Service é um serviço de hospedagem na Web altamente escalonável e com
aplicação automática de patches que pode implementar facilmente
aplicativos no Windows ou no Linux. Quando terminar, você terá um
aplicativo Quarkus em execução no Azure App Service no Linux.

**Pré-requisitos:**

**Conta do GitHub** – Espera-se que você tenha suas próprias credenciais
de login do GitHub. Se você não tiver, crie um aqui -
+++<https://github.com/signup?user_email=&source=form-home-signup+++>

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

3.  A guia **Help** contém as informações de suporte. O valor do **ID**
    aqui é o **Lab instance ID** que será usado durante a execução do
    laboratório.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image2.png)

## Exercício 1: Executar o exemplo

Primeiro, você configura um aplicativo controlado por dados de exemplo
como ponto de partida. O repositório de exemplo que estamos usando aqui
inclui uma configuração de contêiner de desenvolvimento. O contêiner dev
tem tudo o que você precisa para desenvolver um aplicativo, incluindo o
banco de dados, o cache e todas as variáveis de ambiente necessárias
para o aplicativo de exemplo. O contêiner dev pode ser executado em um
codespace GitHub, o que significa que você pode executar o exemplo em
qualquer computador com um navegador da Web.

1.  Em um navegador, entre na sua conta do GitHub
    +++\*\*<https://github.com/login**+++>.

2.  Abra este url em uma nova guia,
    +++\*\*<https://github.com/technofocus-pte/msdocs-quarkus-postgresql-sample-app**+++>.

3.  Selecione **Fork -\> Create a new fork**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image3.jpeg)

4.  Clique em **Create fork** na página Create a new fork.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image4.jpeg)

5.  Na página forked do repositório, selecione **Code** \> **Create
    codespace on main**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image5.jpeg)

**Observação:** se a opção Create Codespace on main não aparecer, clique
no símbolo + próximo a Codespaces.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image6.jpeg)

**Observação:** a criação do codespace leva cerca de 10 minutos para ser
configurada.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image7.jpeg)

6.  Execute +++mvn quarkus:dev+++ no Terminal. Clique em **Allow** no
    pop-up.

![Uma captura de tela de um navegador O conteúdo gerado por IA pode
estar incorreto.](./media/image8.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image9.jpeg)

7.  Quando você vir a notificação, **Your application running on port
    8080** está disponível, selecione **Open in Browser**. Você deve ver
    o aplicativo de exemplo em uma nova guia do navegador.

Se você vir uma **notification** com a porta **5005**, **ignore-a**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image10.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image11.jpeg)

8.  Para interromper o servidor de desenvolvimento do Quarkus, digite
    **Ctrl+C** no terminal do Codespace.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image12.jpeg)

## Exercício 2: Criar o App Service e o PostgreSQL

Primeiro, você cria os recursos do Azure. As etapas usadas neste
laboratório criam um conjunto de recursos seguros por padrão que incluem
o App Service e o Azure Database for PostgreSQL.

1.  Abra o portal do Azure em +++<https://portal.azure.com/+++> e faça
    **login** com as credenciais de login do Azure na guia **Resources**
    da VM.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image13.png)

2.  Selecione **Cancel** ou o botão fechar na página de boas-vindas.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image14.jpeg)

3.  Insira +++**web app database**+++ na barra de pesquisa na parte
    superior do portal do Azure. Selecione o item rotulado **Web App +
    Database** sob o título **Marketplace**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image15.jpeg)

4.  Em **Create Web App + Database**, preencha os detalhes abaixo e
    selecione **Review + create**

[TABLE]

5.  ![Uma captura de tela de um computador O conteúdo gerado por IA pode
    estar incorreto.](./media/image16.png)

6.  ![Uma captura de tela de um aplicativo da web O conteúdo gerado por
    IA pode estar incorreto.](./media/image17.jpeg)

7.  Assim que a validação for aprovada, clique em **Create**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image18.png)

**Observação:** a criação do aplicativo leva cerca de 15 minutos.

8.  Quando a implementação estiver concluída, clique em **Go to
    resource**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image19.jpeg)

9.  Você será direcionado diretamente para a **página do App Service.**
    Clique em **Home** no canto superior esquerdo.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image20.jpeg)

10. Clique no menu Portal e selecione **Resource Groups** nele.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image21.jpeg)

11. Selecione o grupo de recursos atribuído a você e veja se os recursos
    a seguir são criados a partir da implementação que acabamos de
    executar.

> \- App Service plan

\- App Service

\- Virtual network

\- Azure Database for PostgreSQL flexible server

\- Private DNS zone

![Uma captura de tela de um grupo O conteúdo gerado por IA pode estar
incorreto.](./media/image22.png)

## Exercício 3: Verificar as configurações de conexão

O assistente de criação gerou as variáveis de conectividade para você já
como configurações do aplicativo. Nesta etapa, você aprenderá onde
encontrar as configurações do aplicativo e como criar as suas próprias.

1.  Clique no **App Service** na lista de recursos em Resource group.

![Uma captura de tela de um telefone O conteúdo gerado por IA pode estar
incorreto.](./media/image23.jpeg)

2.  Na página App Service, no menu à esquerda, selecione **Environment
    variables** em **Settings**.

3.  Na guia **App settings** da página **Environment variables**,
    verifique se **AZURE_POSTGRESQL_CONNECTIONSTRING** está presente.
    Ele é injetado em tempo de execução como uma variável de ambiente.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image24.jpeg)

4.  Selecione **+ Add**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image25.jpeg)

5.  Nomeie a configuração **+++PORT+++** e defina seu valor como
    **+++8080+++**, que é a porta padrão do aplicativo Quarkus.
    Selecione **Apply**.

![Uma captura de tela de um login O conteúdo gerado por IA pode estar
incorreto.](./media/image26.jpeg)

6.  Selecione **Apply**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image27.jpeg)

7.  Selecione **Confirm**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image28.jpeg)

8.  Você receberá uma notificação informando que as configurações do
    aplicativo foram atualizadas.

![Uma captura de tela de um telefone O conteúdo gerado por IA pode estar
incorreto.](./media/image29.jpeg)

## Exercício 4: Implementar código de exemplo

Nesta etapa, você configurará a implementação do GitHub usando GitHub
Actions. É apenas uma das muitas maneiras de implementar no App Service,
mas também uma ótima maneira de ter integração contínua em seu processo
de implementação. Por padrão, cada git push para o repositório GitHub
iniciará a ação de criar e implementar.

1.  Na página do App Service, no menu à esquerda, selecione Centro de
    **Deployment Center**  em **Deployment**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image30.jpeg)

2.  Em Origem, selecione **GitHub**. Por padrão, GitHub Actions é
    selecionado como o provedor de criação.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image31.jpeg)

3.  Clique em **Authorize** e entre em sua conta do GitHub e siga o
    prompt para autorizar o Azure.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image32.jpeg)

4.  Preencha os detalhes abaixo, deixe o restante para o padrão e clique
    em **Save**.

[TABLE]

5.  ![Uma captura de tela de um computador O conteúdo gerado por IA pode
    estar incorreto.](./media/image33.jpeg)

6.  Depois que **Salve** é clicado, o App Service confirma um arquivo de
    fluxo de trabalho no repositório GitHub escolhido, no diretório
    .github/workflows.

7.  De volta ao espaço de código do GitHub de seu fork de amostra,
    execute +++**git pull origin main+++.** Isso puxa o arquivo de fluxo
    de trabalho recém-confirmado para o codespace.

\[! Nota\] **Observação:** Se você encontrar casos de teste ainda em
execução no terminal, pressione Ctrl+C e execute o comando acima.

![Uma captura de tela de um código de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image34.jpeg)

8.  Abra **src/main/resources/application.properties** no explorer. O
    Quarkus usa esse arquivo para carregar propriedades Java.

9.  Encontre o código (linhas 10-11). Esse código define a variável de
    produção **%prod.quarkus.datasource.jdbc.url** para a configuração
    do aplicativo que o assistente de criação para você. O
    **quarkus.package.type** é definido para criar um Uber-Jar, que você
    precisa executar no App Service.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image35.jpeg)

10. Abra **.github/workflows/main_quarkuwebapp\[lab instance id\].yml**
    no explorer. Esse arquivo foi criado pelo assistente de criação do
    App Service.

11. Na etapa Compilar com Maven, altere o comando Maven para +++**mvn
    clean install -DskipTests**+++.

**-DskipTests** ignora os testes em seu projeto Quarkus, para evitar que
o fluxo de trabalho do GitHub falhe prematuramente.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image36.jpeg)

12. Selecione a extensão **Source Control** .

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image37.jpeg)

13. Na caixa de texto, digite uma mensagem de confirmação como
    +++**Configure DB and deployment workflow**+++. Selecione **Commit**
    e confirme com **Yes**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image38.jpeg)

14. Selecione **Sync changes 1** e confirme com **OK.**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image39.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image40.jpeg)

15. De volta à página Deployment Center no portal do Azure, selecione
    **Logs**. Uma nova execução de implementação já teria começado a
    partir de suas alterações confirmadas.

16. No item de log da execução de implementação, selecione
    **Build/Deploy Logs**  com o carimbo de data/hora mais recente.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image41.jpeg)

17. Você será direcionado ao seu repositório do GitHub e verá que a ação
    do GitHub está em execução. O arquivo de fluxo de trabalho define
    dois estágios separados, criação e implementação. Aguarde até que a
    execução do GitHub mostre um status de Concluído. Isso leva cerca de
    5 minutos.

![Uma captura de tela de uma página da web O conteúdo gerado por IA pode
estar incorreto.](./media/image42.jpeg)

## Exercício 5: Navegue até o aplicativo

1.  No portal do
    Azure(+++[https://portal.azure.com+++](https://portal.azure.com+++/)),
    abra o Resource group **ResourceGroup1** e selecione o recurso **App
    Service**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image43.png)

2.  No menu à esquerda, selecione **Overview** e selecione a URL do seu
    aplicativo em **Default domain**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image44.jpeg)

3.  Cole a URL copiada em um novo navegador para abrir o aplicativo.

![Uma captura de tela de uma lista de frutas O conteúdo gerado por IA
pode estar incorreto.](./media/image45.jpeg)

4.  Adicione algumas frutas à lista. Agora, você está executando um
    aplicativo Web no Azure App Service, com conectividade segura com o
    Azure Database for PostgreSQL.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image46.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image47.jpeg)

## Exercício 6: Transmitir logs de diagnóstico

O Azure App Service captura todas as mensagens enviadas para o console
para auxiliar no diagnostico de problemas com seu aplicativo. O
aplicativo de exemplo inclui instruções de log padrão do JBoss para
demonstrar essa funcionalidade, conforme mostrado abaixo.

1.  Na página Portal do App Service do Azure, no menu à esquerda,
    selecione **App Service logs** em **Monitoring.**

![Uma captura de tela de um telefone O conteúdo gerado por IA pode estar
incorreto.](./media/image48.jpeg)

2.  Em **Application logging**, selecione **File System**. No menu
    superior, selecione **Save**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image49.jpeg)

3.  No menu à esquerda, selecione **Log stream**. Você vera os logs do
    seu aplicativo, incluindo os logs da plataforma e logs gerados
    dentro do contêiner.

![Uma captura de tela de computador de uma tela de computador O conteúdo
gerado por IA pode estar incorreto.](./media/image50.jpeg)

## Exercício 7: Limpar recursos

1.  Na página inicial do portal do Azure, selecione Resource groups.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image51.jpeg)

2.  Selecione o **NetworkWatcherRG** e clique em **Delete resource
    group**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image52.png)

3.  Digite +++NetworkWatcherRG+++ na caixa de texto e clique em
    **Delete**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image53.png)

![Uma captura de tela de um erro do computador O conteúdo gerado por IA
pode estar incorreto.](./media/image54.png)

4.  Em seguida, na página Resource group, selecione o Resource group
    atribuído.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image55.png)

5.  Selecione todos os **resources** e, em seguida, selecione
    **Delete**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image56.png)

6.  Digite **+++delete+++** na caixa de texto e clique em **Delete**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image57.png)

![Uma captura de tela de um erro do computador O conteúdo gerado por IA
pode estar incorreto.](./media/image58.png)

7.  Uma mensagem de confirmação vai aparecer avisando que os recursos
    foram excluídos com sucesso.

8.  De volta ao GitHub Workspace, clique no menu suspenso ao lado de
    **Code**, selecione os três pontos ao lado do nome do codespace e
    clique em **Delete**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image59.jpeg)

**Resumo:**

Aprendemos a implementar um aplicativo Quarkus seguro no Azure App
Service, conectando-o ao banco de dados PostgreSQL para adicionar nomes
de frutas da interface do usuário do aplicativo.
