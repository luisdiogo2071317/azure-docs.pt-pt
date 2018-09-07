---
title: Suporte de feed de trabalhar com a alteração no Azure Cosmos DB | Documentos da Microsoft
description: Utilize o suporte de feed de alterações do Azure Cosmos DB para controlar as alterações em documentos e executar o processamento baseado em evento, como acionadores e manter atualizados os sistemas de caches e análise.
keywords: feed de alterações
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: 3aeef8aff33cf19eb8f9c7037c56d04e38b9ed23
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44050379"
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Trabalhar com a alteração de suporte de feed no Azure Cosmos DB

[O Azure Cosmos DB](../cosmos-db/introduction.md) são um rápido e flexível globalmente replicados base de dados adequada para IoT, varejo, jogos e aplicações de registo operacional. Um padrão de design comum nesses aplicativos é usar as alterações dos dados para disparar ações adicionais. Estas ações adicionais podem ser qualquer um dos seguintes: 

* Acionar uma notificação ou uma chamada para uma API quando um documento é inserido ou modificado.
* Stream de processamento de IoT ou realizar as análises.
* Movimento de dados adicionais ao sincronizar com uma cache, o motor de busca ou o armazém de dados ou arquivamento de dados no armazenamento amovível.

O **suporte de feed de alterações** no Azure Cosmos DB permite-lhe criar soluções eficientes e dimensionáveis para cada um desses padrões, conforme mostrado na imagem seguinte:

