# Caso de uso 07 – Habilitando a busca semântica no Azure Database for PostgreSQL Flexible Server utilizando Azure OpenAI para gerar embeddings vetoriais.

**Objetivo** :

Nesse caso de uso, você irá implementar a busca semântica para gerar e
armazenar embeddings, instalar as extensões vector e azure_ai em uma
instância do Azure Database for PostgreSQL Flexible Server e aplicar
essas extensões para armazenar vetores de embeddings gerados pelo Azure
OpenAI.

**Principais tecnologias usadas** – Azure OpenAI, Azure Database for
PostgreSQL, extensão Azure AI

**Duração estimada** - 45 minutos

**Tipo de laboratório:** Conduzido por instrutor

## Exercício 1: Gerar embeddings vetoriais com o Azure OpenAI

Para realizar buscas semânticas, você deve primeiro gerar vetores de
embedding a partir de um modelo, armazená-los em um banco de dados
vetorial e, em seguida, consultar os embeddings. Você criará um banco de
dados, o preencherá com dados de exemplo e executará buscas semânticas
com base nesses registros.

Ao final deste exercício, você terá uma instância do Azure Database for
PostgreSQL Flexible Server com as extensões vector e azure_ai
habilitadas. Você irá gerar embeddings para os dados da tabela listings
do conjunto de dados públicos de Airbnb em Seattle, e também fará buscas
semânticas nesses registros utilizando busca por distância cosseno
vetorial.

1.  Abra um navegador da Web e navegue até
    ''https:\\portal.azure.com/''e entre com suas credenciais do Azure.

2.  Selecione o ícone do **Cloud Shell** na barra de ferramentas do
    portal do Azure para abrir um novo painel do Cloud Shell na parte
    inferior da janela do navegador. Selecione a opção **Bash**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image1.jpeg)

3.  Selecione o botão de opção **No storage account required**,
    selecione sua assinatura e clique em **Apply**.

![](./media/image2.png)

4.  No prompt do Cloud Shell, execute o comando abaixo para clonar o
    projeto

