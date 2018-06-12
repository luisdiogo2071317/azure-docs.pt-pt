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
ms.openlocfilehash: 2600565493a334c7227e5c0d67a5808f30751108
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261075"
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

* Microsoft.Azure.DocumentDB, versão 1.13.1 ou superior 
* Newtonsoft, versão 9.0.1 ou superior

Em seguida, instale o [pacote Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) e incluí-la como uma referência.

Para implementar a biblioteca de feed de processador de alteração que precisa de executar os seguintes:

1. Implementar um **DocumentFeedObserver** objeto que implementa **IChangeFeedObserver**.

2. Implementar um **DocumentFeedObserverFactory**, que implementa um **IChangeFeedObserverFactory**.

3. No **CreateObserver** método **DocumentFeedObserverFacory**, instanciar o **ChangeFeedObserver** que criou no passo 1 e devolvê-la.

    ```
    public IChangeFeedObserver CreateObserver()
    {
              DocumentFeedObserver newObserver = new DocumentFeedObserver(this.client, this.collectionInfo);
              return newObserver;
    }
    ```

4. Instanciar **DocumentObserverFactory**.

5. Instanciar um **ChangeFeedEventHost**:

    ```csharp
    ChangeFeedEventHost host = new ChangeFeedEventHost(
                     hostName,
                     documentCollectionLocation,
                     leaseCollectionLocation,
                     feedOptions,
                     feedHostOptions);
    ```

6. Registar o **DocumentFeedObserverFactory** com o anfitrião.

O código para os passos 4 a 6 é: 

```
ChangeFeedOptions feedOptions = new ChangeFeedOptions();
feedOptions.StartFromBeginning = true;

ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();
 
// Customizing lease renewal interval to 15 seconds.
// Can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay
feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);
 
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
{
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);
        await host.RegisterObserverFactoryAsync(docObserverFactory);
        await host.UnregisterObserversAsync();
}
```

Já está. Após estes passos alguns documentos iniciará entra o **DocumentFeedObserver ProcessChangesAsync** método. Encontrar o código acima no [repositório do GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)

## <a name="faq"></a>FAQ

### <a name="what-are-the-different-ways-you-can-read-change-feed-and-when-to-use-each-method"></a>Quais são as diferentes formas pode ler o Feed de alteração? e quando utilizar cada método?

Existem três opções para ler o feed de alteração:

