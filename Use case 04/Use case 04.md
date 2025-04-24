# Caso de uso 04 – Criar um aplicativo de lista de tarefas em ASP.NET e implementá-lo no Azure App Service conectá-lo ao bando de dados SQL

**Duração estimada:** 40 minutos

**Tipo de laboratório:** Conduzido por instrutor

**Objetivo:**

O Azure App Service oferece um serviço de hospedagem na Web altamente
escalável e com atualizações automáticas. Neste laboratório, você
aprenderá como implementar um aplicativo ASP.NET baseado em dados no App
Service e conectá-lo ao Azure SQL Database. Quando terminar, você terá
um aplicativo ASP.NET em execução no Azure e conectado ao Banco de Dados
SQL.

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
    laboratório.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image2.png)

## Exercício 1: Implementando um aplicativo ASP.NET no Azure com o Azure SQL Database

### Tarefa 1: Configurar o Visual Studio 2022 e executar o aplicativo

1.  Na barra de **Pesquisa** do Windows , digite **+++Visual Studio+++**
    e selecione Visual Studio 2022. Se ele solicitar que você faça
    login, continue com as etapas 2 e 3 ou continue a partir da Etapa 4.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image3.jpeg)

2.  Clique em **Sign in** e **digite** o **Username** e o **password**
    na seção **User Credentials** na guia Recursos da VM.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image4.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image5.png)

3.  Selecione **Start Visual Studio**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image6.jpeg)

4.  Selecione **Open a local folder**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image7.jpeg)

5.  Selecione a pasta **webappwithsqldb** em **C:\Labfiles** e clique em
    **Select Folder**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image8.jpeg)

6.  Depois que a pasta for aberta, clique duas vezes em
    **DotNetAppSqlDb.sln** no **Solution Explorer**.

**Observação:** Se o Gerenciador de Soluções não for aberto
automaticamente, clique em **View -\> Solution Explorer.**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image9.jpeg)

7.  Clique em **Build** -\> **Build Solution**.

![Uma captura de tela de computador de uma tela preta O conteúdo gerado
por IA pode estar incorreto.](./media/image10.jpeg)

8.  Depois que o Build for concluído, selecione **Debug -\>** **Start
    Debugging**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image11.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image12.jpeg)

9.  Isso abrirá um navegador com o **Todos web app** em execução nele.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image13.jpeg)

10. Adicione alguns itens ao aplicativo clicando em **Create New** como
    nas capturas de tela abaixo.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image14.jpeg)

![Uma captura de tela de um aplicativo O conteúdo gerado por IA pode
estar incorreto.](./media/image15.jpeg)

11. Adicione mais alguns itens à lista.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image16.jpeg)

12. No Visual Studio 2022, clique em **Debug -\>** **Stop Debugging**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image17.jpeg)

### Tarefa 2: Publicar o aplicativo ASP.NET no Azure

1.  No **Solution Explorer**, clique com o botão direito do mouse no
    projeto **DotNetAppSqlDb** e selecione **Publicar**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image18.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image19.jpeg)

2.  Selecione **Azure** e clique em **Next**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image20.jpeg)

3.  Selecione **Azure App Service(Windows)** em **Which Azure service
    would you like to use to host your application?**  e clique em
    **Next**.

![Uma captura de tela de um aplicativo de computador O conteúdo gerado
por IA pode estar incorreto.](./media/image21.jpeg)

4.  Na caixa de diálogo Publish, clique em **Entrar** e acesse sua
    assinatura do Azure, se ainda não estiver conectado.

**Observação:** se você já estiver conectado a uma conta da Microsoft,
verifique se essa conta contém sua assinatura do Azure. Se a conta da
Microsoft conectada não tiver sua assinatura do Azure, clique nela para
adicionar a conta correta.

5.  Clique em **Create new** para criar um novo App service.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image22.jpeg)

6.  Insira os detalhes abaixo.

[TABLE]

7.  Clique em **New** no **Hosting Plan**.

8.  ![Uma captura de tela de um computador O conteúdo gerado por IA pode
    estar incorreto.](./media/image23.png)

9.  Clique na opção **New** no **Hosting Plan**, insira os detalhes
    abaixo e clique em **OK.**

[TABLE]

10. ![Uma captura de tela de um computador O conteúdo gerado por IA pode
    estar incorreto.](./media/image24.png)

11. Clique em **Create** na janela App Service e aguarde a criação dos
    recursos do Azure.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image25.png)

12. A caixa de diálogo **Publish** mostra os recursos que você
    configurou. Clique em **Finish**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image26.png)

13. Clique em **Close**.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image27.jpeg)

14. Role para baixo até a seção Server Dependencies e clique no + sign
    para adicionar dependência.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image28.jpeg)

15. Selecione **Azure SQL Database** na página **Add dependency** e
    clique em **Next**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image29.jpeg)

16. Clique em **Create New**  ao lado de Bancos de dados SQL, na caixa
    de diálogo **Connect to Azure SQL Database**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image30.jpeg)

17. Na caixa de diálogo **Azure SQL Database Create new**, clique em
    **New** próximo a Database server.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image31.png)