![Usar as alterações do Azure Cosmos DB feed para análise em tempo real de energia e cenários de computação orientada para eventos](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Suporte de feed de alterações é fornecida para todos os modelos de dados e contentores no Azure Cosmos DB. No entanto, o feed de alterações é de leitura com o cliente do SQL e serializa os itens no formato JSON. Devido ao JSON de formatação, MongoDB, os clientes terão um erro de correspondência entre documentos BSON formatado e o JSON formatado de feed de alterações.

## <a name="how-does-change-feed-work"></a>Como é que o alterar feed trabalho?

Altere feed de suporte no Azure Cosmos DB funciona através da escuta de uma coleção do Azure Cosmos DB para efetuar quaisquer alterações. Em seguida, gera a saída de lista classificada de documentos que foram alteradas pela ordem em que foram modificadas. As alterações são mantidas, podem ser processadas de forma assíncrona e de forma incremental e a saída pode ser distribuída por um ou mais consumidores para processamento paralelo. 

Pode ler a alteração do feed de três formas diferentes, tal como explicado neste artigo:

*   [Com as funções do Azure](#azure-functions)
*   [Utilizar o Azure Cosmos DB SDK](#sql-sdk)
*   [Usando a alteração do Azure Cosmos DB biblioteca processador do feed](#change-feed-processor)

O feed de alterações está disponível para cada intervalo da chave de partição dentro da coleção de documentos e, portanto, pode ser distribuído por um ou mais consumidores para processamento paralelo conforme mostrado na imagem seguinte.

![Processamento distribuído de feed de alterações do Azure Cosmos DB](./media/change-feed/changefeedvisual.png)

Detalhes adicionais:
* Feed de alterações está ativado por predefinição para todas as contas.
* Pode utilizar o seu [débito aprovisionado](request-units.md) na sua região de escrita ou qualquer [região de leitura](distribute-data-globally.md) para ler a partir do feed de alterações, assim como qualquer outra operação do Azure Cosmos DB.
* O feed de alterações inclui inserções e operações de atualização feitas para documentos dentro da coleção. Pode capturar eliminações ao definir um sinalizador de "eliminação de forma recuperável" dentro dos documentos em vez de eliminações. Em alternativa, pode definir um período de expiração finita para os seus documentos através de [capacidade TTL](time-to-live.md), por exemplo, 24 horas e utilize o valor dessa propriedade para capturar as eliminações. Com esta solução, deve processar alterações dentro de um intervalo de tempo mais curto do que o período de expiração do TTL.
* Cada alteração a um documento exatamente uma vez aparece no feed de alterações e os clientes gerir a sua lógica de ponto de verificação. A biblioteca de processador do feed de alterações fornece pontos de verificação automática e ", pelo menos, uma vez" semântica.
* A alteração mais recente para um determinado documento está incluída no log de alterações. Alterações intermédias poderão não estar disponíveis.
* O feed de alterações é ordenado by order of modificação dentro de cada valor de chave de partição. Não há nenhuma ordem garantida em valores de chave de partição.
* As alterações podem ser sincronizadas a partir de qualquer ponto anterior no tempo, ou seja, não há nenhum período de retenção de dados fixa para o qual as alterações ficam disponíveis.
* As alterações estão disponíveis em blocos de intervalos de chaves de partição. Esta capacidade permite que as alterações de coleções de grandes para serem processados em paralelo por vários consumidores/servidores.
* As aplicações podem solicitar vários feeds de mudança em simultâneo na mesma coleção.
* ChangeFeedOptions.StartTime pode ser utilizado para fornecer um ponto de partida inicial, por exemplo, para encontrar o token de continuação correspondente a fornecido tempo do relógio. ContinuationToken, se for especificado, wins sobre os valores StartTime e StartFromBeginning. A precisão da ChangeFeedOptions.StartTime é ~ 5 segundos. 

## <a name="use-cases-and-scenarios"></a>Cenários e casos de utilização

O feed de alterações permite o processamento eficiente de grandes conjuntos de dados com um grande volume de escritas e oferece uma alternativa ao consultar o conjunto de dados inteiro para identificar o que foi alterado. 

Por exemplo, com um feed de alterações, pode efetuar as seguintes tarefas com eficiência:

* Atualize uma cache, o índice de pesquisa ou um armazém de dados com dados armazenados no Azure Cosmos DB.
* Implementar dados de nível de aplicativo e o arquivos de disposição em camadas, ou seja, armazenar os "dados" no Azure Cosmos DB e preparar o "dados amovíveis" [armazenamento de Blobs do Azure](../storage/common/storage-introduction.md) ou [do Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Execute migrações zero tempo de inatividade para outra conta do Azure Cosmos DB com um esquema de particionamento diferente.
* Implemente [pipelines de lambda no Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) com o Azure Cosmos DB. O Azure Cosmos DB fornece uma solução de base de dados dimensionável que pode processar a ingestão e consulta e implementar arquiteturas de lambda com baixo custo total de propriedade. 
* Receber, armazenar dados de eventos de dispositivos, sensores, infraestrutura e aplicações e processar esses eventos em tempo real com [do Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/storm/apache-storm-overview.md), ou [Apache Spark](../hdinsight/spark/apache-spark-overview.md). 

A imagem seguinte mostra como os pipelines de lambda que ingerir e pode utilizar a consulta com o Azure Cosmos DB alteram feed de suporte: 

![Pipeline de lambda com base no Cosmos DB do Azure para ingestão e consulta](./media/change-feed/lambda.png)

Além disso, no seu [sem servidor](http://azure.com/serverless) aplicações web e móveis, pode acompanhar eventos como alterações do seu cliente Perfil, as preferências ou localização para acionar a determinadas ações como enviar notificações push para os seus dispositivos com [Funções do azure](#azure-functions). Se estiver a utilizar o Azure Cosmos DB para criar um jogo, pode, por exemplo, utilize alterar feed para implementar as classificações em tempo real com base em classificações de jogos concluídos.

<a id="azure-functions"></a>
## <a name="using-azure-functions"></a>Com as funções do Azure 

Se estiver a utilizar as funções do Azure, a forma mais simples para ligar a um feed de alterações do Azure Cosmos DB é adicionar um acionador do Azure Cosmos DB para a sua aplicação de funções do Azure. Quando cria um acionador do Azure Cosmos DB numa aplicação de funções do Azure, selecionar a coleção do Azure Cosmos DB para ligar a e a função é acionada sempre que for feita uma alteração à coleção. 

Acionadores podem ser criados no portal das funções do Azure, no portal do Azure Cosmos DB, ou por meio de programação. Para obter mais informações, consulte [do Azure Cosmos DB: computação de base de dados sem servidor com funções do Azure](serverless-computing-database.md).

<a id="sql-sdk"></a>
## <a name="using-the-sdk"></a>Utilizar o SDK

O [SDK de SQL](sql-api-sdk-dotnet.md) para o Azure Cosmos DB dá-lhe todo o poder para ler e gerir um feed de alterações. Mas com muito poder, vem muita responsabilidades, demasiado. Se pretender gerir pontos de verificação, lidar com números de sequência de documento e ter um controlo granular sobre as chaves de partição, em seguida, utilizar o SDK pode ser a abordagem correta.

Esta secção descreve como utilizar o SDK de SQL para trabalhar com um feed de alterações.

1. Comece por ler os seguintes recursos de appconfig. Instruções sobre como obter a chave de ponto final e a autorização estão disponíveis no [atualizar a cadeia de ligação](create-sql-api-dotnet.md#update-your-connection-string).

    ``` csharp
    DocumentClient client;
    string DatabaseName = ConfigurationManager.AppSettings["database"];
    string CollectionName = ConfigurationManager.AppSettings["collection"];
    string endpointUrl = ConfigurationManager.AppSettings["endpoint"];
    string authorizationKey = ConfigurationManager.AppSettings["authKey"];
    ```

2. Crie o cliente da seguinte forma:

    ```csharp
    using (client = new DocumentClient(new Uri(endpointUrl), authorizationKey,
    new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    {
    }
    ```

3. Obter a partição de intervalos de chaves:

    ```csharp
    FeedResponse pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri,
        new FeedOptions
            {RequestContinuation = pkRangesResponseContinuation });
     
    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    ```

4. Chame ExecuteNextAsync para cada intervalo da chave de partição:

    ```csharp
    foreach (PartitionKeyRange pkRange in partitionKeyRanges){
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);
        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collectionUri,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = -1,
                // Set reading time: only show change feed results modified since StartTime
                StartTime = DateTime.Now - TimeSpan.FromSeconds(30)
            });
        while (query.HasMoreResults)
            {
                FeedResponse<dynamic> readChangesResponse = query.ExecuteNextAsync<dynamic>().Result;
    
                foreach (dynamic changedDocument in readChangesResponse)
                    {
                         Console.WriteLine("document: {0}", changedDocument);
                    }
                checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
            }
    }
    ```

> [!NOTE]
> Em vez de `ChangeFeedOptions.PartitionKeyRangeId`, pode utilizar `ChangeFeedOptions.PartitionKey` para especificar uma chave de partição única para o qual pretende obter um feed de alterações. Por exemplo, `PartitionKey = new PartitionKey("D8CFA2FD-486A-4F3E-8EA6-F3AA94E5BD44")`.
> 
>

Se tiver vários leitores, pode usar **ChangeFeedOptions** distribuam a carga de leitura para diferentes threads ou de diferentes clientes.

E isso é tudo, com estas algumas linhas de código pode começar a ler o feed de alterações. Pode obter o código completo usado neste artigo a partir da [repositório do GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed).

No código no passo 4 acima, o **ResponseContinuation** nos últimos linha tem o último número de sequência lógica (LSN) do documento, que irá utilizar na próxima vez que ler novos documentos após o seguinte número de sequência. Ao utilizar o **StartTime** da **ChangeFeedOption** pode alargá sua rede para obter os documentos. Portanto, se sua **ResponseContinuation** for nulo, mas sua **StartTime** volta no tempo, em seguida, irá obter todos os documentos que sofreram alterações desde a **StartTime**. Mas, se sua **ResponseContinuation** tem um valor, em seguida, o sistema irá ajudá-lo a todos os documentos desde esse LSN.

Por isso, a sua matriz de ponto de verificação apenas é manter o LSN para cada partição. Mas, se não pretender que lidar com as partições, pontos de verificação, LSN, hora de início, etc. a opção mais simples é usar o biblioteca processador do feed de alterações.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Usando a alteração de biblioteca processador do feed 

O [biblioteca processador do feed de alterações do Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) pode ajudá-lo facilmente distribuir o processamento de eventos por vários consumidores. Esta biblioteca simplifica as alterações de leitura em partições e de vários threads trabalhando em paralelo.

O principal benefício da biblioteca de processador do feed de alterações é que não precisa de gerir cada partição e o token de continuação e não tem que consultar cada coleção manualmente.

A biblioteca de processador do feed de alterações simplifica as alterações de leitura em partições e de vários threads trabalhando em paralelo.  Gere automaticamente as alterações de leitura por usando um mecanismo de concessão de partições. Como pode ver na imagem seguinte, se iniciar dois clientes que utilizam o biblioteca processador do feed de alterações, elas dividem o trabalho entre si. À medida que continua a aumentar os clientes, eles continuar dividindo o trabalho entre si.

![Processamento distribuído de feed de alterações do Azure Cosmos DB](./media/change-feed/change-feed-output.png)

O cliente do esquerda foi iniciado primeiro e ele começou a monitorização de todas as partições, em seguida, o segundo cliente foi iniciado e, em seguida, a primeira desistir de algumas das concessões para segundo cliente. Como pode ver isso é excelente maneira de distribuir o trabalho entre clientes e computadores diferentes.

Tenha em atenção que se tiver dois funtions de Azure sem servidor a mesma coleção de monitorização e utilizar a concessão do mesmo, em seguida, as duas funções podem obter documentos diferentes consoante a forma como a biblioteca de processador decide processo as partições.

<a id="understand-cf"></a>
### <a name="understanding-the-change-feed-processor-library"></a>Noções básicas sobre a alteração de biblioteca processador do feed

Existem quatro componentes principais de implementar a biblioteca de processador do feed de alterações: a coleção monitorizada, a coleção de concessão, o anfitrião do processador e os consumidores. 

> [!WARNING]
> Criar uma coleção tem implicações ao nível dos preços, pois está a reservar débito para a aplicação comunicar com o Azure Cosmos DB. Para obter mais detalhes, visite a [página de preços](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

**Coleção de monitorizado:** a coleção monitorizada é os dados a partir do qual o feed de alterações é gerado. Qualquer inserções e as alterações à coleção monitorizada são refletidas no feed de alterações da coleção. 

**Coleção de concessão:** as coordenadas de coleção de concessão do feed de alterações em várias funções de trabalho de processamento. Uma coleção separada é utilizada para armazenar as concessões com uma concessão por partição. É vantajoso para armazenar esta coleção de concessão numa conta diferente com a região de escrita mais perto de onde está a executar o processador do feed de alterações. Um objeto de concessão contém os seguintes atributos: 
* Proprietário: Especifica o host que detém a concessão
* Continuação: Especifica a posição (token de continuação) na alteração do feed para uma partição específica
* Timestamp: Hora da última concessão foi atualizada. o carimbo de hora pode ser usado para verificar se a concessão seja considerada expirada 

**Anfitrião do processador:** cada anfitrião determina quantas partições para o processo com base no número de outro instâncias de anfitriões tem concessões ativas. 
1.  Quando um anfitrião é iniciado, ele adquire concessões a balancear a carga de trabalho em todos os anfitriões. Um anfitrião renova periodicamente concessões, para que as concessões permanecem ativas. 
2.  Leia ao token de continuação última para a concessão para cada um pontos de verificação do anfitrião. Para garantir a segurança de simultaneidade, um anfitrião verifica a ETag para cada atualização de concessão. Também são suportadas outras estratégias de ponto de verificação.  
3.  Após o encerramento, um host libera concessões de todos os mas mantém as informações de continuação, para que ele pode continuá ler a partir do ponto de verificação armazenado mais tarde. 

Neste momento, o número de anfitriões não pode ser superior ao número de partições (concessões).

**Os consumidores:** consumidores ou funções de trabalho, são os threads nos quais realizar o processamento de feed de alterações iniciado por cada anfitrião. Cada anfitrião do processador pode ter vários consumidores. Cada consumidor lê a alteração do feed da partição que está atribuída a e notifica o seu host de alterações e expirado concessões.

Para compreender melhor como essas quatro elementos de feed de alterações trabalho de processador em conjunto, vamos examinar um exemplo no diagrama seguinte. A coleção monitorizada Armazena os documentos e utiliza o argumento "city" como a chave de partição. Podemos ver que a partição azul contém documentos com o campo "city" de "A E" e assim por diante. Existem dois anfitriões, cada uma com dois os consumidores de leitura de quatro partições em paralelo. As setas mostram os consumidores de leitura a partir de um ponto específico no feed de alterações. A primeira partição, a azul mais escura representa as alterações não lidas enquanto o azul claro representa as alterações já leitura no feed de alterações. Os anfitriões usam a coleção de concessão para armazenar um valor de "continuação" para controlar a posição atual de leitura para cada consumidor. 

![Anfitrião do processador do feed usando a alteração do Azure Cosmos DB](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>Trabalhar com o biblioteca processador do feed de alterações

Antes de instalar a alteração do feed pacote NuGet de processador, primeiro instale: 

* Microsoft.Azure.DocumentDB, a versão mais recente.
* Newtonsoft, a versão mais recente

Em seguida, instale o [pacote Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) e incluí-la como uma referência.

Para implementar a biblioteca de processador do feed de alterações que precisa fazer a seguir:

1. Implementar um **DocumentFeedObserver** objeto, que implementa **IChangeFeedObserver**.
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;
    using Microsoft.Azure.Documents.Client;

    /// <summary>
    /// This class implements the IChangeFeedObserver interface and is used to observe 
    /// changes on change feed. ChangeFeedEventHost will create as many instances of 
    /// this class as needed. 
    /// </summary>
    public class DocumentFeedObserver : IChangeFeedObserver
    {
    private static int totalDocs = 0;

        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserver" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        /// <param name="client"> Client connected to destination collection </param>
        /// <param name="destCollInfo"> Destination collection information </param>
        public DocumentFeedObserver()
        {
            
        }

        /// <summary>
        /// Called when change feed observer is opened; 
        /// this function prints out observer partition key id. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task OpenAsync(IChangeFeedObserverContext context)
        {
            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine("Observer opened for partition Key Range: {0}", context.PartitionKeyRangeId);
            return Task.CompletedTask;
        }

        /// <summary>
        /// Called when change feed observer is closed; 
        /// this function prints out observer partition key id and reason for shut down. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <param name="reason">Specifies the reason the observer is closed.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task CloseAsync(IChangeFeedObserverContext context, ChangeFeedObserverCloseReason reason)
        {
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("Observer closed, {0}", context.PartitionKeyRangeId);
            Console.WriteLine("Reason for shutdown, {0}", reason);
            return Task.CompletedTask;
        }

        public Task ProcessChangesAsync(IChangeFeedObserverContext context, IReadOnlyList<Document> docs, CancellationToken cancellationToken)
        {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Change feed: PartitionId {0} total {1} doc(s)", context.PartitionKeyRangeId, Interlocked.Add(ref totalDocs, docs.Count));
            foreach (Document doc in docs)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine(doc.Id.ToString());
            }

            return Task.CompletedTask;
        }
    }
    ```

2. Implementar um **DocumentFeedObserverFactory**, que implementa um **IChangeFeedObserverFactory**.
    ```csharp
     using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;

    /// <summary>
    /// Factory class to create instance of document feed observer. 
    /// </summary>
    public class DocumentFeedObserverFactory : IChangeFeedObserverFactory
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserverFactory" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        public DocumentFeedObserverFactory()
        {
        }

        /// <summary>
        /// Creates document observer instance with client and destination collection information
        /// </summary>
        /// <returns>DocumentFeedObserver with client and destination collection information</returns>
        public IChangeFeedObserver CreateObserver()
        {
            DocumentFeedObserver newObserver = new DocumentFeedObserver();
            return newObserver as IChangeFeedObserver;
        }
    }
    ```

3. Definir *CancellationTokenSource* e *ChangeFeedProcessorBuilder*

    ```csharp
    private readonly CancellationTokenSource cancellationTokenSource = new CancellationTokenSource();
    private readonly ChangeFeedProcessorBuilder builder = new ChangeFeedProcessorBuilder();
    ```

5. criar a **ChangeFeedProcessorBuilder** depois de definir os objetos relevantes 

    ```csharp
            string hostName = Guid.NewGuid().ToString();
      
            // monitored collection info 
            DocumentCollectionInfo documentCollectionInfo = new DocumentCollectionInfo
            {
                Uri = new Uri(this.monitoredUri),
                MasterKey = this.monitoredSecretKey,
                DatabaseName = this.monitoredDbName,
                CollectionName = this.monitoredCollectionName
            };
            
            DocumentCollectionInfo leaseCollectionInfo = new DocumentCollectionInfo
                {
                    Uri = new Uri(this.leaseUri),
                    MasterKey = this.leaseSecretKey,
                    DatabaseName = this.leaseDbName,
                    CollectionName = this.leaseCollectionName
                };
            DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory();
            ChangeFeedOptions feedOptions = new ChangeFeedOptions();

            /* ie customize StartFromBeginning so change feed reads from beginning
                can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
            */

            feedOptions.StartFromBeginning = true;
        
            ChangeFeedProcessorOptions feedProcessorOptions = new ChangeFeedProcessorOptions();

            // ie. customizing lease renewal interval to 15 seconds
            // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
            feedProcessorOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

            this.builder
                .WithHostName(hostName)
                .WithFeedCollection(documentCollectionInfo)
                .WithLeaseCollection(leaseCollectionInfo)
                .WithProcessorOptions (feedProcessorOptions)
                .WithObserverFactory(new DocumentFeedObserverFactory());               
                //.WithObserver<DocumentFeedObserver>();  If no factory then just pass an observer

            var result =  await this.builder.BuildAsync();
            await result.StartAsync();
            Console.Read();
            await result.StopAsync();    
    ```

Isso é tudo. Após estes passos alguns documentos serão comecem a aparecer para o **DocumentFeedObserver.ProcessChangesAsync** método.

Acima de código é para fins de ilustração mostrar um tipo diferente de objetos e suas interações. Tem de definir variáveis adequadas e iniciá-las com valores corretos. Pode obter o código completo usado neste artigo a partir da [repositório do GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessorV2).

> [!NOTE]
> Nunca deve ter uma chave mestra em seu código ou no ficheiro de configuração, como mostra acima de código. Veja [como utilizar o Cofre de chaves foi possível obter as chaves](https://sarosh.wordpress.com/2017/11/23/cosmos-db-and-key-vault/).


## <a name="faq"></a>FAQ

### <a name="what-are-the-different-ways-you-can-read-change-feed-and-when-to-use-each-method"></a>Quais são as diferentes formas em que pode ler o Feed de alterações? e quando usar cada método?

Existem três opções para a leitura do feed de alterações:

* **[Utilizar o Azure Cosmos DB SQL API .NET SDK](#sql-sdk)**
   
   Ao utilizar este método, obtém um nível baixo de controlo no feed de alterações. Pode gerir o ponto de verificação, pode acessar um etc de chave de partição específica. Se tiver vários leitores, pode usar [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) distribuam a carga de leitura para diferentes threads ou de diferentes clientes. .

* **[Usando a alteração do Azure Cosmos DB biblioteca processador do feed](#change-feed-processor)**

   Se quiser que a fim de terceirizar muito da complexidade de feed de alterações, em seguida, pode usar a biblioteca de processador do feed de alterações. Esta biblioteca oculta muito da complexidade, mas ainda o feed de alterações de dá que concluir o controlo de sessão. Esta biblioteca segue uma [padrão observador](https://en.wikipedia.org/wiki/Observer_pattern), sua função de processamento é chamada pelo SDK. 

   Se tiver um feed de alterações de alto débito, pode instanciar vários clientes para ler o feed de alterações. Porque está a utilizar "alterar biblioteca processador do feed", ele dividirá automaticamente a carga entre diferentes clientes. Não tem de fazer nada. Toda aquela complexidade é processada pelo SDK. No entanto, se quiser ter seu próprio Balanceador de carga, em seguida, pode implementar IParitionLoadBalancingStrategy para a estratégia de partição personalizada. Implemente IPartitionProcessor – para que as alterações do processamento personalizado numa partição. No entanto, com o SDK, pode processar um intervalo de partição, mas se quiser processar uma chave de partição específica, em seguida, terá de utilizar o SDK para a API de SQL.

* **[Com as funções do Azure](#azure-functions)** 
   
   A última opção função do Azure é a opção mais simples. Recomendamos que utilize esta opção. Quando cria um acionador do Azure Cosmos DB numa aplicação de funções do Azure, selecionar a coleção do Azure Cosmos DB para ligar a e a função é acionada sempre que for feita uma alteração à coleção. Veja uma [ecrã cast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) da utilização do Azure de função e feed de alterações

   Acionadores podem ser criados no portal das funções do Azure, no portal do Azure Cosmos DB, ou por meio de programação. O Visual Studio e o VS Code tem excelente suporte para escrever a função do Azure. Pode escrever e depurar o código no seu ambiente de trabalho e, em seguida, implementar a função com um só clique. Para obter mais informações, consulte [do Azure Cosmos DB: computação de base de dados sem servidor com funções do Azure](serverless-computing-database.md) artigo.

### <a name="what-is-the-sort-order-of-documents-in-change-feed"></a>O que é a ordem de classificação dos documentos no feed de alterações?

Documentos de feed de alterações está disponível na ordem de seu tempo de modificação. Esta sequência de ordenação é garantida apenas por partição.

### <a name="for-a-multi-region-account-what-happens-to-the-change-feed-when-the-write-region-fails-over-does-the-change-feed-also-failover-would-the-change-feed-still-appear-contiguous-or-would-the-fail-over-cause-change-feed-to-reset"></a>Para uma conta de várias regiões, o que acontece com a alteração do feed quando a região de escrita falha a ativação pós-falha? A feed de alterações também ativação pós-falha? A alteração do feed ainda apareceria contígua ou alteraria feed para repor a causa de ativação pós-falha?

Sim, o feed de alterações irão funcionar em toda a operação de ativação pós-falha manual e vai ser contíguo.

### <a name="how-long-change-feed-persist-the-changed-data-if-i-set-the-ttl-time-to-live-property-for-the-document-to--1"></a>O tempo que o feed de alterações manter os dados alterados se defino a propriedade de valor de TTL (Time TTL) para o documento como -1?

Feed de alterações serão mantidos para sempre. Se não for eliminados dados, irá permanecer no feed de alterações.

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region-as-change-feed-is-available-in-all-the-read-regions-by-default"></a>Como configurar as funções do Azure para ler a partir de uma região específica, como feed de alterações está disponível em todas as regiões de leitura por padrão?

Atualmente não é possível configurar funções do Azure para ler a partir de uma região em particular. Existe um problema do GitHub no repositório das funções do Azure para definir as regiões preferenciais de quaisquer enlace do Azure Cosmos DB e o acionador.

As funções do Azure utiliza a política de ligação predefinida. Pode configurar o modo de ligação nas funções do Azure e, por predefinição, ele lê a partir da região de escrita, então é melhor localizar conjuntamente as funções do Azure na mesma região.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>O que é o tamanho predefinido de lotes nas funções do Azure?

100 documentos em cada invocação das funções do Azure. No entanto, este número pode ser configurado no arquivo Function. Aqui está concluída [lista de opções de configuração](../azure-functions/functions-run-local.md). Se estiver a desenvolver localmente, Atualize as definições de aplicação dentro do [Settings](../azure-functions/functions-run-local.md) ficheiro.

### <a name="i-am-monitoring-a-collection-and-reading-its-change-feed-however-i-see-i-am-not-getting-all-the-inserted-document-some-documents-are-missing-what-is-going-on-here"></a>Estou monitorando uma coleção e ler sua alteração feed, no entanto vejo não estou a obter o documento inserido, alguns documentos estão em falta. O que está acontecendo aqui?

Certifique-se de que não existe nenhuma outra função da mesma coleção com a mesma coleção de concessão de leitura. Isso aconteceu comigo e, mais tarde percebi que os documentos em falta são processados pelo meu outras funções do Azure, que também está a utilizar a concessão do mesmo.

Por conseguinte, se estiver a criar várias funções do Azure para ler o mesmo feed de alterações, em seguida, tem de utilizar a coleção de concessão diferentes ou utilizar a configuração de "leasePrefix" para partilhar a mesma coleção. No entanto, quando usa a biblioteca de processador do feed de alterações pode iniciar várias instâncias da sua função e o SDK irá dividir os documentos entre instâncias diferentes automaticamente por si.

### <a name="my-document-is-updated-every-second-and-i-am-not-getting-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Meu documento é atualizado a cada segundo, e não estou a obter todas as alterações nas funções do Azure à escuta para o feed de alterações.

Feed de alteração de inquéritos de funções do Azure para cada 5 segundos, para que as alterações feitas entre 5 segundos perdem. O Azure Cosmos DB armazena apenas uma versão para cada cinco segundos, então obterá a alteração 5th no documento. No entanto, se pretender ir abaixo dos 5 segundos e pretende consultar a cada segundo de Feed de alterações, pode configurar o tempo de consulta "feedPollTime", consulte [do Azure Cosmos DB enlaces](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). Ele é definido em milissegundos com padrão de 5000. Abaixo de 1 segundo é possível, mas não é mais adequada, começará a gravação de mais CPU.

### <a name="i-inserted-a-document-in-the-mongo-api-collection-but-when-i-get-the-document-in-change-feed-it-shows-a-different-id-value-what-is-wrong-here"></a>Eu inserido um documento da coleção da API do Mongo, mas quando receber o documento no feed de alterações, mostra um valor de id diferente. O que há de errado aqui?

A coleção é a coleção de API do Mongo. Lembre-se de feed de alterações é de leitura com o cliente do SQL e serializa os itens no formato JSON. Devido ao JSON de formatação, MongoDB, os clientes terão um erro de correspondência entre documentos BSON formatado e o JSON formatado de feed de alterações. Está a ver é a representação de um documento BSON em JSON. Se usar atributos binários em contas de Mongo, eles são convertidos em JSON.

### <a name="is-there-a-way-to-control-change-feed-for-updates-only-and-not-inserts"></a>Existe alguma forma para controlar apenas para atualizações do feed de alterações e não insere?

Não hoje em dia, mas esta funcionalidade está no plano. Hoje em dia, pode adicionar um marcador de forma recuperável no documento para atualizações.

### <a name="is-there-a-way-to-get-deletes-in-change-feed"></a>Existe uma forma de obter as eliminações no feed de alterações?

Atualmente o feed de alterações não registar eliminações. Feed de alterações está continuamente a melhorar e esta funcionalidade está no plano. Hoje em dia, pode adicionar um marcador de forma recuperável no documento para eliminação. Adicione um atributo no documento chamado "eliminado" e defini-lo como "true" e definir um valor de TTL do documento para que sejam eliminado automaticamente.

### <a name="can-i-read-change-feed-for-historic-documentsfor-example-documents-that-were-added-5-years-back-"></a>Pode ler feed para documentos (por exemplo, os documentos que foram adicionados cinco anos atrás) do históricos de alterações?

Sim, se o documento não é eliminado pode ler a alteração do feed com relação às General a origem da sua coleção.

### <a name="can-i-read-change-feed-using-javascript"></a>Pode ler o feed de alterações usando o JavaScript?

Sim, suporte inicial do SDK node. js para o feed de alterações é adicionado recentemente. Pode ser utilizado como mostrado no exemplo seguinte, em, módulo documentdb de atualização para a versão atual antes de executar o código:

```js

var DocumentDBClient = require('documentdb').DocumentClient;
const host = "https://your_host:443/";
const masterKey = "your_master_key==";
const databaseId = "db";
const collectionId = "c1";
const dbLink = 'dbs/' + databaseId;
const collLink = dbLink + '/colls/' + collectionId;
var client = new DocumentDBClient(host, { masterKey: masterKey });
let options = {
    a_im: "Incremental feed",
    accessCondition: {
        type: "IfNoneMatch",        // Use: - empty condition (or remove accessCondition entirely) to start from beginning.
        //      - '*' to start from current.
        //      - specific etag value to start from specific continuation.
        condition: ""
    }
};
 
var query = client.readDocuments(collLink, options);
query.executeNext((err, results, headers) =&gt; {
    // Now we have headers.etag, which can be used in next readDocuments in accessCondition option.
    console.log(results);
    console.log(headers.etag);
    console.log(results.length);
    options.accessCondition = { type: "IfNoneMatch", condition: headers.etag };
    var query = client.readDocuments(collLink, options);
    query.executeNext((err, results, headers) =&gt; {
        console.log("next one:", results[0]);
    });
});<span id="mce_SELREST_start" style="overflow:hidden;line-height:0;"></span>

```

### <a name="can-i-read-change-feed-using-java"></a>Pode ler o feed de alterações com Java?

Biblioteca de Java para ler o feed de alterações está disponível no [repositório do Github](https://github.com/Azure/azure-documentdb-changefeedprocessor-java). No entanto, atualmente biblioteca Java é trás biblioteca .NET. Em breve, ambas as bibliotecas será sincronizadas.

### <a name="can-i-use-etag-lsn-or-ts-for-internal-bookkeeping-which-i-get-in-response"></a>Posso utilizar _etag, _lsn ou TS para contábeis interno, o que posso obter na resposta?

formato de _etag é interno e que não deve confiar nele (não analisá-lo) porque ele pode mudar em qualquer altura.
TS é o carimbo de hora de criação ou modificação. Pode usar TS para comparação cronológica.
_lsn é um id de lote que é adicionada apenas para o feed de alterações, ele representa o id de transação a partir da loja.... Muitos documentos podem ter o mesmo _lsn.
Mais uma coisa a observar, a ETag no FeedResponse é diferente de _etag que vê no documento. _etag é um identificador interno e usado para simultaneidade, ele instrui sobre a versão do documento e ETag é utilizado para o feed de sequenciamento.

### <a name="does-reading-change-feed-add-any-additional-cost-"></a>O feed de alterações de leitura adiciona quaisquer custos adicionais?

São cobrados para o RU consumidos ou seja, o movimento de dados e para coleções do Azure Cosmos DB sempre consumir RU. Os utilizadores são cobrados RU consumido por coleção de concessão.

### <a name="can-multiple-azure-functions-read-one-collections-change-feed"></a>Várias funções do Azure pode ler a feed de alterações de uma coleção?

Sim. Várias funções do Azure pode ler o feed de alterações da mesma coleção. No entanto, as funções do Azure tem de ter um leaseCollectionPrefix separado definido.

### <a name="should-the-lease-collection-be-partitioned"></a>A coleção de concessão deve ser particionada?

Não, a coleção de concessão pode ser corrigida. Não é necessária a coleção de concessão particionada e atualmente não é suportado.

### <a name="can-i-read-change-feed-from-spark"></a>Posso ler alterar feed do Spark?

Sim, pode. Veja [conector do Spark do Azure Cosmos DB](spark-connector.md). Aqui está uma [ecrã cast](https://www.youtube.com/watch?v=P9Qz4pwKm_0&t=1519s) que mostra como pode processar como uma transmissão em fluxo estruturada do feed de alterações.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-say-a-batch-of-10-documents-and-i-get-an-error-at-7th-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-documentie-7th-document-in-my-next-feed"></a>Se estiver a processar feed ao utilizar as funções do Azure de alterações, digamos que um lote de 10 documentos e, recebo um erro no documento iniciados a 7. Nesse caso os últimos três documentos não são processados como posso iniciar processamento do documento (ou seja, com falhas iniciados a 7 de documento) no meu feed seguinte?

Para lidar com o erro, o padrão recomendado é dispor seu código com o bloco try-catch. Capturar o erro e colocar esse documento numa fila (mensagens não entregues) e, em seguida, definir a lógica para lidar com os documentos que produziu o erro. Com este método se tiver um lote de documento de 200 e apenas um documento falhou, é necessário de jogar fora o lote inteiro.

No caso de erro não deve de retroceder o ponto de verificação de volta para a partir de outro será pode continuar a receber esses documentos de feed de alterações. Lembre-se de que mantém de feed de alterações a captura de final snap última dos documentos, devido a este poderá perder o instantâneo anterior no documento. feed de alterações mantém apenas uma última versão do documento, e entre os outros processos podem voltar e alterar o documento.

Como manter a corrigir o seu código, descobrirá em breve não existem documentos em fila de mensagens não entregues.
As funções do Azure é chamado automaticamente pelo sistema de feed de alterações e o ponto de verificação etc é mantido internamente pela função do Azure. Se quiser reverter o ponto de verificação e controlar cada aspecto dela, deve considerar usar alteração SDK de processador do feed.


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como utilizar o Azure Cosmos DB com as funções do Azure, veja [do Azure Cosmos DB: computação de base de dados sem servidor com funções do Azure](serverless-computing-database.md).

Para obter mais informações sobre como utilizar o biblioteca processador do feed de alterações, utilize os seguintes recursos:

* [Página de informações](sql-api-sdk-dotnet-changefeed.md) 
* [Pacote Nuget](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Código de exemplo que mostra os passos 1 a 6 acima](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [Exemplos adicionais no GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

Para obter mais informações sobre como utilizar o através do SDK do feed de alterações, utilize os seguintes recursos:

* [Página de informações de SDK](sql-api-sdk-dotnet.md)
