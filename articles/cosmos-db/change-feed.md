---
title: Suporte de feed de trabalhar com a alteração do BD Azure Cosmos | Microsoft Docs
description: Utilize o suporte de feed de alteração de base de dados do Azure Cosmos para controlar as alterações em documentos e executar processamento baseadas em eventos como acionadores e manter os sistemas de caches e análise atualizado.
keywords: Alteração do feed
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: 8475c79782730e989f9590566c31ccd50af9f144
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36302051"
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Trabalhar com a alteração de feed de suporte do BD Azure Cosmos

[BD do Azure do Cosmos](../cosmos-db/introduction.md) são um fast e flexível globalmente replicados base de dados adequada para IoT, revenda, jogos e aplicações de registo operacional. É um padrão de conceção comuns nestas aplicações ao utilizar as alterações dos dados para iniciar ações adicionais. Estas ações adicionais pode ser qualquer um dos seguintes procedimentos: 

* Acionar uma notificação ou uma chamada a uma API, quando um documento é inserido ou modificado.
* Fluxo de processamento de IoT ou efetuar a análise.
* Movimento de dados adicionais por sincronizar com uma cache, o motor de busca ou do armazém de dados ou arquivar dados para armazenamento de frio.

O **alteração feed suporte** do BD Azure Cosmos permite-lhe criar soluções eficientes e dimensionáveis para cada um desses padrões, conforme mostrado na imagem seguinte:

