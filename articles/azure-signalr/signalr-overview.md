---
title: O que é o Azure SignalR | Microsoft Docs
description: Descrição geral do Azure SignalR Service.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.service: signalr
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: a159833936ec4762213f063e235fa4f9237af95b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951105"
---
# <a name="what-is-azure-signalr-service"></a>O que é o Azure SignalR Service

O Azure SignalR Service simplifica o processo de acrescentar a funcionalidade Web de tempo real a aplicações através de HTTP. Esta funcionalidade em tempo real permite que o serviço envie atualizações de conteúdo a clientes ligados, como uma única página Web ou aplicação móvel. Como resultado, os clientes são atualizados sem que tenham de consultar o servidor ou submeter novos pedidos HTTP para as atualizações.

Este artigo disponibiliza uma descrição geral do Azure SignalR Service.

## <a name="what-is-azure-signalr-service-used-for"></a>Para que serve o Azure SignalR Service? 

Existem muitos tipos de aplicações que precisam de atualizações de conteúdos em tempo real. Os seguintes exemplos são ideais para utilizar o Azure SignalR Service:

* Aplicações que precisam de atualizações de alta frequência a partir do servidor. Alguns exemplos incluem aplicações de jogos, votações, leilões, mapas e GPS.
* Aplicações de dashboards e monitorização. Os exemplos incluem dashboards empresariais e atualizações de vendas instantâneas.
* Aplicações de colaboração. As aplicações de quadros brancos e o software de reuniões de equipas são alguns exemplos de aplicações de colaboração.
* Aplicações que requerem notificações. As aplicações de redes sociais, e-mails, chats, jogos, alertas de viagens, entre muitas outras, utilizam notificações.

O SignalR oferece uma abstração através de um número de técnicas utilizadas para compilar aplicações Web em tempo real. [WebSockets](https://wikipedia.org/wiki/WebSocket) é o transporte ideal, mas podem ser utilizadas outras técnicas como [Server-Sent Events (SSE)](https://wikipedia.org/wiki/Server-sent_events) e Long Polling, se não estiverem disponíveis outras opções. O SignalR deteta e inicializa automaticamente o transporte adequado com base nas funcionalidades que o servidor e o cliente suportam.

Além disso, o SignalR oferece um modelo de programação para aplicações em tempo real, que permite ao servidor enviar mensagens para todas as ligações ou para um subconjunto de ligações que pertencem a um utilizador específico ou foram colocadas num grupo arbitrário.

## <a name="how-to-use-azure-signalr-service"></a>Como utilizar o Azure SignalR Service

Atualmente, existem três formas de utilizar o Azure SignalR Service:

- **[Dimensionar uma Aplicação do SignalR do ASP.NET Core](signalr-overview-scale-aspnet-core.md)** - Integrar o Azure SignalR Service numa aplicação do SignalR do ASP.NET Core para aumentar horizontalmente para centenas de milhares de ligações.
- **[Criar aplicações em tempo real sem servidor](signalr-overview-azure-functions.md)** - Utilize a integração das Funções do Azure com o Azure SignalR Service para criar aplicações em tempo real sem servidor em linguagens como JavaScript, C# e Java.
- **[Enviar mensagens de servidor a clientes através da API REST](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)** - O Azure SignalR Service apresenta a API REST para permitir que as aplicações publiquem mensagens para clientes ligados com o SignalR Service, em quaisquer linguagens de programação compatíveis com REST.

