---
title: Como gerir ligações nas funções do Azure
description: Saiba como evitar problemas de desempenho nas funções do Azure através da utilização de clientes de ligação estático.
services: functions
documentationcenter: ''
author: tdykstra
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: tdykstra
ms.openlocfilehash: 4ea2b033d8d67dd3c921fb833462605ba0aeb687
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655378"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Como gerir ligações nas funções do Azure

As funções de uma aplicação de função partilhar recursos e entre esses recursos partilhados são ligações &mdash; ligações HTTP, ligações de base de dados e as ligações aos serviços do Azure, como o armazenamento. Quando muitas funções em execução em simultâneo é possível executar fora ligações disponíveis. Este artigo explica como as suas funções para evitar a utilização mais ligações do que necessitam, na verdade, de código.

## <a name="connections-limit"></a>Limite de ligações

O número de ligações disponíveis é limitado parcialmente porque uma aplicação de função é executada no [sandbox do App Service do Azure](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Uma das restrições que impõe a sandbox no seu código é uma [limite no número de ligações, atualmente 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Quando atingir este limite, o tempo de execução de funções cria um registo com a seguinte mensagem: `Host thresholds exceeded: Connections`.

Possibilidades de exceder o limite de aumentam quando o [controlador escala adiciona instâncias da aplicação de função](functions-scale.md#how-the-consumption-plan-works). Cada instância da aplicação de função pode ser invocar funções demasiadas vezes em simultâneo e todas estas funções estão a utilizar ligações contam para o limite de 300.

## <a name="use-static-clients"></a>Utilizar clientes estáticos

Para evitar que contém mais ligações que o necessário, reutilize instâncias de cliente em vez de criar novas com cada invocação de função. Os clientes de .NET como o `HttpClient`, `DocumentClient`, e os clientes do Storage do Azure podem gerir ligações se utilizar um único cliente estático.

Eis algumas diretrizes a seguir ao utilizar um cliente de específicos do serviço numa aplicação das funções do Azure:

- **NÃO** criar um novo cliente com cada invocação de função.
- **EFETUE** criar um único estático de cliente que pode ser utilizado por cada invocação de função.
- **CONSIDERE** criar um único cliente estático de uma classe de programa auxiliar partilhado se diferentes funções utilizarem o mesmo serviço.

## <a name="httpclient-code-example"></a>Exemplo de código HttpClient

Eis um exemplo de código de função que cria uma estática `HttpClient`:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("http://example.com");
    // Rest of function
}
```

Uma pergunta comum sobre .NET `HttpClient` é "Deve posso ser disposing os meus clientes?" Em geral, pode eliminar objetos que implementem `IDisposable` quando tiver terminado a utilizá-los. Mas não eliminar um cliente estático porque não estão a ser efetuada utilizá-lo quando a função termina. Pretende que o cliente estático TTL durante a duração da sua aplicação.

## <a name="documentclient-code-example"></a>Exemplo de código DocumentClient

`DocumentClient` estabelece ligação a uma instância de base de dados do Cosmos. A documentação do Cosmos DB recomenda que lhe [utilizar um cliente de base de dados do Azure Cosmos singleton para a duração da sua aplicação](https://docs.microsoft.com/en-us/azure/cosmos-db/performance-tips#sdk-usage). O exemplo seguinte mostra um padrão para fazer de uma função.

```cs
#r "Microsoft.Azure.Documents.Client"
using Microsoft.Azure.Documents.Client; 

private static Lazy<DocumentClient> lazyClient = new Lazy<DocumentClient>(InitializeDocumentClient);
private static DocumentClient documentClient => lazyClient.Value;

private static DocumentClient InitializeDocumentClient()
{
    // Perform any initialization here
    var uri = new Uri("example");
    var authKey = "authKey";
    
    return new DocumentClient(uri, authKey);
}

public static async Task Run(string input)
{
    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("database", "collection");
    object document = new { Data = "example" };
    await documentClient.UpsertDocumentAsync(collectionUri, document);
    
    // Rest of function
}
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o motivo clientes estáticos são recomendados, consulte [antipattern instanciação inadequada](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Para mais sugestões de desempenho de funções do Azure, consulte [otimizar o desempenho e fiabilidade das funções do Azure](functions-best-practices.md).