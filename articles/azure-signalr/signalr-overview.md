---
title: O que é o Azure SignalR | Microsoft Docs
description: Descrição geral do Azure SignalR Service.
services: signalr
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: signalr
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 04/17/2018
ms.author: wesmc
ms.openlocfilehash: bc144fb1d7db9251871e7e181b012417a32de7e6
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33868114"
---
# <a name="what-is-azure-signalr-service"></a>O que é o Azure SignalR Service

O Microsoft Azure SignalR Service está atualmente em [Pré-visualização Pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

O Azure SignalR Service é um serviço do Azure baseado no [ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction). O ASP.NET Core SignalR é uma [biblioteca de código aberto](https://github.com/aspnet/signalr) que simplifica o processo de acrescentar a funcionalidade Web de tempo real a aplicações através de HTTP. Esta funcionalidade de tempo real permite que o servidor Web envie atualizações de conteúdos aos clientes ligados. Como resultado, os clientes são atualizados sem que tenham de consultar o servidor ou submeter novos pedidos HTTP para as atualizações.

Este artigo disponibiliza uma descrição geral do Azure SignalR Service. Se quiser começar, comece com [ASP.NET Core quickstart](signalr-quickstart-dotnet-core.md) (Início rápido de ASP.NET Core).

## <a name="what-is-signalr-service-used-for"></a>Para que serve o SignalR Service? 

Existem muitos tipos de aplicações que precisam de atualizações de conteúdos em tempo real. Os seguintes tipos de aplicações de exemplo são ideais para utilizar o Azure SignalR Service:

* Aplicações que precisam de atualizações de alta frequência a partir do servidor. Alguns exemplos incluem aplicações de jogos, redes sociais, votações, leilões, mapas e GPS.
* Aplicações de dashboards e monitorização. Os exemplos incluem dashboards empresariais, atualizações de vendas instantâneas ou alertas de viagens.
* Aplicações de colaboração. As aplicações de quadros brancos e o software de reuniões de equipas são alguns exemplos de aplicações de colaboração.
* Aplicações que requerem notificações. As aplicações de redes sociais, e-mails, chats, jogos, alertas de viagens, entre muitas outras, utilizam notificações.

Internamente, o SignalR é uma abstração através de um número de técnicas utilizadas para compilar aplicações Web em tempo real. [WebSockets](https://wikipedia.org/wiki/WebSocket) é o transporte ideal, mas podem ser utilizadas outras técnicas como [Server-Sent Events (SSE)](https://wikipedia.org/wiki/Server-sent_events) e Long Polling, se não estiverem disponíveis outras opções. O SignalR deteta e inicializa automaticamente o transporte adequado com base nas funcionalidades que o servidor e o cliente suportam.

## <a name="developing-signalr-apps"></a>Desenvolver aplicações SignalR

Atualmente, pode utilizar duas versões do SignalR com as suas aplicações Web: SignalR para ASP.NET e ASP.NET Core SignalR, que é a versão mais recente. O Azure SignalR Service é um serviço gerido pelo Azure criado sobre o ASP.NET Core SignalR. 

O ASP.NET Core SignalR é uma reescrita da versão anterior. Como resultado, o ASP.NET Core SignalR não é retrocompatível com a versão anterior do SignalR. As APIs e os comportamentos são diferentes. O ASP.NET Core SignalR SDK é .NET Standard, pelo que pode continuar a utilizá-lo com o .NET Framework. No entanto, tem de utilizar as APIs novas em vez das antigos. Se estiver a utilizar o SignalR e quiser passar para o ASP.NET Core SignalR ou para o Azure SignalR, tem de alterar o código para que este possa lidar com as diferenças nas APIs.

Com o Azure SignalR Service, o componente do lado do servidor do ASP.NET Core SignalR é alojado no Azure. Contudo, uma vez que a tecnologia é criada sobre o ASP.NET Core, tem a possibilidade de executar a aplicação Web real em várias plataformas (Windows, Linux e MacOS) enquanto aloja com o [Serviço de Aplicações do Azure](../app-service/app-service-web-overview.md), o [IIS](https://docs.microsoft.com/aspnet/core/host-and-deploy/iis/index), o [Nginx](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-nginx), o [Apache](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-apache) e o [Docker](https://docs.microsoft.com/aspnet/core/host-and-deploy/docker/index). Também pode utilizar o alojamento automático no seu próprio processo.

Se os objetivos da sua aplicação incluírem suportar a mais recente funcionalidade para atualizar clientes Web com atualizações de conteúdos em tempo real, executar em várias plataformas (Azure, Windows, Linux e MacOS) e alojar em diferentes ambientes, a melhor opção é tirar partido do Azure SignalR Service.


## <a name="why-not-deploy-signalr-myself"></a>Por que não implementar o SignalR eu próprio?

Implementar a sua própria aplicação Web do Azure que suporte o ASP.NET Core SignalR como componente do back-end na sua aplicação Web geral continua a ser uma abordagem válida.

Um dos principais motivos para utilizar o Azure SignalR Service é a simplicidade. Com o Azure SignalR Service, não tem de lidar com problemas como desempenho, escalabilidade e disponibilidade. Estes problemas são tratados por si com um contrato de nível de serviço de 99,9%.

Além disso, os WebSockets são, normalmente, a técnica preferencial para suportar as atualizações de conteúdos em tempo real. No entanto, à medida que dimensiona, fazer o balanceamento de carga de um grande número de ligações persistentes de WebSocket pode tornar-se problemático. As soluções comuns tiram partido do balanceamento de carga do DNS, dos balanceadores de carga do hardware e do balanceamento de carga do software. O Azure SignalR Service lida com este problema por si.

Outro motivo poderá ser o facto de que talvez não tenha necessidade de realmente alojar uma aplicação Web de todo. A lógica da aplicação Web poderá tirar partido da [computação sem servidor](https://azure.microsoft.com/overview/serverless-computing/). Por exemplo, talvez o código só seja alojado e executado a pedido com acionadores das [Funções do Azure](https://docs.microsoft.com/azure/azure-functions/). Este cenário pode ser complicado, porque o seu código só é executado a pedido e não mantém ligações longas com os clientes. O Azure SignalR Service pode tratar desta situação, uma vez que já faz a gestão das ligações por si.

## <a name="how-does-it-scale"></a>Como é dimensionado?

É comum dimensionar o SignalR com o SQL Server, o Azure Service Bus ou a Cache de Redis. O Azure SignalR Service lida com a abordagem de dimensionamento por si. O desempenho e o custo são comparáveis a estas abordagens, sem a complexidade de ter de lidar com estes outros serviços. Tudo o que tem de fazer é atualizar a contagem de unidades do seu serviço. Cada unidade de serviço suporta até 1000 ligações de cliente.

## <a name="next-steps"></a>Passos seguintes
* [Quickstart: Create a chat room with Azure SignalR](signalr-quickstart-dotnet-core.md) (Início rápido: Criar uma sala de chat com o Azure SignalR)  
  