18. Preencha os detalhes abaixo e clique em **OK.**

[TABLE]

19. ![Uma captura de tela de um computador O conteúdo gerado por IA pode
    estar incorreto.](./media/image32.png)

20. Clique na caixa de diálogo **Create** para Criar um novo banco de
    dados.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image33.png)

### Tarefa 3: Configurar a conexão com o banco de dados

1.  Quando o assistente terminar de criar os recursos do banco de dados,
    clique em **Next**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image34.png)

2.  Preencha os detalhes abaixo na caixa de diálogo **Connect to Azure
    SQL Database** e clique em **Finish**.

[TABLE]

3.  ![Uma captura de tela de um computador O conteúdo gerado por IA pode
    estar incorreto.](./media/image35.jpeg)

4.  Clique em **Finish** depois de revisar o **summary of changes**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image36.png)

5.  Aguarde a conclusão do assistente de configuração e clique em
    **Close**. O Azure SQL Db agora está **conectado** ao seu
    aplicativo.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image37.jpeg)

6.  Na página Publish, clique em **Publish** no canto superior direito.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image38.png)

**Observação:** Isso levará cerca de 5 minutos

7.  Depois que o aplicativo ASP.NET é implementado no Azure, o navegador
    padrão é iniciado com a URL do aplicativo implementando. **Add a few
    to-do items**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image39.jpeg)

### Tarefa 4: Acessar o banco de dados localmente

O Visual Studio permite que você explore e gerencie seu novo banco de
dados no Azure facilmente no **SQL Server Object Explorer**. O novo
banco de dados já abriu seu firewall para o aplicativo App Service que
você criou. Mas para acessá-lo do computador local (como do Visual
Studio), você deve abrir um firewall para o endereço IP público do
computador local. Se o provedor de serviços de Internet alterar seu
endereço IP público, você precisará reconfigurar o firewall para acessar
o banco de dados do Azure novamente.

1.  No menu **View** do Visual Studio 2022 , selecione **SQL Server
    Object Explorer**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image40.jpeg)

2.  Na parte superior do **SQL Server Object Explorer**, clique no botão
    **Add SQL Server**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image41.jpeg)

### Tarefa 5: Configurar a conexão com o banco de dados

1.  Na caixa de diálogo **Connect**, expanda o nó do **Azure**. Todas as
    suas instâncias do Banco de Dados SQL no Azure estão listadas aqui.

2.  Selecione o banco de dados que você criou anteriormente
    (**dotnetappsqldbdbserver98**). A conexão que você criou
    anteriormente é preenchida automaticamente na parte inferior.

3.  Digite a **senha** de administrador do banco de dados que você criou
    anteriormente (**+++PassWord98+++**) e clique em Connect.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image42.jpeg)

### Tarefa 6: Permitir conexão de cliente do seu computador

A caixa de diálogo Create uma nova regra de firewall é aberta. Por
padrão, um servidor só permite conexões com seus bancos de dados de
serviços do Azure, como seu aplicativo do Azure. Para se conectar ao
banco de dados de fora do Azure, crie uma regra de firewall no nível do
servidor. A regra de firewall permite o endereço IP público do seu
computador local.

A caixa de diálogo já está preenchida com o endereço IP público do seu
computador.

1.  Certifique-se de que Add my client IP esteja selecionado e clique em
    OK.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image43.jpeg)

2.  Depois que o Visual Studio concluir a criação da configuração de
    firewall para sua instância do Banco de Dados SQL, sua conexão será
    exibida no **SQL Server Object Explorer**..

3.  Expanda sua **connection \> Databases \> \< YOUR DATABASE \> \>
    Tables**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image44.jpeg)

4.  Clique com o botão direito do mouse na tabela **Todoes** e selecione
    **View Data**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image45.jpeg)

5.  Visualize o conteúdo da tabela. Os dados adicionados da interface do
    usuário do aplicativo devem ser listados aqui.

![](./media/image46.jpeg)

## Exercício 2: Atualizar o aplicativo com Code First Migrations

1.  No **Solution Explorer**, abra o arquivo **Modelos\Todo.cs** no
    editor de código. Adicione a seguinte propriedade à classe **ToDo**
    como a última linha (após o **public DateTime CreatedDate { get;
    set; }** ) e clique em **Salve**.

+++**public bool Done { get; set; }**+++

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image47.jpeg)

### Tarefa 1: Executar migrações do Code First localmente

Execute alguns comandos para fazer atualizações no banco de dados local.

1.  No menu **Tools**, clique em **NuGet Package Manager** \> **Package
    Manager Console**.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image48.jpeg)

2.  Na janela Package Manager Console, habilite as Code First Migrations
    executando este comando.

**+++Habilitar-Migrações+++**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image49.jpeg)

3.  Adicione uma migração executando o comando abaixo.

+++**Add-Migration AddProperty**+++

![](./media/image50.jpeg)

4.  Atualize o banco de dados local executando o comando abaixo.

+++**Update-Database**+++

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image51.jpeg)

5.  Digite **Ctrl+F5** para executar o aplicativo ou clique em **Debug
    -\> Start without Debugging**. Teste a edição, os detalhes e crie
    links.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image52.jpeg)

