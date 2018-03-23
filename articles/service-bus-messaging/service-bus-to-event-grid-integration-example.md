---
title: Exemplos de integração do Azure Service Bus para o Event Grid | Microsoft Docs
description: Exemplos de mensagens do Service Bus e integração do Event Grid
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: ''
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: 3819a274696762861fbe76a9684b8495f1724f6a
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="azure-service-bus-to-azure-event-grid-examples"></a>Exemplos do Azure Service Bus para Azure Event Grid

Neste documento, irá obter informações sobre como configurar funções do Azure e uma aplicação lógica que recebe mensagens com base na receção de eventos do Event Grid. Este exemplo pressupõe um tópico do Service Bus com duas subscrições e que a subscrição do Event Grid é criada de forma a enviar eventos apenas para uma subscrição do Service Bus. Depois, envia mensagens para o tópico do Service Bus e verifica que o evento é gerado para esta subscrição do Service Bus e, em seguida, a função ou aplicação lógica recebe as mensagens dessa subscrição do Service Bus e completa-a.

* Primeiro, certifique-se de que todos os [Pré-requisitos](#prerequisites) estão implementados.
* Crie uma [Função do Azure de teste simples](#test-function-setup) para depurar e ver um fluxo inicial de eventos do Event Grid.  **Este passo deve ser efetuado independentemente de executar ou não os passos 3. ou 4.**
* Crie uma [Função do Azure para receber e processar mensagens do Service Bus](#receive-messages-using-azure-function) com base em eventos do Event Grid.
* Utilize o [Logic Apps](#receive-messages-using-azure-logic-app).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="service-bus-namespace"></a>Espaço de Nomes do Service Bus

Crie um Espaço de Nomes Premium do Service Bus. Crie um Tópico do Service Bus com duas subscrições.

### <a name="code-to-send-message-to-the-service-bus-topic"></a>Código para Enviar Mensagem ao tópico do Service Bus

Pode utilizar qualquer meio para enviar uma mensagem ao seu tópico do Service Bus. Em alternativa, pode utilizar o exemplo de abaixo. O código de exemplo pressupõe que está a utilizar o Visual Studio 2017.

Clone [este repositório do GitHub](https://github.com/Azure/azure-service-bus/).

Navegue para a seguinte pasta:

\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration e abra o ficheiro: SBEventGridIntegration.sln.

Em seguida,navegue para o projeto MessageSender e abra o ficheiro Program.cs.

![8][]

Preencha o seu nome de tópico, bem como a sua cadeia de ligação, e execute a aplicação da consola:

```CSharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
const string TopicName = "YOUR TOPIC NAME";
```

## <a name="test-function-setup"></a>Configuração de função de teste

Antes de trabalhar em todo o cenário de ponto a ponto, é aconselhável ter pelo menos uma pequena função de teste, a qual pode utilizar para depurar e ver que eventos estão a fluir.

No portal, crie uma nova Aplicação de Função do Azure. Siga esta [ligação](https://docs.microsoft.com/en-us/azure/azure-functions/) para conhecer os aspetos básicos das Funções do Azure.

Na função acabada de criar, clique no pequeno sinal de adição para adicionar uma função de acionador http:

![2][]

![3][]

Em seguida, copie o seguinte código para a função:

```CSharp
#r "Newtonsoft.Json"
using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    // parse query parameter
    var content = req.Content;

    string jsonContent = await content.ReadAsStringAsync(); 
    log.Info($"Received Event with payload: {jsonContent}");

IEnumerable<string> headerValues;
if (req.Headers.TryGetValues("Aeg-Event-Type", out headerValues))
{
var validationHeaderValue = headerValues.FirstOrDefault();
if(validationHeaderValue == "SubscriptionValidation")
{
var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
     var code = events[0].Data["validationCode"];
     return req.CreateResponse(HttpStatusCode.OK,
     new { validationResponse = code });
}
}

    return jsonContent == null
    ? req.CreateResponse(HttpStatusCode.BadRequest, "Pass a name on the query string or in the request body")
    : req.CreateResponse(HttpStatusCode.OK, "Hello " + jsonContent);
}

public class GridEvent
{
    public string Id { get; set; }
    public string EventType { get; set; }
    public string Subject { get; set; }
    public DateTime EventTime { get; set; }
    public Dictionary<string, string> Data { get; set; }
    public string Topic { get; set; }
}
```

Clique em Guardar e executar.

## <a name="connect-function-and-namespace-via-event-grid"></a>Ligar a Função ao Espaço de Nomes através do Event Grid

O próximo passo é ligar a função ao espaço de nomes do Service Bus. Neste exemplo, utilize o portal do Azure. Consulte a página [conceitos](service-bus-to-event-grid-integration-concept.md), para saber como utilizar o PowerShell ou a CLI do Azure para o mesmo fim.

Para criar uma nova subscrição do Azure Event Grid, navegue para o seu espaço de nomes no portal do Azure e selecione o painel Event Grid. Clique em "+ Subscrição de Eventos".

A seguinte captura de ecrã mostra um espaço de nomes que já tem algumas subscrições do Event Grid.

![20][]

O ecrã seguinte mostra como subscrever uma Função do Azure ou um Hook de Web sem filtragem específica. Lembre-se de adicionar o respetivo filtro para a sua subscrição do Service Bus como "Filtro de Sufixo":

![21][]

Envie uma mensagem ao seu tópico do Service Bus conforme mencionado nos pré-requisitos e certifique-se de que os eventos estão a fluir através da funcionalidade Monitorização da Função do Azure.

![9][]

### <a name="receive-messages-using-azure-function"></a>Receber mensagens através da Função do Azure

Na secção anterior, viu um cenário simples de teste e depuração e garantiu que os eventos estão a fluir. Nesta parte da documentação, irá ver como receber e processar mensagens aquando da receção de um evento.

O motivo para adicionar uma Função do Azure da seguinte forma é o facto de as Funções do Service Bus nas próprias Funções do Azure não darem ainda suporte nativo à nova integração no Event Grid.

Na mesma Solução do Visual Studio que abriu nos pré-requisitos, selecione ReceiveMessagesOnEvent.cs. Introduza a sua cadeia de ligação no código:

![10][]

```Csharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
```

Em seguida, aceda ao portal do Azure e transfira o perfil de publicação para a Função do Azure que criou anteriormente em [2. Configuração de função de teste](#2-test-function-setup).

![11][]

Em seguida, no Visual Studio, clique com o botão direito do rato em SBEventGridIntegration e selecione Publicar. Utilize o perfil de publicação que transferiu anteriormente, selecione Importar perfil e clique em Publicar.

![12][]

Após publicar a nova função do Azure, crie uma nova Subscrição do Azure Event Grid que aponte para a nova Função do Azure. Certifique-se de que aplica o filtro "Termina em" correto, o qual deve conter o seu Nome de Subscrição do Service Bus.

Depois, envie uma mensagem para o tópico do Azure Service Bus que criou anteriormente e examine no registo de Função do Azure no portal se os eventos estão a fluir e as mensagens estão a ser recebidas.

![12-1][]

### <a name="receive-messages-using-azure-logic-app"></a>Receber mensagens através da Aplicação Lógica do Azure

As seguintes instruções mostram como ligar uma Aplicação Lógica do Azure ao Azure Service Bus e Azure Event Grid:

Primeiro, crie uma nova Aplicação Lógica no portal do Azure e selecione Event Grid como ação inicial.

![13][]

A vista inicial no estruturador do Logic Apps deverá ter o aspeto da seguinte captura de ecrã, sendo que o "Nome de Recurso" terá de ser substituído pelo nome do seu espaço de nomes. Certifique-se também de que expande as opções avançadas e adiciona o filtro de sufixo para a sua subscrição:

![14][]

Em seguida, adicione uma ação de Receção do Service Bus para receber de uma subscrição de tópico. A ação final deve ter um aspeto semelhante à seguinte Captura de ecrã.

![15][]

Em seguida, adicione um evento completo, que deverá ter este aspeto.

![16][]

Guarde a aplicação lógica e envie uma mensagem para o seu tópico do Service Bus, conforme mencionado nos pré-requisitos. Em seguida, observe a execução das Aplicações lógicas. Se clicar em "Descrição geral" e, em seguida, em "Histórico de execuções", pode também obter mais dados sobre a execução.

![17][]

![18][]

## <a name="next-steps"></a>Passos seguintes

* Saiba mais sobre o [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/).
* Saiba mais sobre as [Funções do Azure](https://docs.microsoft.com/azure/azure-functions/).
* Saiba mais sobre o [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/).
* Saiba mais sobre o [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

[2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid2.png
[3]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid3.png
[7]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid7.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[10]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid10.png
[11]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid11.png
[12]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12.png
[12-1]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-1.png
[12-2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-2.png
[13]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid13.png
[14]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid14.png
[15]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid15.png
[16]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid16.png
[17]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid17.png
[18]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid18.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
