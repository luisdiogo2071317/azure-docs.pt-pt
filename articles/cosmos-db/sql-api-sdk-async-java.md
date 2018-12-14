---
title: 'o Azure Cosmos DB: SQL Async Java API, SDK e recursos'
description: Saiba tudo sobre o SQL Async Java API e o SDK, incluindo as datas de lançamento, datas de extinção e as alterações feitas entre cada versão do SDK de Java do Azure Cosmos DB SQL Async.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 11/29/2018
ms.author: moderakh
ms.openlocfilehash: 5284de9a5b0f4f78b3b8b68e3848c2cb2783b839
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338622"
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Async Java SDK para a API de SQL: Notas de versão e recursos
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
> * [SQL](sql-api-query-reference.md)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

O SDK de Java de Async de API de SQL é diferente do SDK de Java API do SQL ao fornecer operações assíncronas com suporte do [biblioteca Netty](https://netty.io/). O pré-existente [SDK de Java API de SQL](sql-api-sdk-java.md) não suporta operações assíncronas. 

<table>

<tr><td>**Transferência de SDK**</td><td>[Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb)</td></tr>

<tr><td>**Documentação da API**</td><td>[Documentação de referência da API de Java](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client?view=azure-java-stable)</td></tr>

<tr><td>**Contribuir para o SDK**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java)</td></tr>

<tr><td>**Introdução**</td><td>[Introdução ao SDK de Java assíncrono](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started)</td></tr>

<tr><td>**Exemplo de código**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)</td></tr>

<tr><td>**Sugestões de desempenho**</td><td>[Leiame do GitHub](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)</td></tr>

<tr><td>**Mínimo de runtime suportada**</td><td>[JDK 8](https://aka.ms/azure-jdks)</td></tr>
</table></br>

## <a name="release-notes"></a>Notas de versão

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Foi corrigido um erro de vazamento de recursos.
* Foi adicionado suporte para MultiPolygon
* Foi adicionado suporte para os cabeçalhos personalizados no RequestOptions.

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2
* Foi corrigido um erro de empacotamento.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Foi corrigido um erro NPE no caminho de repetição de escrita.
* Foi corrigido um erro NPE na gestão de ponto final.
* Atualizado vulneráveis dependências ([GitHub #68](https://github.com/Azure/azure-cosmosdb-java/issues/68)).
* Foi adicionado suporte para o registo de rede Netty para resolução de problemas.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Foi adicionado suporte para várias regiões de escrita.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Foi adicionado suporte para o Proxy.
* Foi adicionado suporte para autorização de token de recurso.
* Foi corrigido um erro no processamento de chaves de partição de grandes dimensões ([GitHub #63](https://github.com/Azure/azure-cosmosdb-java/issues/63)).
* Melhorada a documentação.
* SDK reestruturado em módulos mais granulares.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Foi corrigido um erro para localidades inglês ([GitHub #51](https://github.com/Azure/azure-cosmosdb-java/issues/51)).
* Métodos de helper adicionado no recurso de conflito.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Substituído org.json dependência por jackson devido a motivos de desempenho e de licenciamento ([GitHub #29](https://github.com/Azure/azure-cosmosdb-java/issues/29)).
* Classe OfferV2 preterido foi removido.
* Método do acessador foi adicionado à classe de oferta para o conteúdo de débito.
* Qualquer método no documento/recurso retornando tipos org.json alterados para devolver uma jackson tipo de objeto.
* tipo de método de getObject(.) das classes JsonSerializable expansão alterado para devolver uma ObjectNode jackson.
* método getCollection(.) alterado para devolver a coleção de ObjectNode.
* Construtores das JsonSerializable subclasses removidos com org.json.JSONObject arg.
* Agora, o JsonSerializable.toJson (SerializationFormattingPolicy.Indented) utiliza os dois espaços para avanço.
  
### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2
* Foi adicionado suporte para índice exclusivo da política.
* Foi adicionado suporte para limitar o tamanho de token de continuação de resposta nas opções de feeds.
* Foi adicionado suporte para a divisão de partição na consulta de partição cruzada.
* Foi corrigido um erro na serialização do Json timestamp ([GitHub #32](https://github.com/Azure/azure-cosmosdb-java/issues/32)).
* Foi corrigido um erro na serialização de enumeração de Json.
* Foi corrigido um erro no gerenciamento de documentos do tamanho de 2MB ([GitHub #33](https://github.com/Azure/azure-cosmosdb-java/issues/33)).
* Dependência com.fasterxml.jackson.core:jackson-databind atualizado para 2.9.5 devido a um erro ([jackson databind: GitHub #1599](https://github.com/FasterXML/jackson-databind/issues/1599))
* Dependência de rxjava-extras atualizado para 0.8.0.17 devido a um erro ([rxjava extras: GitHub #30](https://github.com/davidmoten/rxjava-extras/issues/30)).
* A descrição de metadados no ficheiro pom atualizada para serem inline com o restante da documentação.
* Melhoria da sintaxe ([GitHub #41](https://github.com/Azure/azure-cosmosdb-java/issues/41)), ([GitHub #40](https://github.com/Azure/azure-cosmosdb-java/issues/40)).

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Foi adicionado suporte de back-pressão na consulta.
* Foi adicionado suporte para o id de intervalo da chave de partição na consulta.
* Correção para permitir que o token de continuação maior no cabeçalho do pedido (bugfix GitHub #24).
* Dependência netty atualizada para 4.1.22.Final para garantir que o JVM encerra o tempo limite após a conclusão do thread principal.
* Corrigi para evitar a transmitir o token de sessão durante a leitura de recursos globais.
* Adicionar mais exemplos.
* Adicionar mais cenários de benchmark.
* Arquivos de cabeçalho de Java fixos para a geração de documentos de java adequado.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* SDK de disponibilidade geral com suporte ponto a ponto para a utilização de e/s sem bloqueio a [biblioteca Netty](https://netty.io/) no modo de gateway. 

## <a name="release-and-retirement-dates"></a>Datas de lançamento e de extinção
A Microsoft irá fornecer, pelo menos, notificação **12 meses** antecedência extinguir um SDK para facilitar a transição para uma versão mais recente/suportadas.

Novos recursos e funcionalidade e otimizações apenas são adicionadas ao SDK do atual. Por isso, recomenda-se que sempre atualiza para a versão mais recente do SDK mais cedo possível.

Qualquer pedido ao Cosmos DB com um SDK extinto será rejeitado pelo serviço.

<br/>

| Versão | Data de lançamento | Data de retirada |
| --- | --- | --- |
| [2.3.0](#2.3.0) |29 de Novembro de 2018|--- |
| [2.2.2](#2.2.2) |8 de Novembro de 2018|--- |
| [2.2.1](#2.2.1) |2 de Novembro de 2018|--- |
| [2.2.0](#2.2.0) |22 de Setembro de 2018|--- |
| [2.1.0](#2.1.0) |5 de Setembro de 2018|--- |
| [2.0.1](#2.0.1) |16 de Agosto de 2018|--- |
| [2.0.0](#2.0.0) |20 de junho de 2018|--- |
| [1.0.2](#1.0.2) |18 de Maio de 2018|--- |
| [1.0.1](#1.0.1) |20 de Abril de 2018|--- |
| [1.0.0](#1.0.0) |27 de fevereiro de 2018|--- |

## <a name="faq"></a>FAQ
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Consulte também
Para saber mais sobre o Cosmos DB, veja [do Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) página do serviço.