6.  A página do aplicativo é aberta e ainda tem a mesma aparência porque
    a lógica do aplicativo ainda não está usando essa nova propriedade.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image53.jpeg)

### Tarefa 2: Usar a nova propriedade

Faça algumas alterações em seu código para usar a propriedade Done.

1.  No Visual Studio, abra **Controllers\TodosController.cs**. Encontre
    o método **Create()** na linha 52 e adicione **+++Done+++** à lista
    de propriedades no atributo Bind. Quando terminar, a assinatura do
    método Create() será semelhante ao seguinte código:

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image54.jpeg)

2.  Abra **Views\Todos\Create.cshtml**. Adicione o código a seguir após
    o \< div class="form-group" \> para **CreatedDate**.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image55.jpeg)

\<div class="form-group"\>

@Html.LabelFor(model =\> model.Done, htmlAttributes: new { @class =
"control-label col-md-2" })

\<div class="col-md-10"\>

\<div class="checkbox"\>

@Html.EditorFor(model =\> model.Done)

@Html.ValidationMessageFor(model =\> model.Done, "", new { @class =
"text-danger" })

\</div\>

\</div\>

\`\`\`

3.  Abra **Views\Todos\Index.cshtml**. Adicione o código a seguir no
    elemento **th** vazio, logo após o elemento **th** para o
    **CreatedDate**.

<+++@Html.DisplayNameFor>(model =\> model.Done)+++

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image56.jpeg)

4.  Adicione este código logo acima do html. ActionLink().

5.  \<td\>

6.  @Html.DisplayFor(modelItem =\> item.Done)

\`\`\`

! \[\](./media/image53.jpeg)

5.  Digite **Ctrl+F5** para executar o aplicativo.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image57.jpeg)

### Tarefa 3: Habilitar Code First Migrations no Azure

1.  Clique com o botão direito do mouse no projeto e selecione
    **Publish**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image58.jpeg)

2.  Clique em **More actions** \> **Edit**  para abrir as configurações
    de publicação.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image59.jpeg)

3.  No menu suspenso **MyDatabaseContext**, selecione a conexão de banco
    de dados para o Azure SQL Database.

4.  Selecione **Execute Code First Migrations** (é executado na
    inicialização do aplicativo) e clique em **Salve**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image60.jpeg)

5.  Na página Publish, clique em **Publish**.

![Um objeto retangular preto com texto branco O conteúdo gerado por IA
pode estar incorreto.](./media/image61.jpeg)

6.  O aplicativo atualizado agora está disponível no Azure.

7.  Tente adicionar itens de tarefas novamente e selecione **Done**, e
    eles devem aparecer em sua página inicial como um item concluído.

![Uma captura de tela de um aplicativo O conteúdo gerado por IA pode
estar incorreto.](./media/image62.jpeg)

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image63.jpeg)

## Exercício 3: Transmitir logs de aplicativos

1.  Na página de publicação, role para baixo até a seção **Hosting**. No
    canto direito, clique em **...** \> **View Streaming Logs**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image64.jpeg)

2.  Os logs agora são transmitidos para a janela Output.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image65.jpeg)

3.  Você ainda não vê nenhuma das mensagens de rastreamento porque,
    quando você seleciona View Streaming Logs pela primeira vez, seu
    aplicativo do Azure define o nível de rastreamento como Erro, que
    registra apenas eventos de erro.

\[! Observação\] **Observação:** reinicie o fluxo de log do Visual
Studio se você ainda não os vir.

### Tarefa 1: Alterar níveis de rastreamento

1.  Vá para a página de publicação. Na seção Hospedagem, clique em **...
    \> Open in Azure portal**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image66.jpeg)

2.  No Portal do Azure – página do aplicativo, selecione **Open in Azure
    portal** no painel à esquerda na seção **Monitoring**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image67.jpeg)

3.  Em **Application Logging** (Sistema de Arquivos), selecione
    **Verbose** em Nível. Clique em **Salve**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image68.jpeg)

4.  No navegador, acesse o aplicativo Web no Azure e execute algumas
    atividades.

![Uma captura de tela de um aplicativo O conteúdo gerado por IA pode
estar incorreto.](./media/image69.jpeg)

5.  As mensagens de rastreamento agora são transmitidas para a janela
    Output no Visual Studio.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image70.jpeg)

6.  Para interromper o serviço de streaming de logs, clique no botão
    **Stop monitoring** na janela Output.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image71.jpeg)

7.  Feche o Visual Studio.

## Exercício 4: Limpar recursos

1.  No portal do Azure, abra o Resourcegroup atribuído.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image72.png)

2.  Selecione todos os recursos e clique em Delete.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image73.png)

3.  Digite +++delete+++ na caixa de texto e selecione Delete. Selecione
    Delete na caixa de diálogo de confirmação.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image74.png)

![Uma captura de tela de um erro do computador O conteúdo gerado por IA
pode estar incorreto.](./media/image75.png)

**Resumo**

Neste laboratório, você aprendeu a implementar um aplicativo ASP.NET
controlado por dados no App Service e conectá-lo ao Azure SQL Database.
