---
title: Sugestões de desempenho do Cosmos BD do Azure para Async Java | Microsoft Docs
description: Saiba mais opções de configuração de cliente para melhorar o desempenho da base de dados de base de dados do Azure Cosmos
keywords: como melhorar o desempenho de base de dados
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: sngun
ms.openlocfilehash: e3ee75a07f19fef50d9aca61773bd7ea860f2ca4
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102475"
---
> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 
> 

# <a name="performance-tips-for-azure-cosmos-db-and-async-java"></a>Sugestões de desempenho para a base de dados do Azure Cosmos e Async Java
BD do Cosmos do Azure é um rápido e flexível base de dados distribuída que dimensiona de forma totalmente integrada com latência garantida e débito. Não é necessário efetuar alterações de arquitetura de principais ou escrever código complexo para dimensionar a base de dados com o Azure Cosmos DB. Aumentar e reduzir verticalmente é tão fácil como realização de uma única chamada de API ou chamada de método SDK. No entanto, porque a base de dados do Azure Cosmos é acedida através de chamadas de rede existem otimizações de lado do cliente que pode efetuar para alcançar o pico do desempenho ao utilizar o [SQL Async Java SDK](sql-api-sdk-async-java.md).

Para que o se estiver a pedir "como posso melhorar o meu desempenho de base de dados?" Considere as seguintes opções:

## <a name="networking"></a>Redes
   <a id="same-region"></a>
