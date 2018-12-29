---
title: Mensagens e as ligações no Azure SignalR
description: Uma visão geral dos conceitos chave em torno de mensagens e as ligações no serviço Azure SignalR.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 5a0430e9ad124319147342c49fc51e11472ac8ff
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811892"
---
# <a name="message-and-connection-in-azure-signalr-service"></a>Mensagem e a ligação no serviço Azure SignalR

Serviço Azure SignalR tem o modelo de faturação com base no número de ligações e número de mensagens. Como as mensagens e as ligações são definidas e contadas para fins de faturação são explicadas abaixo.

## <a name="message-formats-supported"></a>Suportada de formatos de mensagem

Serviço Azure SignalR suporta os formatos mesmo que suporta o SignalR de núcleo do ASP.NET: [JSON](https://www.json.org/) e [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)

## <a name="message-size"></a>Tamanho da mensagem

Serviço Azure SignalR tem sem limite de tamanho de mensagem.

Na prática, mensagem grande é dividida em menores não mais do que 2 KB cada, de mensagens e transmitida como mensagens separadas. Mensagem de divisão e montagem é processada pelo SDKs. São necessários sem esforços de desenvolvimento.

Mas a mensagem grande ter um impacto negativo no desempenho do sistema de mensagens. Utilizar tamanho mais pequeno de mensagem sempre que possível e teste para escolher o tamanho da mensagem ideal para cada cenário de caso de utilização.

## <a name="how-to-count-messages-for-billing-purpose"></a>Como a contagem de mensagens para fins de faturação?

Estamos apenas a contar as mensagens de saída do serviço SignalR e ignorar as mensagens de ping entre clientes e servidores.

Mensagem é superior a 2 KB é contabilizado como vários mensagens de 2 KB. Gráfico de contagem de mensagens no portal do Azure será atualizado a cada 100 mensagens por hub.

Por exemplo, um utilizador tem 3 clientes e servidor de 1 aplicativos. Um cliente envia uma mensagem de 4 KB para permitir que o servidor de difusão para todos os clientes. A contagem de mensagens será 8: 1 mensagem do serviço do servidor de aplicativos, 3 mensagens do serviço para clientes e cada mensagem é contabilizada como 2 de 2 KB de mensagens.

Contagem de mensagens apresentada no portal do Azure é ainda 0, até que ele acumula para ser mais do que 100.

## <a name="how-to-count-connections"></a>Como contar ligações?

Existem ligações de servidor e ligações de cliente. Por predefinição, cada servidor de aplicativos tem 5 ligações por hub com o serviço SignalR e cada cliente tem 1 conexão de cliente com o serviço SignalR.

Contagem de ligação apresentada no portal do Azure inclui ligações de servidor e ligações de cliente.

Por exemplo, um utilizador tiver dois servidores de aplicativo e define o 5 hubs na códigos. Contagem de ligação do servidor apresentada no portal do Azure será 2 servidores de aplicações * 5 hubs * 5 ligações/hub = 50 ligações de servidor.

## <a name="related-resources"></a>Recursos relacionados

- [Configuração do ASP.NET Core SignalR](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)