\`\`git clone https://github.com/technofocus-pte/postgresql-case\`\`

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image3.jpeg)

5.  Navegue até a pasta do projeto.

**''cd postgresql-case''**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image4.jpeg)

6.  Em seguida, você executa três comandos para definir variáveis que
    reduzem a digitação redundante ao usar os comandos do Azure CLI para
    criar recursos do Azure. As variáveis representam o nome a ser
    atribuído ao seu grupo de recursos (RG_NAME), a região do Azure
    (REGION) para a qual os recursos serão implantados e uma senha
    gerada aleatoriamente para o login do administrador PostgreSQL
    (ADMIN_PASSWORD).

7.  No primeiro comando, a região atribuída à variável correspondente é
    eastus ou westus, **\[but you can replace it with a location of your
    preference.\]{.mark}**. No entanto, se substituir a região padrão,
    você deve selecionar outra \[região do Azure que suporte sumarização
    abstrativa\]{.underline} para garantir que você possa concluir todas
    as tarefas nos módulos deste caminho de aprendizado.

\`\`REGION=westus\`\`

8.  O seguinte comando atribui o nome do grupo de recursos existente
    para ser usado no grupo de recursos que abrigará todos os recursos
    utilizados neste exercício.

> \`\`RG_NAME=Your existing resource group name\`\`

![](./media/image5.png)

9.  O comando final gera uma senha aleatória para o login do
    administrador do PostgreSQL. **Certifique-se de copiá-la** para um
    local seguro, para usá-la posteriormente ao conectar-se ao seu
    servidor PostgreSQL flexível.

> a=()
>
> for i in {a..z} {A..Z} {0..9};
>
> do
>
> a\[$RANDOM\]=$i
>
> done
>
> ADMIN_PASSWORD=$(IFS=; echo "${a\[\*\]::18}")
>
> echo "Your randomly generated PostgreSQL admin user's password is:"

echo $ADMIN_PASSWORD

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image6.jpeg)

### Tarefa 1: Atribuir Colaborador dos Serviços Cognitivos

1.  Abra uma nova guia e vá para \`\`**https://portal.azure.com\`\`**.
    Faça login com suas credenciais do Azure e, em seguida, clique na
    caixa de diálogo **Subscription**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image7.jpeg)

2.  Clique em subscription name.

![](./media/image8.png)

3.  Clique em Access control (IAM) no menu de navegação à esquerda.
    Clique em **Add** e selecione **Add role assignment.**

![](./media/image9.png)

4.  Pesquise por \`\`Cognitive Services Contributor\`\`, selecione-o e
    clique no botão **Next**.

![Uma captura de tela de uma atribuição de serviço Descrição gerada
automaticamente](./media/image10.jpeg)

5.  Selecione **User, group or service principal** e clique no link
    **select member**. Pesquise sua conta de assinatura do Azure e
    selecione-a. Por fim, clique no **botão Select.**

![](./media/image11.png)

6.  Clique no botão **Review + assign**.

![](./media/image12.png)

7.  Clique no botão **Review + assign** novamente.

> ![](./media/image13.png)

### Tarefa 2: Executar o script de implementação do Bicep para provisionar recursos do Azure

1.  Volte para a primeira aba do portal do Azure com o Azure CLI para
    executar um script de implementação Bicep e provisionar os recursos
    do Azure no seu grupo de recursos: A implementação leva
    aproximadamente de 3 a 5 minutos

\`\`cd\`\`

\`\`az deployment group create --resource-group $RG_NAME --template-file
"postgresql-case/Allfiles/Labs/Shared/deploy.bicep" --parameters
restore=false adminLogin=pgAdmin adminLoginPassword=$ADMIN_PASSWORD\`\`

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image14.jpeg)

![Uma captura de tela de computador de uma tela preta Descrição gerada
automaticamente](./media/image15.jpeg)

2.  O script de implementação Bicep provisiona os serviços do Azure
    necessários para concluir este exercício dentro do seu grupo de
    recursos. Os recursos implantados incluem um Azure Database for
    PostgreSQL - Flexible Server. Você pode verificar os recursos no seu
    grupo de recursos.

- **Azure OpenAI,**

- **Azure AI Language service.**

![](./media/image16.png)

3.  Clique no recurso Open AI.

![](./media/image17.png)

4.  Clique em **Keys and Endpoint** em **Resource Management** no menu
    de navegação à esquerda. Anote a key 1 e o endpoint para usá-los na
    Tarefa 5

![](./media/image18.png)

5.  O script Bicep também executa algumas etapas de configuração, como
    adicionar as extensões azure_ai e vector à lista de permissões do
    servidor PostgreSQL (por meio do parâmetro de servidor
    azure.extensions), criar um banco de dados chamado rentals no
    servidor e adicionar uma implementação chamada embedding usando o
    modelo **text-embedding-ada-002** ao serviço Azure OpenAI.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image19.jpeg)

6.  A implementação normalmente leva vários minutos para ser concluída.
    Você pode monitorá-lo no Cloud Shell ou navegar até a página
    **Deployments** do grupo de recursos que você criou acima e
    acompanhar o progresso da implementação por lá.

7.  Feche o painel do Cloud Shell quando a implementação do recurso for
    concluída.

### Tarefa 3: Conectar-se ao banco de dados usando psql no Azure Cloud Shell

Nesta tarefa, você se conecta ao banco de dados rentals no seu servidor
Azure Database for PostgreSQL - Flexible Server utilizando o utilitário
de linha de comando psql a partir do Azure Cloud Shel.

1.  No portal do Azure (https://portal.azure.com/), navegue até seu
    servidor recém-criado do Azure Database for PostgreSQL - Flexible
    Server.

![](./media/image20.png)

1.  Na barra lateral, selecione **Server Parameters**. Pesquise o
    parâmetro **azure.extensions**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image21.png)

2.  Selecione as extensões **Vector** e **AZURE_AI** se ainda não
    estivejam selecionadas.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image22.png)

2.  No menu de recursos, em **Settings**, selecione **Databses** e
    selecione **Connect** para o banco de dados rentals.

![](./media/image23.png)

3.  No prompt "Password for user pgAdmin" no Cloud Shell, insira a senha
    gerada aleatoriamente para o **login** do pgAdmin.

Uma vez conectado, o prompt psql para o banco de dados rentals será
exibido.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image24.jpeg)

4.  Durante o restante deste exercício, você continuará trabalhando no
    Cloud Shell, portanto, pode ser útil expandir o painel na janela do
    navegador selecionando o botão **Maximize** no canto superior
    direito do painel.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image25.jpeg)

### Tarefa 4: Configurar extensões

Para armazenar e consultar vetores e gerar embeddings, você precisa
permitir e habilitar duas extensões no Azure Database for PostgreSQL
Flexible Server: vector e azure_ai.

1.  Volte para a guia do Azure Portal com o Azure CLI e execute o
    seguinte comando SQL para habilitar a extensão vector. Para
    instruções detalhadas:

\`\`CREATE EXTENSION vector;\`\`

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image26.jpeg)

3.  Para habilitar a extensão azure_ai, **update and run** o comando SQL
    a seguir. Você precisará do endpoint e da chave da API do seu
    recurso do Azure OpenAI:

> \`\`CREATE EXTENSION azure_ai;\`\`

\`\`SELECT azure_ai.set_setting('azure_openai.endpoint',
'https://\<endpoint\>.openai.azure.com');\`\`

\`\`SELECT azure_ai.set_setting('azure_openai.subscription_key', '\<API
Key\>');\`\`

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image27.jpeg)

### Tarefa 5: Preencher o banco de dados com dados de exemplo

Antes de explorar a extensão azure_ai, adicione algumas tabelas ao banco
de dados rentals e preencha-as com dados de exemplo, para que você tenha
informações com as quais trabalhar ao revisar a funcionalidade da
extensão.

1.  Execute os seguintes comandos para criar as tabelas listings e
    reviews para armazenar dados sobre imóveis para locação e avaliações
    de clientes:

> DROP TABLE IF EXISTS listings;
>
> CREATE TABLE listings (
>
> id int,
>
> name varchar(100),
>
> description text,
>
> property_type varchar(25),
>
> room_type varchar(30),
>
> price numeric,
>
> weekly_price numeric
>
> );

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image28.jpeg)

DROP TABLE IF EXISTS reviews;

CREATE TABLE reviews (

id int,

listing_id int,

date date,

comments text

);

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image29.jpeg)

2.  Em seguida, use o comando COPY para carregar dados de arquivos CSV
    em cada tabela criada acima. Comece executando o seguinte comando
    para preencher a tabela listings:

''\COPY listings FROM
'postgresql-case/Allfiles/Labs/Shared/listings.csv' CSV HEADER''

A saída do comando deve ser COPY 50, o que indica que 50 linhas foram
gravadas na tabela a partir do arquivo CSV.

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image30.jpeg)

3.  Por fim, execute o comando abaixo para carregar as avaliações dos
    clientes na tabela de avaliações:

''\COPY reviews FROM 'postgresql-case/Allfiles/Labs/Shared/reviews.csv'
CSV HEADER''

A saída do comando deve ser COPY 354, o que indica que 354 linhas foram
gravadas na tabela a partir do arquivo CSV.

![](./media/image31.jpeg)

4.  Para redefinir seus dados de amostra, você pode executar listagens
    DROP TABLE e repetir essas etapas.

### Tarefa 6: Criar e armazenar vetores de embedding

Agora que temos alguns dados de exemplo, é hora de gerar e armazenar os
vetores de embedding. A extensão azure_ai facilita a chamada à API de
embedding do Azure OpenAI.

1.  Adicionar a coluna de vetor de embedding.

O modelo text-embedding-ada-002 está configurado para retornar com 1.536
dimensões, então você deve usar esse valor para o tamanho da coluna de
vetor.

''ALTER TABLE listings ADD COLUMN listing_vector vector(1536);''

![Uma captura de tela de computador de uma tela preta Descrição gerada
automaticamente](./media/image32.jpeg)

2.  Gere um vetor de embedding para a descrição de cada listagem
    chamando o Azure OpenAI por meio da função definida pelo usuário
    chamada create_embeddings, que é implementada pela extensão
    azure_ai:

> UPDATE listings SET listing_vector =
> azure_openai.create_embeddings('embedding', description, max_attempts
> =\> 5, retry_delay_ms =\> 500) WHERE listing_vector IS NULL;

Observe que isso pode levar vários minutos, dependendo da cota
disponível.

![Uma captura de tela de uma tela de computador Descrição gerada
automaticamente](./media/image33.png)

### Tarefa 7: Executar uma consulta de busca semântica

Agora que os dados das listagens foram enriquecidos com vetores de
embedding, é hora de executar uma consulta de busca semântica. Para
isso, será necessário obter o vetor de embedding da string de consulta
e, em seguida, realizar uma busca por similaridade usando a distância
cosseno, para encontrar as listagens cujas descrições são mais
semanticamente semelhantes à consulta.

1.  Use o vetor de embedding na busca cosseno (\\=\> representa a
    operação de distância cosseno), buscando as 10 listagens mais
    semelhantes à consulta.

SELECT id, nome FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;

Você verá um resultado semelhante ao exemplo mostrado. Os resultados
podem variar, já que os vetores de embedding não são garantidamente
determinísticos:

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image34.jpeg)

2.  Você também pode projetar a coluna description para poder ler o
    texto das linhas correspondentes cujas descrições foram
    semanticamente semelhantes à consulta:

SELECT id, description FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 1;

Essa consulta exibirá algo como:

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image35.jpeg)

Para entender intuitivamente a busca semântica, observe que a descrição
não contém literalmente os termos “brilhante” ou “natural”.  
No entanto, ela destaca palavras como “verão”, “luz do sol”, “janelas” e
até uma “janela de teto”.

### Tarefa 8 : Verifique seu trabalho

Após executar os passos anteriores, a tabela listings agora contém dados
de exemplo do Seattle Airbnb Open Data, disponível no Kaggle. Essas
listagens foram enriquecidas com vetores de embedding para permitir
buscas semânticas.

1.  Confirme se a tabela listings possui quatro colunas: id, name,
    description e listing_vector.

''\d listagens''

Esse comando deve retornar algo parecido com:

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image36.jpeg)

2.  Confirme se pelo menos uma linha possui a coluna listing_vector
    preenchida.

\`\`SELECT COUNT(\*) \> 0 FROM listings WHERE listing_vector IS NOT
NULL;\`\`

O resultado deve mostrar um t, que significa verdadeiro. Uma indicação
de que há pelo menos uma linha com embeddings de sua coluna de descrição
correspondente:

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image37.jpeg)

3.  Confirme se o vetor de embedding possui 1536 dimensões:

\`\`SELECT vector_dims(listing_vector) FROM listings WHERE
listing_vector IS NOT NULL LIMIT 1;\`\`

O resultado deve mostrar:

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image38.jpeg)

4.  Confirme se as buscas semânticas estão retornando resultados

Utilize o vetor de embedding em uma busca por distância cosseno,
retornando os 10 resultados mais semelhantes à consulta:

\`\`SELECT id, name FROM listings ORDER BY listing_vector \<=\>
azure_openai.create_embeddings('embedding', 'bright natural
light')::vector LIMIT 10;\`\`

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image39.jpeg)

5.  Permaneça na mesma página para continuar com a próxima tarefa.

## Exercício 2 – Criar uma função de busca para um sistema de recomendação

Vamos envolver a lógica de vetores de embedding e chamadas à API do
Azure OpenAI dentro de uma função. Neste exercício, você irá instalar as
extensões vector e azure_ai em um Azure Database for PostgreSQL -
Servidor Flexível, e explorar as capacidades dessas extensões para
integrar o [Azure
OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/overview)
diretamente ao banco de dados.

### Tarefa 1: Criar uma função de busca para um sistema de recomendação

Vamos criar um sistema de recomendações baseado em busca semântica. O
sistema irá recomendar diversas listagens similares, com base em uma
listagem de exemplo.  
Essa listagem pode ser a que o usuário está visualizando ou pode
representar suas preferências pessoais. Implementaremos esse sistema
como uma função do PostgreSQL, utilizando a extensão azure_openai.

Ao final deste exercício, você terá definido uma função chamada
recommend_listing, que retorna no máximo numResults listagens mais
semelhantes à listagem fornecida via sampleListingId. Você poderá usar
esses dados para gerar novas oportunidades novas oportunidades de
negócio e experiências personalizadas para o usuário.

Implemente os recursos na sua assinatura do Azure

Esta etapa orienta você no uso de comandos Azure CLI no Azure Cloud
Shell para criar um grupo de recursos e executar um script Bicep para
implantar os serviços do Azure necessários para conclusão deste
exercício em sua assinatura do Azure.

**Observação:** se você estiver fazendo vários módulos neste roteiro de
aprendizagem, poderá compartilhar o ambiente do Azure entre eles. Nesse
caso, você só precisa concluir essa etapa de implementação de recursos
uma vez.

### Tarefa 2: Criar a função de recomendação

1.  A função de recomendação recebe um sampleListingId e retorna as
    numResults listagens mais semelhantes. Para isso, ela cria um
    embedding com base no nome e na descrição da listagem de exemplo e
    executa uma busca semântica usando esse vetor de consulta em
    comparação com os embeddings das demais listagens.

> CREATE FUNCTION
>
> recommend_listing(sampleListingId int, numResults int)
>
> RETURNS TABLE(
>
> out_listingName text,
>
> out_listingDescription text,
>
> out_score real)
>
> AS $$
>
> DECLARE
>
> queryEmbedding vector(1536);
>
> sampleListingText text;
>
> BEGIN
>
> sampleListingText := (
>
> SELECT
>
> name || ' ' || description
>
> FROM
>
> listings WHERE id = sampleListingId
>
> );
>
> queryEmbedding := (
>
> azure_openai.create_embeddings('embedding', sampleListingText,
> max_attempts =\> 5, retry_delay_ms =\> 500)
>
> );
>
> RETURN QUERY
>
> SELECT
>
> name::text,
>
> description,
>
> -- cosine distance:
>
> (listings.listing_vector \<=\> queryEmbedding)::real AS score
>
> FROM
>
> listings
>
> ORDER BY score ASC LIMIT numResults;
>
> END $$
>
> LANGUAGE plpgsql;
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image40.jpeg)

### Tarefa 3: Consultar a função de recomendação

1.  Para consultar a função de recomendação, passe para ela um ID de
    listagem e o número de recomendações que ela deve retornar.

> select out_listingName, out_score from recommend_listing( (SELECT id
> from listings limit 1), 20); -- search for 20 listing recommendations
> closest to a listing

O resultado será algo como:

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image41.jpeg)

2.  Para visualizar o tempo de execução da função, certifique-se de que
    o parâmetro **track_functions** esteja ativado na seção **Server
    Parameters** no Portal do Azure (você pode usar PL ou ALL):

![](./media/image42.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image43.png)

### Tarefa 4 : Verifique seu trabalho

1.  Certifique-se de que a função existe com a assinatura correta

''\df recommend_listing''

Você deve ver o seguinte:

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image44.jpeg)

2.  Certifique-se de que você pode consultá-lo usando a seguinte
    consulta:

select out_listingName, out_score from recommend_listing( (SELECT id
from listings limit 1), 20); -- search for 20 listing recommendations
closest to a listing

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image45.jpeg)

### Tarefa 5 : Limpar

Assim que você concluir este exercício, exclua os recursos do Azure que
foram criados. A cobrança é feita com base na capacidade configurada e
não na quantidade de uso do banco de dados. Siga estas instruções para
excluir o grupo de recursos e todos os recursos criados durante o
laboratório.

1.  Na página inicial, pesquise **Azure Open AI** e selecione-o.

![](./media/image46.png)

2.  Selecione o recurso Open AI e clique em **Delete**.

![](./media/image47.png)

3.  Digite **delete** na caixa de texto e clique em **\*\*Delete.**
    Confirme a exclusão.

![](./media/image48.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image49.png)

4.  Clique em **Manage deleted resources**, selecione o recurso e, em
    seguida, clique no botão **Purge**, conforme mostrado na imagem
    abaixo.

![](./media/image50.png)

5.  Confirme a limpeza clicando em **Yes**.

![](./media/image51.png)

6.  Na página inicial, selecione **Resource groups** em Serviços do
    Azure.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image52.jpeg)

7.  Clique em Nome do Resource group.

![](./media/image53.png)

8.  Na página **Overview** do resource group, selecione **all the
    resource** e clique em **Delete . DO NOT delete RESOURCE Group.**

> ![](./media/image54.png)

9.  Digite **Delete** e clique em Delete. Confirme a exclusão do recurso
    clicando no botão **Delete**.

![](./media/image55.png)

**Resumo**:

Você aprendeu a usar pesquisa semântica no Azure Database for PostgreSQL
Flexible Server para realizar consultas utilizando embeddings gerados
pelo Azure OpenAI. Essa pesquisa foi realizada por meio das seguintes
etapas:

- Habilitação das extensões vector e azure_ai.

- Criação de colunas vetoriais para armazenar os embeddings.

- Geração e armazenamento dos embeddings.

- Consulta ao banco de dados usando um vetor de consulta.
