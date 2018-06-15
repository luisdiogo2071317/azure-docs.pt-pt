---
title: Descrição geral de API de reencaminhamento do Azure | Microsoft Docs
description: Descrição geral das APIs de reencaminhamento do Azure disponíveis
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2018
ms.author: sethm
ms.openlocfilehash: 00496ca6c0138a840322c053d7d20944df228e9f
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33893449"
---
# <a name="available-relay-apis"></a>APIs de reencaminhamento disponíveis

## <a name="runtime-apis"></a>APIs de tempo de execução

A tabela seguinte apresenta uma lista de todos os clientes de tempo de execução de reencaminhamento atualmente disponíveis.

O [informações adicionais](#additional-information) secção contém mais informações sobre o estado de cada biblioteca de tempo de execução.

| Idioma/plataforma | Funcionalidade disponível | Pacote de cliente | Repositório |
| --- | --- | --- | --- |
| Padrão de .NET | Ligações Híbridas | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET framework | Reencaminhamento do WCF | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | N/A |
| Nó | Ligações Híbridas | [Websockets: `hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[Websockets: `hyco-websocket`](https://www.npmjs.com/package/hyco-websocket)<br/>[Pedidos de HTTP: `hyco-https`](https://www.npmjs.com/package/hyco-https) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Informações adicionais

#### <a name="net"></a>.NET

A ecossistema de .NET tem vários tempos de execução, por conseguinte, existem vários bibliotecas .NET para o reencaminhamento. A biblioteca .NET padrão pode ser executada através do .NET Core ou o .NET Framework, enquanto a biblioteca de .NET Framework só pode ser executada num ambiente de .NET Framework. Para obter mais informações sobre estruturas de .NET, consulte [versões framework](/dotnet/articles/standard/frameworks#framework-versions).

A biblioteca de .NET Framework só suporta o modelo de programação do WCF e depende de um protocolo proprietário de binário com base no WCF `net.tcp` transporte. Este protocolo e a biblioteca é mantido para efeitos de compatibilidade com as aplicações existentes.

A biblioteca .NET padrão baseiam-se a definição do protocolo aberta para o reencaminhamento de ligações híbridas que se baseia-se em HTTP e WebSockets. A biblioteca suporta uma abstração de fluxo através de Websockets e um gesto de API simples de resposta-pedido para responder pedidos de HTTP. O [Web API](https://github.com/Azure/azure-relay-dotnet) exemplo mostra como integrar ligações híbridas com o ASP.NET Core para serviços web.

#### <a name="nodejs"></a>Node.js

Os módulos de ligações híbridas listados na tabela acima, substituem ou corrigir existentes módulos do Node.js com implementações alternativos que escutar o serviço de reencaminhamento do Azure em vez da pilha de rede local.

O `hyco-https` módulo amends e parcialmente substitui os módulos do Node.js core `http` e `https`, fornecer uma implementação de serviço de escuta HTTPS é compatível com muitos módulos do Node.js existentes e as aplicações que dependem destes principal módulos.

O `hyco-ws` e `hyco-websocket` módulos corrigir o populares `ws` e `websocket` módulos para Node.js, fornecer implementações de serviço de escuta alternativo que ativar módulos e aplicações depender de um módulo de trabalho atrás da híbrida Reencaminhamento de ligações.

Podem ser encontrados detalhes sobre esses módulos no [reencaminhamento-azure-nó](https://github.com/Azure/azure-relay-node) repositório do GitHub.

## <a name="next-steps"></a>Passos Seguintes

Para mais informações sobre o reencaminhamento do Azure, visite estas ligações:
* [O que é o Reencaminhamento do Azure?](relay-what-is-it.md)
* [FAQ de Reencaminhamento](relay-faq.md)