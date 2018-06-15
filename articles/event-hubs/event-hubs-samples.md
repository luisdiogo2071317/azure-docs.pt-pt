---
title: Exemplos de Event Hubs do Azure | Microsoft Docs
description: Exemplos de Event Hubs do Azure
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
ms.date: 03/05/2018
ms.author: sethm
ms.openlocfilehash: a581b7039a3631b7f1dc35816175242f892bd7dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
ms.locfileid: "29802792"
---
# <a name="event-hubs-samples"></a>Exemplos de Hubs de eventos 

O conjunto de exemplos de Event Hubs do Azure demonstra as principais funcionalidades no [Event Hubs do Azure](/azure/event-hubs/). Este artigo categoriza e descreve os exemplos disponíveis, com ligações para cada um.

No momento desta redação, exemplos de Event Hubs estão localizados em vários locais diferentes:

- [Exemplos de código do MSDN para programadores](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

Para obter mais informações sobre as diferentes versões do .NET Framework, consulte [estruturas e destinos](/dotnet/articles/standard/frameworks).

Mais exemplos será adicionado ao longo do tempo, por isso, verifique novamente aqui frequentemente para atualizações.

## <a name="net-standard"></a>Padrão de .NET

Os exemplos seguintes demonstram como enviar e receber eventos utilizando o [cliente dos Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) para o [biblioteca .NET padrão](/dotnet/articles/standard/library).

### <a name="send-events"></a>Enviar eventos 

O [começar a enviar](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) exemplo demonstra como escrever uma aplicação de consola .NET Core envia eventos para um hub de eventos.

### <a name="receive-events"></a>Receber eventos 

O [começar a receber com o anfitrião do processador de eventos](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) exemplo é uma aplicação de consola .NET Core que recebe mensagens do hub de eventos utilizando o anfitrião do processador de eventos.

## <a name="net-framework"></a>.NET Framework   

Estes exemplos demonstram várias outras funcionalidades do Event Hubs do Azure, como objetivo o [biblioteca .NET Framework](/dotnet/framework/index).
 
### <a name="notify-users-of-events-received"></a>Notificar os utilizadores dos eventos recebidos

O [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) exemplo notifica os utilizadores de dados recebidos de sensores ou outros sistemas.

### <a name="get-started-with-event-hubs"></a>Introdução ao Event Hubs 

O [Event Hubs introdução](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) exemplo demonstra as funcionalidades básicas dos Event Hubs, tais como criar um hub de eventos, enviar eventos para um hub de eventos e consumir eventos utilizando o [anfitrião do processador de eventos](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) .

### <a name="scale-out-event-processing"></a>Aumentar horizontalmente o processamento de eventos 

O [aumentar horizontalmente o processamento de eventos](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) exemplo demonstra como utilizar o [anfitrião do processador de eventos](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) para distribuir a carga de trabalho de consumo de fluxo de Event Hubs. Mostra como implementar o **EventProcessor** e **EventProcessorFactory** objetos a gerir o fluxo de eventos. 

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as versões do .NET Framework, visitando as seguintes ligações:

- [Estruturas e destinos](/dotnet/articles/standard/frameworks)
- [.NET framework 4.6 e 4.5](/dotnet/framework/index)

Pode saber mais sobre os Event Hubs nos seguintes artigos:

- [Descrição geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
- [Funcionalidades dos Hubs de Eventos](event-hubs-features.md)
- [FAQ dos Hubs de Eventos](event-hubs-faq.md)