![Utilizar base de dados do Azure Cosmos alteração feed análise em tempo real de energia e condicionada por eventos cenários de computação](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Alteração de feed de suporte é fornecida para todos os contentores do BD Azure Cosmos e modelos de dados. No entanto, o feed de alteração de leitura com o cliente do SQL Server e serializa itens em formato JSON. Devido ao JSON de formatação, MongoDB, os clientes irão sentir um erro de correspondência entre documentos BSON formatado e o JSON com formato alteração feed.

No vídeo seguinte, o Azure Gestor de programa do Cosmos DB Andrew Liu demonstra como a alteração da base de dados do Azure Cosmos feed funciona.

> [!VIDEO https://www.youtube.com/embed/mFnxoxeXlaU]
>
>

## <a name="how-does-change-feed-work"></a>Como a alteração feed trabalho?

Altere o feed suporte na base de dados do Azure Cosmos funciona através da escuta de uma coleção de BD do Cosmos do Azure para efetuar quaisquer alterações. -Lo, em seguida, devolve a lista ordenada de documentos que foram alteradas na ordem em que foram modificadas. As alterações são mantidas, podem ser processados no modo assíncrono e incremental e a saída pode ser distribuída por um ou mais consumidores para processamento paralelo. 

Pode ler a alteração do feed de três formas diferentes, tal como explicado posteriormente neste artigo:

*   [Utilizar as funções do Azure](#azure-functions)
*   [Utilizar o Azure Cosmos DB SDK](#sql-sdk)
*   [Utilizar a alteração da base de dados do Azure Cosmos feed biblioteca de processador](#change-feed-processor)

O feed de alteração está disponível para cada intervalo de chave de partição dentro da coleção de documentos e, por conseguinte, pode ser distribuído por um ou mais consumidores para processamento paralelo conforme mostrado na imagem seguinte.

![Feed de processamento distribuído de alteração de base de dados do Azure Cosmos](./media/change-feed/changefeedvisual.png)

Detalhes adicionais:
* O feed de alteração está ativado por predefinição para todas as contas.
* Pode utilizar o [débito aprovisionado](request-units.md) na sua região de escrita ou qualquer [ler região](distribute-data-globally.md) ler a partir da alteração do feed, tal como qualquer outra operação de base de dados do Azure Cosmos.
* O feed de alteração inclui inserções e operações de atualização efetuadas para documentos dentro da coleção. Pode capturar eliminações ao definir um sinalizador de "eliminar de forma recuperável" dentro os documentos em vez de eliminações. Em alternativa, pode definir um período de expiração finita para os documentos através de [capacidade TTL](time-to-live.md), por exemplo, 24 horas e utilize o valor da propriedade para capturar as eliminações. Com esta solução, tem de processar alterações de dentro de um intervalo de tempo mais curto que o período de expiração do valor TTL.
* Cada alteração a um documento é apresentada exatamente uma vez na alteração do feed, e clientes gerir os respetivos lógica de pontos de verificação. A biblioteca de processador de feed de alteração fornece pontos de verificação automático e ", pelo menos, uma vez" semântica.
* A alteração mais recente para um determinado documento está incluída no registo de alterações. Alterações intermédias poderão não estar disponíveis.
* O feed de alteração é ordenado by order of modificação dentro de cada valor de chave de partição. Não há nenhuma ordem garantida entre os valores de chave de partição.
* As alterações podem ser sincronizadas a partir de qualquer ponto no tempo, ou seja, não existe nenhum período de retenção de dados fixa para o qual as alterações estão disponíveis.
* As alterações estão disponíveis em segmentos de intervalos de chaves de partição. Esta capacidade permite alterações de coleções de grandes dimensões para ser processados em paralelo pelos vários consumidores/servidores.
* As aplicações podem pedir vários feeds de alteração em simultâneo na mesma coleção.
* ChangeFeedOptions.StartTime podem ser utilizados para fornecer um ponto de partida inicial, por exemplo, para localizar o token de continuação correspondente fornecido tempo relógio. ContinuationToken, se for especificado, wins sobre os valores StartTime e StartFromBeginning. A precisão da ChangeFeedOptions.StartTime é ~ 5 seg. 

## <a name="use-cases-and-scenarios"></a>Cenários e casos de utilização

O feed de alteração permite o processamento eficiente de grandes conjuntos de dados com um grande volume de escritas e oferece uma alternativa à consulta de um conjunto completo de dados para identificar o que mudou. 

Por exemplo, com uma alteração do feed, pode realizar as seguintes tarefas de forma eficiente:

* Atualize uma cache, o índice de pesquisa ou um armazém de dados com dados armazenados na base de dados do Azure Cosmos.
* Implementar dados ao nível da aplicação e o arquivo de camadas, ou seja, armazenar "dados" na base de dados do Azure Cosmos e expira "dados amovíveis" para [Blob Storage do Azure](../storage/common/storage-introduction.md) ou [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Execute zero migrações de tempo inferior para outra conta de base de dados do Azure Cosmos com um esquema de partições diferente.
* Implementar [pipelines de lambda no Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) com base de dados do Azure Cosmos. BD do Cosmos do Azure fornece uma solução de bases de dados dimensionável que pode processar ingestão e consulta e implementa arquiteturas de lambda com TCO baixa. 
* Receber e armazenar dados de eventos de dispositivos, sensores, infraestrutura e de aplicações e processar esses eventos em tempo real com [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/storm/apache-storm-overview.md), ou [Apache O Spark](../hdinsight/spark/apache-spark-overview.md). 

A imagem seguinte mostra como pipelines de lambda que ambos ingestão e pode utilizar a consulta utilizando a base de dados do Azure Cosmos alteram feed suporte: 

![Pipeline de lambda com base em Cosmos BD do Azure para ingestão e consulta](./media/change-feed/lambda.png)

Além disso, no seu [sem servidor](http://azure.com/serverless) web apps e móveis, pode controlar eventos, tais como as alterações ao seu cliente Perfil, as preferências ou localização para acionar determinadas ações como enviar notificações push para os respetivos dispositivos utilizando [As funções do azure](#azure-functions). Se estiver a utilizar a base de dados do Azure Cosmos para criar um jogo, pode, por exemplo, utilize alterar o feed para implementar leaderboards em tempo real com base no pontuações de jogos concluídas.

<a id="azure-functions"></a>
## <a name="using-azure-functions"></a>Utilizar as funções do Azure 

Se estiver a utilizar as funções do Azure, é a forma mais simples para ligar a um feed de alteração de BD do Cosmos do Azure adicionar um acionador de base de dados do Azure Cosmos à sua aplicação de funções do Azure. Quando cria um acionador de BD do Cosmos Azure numa aplicação das funções do Azure, selecione a coleção de BD do Cosmos do Azure para ligar a e a função é acionada sempre que for efetuada uma alteração à coleção. 

Acionadores que podem ser criados no portal das funções do Azure, no portal do Azure Cosmos DB ou através de programação. Para obter mais informações, consulte [BD do Cosmos do Azure: através das funções do Azure de computação de base de dados sem servidor](serverless-computing-database.md).

<a id="sql-sdk"></a>
## <a name="using-the-sdk"></a>Utilizar o SDK

O [SQL SDK](sql-api-sdk-dotnet.md) para a BD do Cosmos Azure dá-lhe potência para ler e gerir uma alteração do feed. Mas com energia excelente inclui muitas das responsabilidades, demasiado. Se pretender gerir pontos de verificação, lidar com os números de sequência de documento e ter um controlo granular sobre chaves de partição, em seguida, utilizando o SDK pode estar a abordagem à direita.

Esta secção explica como utilizar o SDK do SQL Server para funcionar com uma alteração do feed.

1. Inicie o lendo os seguintes recursos da appconfig. Instruções sobre como obter a chave de autorização e de ponto final estão disponíveis no [atualizar a cadeia de ligação](create-sql-api-dotnet.md#update-your-connection-string).

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

4. Chamada ExecuteNextAsync para cada intervalo de chave de partição:

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
> Em vez de `ChangeFeedOptions.PartitionKeyRangeId`, pode utilizar `ChangeFeedOptions.PartitionKey` para especificar uma chave de partição única para o qual pretende obter de uma alteração do feed. Por exemplo, `PartitionKey = new PartitionKey("D8CFA2FD-486A-4F3E-8EA6-F3AA94E5BD44")`.
> 
>

Se tiver vários leitores, pode utilizar **ChangeFeedOptions** distribuam a carga de leitura para diferentes threads ou diferentes clientes.

E que é, com estes algumas linhas de código pode começar a ler o feed de alteração. Pode obter o código de conclusão utilizado neste artigo o [repositório do GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed).

No código no passo 4 acima, o **ResponseContinuation** nos últimos linha tem o último número de sequência lógica (LSN) do documento, que irá utilizar na próxima vez que o leia novos documentos após o seguinte número de sequência. Utilizando o **StartTime** do **ChangeFeedOption** pode alargar o net para obter os documentos. Se Sim, o **ResponseContinuation** é nulo, mas o **StartTime** ficar atrás no tempo, em seguida, irá obter todos os documentos que foram alterados desde o **StartTime**. No entanto, se sua **ResponseContinuation** tem um valor, em seguida, o sistema irá ajudá-lo a todos os documentos desde que LSN.

Por isso, a matriz de ponto de verificação apenas consiste em manter o LSN para cada partição. Mas, se não quiser lidar com as partições, pontos de verificação, LSN, a hora de início, etc. a opção mais simples consiste em utilizar a biblioteca de processador de feed de alteração.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Utilizar a alteração de feed de biblioteca de processador 

O [biblioteca de processador de feed de alteração de base de dados do Azure Cosmos](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) podem ajudar a distribuir facilmente o processamento de eventos por vários consumidores. Esta biblioteca simplifica as alterações de leitura em partições e de vários threads funcionam em paralelo.

A vantagem principal da biblioteca de processador de feed de alteração é que não tem de gerir cada partição e o token de continuação e não tem de consultar manualmente cada coleção.

A biblioteca de processador de feed de alteração simplifica as alterações de leitura em partições e de vários threads funcionam em paralelo.  Gere automaticamente as alterações de leitura em partições utilizando um mecanismo de concessão. Como pode ver na imagem seguinte, se iniciar duas clientes que utilizam a biblioteca de processador de feed de alteração, se dividem o trabalho entre si próprios. Como continuar a aumentar os clientes, estes mantém dividindo o trabalho entre si próprios.

![Feed de processamento distribuído de alteração de base de dados do Azure Cosmos](./media/change-feed/change-feed-output.png)

O cliente esquerdo foi iniciado primeiro e iniciar a monitorização de todas as partições, em seguida, o cliente segundo foi iniciado e, em seguida, o primeiro permitem aceder de algumas das concessões a segunda cliente. Como pode ver esta é a forma nice para distribuir trabalho entre vários computadores e os clientes.

Tenha em atenção que se tiver dois funtions de Azure sem servidor a mesma coleção de monitorização e utilizar o mesmo período de concessão, em seguida, as duas funções podem obter documentos diferentes consoante o modo como a biblioteca de processador decide processs as partições.

<a id="understand-cf"></a>
### <a name="understanding-the-change-feed-processor-library"></a>Noções sobre a alteração de feed de biblioteca de processador

Existem quatro componentes principais de implementação a biblioteca de processador de feed de alteração: a coleção monitorizada, a coleção de concessão, o anfitrião do processador e os consumidores. 

> [!WARNING]
> Criar uma coleção tem implicações ao nível dos preços, pois está a reservar débito para a aplicação comunicar com o Azure Cosmos DB. Para obter mais detalhes, visite a [página de preços](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

**Coleção de monitorizado:** a coleção monitorizada é os dados a partir do qual o feed de alteração é gerado. Qualquer inserções e as alterações à coleção monitorizada são refletidas do feed de alteração da coleção. 

**Coleção de concessão:** as coordenadas de coleção de concessão processar a alteração entre vários workers do feed. Uma coleção separada é utilizada para armazenar as concessões com uma concessão por partição. É vantajoso para armazenar esta coleção de concessão numa conta com a região de escrita próximo para onde está a executar a alteração de feed de processador diferente. Um objeto de concessão contém os seguintes atributos: 
* Proprietário: Especifica o anfitrião que é proprietário da concessão
* Continuação: Especifica a posição (token de continuação) para uma determinada partição do feed de alterações do
* Timestamp: Hora da última concessão foi atualizada; o carimbo pode ser utilizado para verificar se é considerada a concessão expirou 

**Anfitrião do processador:** cada anfitrião determina quantos partições para o processo com base em quantos outras instâncias dos anfitriões têm concessões ativas são. 
1.  Quando um anfitrião é iniciado, adquire concessões para balancear a carga de trabalho em todos os anfitriões. Um anfitrião periodicamente é renovada de concessões de, pelo que concessões permanecem ativos. 
2.  Pontos de verificação um anfitrião do token de continuação último para a concessão para cada ler. Para garantir a segurança de simultaneidade, o anfitrião verifica ETag para cada atualização de concessão. Também são suportadas outras estratégias de ponto de verificação.  
3.  Após encerramento, um anfitrião versões todas as concessões, mas mantém as informações de continuação, para que possa retomar ler a partir do ponto de verificação armazenado mais tarde. 

Neste momento, o número de anfitriões não pode ser superior ao número de partições (concessões).

**Os consumidores:** consumidores ou técnicos, existem threads que efetuam o processamento de alteração feed iniciado por cada anfitrião. Cada anfitrião do processador pode ter vários consumidores. Cada consumidor lê a alteração do feed da partição que está atribuído à e notifica o respetivo anfitrião de alterações e expirado concessões.

Para compreender melhor como estes quatro elementos do feed de alteração de trabalho do processador em conjunto, vamos ver um exemplo no diagrama seguinte. A coleção monitorizada armazena documentos e utiliza a "Cidade" como a chave de partição. Vemos que a partição azul contém documentos com o campo "Cidade" de "I A" e assim sucessivamente. Existem dois anfitriões, cada um com dois os consumidores de ler a partir de quatro partições em paralelo. As setas mostram os consumidores ler a partir de um lugar para cima específico na alteração do feed. Na primeira partição, o darker azul representa alterações unread enquanto o leve azul representa as alterações efetuadas já leitura a alteração do feed. Os anfitriões de utilizam a coleção de concessão para armazenar um valor de "continuação" para controlar a posição actual de leitura para cada consumidor. 

![Utilizar a alteração da base de dados do Azure Cosmos feed anfitrião do processador](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>Trabalhar com a alteração de feed de biblioteca de processador

Antes de instalar a alteração de feed de pacote NuGet de processador, instale primeiro: 

* Microsoft.Azure.DocumentDB, a versão mais recente.
* Newtonsoft, a versão mais recente

Em seguida, instale o [pacote Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) e incluí-la como uma referência.

Para implementar a biblioteca de feed de processador de alteração que precisa de executar os seguintes:

1. Implementar um **DocumentFeedObserver** objeto que implementa **IChangeFeedObserver**.
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

5. criar o **ChangeFeedProcessorBuilder** depois de definir os objetos relevantes 

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

That’s it. After these few steps documents will start showing up into the **DocumentFeedObserver.ProcessChangesAsync** method.

Above code is for illustration purpose to show different kind of objects and their interaction. You have to define proper variables and initiate them with correct values. You can get the complete code used in this article from the [GitHub repo](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor).

> [!NOTE]
> You should never have a master key in your code or in config file as shown in above code. Please see [how to use Key-Vault to retrive the keys](https://sarosh.wordpress.com/2017/11/23/cosmos-db-and-key-vault/).


## FAQ

### What are the different ways you can read Change Feed? and when to use each method?

There are three options for you to read change feed:

* **[Using Azure Cosmos DB SQL API .NET SDK](#sql-sdk)**
   
   By using this method, you get low level of control on change feed. You can manage the checkpoint, you can access a particular partition key etc. If you have multiple readers, you can use [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) to distribute read load to different threads or different clients. .

* **[Using the Azure Cosmos DB change feed processor library](#change-feed-processor)**

   If you want to outsource lot of complexity of change feed then you can use change feed processor library. This library hides lot of complexity, but still gives you complete control on change feed. This library follows an [observer pattern](https://en.wikipedia.org/wiki/Observer_pattern), your processing function is called by the SDK. 

   If you have a high throughput change feed, you can instantiate multiple clients to read the change feed. Because you are using “change feed processor library”, it will automatically divide the load among different clients. You do not have to do anything. All the complexity is handled by SDK. However, if you want to have your own load balancer, then you can implement IParitionLoadBalancingStrategy for custom partition strategy. Implement IPartitionProcessor – for custom processing changes on a partition. However, with SDK, you can process a partition range but if you want to process a particular partition key then you have to use SDK for SQL API.

* **[Using Azure Functions](#azure-functions)** 
   
   The last option Azure Function is the simplest option. We recommend using this option. When you create an Azure Cosmos DB trigger in an Azure Functions app, you select the Azure Cosmos DB collection to connect to and the function is triggered whenever a change to the collection is made. watch a [screen cast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) of using Azure function and change feed

   Triggers can be created in the Azure Functions portal, in the Azure Cosmos DB portal, or programmatically. Visual Studio and VS Code has great support to write Azure Function. You can write and debug the code on your desktop, and then deploy the function with one click. For more information, see [Azure Cosmos DB: Serverless database computing using Azure Functions](serverless-computing-database.md) article.

### What is the sort order of documents in change feed?

Change feed documents comes in order of their modification time. This sort order is guaranteed only per partition.

### For a multi-region account, what happens to the change feed when the write-region fails-over? Does the change feed also failover? Would the change feed still appear contiguous or would the fail-over cause change feed to reset?

Yes, change feed will work across the manual failover operation and it will be contiguous.

### How long change feed persist the changed data if I set the TTL (Time to Live) property for the document to -1?

Change feed will persist forever. If data is not deleted, it will remain in change feed.

### How can I configure Azure functions to read from a particular region, as change feed is available in all the read regions by default?

Currently it’s not possible to configure Azure Functions to read from a particular region. There is a GitHub issue in the Azure Functions repo to set the preferred regions of any Azure Cosmos DB binding and trigger.

Azure Functions uses the default connection policy. You can configure connection mode in Azure Functions and by default, it reads from the write region, so it is best to co-locate Azure Functions on the same region.

### What is the default size of batches in Azure Functions?

100 documents at every invocation of Azure Functions. However, this number is configurable within the function.json file. Here is complete [list of configuration options](../azure-functions/functions-run-local.md). If you are developing locally, update the application settings within the [local.settings.json](../azure-functions/functions-run-local.md) file.

### I am monitoring a collection and reading its change feed, however I see I am not getting all the inserted document, some documents are missing. What is going on here?

Please make sure that there is no other function reading the same collection with the same lease collection. It happened to me, and later I realized the missing documents are processed by my other Azure functions, which is also using the same lease.

Therefore, if you are creating multiple Azure Functions to read the same change feed then they must use different lease collection or use the “leasePrefix” configuration to share the same collection. However, when you use change feed processor library you can start multiple instances of your function and SDK will divide the documents between different instances automatically for you.

### My document is updated every second, and I am not getting all the changes in Azure Functions listening to change feed.

Azure Functions polls change feed for every 5 seconds, so any changes made between 5 seconds are lost. Azure Cosmos DB stores just one version for every 5 seconds so you will get the 5th change on the document. However, if you want to go below 5 second, and want to poll change Feed every second, You can configure the polling time “feedPollTime”, see [Azure Cosmos DB bindings](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). It is defined in milliseconds with a default of 5000. Below 1 second is possible but not advisable, as you will start burning more CPU.

### I inserted a document in the Mongo API collection, but when I get the document in change feed, it shows a different id value. What is wrong here?

Your collection is Mongo API collection. Remember, change feed is read using the SQL client and serializes items into JSON format. Because of the JSON formatting, MongoDB clients will experience a mismatch between BSON formatted documents and the JSON formatted change feed. You are seeing is the representation of a BSON document in JSON. If you use binary attributes in a Mongo accounts, they are converted to JSON.

### Is there a way to control change feed for updates only and not inserts?

Not today, but this functionality is on roadmap. Today, you can add a soft marker on the document for updates.

### Is there a way to get deletes in change feed?

Currently change feed doesn’t log deletes. Change feed is continuously improving, and this functionality is on roadmap. Today, you can add a soft marker on the document for delete. Add an attribute on the document called “deleted” and set it to “true” and set a TTL on the document so that it can be automatically deleted.

### Can I read change feed for historic documents(for example, documents that were added 5 years back) ?

Yes, if the document is not deleted you can read the change feed as far as the origin of your collection.

### Can I read change feed using JavaScript?

Yes, Node.js SDK initial support for change feed is recently added. It can be used as shown in the following example, please update documentdb module to current version before you run the code:

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

### <a name="can-i-read-change-feed-using-java"></a>Pode ler feed de alteração com o Java?

Não está disponível na biblioteca de Java para ler o feed de alteração [repositório do Github](https://github.com/Azure/azure-documentdb-changefeedprocessor-java). No entanto, atualmente biblioteca Java é algumas versões atrás biblioteca .NET. Logo que ambas as bibliotecas serão sincronizadas.

### <a name="can-i-use-etag-lsn-or-ts-for-internal-bookkeeping-which-i-get-in-response"></a>Posso utilizar _etag, _lsn ou _ts para bookkeeping interno, o que posso obter em resposta?

formato de _etag é interno e não deve depender no mesmo (não analisá-lo) porque esta pode alterar em qualquer altura.
_ts é o carimbo de hora de modificação ou criação. Pode utilizar _ts para comparação cronológica.
_lsn é um id de lote que é adicionado apenas para o feed de alteração, representa o id de transação da loja... Documentos muitos podem ter o mesmo _lsn.
Um aspeto mais salientar, ETag no FeedResponse diferente _etag que vê o documento. _etag é um identificador interno e utilizados para simultaneidade, informa sobre a versão do documento e ETag é utilizada para sequenciar do feed.

### <a name="does-reading-change-feed-add-any-additional-cost-"></a>Ler o feed de alteração acrescenta qualquer custo adicional?

São-lhe cobrados para o RU consumido ou seja, movimento de dados e para coleções de base de dados do Azure Cosmos sempre consumir RU. Os utilizadores serão cobrados RU consumida na coleção de concessão.

### <a name="can-multiple-azure-functions-read-one-collections-change-feed"></a>Várias funções do Azure pode ler o feed de alteração de uma coleção?

Sim. Várias funções do Azure podem ler o feed de alteração da mesma coleção. No entanto, as funções do Azure tem de ter um leaseCollectionPrefix separado definido.

### <a name="should-the-lease-collection-be-partitioned"></a>A coleção de concessão deve ser particionada?

Não, a coleção de concessão possam ser corrigida. Coleção de concessão particionada não é necessária e atualmente não é suportado.

### <a name="can-i-read-change-feed-from-spark"></a>Posso ler alteração feed do Spark?

Sim, pode. Consulte [Azure Cosmos DB Spark conector](spark-connector.md). Eis um [ecrã cast](https://www.youtube.com/watch?v=P9Qz4pwKm_0&t=1519s) que mostra como pode processar alterações como uma transmissão em fluxo estruturada do feed.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-say-a-batch-of-10-documents-and-i-get-an-error-at-7th-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-documentie-7th-document-in-my-next-feed"></a>Se estiver a processar alterações feed utilizando as funções do Azure, diga um lote de 10 documentos e receber um erro no documento 7. Nesse caso os últimas três documentos não são processados como devo começar processamento do documento (revertidos falhado documento 7) no meu feed seguinte?

Para lidar com o erro, o padrão recomendado é moldar o seu código com o bloco try-catch. Detetar o erro e colocar em pausa esse documento uma fila (entregues) e, em seguida, definir lógica para lidar com os documentos que produziu o erro. Com este método se tiver um documento de 200 lote e apenas um documento falhou, não terá ausente acionar o lote de todo.

No caso de erro não deve rewind o ponto de verificação para início senão, será pode manter a obter esses documentos do feed de alteração. Lembre-se de que mantém de feed de alteração a captura de final snap último dos documentos, devido a esta que poderá perder o instantâneo anterior no documento. alteração feed mantém apenas um última versão do documento e entre em outros processos podem ficar e alterar o documento.

Como manter a corrigir o seu código, vai encontrar logo que não existem documentos na fila de entregues.
As funções do Azure denomina-se automaticamente pelo sistema de feed de alteração e o ponto de verificação etc é mantido internamente pela função do Azure. Se pretender reverter o ponto de verificação e controlar todos os aspetos do mesmo, deve considerar utilizar alteração feed SDK do processador.


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como utilizar a base de dados do Azure Cosmos com as funções do Azure, consulte [BD do Cosmos do Azure: através das funções do Azure de computação de base de dados sem servidor](serverless-computing-database.md).

Para obter mais informações sobre como utilizar a biblioteca de processador de feed de alteração, utilize os seguintes recursos:

* [Página de informações](sql-api-sdk-dotnet-changefeed.md) 
* [Pacote Nuget](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Código de exemplo que mostra os passos acima 1-6](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [Exemplos adicionais no GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

Para obter mais informações sobre como utilizar a alteração do feed através do SDK, utilize os seguintes recursos:

* [Página de informações do SDK](sql-api-sdk-dotnet.md)