1. **Colocar os clientes na mesma região do Azure para desempenho**

    Sempre que possível, coloque todas as aplicações a chamar base de dados do Azure Cosmos na mesma região que a base de dados do Azure Cosmos DB. Para uma comparação aproximada, as chamadas à base de dados do Azure Cosmos na mesma região concluída dentro do ms 1-2, mas a latência entre o oeste e da Costa Leste dos E.U.A. é > 50 ms. Esta latência pode variar provavelmente de um pedido para o pedido, consoante a rota colocada por pedido, conforme tenha sido enviado a partir do cliente para os limites de datacenter do Azure. A menor latência possível é conseguida ao garantir que a aplicação de chamada está localizada na mesma região do Azure como o ponto final da BD do Cosmos Azure aprovisionado. Para obter uma lista de regiões disponíveis, consulte [regiões do Azure](https://azure.microsoft.com/regions/#services).

    ![Ilustração da política de ligação de base de dados do Azure Cosmos](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>Utilização do SDK
1. **Instalar o SDK mais recente**

    Os SDKs do Azure Cosmos DB estão constantemente a ser melhorados para proporcionar o melhor desempenho. Consulte o [Azure Cosmos DB SDK](sql-api-sdk-async-java.md) páginas para determinar o SDK mais recente e reveja melhoramentos.
2. **Utilizar um cliente de base de dados do Azure Cosmos singleton para a duração da sua aplicação**

    Cada instância de AsyncDocumentClient é seguro para thread e efetua a gestão de ligações eficiente e a colocação em cache do endereço. Para permitir a gestão de ligações eficiente e melhor desempenho por AsyncDocumentClient, é recomendado utilizar uma única instância AsyncDocumentClient por AppDomain para a duração da aplicação.

   <a id="max-connection"></a>

3. **Otimização ConnectionPolicy**

    BD do Azure do Cosmos pedidos são efetuados através de HTTPS/REST ao utilizar o SDK de Java Async e estão sujeitos ao tamanho predefinido do conjunto de ligação máximo (1000). Este valor predefinido deve ser o ideal para a maioria dos casos de utilização. No entanto, no caso de ter uma coleção grande com várias partições, que pode definir o tamanho do conjunto de ligação máximo para um grande número (indiquem, 1500) utilizando setMaxPoolSize.

4. **Otimizar as consultas paralelas para coleções particionadas**

    Azure Cosmos BD SQL Async Java SDK suporta consultas paralelas, que lhe permite consultar uma coleção particionada em paralelo (consulte [trabalhar com os SDKs](sql-api-partition-data.md#working-with-the-azure-cosmos-db-sdks) o relacionados e [exemplos de código](https://github.com/Azure/azure-cosmosdb-java/tree/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples) para obter mais informações). Consultas paralelas são concebidas para melhorar a latência de consulta e débito ao longo do respetivo homólogo série.

    (a) ***otimização setMaxDegreeOfParallelism\:***  paralelo consulta trabalho consultando várias partições em paralelo. No entanto, os dados a partir de uma coleção particionada individuais são obtidos serialmente no que respeita à consulta. Por isso, utilize setMaxDegreeOfParallelism para definir o número de partições que tem a oportunidade de máxima de alcançar a maioria dos consulta performant, fornecida todas as outras condições de sistema permanecer o mesmo. Se não souber o número de partições, pode utilizar setMaxDegreeOfParallelism para definir um elevado número e o sistema escolhe o mínimo (número de partições, entrada fornecido pelo utilizador) como o grau de paralelismo máximo. 

    É importante ter em atenção as consultas paralelas produzem as vantagens de melhor, se os dados é distribuída uniformemente pelos todas as partições no que respeita à consulta. Se a coleção particionada está particionada uma forma que todos os ou a maioria dos dados devolvidos por uma consulta é concentrated em partições uns (uma partição na pior das hipóteses), em seguida, o desempenho da consulta deverá ser condicionada por essas partições apoio.

    (b) ***otimização setMaxBufferedItemCount\:***  paralela da consulta foi concebida para previamente obtenha os resultados enquanto o lote de resultados atual está a ser processado pelo cliente. A pré-obtenção de ajuda na melhoria geral de latência de uma consulta. setMaxBufferedItemCount limita o número de resultados de pré-obtidos. Definição setMaxBufferedItemCount para o número esperado de resultados devolvidos (ou um número mais alto) permite a consulta para recebem benefícios máximo da pré-a obter.

    Pré-obter funciona da mesma forma, independentemente de MaxDegreeOfParallelism e existe uma única memória intermédia de dados de todas as partições.  

5. **Implementar o término intervalos getRetryAfterInMilliseconds**

    Durante os testes de desempenho, deve aumentar a carga até uma pequena taxa de pedidos de obter limitadas. Se limitadas, a aplicação cliente deve término para o intervalo entre tentativas de servidor especificado. Respecting o término garante que passam a quantidade mínima de espera de tempo entre tentativas. Para obter mais informações, consulte [Exceeding reservado limites de débito](request-units.md#RequestRateTooLarge) e DocumentClientException.getRetryAfterInMilliseconds.
6. **Aumentar horizontalmente a cliente carga de trabalho**

    Se estiver a testar em níveis de débito elevado (> 50.000 RU/s), a aplicação cliente pode tornar-se a engarrafamento devido a máquina capping limite na utilização de CPU ou à rede. Se atingir este ponto, pode continuar a emitir a conta de base de dados do Azure Cosmos ainda mais ao aumentar horizontalmente as aplicações de cliente por vários servidores.

7. **Utilize o endereçamento de nome com base**

    Utilizar baseada no nome de endereçamento, onde ligações têm o formato `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, em vez de SelfLinks (\_Self-), que tem o formato `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` para evitar a obter ResourceIds de todos os recursos utilizados para construir a ligação. Além disso, como estes recursos obterem recriados (possivelmente com o mesmo nome), a colocação em cache-las poderá ajuda.

   <a id="tune-page-size"></a>
8. **Ajustar o tamanho da página de feeds de consultas/leitura para um melhor desempenho**

    Quando efetuar uma em massa de leitura de documentos utilizando leitura feed funcionalidade (por exemplo, readDocuments) ou ao emitir uma consulta de SQL Server, os resultados são devolvidos de uma forma segmentada se o conjunto de resultados for demasiado grande. Por predefinição, os resultados são devolvidos em segmentos de 100 itens ou de 1 MB, consoante o limite é atingido primeiro.

    Para reduzir o número de rede arredondar viagens necessárias para obter resultados todos aplicáveis, pode aumentar o tamanho de página utilizando a [x-ms-máx.--número de itens](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) cabeçalho do pedido a até 1000. Nos casos em que seja necessário apresentar apenas alguns resultados, por exemplo, se a API de aplicação ou interface de utilizador devolve apenas os 10 resultados de uma hora, também pode diminuir o tamanho da página para 10 para reduzir o débito consumido para leituras e consultas.

    Também pode definir o tamanho de página utilizando o método setMaxItemCount.
    
9. **Utilizar o programador adequado (evite roubarem os threads Netty de e/s do ciclo de eventos)**

    Utiliza o SDK de Java Async [netty](https://netty.io/) para e/s não bloquear. O SDK utiliza um número fixo de threads de ciclo de eventos netty de e/s (tantos núcleos de CPU sua máquina ter) para executar operações de e/s. Observable devolvido pela API emite o resultado dos partilhado e/s eventos ciclo netty threads. Por isso, é importante bloquear não partilhados e/s eventos ciclo netty threads. Efetuar o trabalho intensiva de CPU ou bloquear a operação no thread de netty do ciclo de eventos de e/s pode causar um impasse ou reduzir significativamente o débito SDK.

    Por exemplo o código seguinte executa um trabalho exigente em termos de cpu num thread netty de e/s no ciclo de eventos:

    ```java
    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribe(
      resourceResponse -> {
        //this is executed on eventloop IO netty thread.
        //the eventloop thread is shared and is meant to return back quickly.
        //
        // DON'T do this on eventloop IO netty thread.
        veryCpuIntensiveWork();
      });
    ```

    Depois do resultado é recebido, se pretender fazer consumo intensivo da CPU de trabalho no resultado deve evitar fazer thread netty de e/s do ciclo de eventos de outros. Em vez disso, pode fornecer o suas próprias programador para fornecer a suas próprias thread para executar o seu trabalho.

    ```java
    import rx.schedulers;

    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribeOn(Schedulers.computation())
    subscribe(
      resourceResponse -> {
        // this is executed on threads provided by Scheduler.computation()
        // Schedulers.computation() should be used only when:
        //   1. The work is cpu intensive 
        //   2. You are not doing blocking IO, thread sleep, etc. in this thread against other resources.
        veryCpuIntensiveWork();
      });
    ```

    Consoante o tipo do seu trabalho deve utilizar o Programador de RxJava existente adequado para o seu trabalho. Ler aqui [ ``Schedulers`` ](http://reactivex.io/RxJava/1.x/javadoc/rx/schedulers/Schedulers.html).

    Para mais informações, consulte o [página do Github](https://github.com/Azure/azure-cosmosdb-java) para Async SDK de Java.

10. **Desativar o registo do netty** registo da biblioteca Netty é chatty e tem de ser desativada (suprimir o início de sessão a configuração pode não ser suficiente) para evitar custos adicionais de CPU. Se não estiver no modo de depuração, netty de Desativação registo completamente. Por isso, se estiver a utilizar log4j para remover os custos de CPU adicionais gasta pelas ``org.apache.log4j.Category.callAppenders()`` de netty de adicionar a seguinte linha à sua base de código:

    ```java
    org.apache.log4j.Logger.getLogger("io.netty").setLevel(org.apache.log4j.Level.OFF);
    ```

11. **Abra OS ficheiros de limite de recursos** sistemas Linux algumas (como Red Hat) tem um limite superior no número de abrir ficheiros e, para o número total de ligações. Execute o seguinte para ver os limites atuais:

    ```bash
    ulimit -a
    ```

    O número de ficheiros abertos (nofile) tem de ser suficientemente grande para ter espaço suficiente para o tamanho do agrupamento de ligação configurados e outros ficheiros abertos pelo sistema operativo. Pode ser modificado para permitir um maior tamanho de conjunto de ligação.

    Abra o ficheiro de limits.conf:

    ```bash
    vim /etc/security/limits.conf
    ```
    
    Adicionar/modificar o as seguintes linhas:

    ```
    * - nofile 100000
    ```

12. **Utilizar a implementação nativa do SSL para netty** Netty pode utilizar OpenSSL diretamente para a pilha de implementação de SSL para melhorar o desempenho. Na ausência de isto netty de configuração irá reverter para a implementação de SSL predefinida do Java.

    no Ubuntu:
    ```bash
    sudo apt-get install openssl
    sudo apt-get install libapr1
    ```

    e adicione a seguinte dependência para as suas dependências do projeto maven:
    ```xml
    <dependency>
      <groupId>io.netty</groupId>
      <artifactId>netty-tcnative</artifactId>
      <version>2.0.7.Final</version>
      <classifier>linux-x86_64</classifier>
    </dependency>
    ```

Para outras plataformas (Red Hat, Windows, Mac, etc.), consulte a estas instruções https://netty.io/wiki/forked-tomcat-native.html

## <a name="indexing-policy"></a>Política de Indexação
 
1. **Excluir os caminhos de indexação para escritas mais rápidas**

    Política de indexação da BD Cosmos do Azure permite-lhe especificar os caminhos do documento para incluir ou excluir da indexação tirando partido dos caminhos de indexação (setIncludedPaths e setExcludedPaths). A utilização de indexação caminhos pode oferecer desempenho melhorado de escrita e de armazenamento de índice inferior para cenários em que os padrões de consulta são conhecidos com antecedência, os custos de indexação diretamente são correlacionados com o número exclusivos caminhos indexados.  Por exemplo, o código seguinte mostra como excluir uma secção completa dos documentos (a.k.a. uma subárvore) de indexação a utilizar o "*" caráter universal.

    ```Java
    Index numberIndex = Index.Range(DataType.Number);
    numberIndex.set("precision", -1);
    indexes.add(numberIndex);
    includedPath.setIndexes(indexes);
    includedPaths.add(includedPath);
    indexingPolicy.setIncludedPaths(includedPaths);
    collectionDefinition.setIndexingPolicy(indexingPolicy);
    ```

    Para obter mais informações, consulte [Azure Cosmos DB indexação políticas](indexing-policies.md).

## <a name="throughput"></a>Débito
<a id="measure-rus"></a>

1. **Medir e otimizar o pedido inferior unidades por segundo utilização**

    BD do Azure do Cosmos oferece um vasto conjunto de operações de base de dados, incluindo consultas relacionais e hierárquicas com tudo funcionar em documentos de uma coleção de base de dados – acionadores, UDFs e procedimentos armazenados. Os custos associados a cada uma destas operações variam consoante a CPU, a e/s e a memória necessária para concluir a operação. Em vez de ter em consideração e a gerir recursos de hardware, pode considerar uma unidade de pedido (RU) como uma medida único para os recursos necessários para desempenhar várias operações de base de dados e um pedido de aplicação de serviço.

    Débito aprovisionado com base no número de [unidades de pedido](request-units.md) definido para cada contentor. Consumo de unidade de pedido é avaliado como uma taxa por segundo. As aplicações que excedem a taxa de unidade de pedido aprovisionado para as respetivas contentor estão limitadas até que a taxa de ignora abaixo do nível de aprovisionamento para o contentor. Se a sua aplicação requer um nível mais elevado de débito, pode aumentar o débito ao aprovisionamento unidades de pedido adicionais. 

    A complexidade de uma consulta afeta o número de unidades de pedido são consumidas para uma operação. O número de predicados, natureza os predicados, número de UDFs e o tamanho do conjunto de dados de origem todos os influenciar o custo das operações de consulta.

    Para medir a sobrecarga de qualquer operação (criar, atualizar ou eliminar), Inspecione o [x-ms-pedido-encargos](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) cabeçalho para medir o número de unidades de pedido consumidos por estas operações. Também pode ver a propriedade RequestCharge equivalente no ResourceResponse<T> ou FeedResponse<T>.

    ```Java
    ResourceResponse<Document> response = asyncClient.createDocument(collectionLink, documentDefinition, null,
                                                     false).toBlocking.single();
    response.getRequestCharge();
    ```             

    A taxa de pedido devolvida neste cabeçalho é fração do seu débito aprovisionado. Por exemplo, se tiver 2000 RU/s aprovisionada e se a consulta anterior devolve 1000 1KB-documentos, o custo da operação é 1000. Como tal, dentro de um segundo, o servidor honra apenas dois esses pedidos antes de taxa de limitação de pedidos subsequentes. Para obter mais informações, consulte [unidades de pedido](request-units.md) e [Calculadora de unidade de pedido](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Taxa de pedidos/limitação de taxa de identificador demasiado grande**

    Quando um cliente tenta exceder o débito reservado para uma conta, há sem degradação do desempenho no servidor e sem utilização de capacidade de débito para além do nível reservado. O servidor irá preventivamente terminar o pedido com RequestRateTooLarge (código de estado HTTP 429) e devolver o [x-ms-repetição-após-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) cabeçalho que indica a quantidade de tempo, em milissegundos, que o utilizador terá de aguardar antes de reattempting o pedido.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    Os SDKs implicitamente todos os catch esta resposta, respeitem o servidor especificado depois de repetir cabeçalho e repita o pedido. A menos que a sua conta está a ser acedida em simultâneo por vários clientes, a tentativa seguinte ocorrerá será bem sucedida.

    Se tiver mais do que um cliente cumulativamente e operativo consistentemente acima a taxa de pedidos, a contagem de repetições de predefinição atualmente definida para 9 internamente pelo cliente não poderá suffice; Neste caso, o cliente emite um DocumentClientException com o código de estado 429 à aplicação. A contagem de repetições predefinido pode ser alterada utilizando setRetryOptions na instância ConnectionPolicy. Por predefinição, o DocumentClientException com o código de estado 429 é devolvido após um tempo cumulativo de espera de 30 segundos, se o pedido continua a funcionar acima a taxa de pedidos. Isto ocorre mesmo quando o número de tentativas atual é menor que o número máximo de tentativas, seja a predefinição de 9 ou um valor definido pelo utilizador.

    Enquanto o comportamento de repetição automatizada ajuda a melhorar a resiliência e a facilidade de utilização para a maioria das aplicações, poderá ser at odds ao efetuar testes de desempenho, especialmente quando medir a latência.  A latência observados de cliente será aumentam se a experimentação pedidos com êxito a limitação de servidor e faz com que o cliente SDK para repetir automaticamente. Para evitar picos de latência durante experimentações de desempenho, medir a taxa devolvida por cada operação e certifique-se de que pedidos estão a funcionar abaixo a taxa de pedidos reservado. Para obter mais informações, consulte [unidades de pedido](request-units.md).
3. **Conceção para documentos mais pequenos para maior débito**

    A taxa de pedido (o custo de processamento de pedidos) de uma operação fornecida é diretamente correlacionada com o tamanho do documento. Operações de grande documentos custos mais de operações para documentos pequenos.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre como criar a sua aplicação para o dimensionamento e elevado desempenho, consulte [divisão em partições e o dimensionamento do BD Azure Cosmos](partition-data.md).
