---
title: Exemplos de Hubs de eventos do Azure | Documentos da Microsoft
description: Exemplos de Hubs de eventos do Azure
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2018
ms.author: sethm
ms.openlocfilehash: 8e80587f7b3f6b0cb081fd963848c7aa3ab11e3e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431261"
---
# <a name="event-hubs-samples"></a>Exemplos de Hubs de eventos 
Pode encontrar exemplos de Hubs de eventos no [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples). Estes exemplos demonstra os principais recursos do [os Hubs de eventos do Azure](/azure/event-hubs/). Este artigo categoriza e descreve os exemplos disponíveis, com links para cada um.

## <a name="net-samples"></a>Amostras de .NET

| Nome de exemplo | Descrição | 
| ----------- | ----------- | 
| [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) | Este exemplo mostra como escrever uma aplicação de consola .NET Core que envia um conjunto de eventos para um hub de eventos. |
| [SampleEHReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) | Este exemplo mostra como escrever uma aplicação de consola .NET Core que recebe um conjunto de eventos de um hub de eventos com a biblioteca de anfitrião do processador de eventos.  | 

## <a name="java-samples"></a>Exemplos do Java

| Nome de exemplo | Descrição | 
| ----------- | ----------- | 
| [SendBatch](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SendBatch)  | Este exemplo ilustra como ingerir lotes de eventos no seu hub de eventos. | 
| [SimpleSend](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend) | Este exemplo ilustra como de ingerir eventos no seu hub de eventos. |
| [AdvanceSendOptions](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/AdvancedSendOptions) | Este exemplo ilustra as várias opções disponíveis com os Hubs de eventos para ingerir eventos. |
| [ReceiveByDateTime](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveByDateTime) | Este exemplo ilustra como receber eventos a partir de uma partição do hub de eventos com um desvio de data e hora específico. |
| [ReceiveUsingOffset](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingOffset) | Este exemplo ilustra como receber eventos a partir de uma partição do hub de eventos com um desvio de dados específicos. |  
| [ReceiveUsingSequenceNumber](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingSequenceNumber) | Este exemplo ilustra como podem receber de uma partições do hub de eventos utilizando um número de sequência. |   
| [EventProcessorSample](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/EventProcessorSample) |Este exemplo ilustra como receber eventos de um hub de eventos com o anfitrião de processador de eventos, que fornece a seleção automática de partição e a ativação pós-falha por vários recetores em simultâneo. | 
| [AutoScaleOnIngress](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/AutoScaleOnIngress) | Este exemplo ilustra como um hub de eventos pode automaticamente verticalmente em altas cargas. O exemplo enviará os eventos a uma taxa que apenas excederem a taxa de configurado de um hub de eventos, fazendo com que o hub de eventos aumentar verticalmente. | 
| [IngressBenchmark](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/IngressBenchmark) | Este exemplo permite medir a taxa de entrada. | 

## <a name="next-steps"></a>Passos Seguintes
Pode saber mais sobre os Hubs de eventos nos seguintes artigos:

- [Descrição geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
- [Funcionalidades dos Hubs de Eventos](event-hubs-features.md)
- [FAQ dos Hubs de Eventos](event-hubs-faq.md)