---
title: 'Azure Cosmos DB: API de processador do Feed do .NET alteração, SDK e recursos | Documentos da Microsoft'
description: Saiba tudo sobre a API de processador do Feed de alterações e o SDK, incluindo as datas de lançamento, datas de extinção e as alterações feitas entre cada versão do SDK do processador de .NET de Feed de alterações.
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/21/2018
ms.author: maquaran
ms.openlocfilehash: 553917a29b3564fff71d6ab994ec199891cbaae7
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409106"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>Processador de Feed de alterações de .NET SDK: Transferir e notas de versão
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Feed de alterações de .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Fornecedor de Recursos REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**Transferência de SDK**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**Documentação da API**|[Alterar a documentação de referência da API de biblioteca de processador do Feed](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Introdução**|[Introdução ao SDK de .NET de processador de Feed de alterações](change-feed.md)|
|**Estrutura de suporte atual**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Notas de versão

### <a name="v2-builds"></a>compilações de v2

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Foi adicionado suporte para coleções particionadas concessão. A chave de partição tem de ser definida como/ID.
* Alteração significativa secundárias: os métodos da interface do IChangeFeedDocumentClient e a classe de ChangeFeedDocumentClient foram alterados para incluir parâmetros de RequestOptions e CancellationToken. IChangeFeedDocumentClient é um ponto de extensibilidade avançada que permite-lhe fornecer uma implementação personalizada do cliente do documento para utilizar com o processador do Feed de alterações, por exemplo, para decorar o DocumentClient e interceptar todas as chamadas para ele fazer rastreamento extra, tratamento de erros , etc. Com esta atualização, o código que implementam IChangeFeedDocumentClient tem de ser alterada para incluir novos parâmetros na implementação.
* Melhorias de diagnóstico pequenas.


### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Adicionada a nova API, tarefa&lt;IReadOnlyList&lt;RemainingPartitionWork&gt; &gt; IRemainingWorkEstimator.GetEstimatedRemainingWorkPerPartitionAsync(). Isto pode ser utilizado para fazer o trabalho estimado para cada partição.
* Suporta Microsoft.Azure.DocumentDB SDK 2.0. Requer Microsoft.Azure.DocumentDB 2.0 ou posterior.

### <a name="a-name206206"></a><a name="2.0.6"/>2.0.6
* Foi adicionada ChangeFeedEventHost.HostName propriedade pública para compativility com v1.

### <a name="a-name205205"></a><a name="2.0.5"/>2.0.5
* Corrigido uma condição de corrida que ocorre durante a divisão de partição. A condição de corrida pode levar a adquirir concessão e perdê-lo durante a divisão de partição imediatamente e causar contenção. O problema de condição de corrida é resolvido com esta versão.

### <a name="a-name204204"></a><a name="2.0.4"/>2.0.4
* SDK DE DISPONIBILIDADE GERAL

### <a name="a-name203-prerelease203-prerelease"></a><a name="2.0.3-prerelease"/>2.0.3-prerelease
* Foram corrigidos os problemas seguintes:
  * Quando ocorre a divisão de partição, pode haver duplicado de processamento de documentos modificados antes da divisão.
  * A API de GetEstimatedRemainingWork devolveu 0 quando não existem concessões estavam presentes na coleção de concessão.

* As seguintes exceções são públicos. As extensões que implementam IPartitionProcessor podem emitir essas exceções.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="a-name202-prerelease202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-prerelease
* Pequenas alterações de API:
  * Removido ChangeFeedProcessorOptions.IsAutoCheckpointEnabled que foi marcado como obsoleto.

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-prerelease
* Melhorias de estabilidade:
  * Um manuseio de inicialização de arquivo de concessão. Quando o arquivo de concessão estiver vazio, apenas uma instância de processador pode inicializá-lo, irão esperar a outras pessoas.
  * Renovação de concessão stable/eficiente mais/release. Renovação e lançar uma partição de uma concessão são independente da renovação de outras pessoas. No v1, isso foi feito em seqüência para todas as partições.
* Nova API v2:
  * Padrão de construtor para construção flexível do processador: a classe ChangeFeedProcessorBuilder.
    * Pode demorar qualquer combinação de parâmetros.
    * Pode demorar a instância do DocumentClient para a recolha de monitorização e/ou concessão (não disponível no v1).
  * IChangeFeedObserver.ProcessChangesAsync agora demora CancellationToken.
  * IRemainingWorkEstimator - o avaliador de trabalho restantes pode ser utilizado em separado do processador.
  * Novos pontos de extensibilidade:
    * IParitionLoadBalancingStrategy - para personalizados-balanceamento de carga de partições entre instâncias do processador.
    * ILease, ILeaseManager - para a gestão da concessão personalizado.
    * IPartitionProcessor - para que as alterações do processamento personalizado numa partição.
* Registo - utiliza [LibLog](https://github.com/damianh/LibLog) biblioteca.
* 100% compatível com versões anteriores com a API de v1.
* Nova base de código.
* Compatível com [SDK de .NET de SQL](sql-api-sdk-dotnet.md) versões 1.21.1 e acima.

### <a name="v1-builds"></a>compilações de v1

### <a name="a-name133133"></a><a name="1.3.3"/>1.3.3
* Adicionar mais registo.
* Foi corrigido um vazamento de DocumentClient ao chamar a estimativa do trabalho pendente várias vezes.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* Correções a estimativa do trabalho pendente.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* Melhorias de estabilidade.
  * Correção para a manipulação de problema de cancelamento de tarefas que pode levar a parada observadores em algumas partições.
* Suporte para pontos de verificação manual.
* Compatível com [SDK de .NET de SQL](sql-api-sdk-dotnet.md) versões 1,21 e acima.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Adiciona suporte para .NET Standard 2.0. O pacote suporta agora `netstandard2.0` e `net451` monikers do framework.
* Compatível com [SDK de .NET de SQL](sql-api-sdk-dotnet.md) versões 1.17.0 e acima.
* Compatível com [SQL .NET Core SDK](sql-api-sdk-dotnet-core.md) versões 1.5.1 e acima.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Corrige um problema com o cálculo de estimativa do trabalho restante, quando o Feed de alteração estava vazio ou nenhum trabalho estava pendente.
* Compatível com [SDK de .NET de SQL](sql-api-sdk-dotnet.md) versões 1.13.2 e acima.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Adicionar um método para obter uma estimativa do trabalho restante para serem processados no Feed de alterações.
* Compatível com [SDK de .NET de SQL](sql-api-sdk-dotnet.md) versões 1.13.2 e acima.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* SDK DE DISPONIBILIDADE GERAL
* Compatível com [SDK de .NET de SQL](sql-api-sdk-dotnet.md) versões 1.14.1 e abaixo.


## <a name="release--retirement-dates"></a>Datas de lançamento & retirada
A Microsoft irá fornecer, pelo menos, notificação **12 meses** antecedência extinguir um SDK para facilitar a transição para uma versão mais recente/suportadas.

Novos recursos e funcionalidade e otimizações, apenas são adicionadas ao SDK atual, como tal, é recomendável que sempre atualiza para a versão mais recente SDK mais cedo possível. 

Qualquer pedido ao Cosmos DB com um SDK extinto será rejeitado pelo serviço.

<br/>

| Versão | Data de lançamento | Data de retirada |
| --- | --- | --- |
| [1.3.3](#1.3.3) |08 de Maio de 2018 |--- |
| [1.3.2](#1.3.2) |18 de abril de 2018 |--- |
| [1.3.1](#1.3.1) |13 de março de 2018 |--- |
| [1.2.0](#1.2.0) |31 de outubro de 2017 |--- |
| [1.1.1](#1.1.1) |29 de Agosto de 2017 |--- |
| [1.1.0](#1.1.0) |13 de Agosto de 2017 |--- |
| [1.0.0](#1.0.0) |07 de Julho de 2017 |--- |


## <a name="faq"></a>FAQ
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Consulte também
Para saber mais sobre o Cosmos DB, veja [do Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) página do serviço. 