* **[Utilizar Cosmos SDK .NET API da BD SQL do Azure](#sql-sdk)**
   
   Através deste método, obter nível baixo de controlo no feed de alteração. Pode gerir o ponto de verificação, pode aceder a um etc de chave de partição específica. Se tiver vários leitores, pode utilizar [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) distribuam a carga de leitura para diferentes threads ou diferentes clientes. .

* **[Utilizar a alteração da base de dados do Azure Cosmos feed biblioteca de processador](#change-feed-processor)**

   Se pretender outsource muita complexidade do feed de alteração, em seguida, pode utilizar a biblioteca de processador de feed de alterações. Esta biblioteca oculta muita complexidade, mas ainda fornecem que concluir o controlo de sessão alterar feed. Esta biblioteca segue um [padrão de observador](https://en.wikipedia.org/wiki/Observer_pattern), a função de processamento é chamada pelo SDK. 

   Se tiver uma alteração de débito elevado feed, pode instanciar vários clientes ao ler o feed de alteração. Porque estão a utilizar "alterar a biblioteca de feed de processador", este irá dividir automaticamente a carga entre diferentes clientes. Não é necessário fazer nada. Todos os a complexidade é processada pelo SDK. No entanto, se pretende que o seu próprio Balanceador de carga, em seguida, pode implementar IParitionLoadBalancingStrategy para a estratégia de partição personalizado. Implemente IPartitionProcessor – para processamento personalizado alterações numa partição. No entanto, com o SDK, pode processar um intervalo de partição, mas se pretender processar uma chave de partição específica, em seguida, tem de utilizar o SDK para a API do SQL Server.

* **[Utilizar as funções do Azure](#azure-functions)** 
   
   A última opção função do Azure é a opção mais simples. Recomendamos que utilize esta opção. Quando cria um acionador de BD do Cosmos Azure numa aplicação das funções do Azure, selecione a coleção de BD do Cosmos do Azure para ligar a e a função é acionada sempre que for efetuada uma alteração à coleção. Veja uma [ecrã cast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) da utilização do Azure funcionar e altere o feed

   Acionadores que podem ser criados no portal das funções do Azure, no portal do Azure Cosmos DB ou através de programação. Visual Studio e o VS Code tem suporte excelente ao escrever a função do Azure. Pode escrever e depurar o código no seu ambiente de trabalho e, em seguida, implementar a função com um clique. Para obter mais informações, consulte [BD do Cosmos do Azure: através das funções do Azure de computação de base de dados sem servidor](serverless-computing-database.md) artigo.

### <a name="what-is-the-sort-order-of-documents-in-change-feed"></a>O que é a sequência de ordenação de documentos no feed de alteração?

Alteração de feed de documentos é fornecido por ordem de respetiva hora de modificação. Esta sequência de ordenação é garantida apenas por partição.

### <a name="for-a-multi-region-account-what-happens-to-the-change-feed-when-the-write-region-fails-over-does-the-change-feed-also-failover-would-the-change-feed-still-appear-contiguous-or-would-the-fail-over-cause-change-feed-to-reset"></a>Para uma conta de multirregião, o que acontece da alteração feed quando a região de escrita falha a ativação pós-falha? A alteração do feed também ativação pós-falha? A alteração do feed ainda aparecia contígua ou alteraria o feed para repor a causa de ativação pós-falha?

Sim, a alteração feed irá funcionar em toda a operação de ativação pós-falha manual e irá ser contígua.

### <a name="how-long-change-feed-persist-the-changed-data-if-i-set-the-ttl-time-to-live-property-for-the-document-to--1"></a>Quanto o feed de alteração manter os dados alterados se definir a propriedade o TTL (TTL de) para o documento como -1?

Alteração feed serão mantidas indefinidamente. Se não for eliminados dados, irá permanecer no feed de alteração.

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region-as-change-feed-is-available-in-all-the-read-regions-by-default"></a>Como configurar as funções do Azure para ler a partir de uma determinada região, tal como feed de alteração está disponível em todas as regiões leitura por predefinição?

Atualmente não é possível configurar as funções do Azure para ler a partir de uma determinada região. Não há um problema no GitHub no repositório das funções do Azure para definir as regiões preferenciais de quaisquer enlace de base de dados do Azure Cosmos e acionador.

As funções do Azure utiliza a política de ligação predefinido. Pode configurar o modo de ligação nas funções do Azure e por predefinição, é lida da região de escrita, pelo que é melhor localizar conjuntamente as funções do Azure na mesma região.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>O que é o tamanho predefinido de lotes em funções do Azure?

100 documentos em cada invocação das funções do Azure. No entanto, este número é configurável dentro do ficheiro function.json. Aqui está concluída [lista de opções de configuração](../azure-functions/functions-run-local.md). Se estiver a desenvolver localmente, atualizar as definições da aplicação dentro de [local.settings.json](../azure-functions/functions-run-local.md) ficheiro.

### <a name="i-am-monitoring-a-collection-and-reading-its-change-feed-however-i-see-i-am-not-getting-all-the-inserted-document-some-documents-are-missing-what-is-going-on-here"></a>Posso estou uma recolha de monitorização e ler as respetivas alterações feed, no entanto vejo não estou a obter o documento inserido, alguns documentos estão em falta. Que está a suceder aqui?

Certifique-se que não há nenhuma outra função ao ler a mesma coleção com a mesma coleção de concessão. Ocorreram-me e mais tarde posso realizados que os documentos em falta são processados pelo meu outras funções do Azure, que também está a utilizar o mesmo período de concessão.

Por conseguinte, se estiver a criar várias funções do Azure para ler o mesmo alterar feed, em seguida, tem de utilizar a coleção de concessão diferentes ou utilizar a configuração de "leasePrefix" para partilhar a mesma coleção. No entanto, quando utilizar a biblioteca de processador de feed de alterações pode iniciar múltiplas instâncias da sua função e SDK irá dividir os documentos entre instâncias diferentes automaticamente para si.

### <a name="my-document-is-updated-every-second-and-i-am-not-getting-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Os meus documentos é atualizado a cada segundo e posso estou não a obter todas as alterações nas funções do Azure à escuta para alterar o feed.

Alteração de inquéritos de funções do Azure feed para cada 5 segundos, para que as alterações feitas entre 5 segundos serão perdidas. BD do Azure do Cosmos armazena apenas uma versão para a cada cinco segundos, pelo que irá obter a alteração 5th no documento. No entanto, se pretender aceder abaixo de 5 segundos e pretende consultar a cada segundo do Feed de alteração, pode configurar a hora de consulta "feedPollTime", consulte [enlaces de base de dados do Azure Cosmos](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). Está definido em milissegundos com uma predefinição de 5000. Abaixo 1 segundo é possível mas não recomendado, irá começar a gravar mais CPU.

### <a name="i-inserted-a-document-in-the-mongo-api-collection-but-when-i-get-the-document-in-change-feed-it-shows-a-different-id-value-what-is-wrong-here"></a>Posso inserir um documento na coleção de API do Mongo, mas quando posso obter o documento no feed de alteração, mostra um valor de id diferentes. O que é incorreto em aqui?

A coleção é coleção do Mongo API. Lembre-se de feed de alteração de leitura com o cliente do SQL Server e serializa itens em formato JSON. Devido ao JSON de formatação, MongoDB, os clientes irão sentir um erro de correspondência entre documentos BSON formatado e o JSON com formato alteração feed. Está a ver é a representação de um documento BSON no JSON. Se utilizar atributos binários em contas do Mongo, estes são convertidos em JSON.

### <a name="is-there-a-way-to-control-change-feed-for-updates-only-and-not-inserts"></a>Existe alguma forma para controlar o feed apenas para atualizações de alteração e insere não?

Não hoje em dia, mas esta funcionalidade está no plano. Atualmente, pode adicionar um marcador de forma recuperável no documento para atualizações.

### <a name="is-there-a-way-to-get-deletes-in-change-feed"></a>Existe alguma forma para obter as eliminações no feed de alteração?

Atualmente alteração feed não inicie sessão eliminações. O feed de alteração está melhorar continuamente, não sendo esta funcionalidade no plano. Atualmente, pode adicionar um marcador de forma recuperável no documento para eliminação. Adicione um atributo no documento chamado "eliminado" e defina-o para "true" e defina um valor de TTL no documento que pode ser eliminado automaticamente.

### <a name="can-i-read-change-feed-for-historic-documentsfor-example-documents-that-were-added-5-years-back-"></a>Pode ler alteração feed para documentos históricos (por exemplo, documentos que foram adicionados 5 anos anterior)?

Sim, se o documento não é eliminado pode ler a alteração do feed as far as a origem da sua coleção.

### <a name="can-i-read-change-feed-using-javascript"></a>Pode ler feed de alteração com JavaScript?

Sim, recentemente é adicionado suporte inicial do Node.js SDK para o feed de alteração. Podem ser utilizado como mostrado no exemplo seguinte, em. módulo documentdb de atualização para a versão atual antes de executar o código:

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
