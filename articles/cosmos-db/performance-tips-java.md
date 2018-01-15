---
title: "Sugestões de desempenho do Cosmos BD do Azure para Java | Microsoft Docs"
description: "Saiba mais opções de configuração de cliente para melhorar o desempenho da base de dados de base de dados do Azure Cosmos"
keywords: como melhorar o desempenho de base de dados
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: dfe8f426-3c98-4edc-8094-092d41f2795e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: mimig
ms.openlocfilehash: 4d7657d305332cc0014187d52396ae3af4818d5e
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
> [!div class="op_single_selector"]
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 
> 

# <a name="performance-tips-for-azure-cosmos-db-and-java"></a>Sugestões de desempenho para a base de dados do Azure Cosmos e o Java
BD do Cosmos do Azure é um rápido e flexível base de dados distribuída que dimensiona de forma totalmente integrada com latência garantida e débito. Não é necessário efetuar alterações de arquitetura de principais ou escrever código complexo para dimensionar a base de dados com o Azure Cosmos DB. Aumentar e reduzir verticalmente é tão fácil como fazer uma única chamada API ou [a chamada de método SDK](set-throughput.md#set-throughput-java). No entanto, porque a base de dados do Azure Cosmos é acedida através de chamadas de rede existem otimizações de lado do cliente que pode efetuar para alcançar o pico do desempenho ao utilizar o [SQL Java SDK](documentdb-sdk-java.md).

Para que o se estiver a pedir "como posso melhorar o meu desempenho de base de dados?" Considere as seguintes opções:

## <a name="networking"></a>Redes
<a id="direct-connection"></a>

1. **Modo de ligação: DirectHttps de utilização**

    Como um cliente liga à base de dados do Azure Cosmos tem implicações importantes no desempenho, especialmente em termos de latência observado do lado do cliente. Há uma definição disponível para configurar o cliente de configuração chave [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy) – o [ConnectionMode](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode).  As duas ConnectionModes disponíveis são:

   1. [Gateway (predefinição)](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode.gateway)
   2. [DirectHttps](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode.directhttps)

    O modo de gateway é suportado em todas as plataformas do SDK e é a predefinição configurada.  Se a aplicação é executado dentro de uma rede empresarial com restrições de strict firewall, o Gateway é a melhor opção uma vez que utiliza a porta HTTPS padrão e um único ponto final. O compromisso de desempenho, no entanto, é o modo de Gateway envolve um salto de rede adicionais, sempre que os dados são lidos ou escritos na base de dados do Azure Cosmos. Por este motivo, o modo de DirectHttps oferece um melhor desempenho devido a menos saltos de rede. 

    O SDK de Java utiliza HTTPS como um protocolo de transporte. O HTTPS utiliza SSL para autenticação inicial e a encriptação de tráfego. Ao utilizar o SDK de Java, apenas porta HTTPS 443 tem de ser aberta. 

    O ConnectionMode é configurado durante a construção da instância com o parâmetro ConnectionPolicy DocumentClient. 

    ```Java
    public ConnectionPolicy getConnectionPolicy() {
        ConnectionPolicy policy = new ConnectionPolicy();
        policy.setConnectionMode(ConnectionMode.DirectHttps);
        policy.setMaxPoolSize(1000);
        return policy;
    }
        
    ConnectionPolicy connectionPolicy = new ConnectionPolicy();
    DocumentClient client = new DocumentClient(HOST, MASTER_KEY, connectionPolicy, null);
    ```

    ![Ilustração da política de ligação de base de dados do Azure Cosmos](./media/performance-tips-java/connection-policy.png)

   <a id="same-region"></a>
2. **Colocar os clientes na mesma região do Azure para desempenho**

    Sempre que possível, coloque todas as aplicações a chamar base de dados do Azure Cosmos na mesma região que a base de dados do Azure Cosmos DB. Para uma comparação aproximada, as chamadas à base de dados do Azure Cosmos na mesma região concluída dentro do ms 1-2, mas a latência entre o oeste e da Costa Leste dos E.U.A. é > 50 ms. Esta latência pode variar provavelmente de um pedido para o pedido, consoante a rota colocada por pedido, conforme tenha sido enviado a partir do cliente para os limites de datacenter do Azure. A menor latência possível é conseguida ao garantir que a aplicação de chamada está localizada na mesma região do Azure como o ponto final da BD do Cosmos Azure aprovisionado. Para obter uma lista de regiões disponíveis, consulte [regiões do Azure](https://azure.microsoft.com/regions/#services).

    ![Ilustração da política de ligação de base de dados do Azure Cosmos](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>Utilização do SDK
1. **Instalar o SDK mais recente**

    Os SDKs do Azure Cosmos DB estão constantemente a ser melhorados para proporcionar o melhor desempenho. Consulte o [Azure Cosmos DB SDK](documentdb-sdk-java.md) páginas para determinar o SDK mais recente e reveja melhoramentos.
2. **Utilizar um cliente de base de dados do Azure Cosmos singleton para a duração da sua aplicação**

    Cada [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client) instância é seguro para thread e efetua a gestão de ligações eficiente e endereço a colocação em cache quando funcionar no modo direto. Para permitir a gestão de ligações eficiente e melhor desempenho por DocumentClient, é recomendado utilizar uma única instância DocumentClient por AppDomain para a duração da aplicação.

   <a id="max-connection"></a>
3. **Aumentar MaxPoolSize por anfitrião, ao utilizar o modo de Gateway**

    BD do Azure do Cosmos pedidos são efetuados através de HTTPS/REST quando utilizar o modo de Gateway e estão sujeitos ao limite de ligação predefinido por nome de anfitrião ou endereço IP. Terá de definir o MaxPoolSize para um valor mais alto (200-1000) para que a biblioteca de clientes pode utilizar várias ligações simultâneas a BD do Cosmos do Azure. O SDK de Java, o valor predefinido para [ConnectionPolicy.getMaxPoolSize](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.gsetmaxpoolsize) é 100. Utilize [setMaxPoolSize]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.setmaxpoolsize) para alterar o valor.

4. **Otimizar as consultas paralelas para coleções particionadas**

    Azure Cosmos BD SQL Java SDK versão 1.9.0 e acima consultas paralelas de suporte, o que lhe permite consultar uma coleção particionada em paralelo (consulte [trabalhar com os SDKs](sql-api-partition-data.md#working-with-the-azure-cosmos-db-sdks) o relacionados e [exemplos de código](https://github.com/Azure/azure-documentdb-java/tree/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples) para mais informações). Consultas paralelas são concebidas para melhorar a latência de consulta e débito ao longo do respetivo homólogo série.

    (a) ***otimização setMaxDegreeOfParallelism\:***  paralelo consulta trabalho consultando várias partições em paralelo. No entanto, os dados a partir de uma coleção particionada individuais são obtidos serialmente no que respeita à consulta. Por isso, utilize [setMaxDegreeOfParallelism](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options.setmaxdegreeofparallelism) para definir o número de partições que tem a oportunidade de máxima de alcançar mais performant consulta, fornecido todas as outras condições de sistema permanecerem o mesmo. Se não souber o número de partições, pode utilizar setMaxDegreeOfParallelism para definir um elevado número e o sistema escolhe o mínimo (número de partições, entrada fornecido pelo utilizador) como o grau de paralelismo máximo. 

    É importante ter em atenção as consultas paralelas produzem as vantagens de melhor, se os dados é distribuída uniformemente pelos todas as partições no que respeita à consulta. Se a coleção particionada está particionada uma forma que todos os ou a maioria dos dados devolvidos por uma consulta é concentrated em partições uns (uma partição na pior das hipóteses), em seguida, o desempenho da consulta deverá ser condicionada por essas partições apoio.

    (b) ***otimização setMaxBufferedItemCount\:***  paralela da consulta foi concebida para previamente obtenha os resultados enquanto o lote de resultados atual está a ser processado pelo cliente. A pré-obtenção de ajuda na melhoria geral de latência de uma consulta. setMaxBufferedItemCount limita o número de resultados de pré-obtidos. Por definição [setMaxBufferedItemCount](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options.setmaxbuffereditemcount) para o número esperado de resultados devolvidos (ou um número mais alto), isto permite que a consulta para recebem benefícios máximo da pré-a obter.

    Pré-obter funciona da mesma forma, independentemente de MaxDegreeOfParallelism e existe uma única memória intermédia de dados de todas as partições.  

5. **Implementar o término intervalos getRetryAfterInMilliseconds**

    Durante os testes de desempenho, deve aumentar a carga até uma pequena taxa de pedidos de obter limitadas. Se limitadas, a aplicação cliente deve término no limitação para o intervalo entre tentativas de servidor especificado. Respecting o término garante que passam a quantidade mínima de espera de tempo entre tentativas. Suporte de política de repetição está incluído na versão 1.8.0 e acima do [Java SDK](documentdb-sdk-java.md). Para obter mais informações, consulte [Exceeding reservado limites de débito](request-units.md#RequestRateTooLarge) e [getRetryAfterInMilliseconds](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client_exception.getretryafterinmilliseconds).
6. **Aumentar horizontalmente a cliente carga de trabalho**

    Se estiver a testar em níveis de débito elevado (> 50.000 RU/s), a aplicação cliente pode tornar-se a engarrafamento devido a máquina capping limite na utilização de CPU ou à rede. Se atingir este ponto, pode continuar a emitir a conta de base de dados do Azure Cosmos ainda mais ao aumentar horizontalmente as aplicações de cliente por vários servidores.

7. **Utilize o endereçamento de nome com base**

    Utilizar baseada no nome de endereçamento, onde ligações têm o formato `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, em vez de SelfLinks ( Self), que tem o formato `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` para evitar a obter ResourceIds de todos os recursos utilizados para construir a ligação. Além disso, como estes recursos obterem recriados (possivelmente com o mesmo nome), a colocação em cache estas poderão ajuda.

   <a id="tune-page-size"></a>
8. **Ajustar o tamanho da página de feeds de consultas/leitura para um melhor desempenho**

    Quando efetuar uma em massa de leitura de documentos utilizando leitura feed funcionalidade (por exemplo, [readDocuments]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.readdocuments#com_microsoft_azure_documentdb__document_client_readDocuments_String_FeedOptions_c) ou ao emitir uma consulta de SQL Server, os resultados são devolvidos de uma forma segmentada se o conjunto de resultados for demasiado grande. Por predefinição, os resultados são devolvidos em segmentos de 100 itens ou de 1 MB, consoante o limite é atingido primeiro.

    Para reduzir o número de rede arredondar viagens necessárias para obter resultados todos aplicáveis, pode aumentar o tamanho de página utilizando a [x-ms-máx.--número de itens](https://docs.microsoft.com/rest/api/documentdb/common-documentdb-rest-request-headers) cabeçalho do pedido a até 1000. Nos casos em que seja necessário apresentar apenas alguns resultados, por exemplo, se a API de aplicação ou interface de utilizador devolve apenas os 10 resultados de uma hora, também pode diminuir o tamanho da página para 10 para reduzir o débito consumido para leituras e consultas.

    Também pode definir o tamanho de página utilizando ao [setPageSize método](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options_base.setpagesize#com_microsoft_azure_documentdb__feed_options_base_setPageSize_Integer).

## <a name="indexing-policy"></a>Política de Indexação
 
1. **Excluir os caminhos de indexação para escritas mais rápidas**

    Política de indexação da BD Cosmos do Azure permite-lhe especificar os caminhos de documentos para incluir ou excluir da indexação tirando partido dos caminhos de indexação ([setIncludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._indexing_policy.setincludedpaths) e [setExcludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._indexing_policy.setexcludedpaths)). A utilização de indexação caminhos pode oferecer desempenho melhorado de escrita e de armazenamento de índice inferior para cenários em que os padrões de consulta são conhecidos com antecedência, os custos de indexação diretamente são correlacionados com o número exclusivos caminhos indexados.  Por exemplo, o código seguinte mostra como excluir uma secção completa dos documentos (a.k.a. uma subárvore) de indexação a utilizar o "*" caráter universal.

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

    Para medir a sobrecarga de qualquer operação (criar, atualizar ou eliminar), Inspecione o [x-ms-pedido-encargos](https://docs.microsoft.com/rest/api/documentdb/common-documentdb-rest-response-headers) cabeçalho (ou a propriedade RequestCharge equivalente no [ResourceResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._resource_response) ou [FeedResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_response) para medir o número de unidades de pedido consumidos por estas operações.

    ```Java
    ResourceResponse<Document> response = client.createDocument(collectionLink, documentDefinition, null, false);

    response.getRequestCharge();
    ```             

    A taxa de pedido devolvida neste cabeçalho é fração do seu débito aprovisionado. Por exemplo, se tiver 2000 RU/s aprovisionada e se a consulta anterior devolve 1000 1KB-documentos, o custo da operação é 1000. Como tal, dentro de um segundo, o servidor honra apenas dois esses pedidos antes dos pedidos subsequentes de limitação. Para obter mais informações, consulte [unidades de pedido](request-units.md) e [Calculadora de unidade de pedido](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Taxa de pedidos/limitação de taxa de identificador demasiado grande**

    Quando um cliente tenta exceder o débito reservado para uma conta, há sem degradação do desempenho no servidor e sem utilização de capacidade de débito para além do nível reservado. O servidor irá preventivamente terminar o pedido com RequestRateTooLarge (código de estado HTTP 429) e devolver o [x-ms-repetição-após-ms](https://docs.microsoft.com/rest/api/documentdb/common-documentdb-rest-response-headers) cabeçalho que indica a quantidade de tempo, em milissegundos, que o utilizador terá de aguardar antes de reattempting o pedido.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    Os SDKs implicitamente todos os catch esta resposta, respeitem o servidor especificado depois de repetir cabeçalho e repita o pedido. A menos que a sua conta está a ser acedida em simultâneo por vários clientes, a tentativa seguinte ocorrerá será bem sucedida.

    Se tiver mais do que um cliente cumulativamente e operativo consistentemente acima a taxa de pedidos, a contagem de repetições de predefinição atualmente definida para 9 internamente pelo cliente não poderá suffice; Neste caso, o cliente emite um [DocumentClientException](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client_exception) com o estado de código 429 à aplicação. A contagem de repetições predefinido pode ser alterada utilizando [setRetryOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.setretryoptions) no [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy) instância. Por predefinição, o DocumentClientException com o código de estado 429 é devolvido após um tempo cumulativo de espera de 30 segundos, se o pedido continua a funcionar acima a taxa de pedidos. Isto ocorre mesmo quando o número de tentativas atual é menor que o número máximo de tentativas, seja a predefinição de 9 ou um valor definido pelo utilizador.

    Enquanto o comportamento de repetição automatizada ajuda a melhorar a resiliência e a facilidade de utilização para a maioria das aplicações, poderá ser at odds ao efetuar testes de desempenho, especialmente quando medir a latência.  A latência observados de cliente será aumentam se a experimentação pedidos com êxito a limitação de servidor e faz com que o cliente SDK para repetir automaticamente. Para evitar picos de latência durante experimentações de desempenho, medir a taxa devolvida por cada operação e certifique-se de que pedidos estão a funcionar abaixo a taxa de pedidos reservado. Para obter mais informações, consulte [unidades de pedido](request-units.md).
3. **Conceção para documentos mais pequenos para maior débito**

    A taxa de pedido (o custo de processamento de pedidos) de uma operação fornecida é diretamente correlacionada com o tamanho do documento. Operações de grande documentos custos mais de operações para documentos pequenos.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre como criar a sua aplicação para o dimensionamento e elevado desempenho, consulte [divisão em partições e o dimensionamento do BD Azure Cosmos](partition-data.md).
