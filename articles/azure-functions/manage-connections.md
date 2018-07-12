---
title: Como gerir ligações nas funções do Azure
description: Saiba como evitar problemas de desempenho nas funções do Azure com os clientes de ligação estático.
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
ms.openlocfilehash: 6c0af8f6f7e1d4aea8880a7af311aaa21f474f7e
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969009"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Como gerir ligações nas funções do Azure

As funções na aplicação de função partilhar recursos e entre esses recursos partilhados são ligações &mdash; ligações HTTP, ligações de base de dados e ligações para serviços do Azure, como o armazenamento. Quando muitas funções estão em execução em simultâneo é possível a ficar sem ligações disponíveis. Este artigo explica como codificar as suas funções para evitar a utilização de ligações de mais do que na verdade, o necessário.

## <a name="connections-limit"></a>Limite de ligações

O número de ligações disponíveis é limitado em parte porque uma aplicação de função é executada no [sandbox do serviço de aplicações do Azure](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Uma das restrições que impõe de área de segurança no seu código é um [limite no número de ligações, atualmente 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Quando atingir este limite, o runtime das funções cria um registo com a seguinte mensagem: `Host thresholds exceeded: Connections`.

As chances de que exceda o limite de aumentam quando o [controlador de escala adiciona instâncias de aplicações de função](functions-scale.md#how-the-consumption-plan-works). Cada instância de aplicação de função pode ser invocar funções muitas vezes ao mesmo tempo, e todas essas funções estão a utilizar ligações que contam para o limite de 300.

## <a name="use-static-clients"></a>Utilizar clientes estáticos

Para evitar que contém mais ligações do que o necessário, reutilize instâncias de cliente, em vez de criar aplicações novas com cada invocação de função. Clientes .NET, como o `HttpClient`, `DocumentClient`, e os clientes de armazenamento do Azure podem gerir ligações se utilizar um único cliente estático.

Aqui estão algumas diretrizes a seguir ao utilizar um cliente de serviços específicos de uma aplicação de funções do Azure:

- **Isso não é possível** criar um novo cliente com cada invocação de função.
- **FAZER** criar um único cliente estático que pode ser utilizado por cada invocação de função.
- **CONSIDERE** criação de um único cliente estático numa classe auxiliar partilhado se funções diferentes usam o mesmo serviço.

## <a name="httpclient-code-example"></a>Exemplo de código do HttpClient

Eis um exemplo de código de função que cria um estático `HttpClient`:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("http://example.com");
    // Rest of function
}
```

Uma pergunta comum sobre o .NET `HttpClient` é "Deve, ser descartar meu cliente?" Em geral, descartar objetos que implementam `IDisposable` quando terminar a utilizá-los. Mas não descartar um cliente estático porque não estão a ser feito usá-lo quando a função termina. Pretender que o cliente estático vivem pela duração da sua aplicação.

## <a name="documentclient-code-example"></a>Exemplo de código do DocumentClient

`DocumentClient` se liga a uma instância de Cosmos DB. A documentação do Cosmos DB recomenda que [utilizar o cliente do Azure Cosmos DB singleton durante o ciclo de vida do seu aplicativo](https://docs.microsoft.com/azure/cosmos-db/performance-tips#sdk-usage). O exemplo seguinte mostra um padrão para fazer isso de uma função.

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

Para obter mais informações sobre por que clientes estáticos são recomendados, consulte [anti-padrão de instâncias impróprias](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Para mais sugestões de desempenho de funções do Azure, consulte [otimizar o desempenho e fiabilidade das funções do Azure](functions-best-practices.md).