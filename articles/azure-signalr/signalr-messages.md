---
title: Mensagens e as ligações no Azure SignalR
description: Uma visão geral dos conceitos chave em torno de mensagens e as ligações no serviço Azure SignalR.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: c2348df7a1a55584807a03216e294486ddadfc52
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352602"
---
# <a name="message-and-connection-in-azure-signalr-service"></a>Mensagem e a ligação no serviço Azure SignalR

Serviço Azure SignalR tem o modelo de faturação com base no número de ligações e número de mensagens. Como ligações e as mensagens são definidas e contadas para fins de faturação são explicadas abaixo.

## <a name="message-formats-supported"></a>Suportada de formatos de mensagem

Serviço Azure SignalR suporta os formatos mesmo que suporta o SignalR de núcleo do ASP.NET: [JSON](https://www.json.org/) e [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)

## <a name="message-size"></a>Tamanho da mensagem

Serviço Azure SignalR tem sem limite de tamanho de mensagem.

Na prática, mensagem grande é dividida em menores não mais do que 2 KB cada, de mensagens e transmitida como mensagens separadas. Mensagem de identificador de SDKs divisão e montagem. São necessários sem esforços de desenvolvimento.

Mas a mensagem grande ter um impacto negativo no desempenho do sistema de mensagens. Utilizar tamanho mais pequeno de mensagem sempre que possível e teste para escolher o tamanho da mensagem ideal para cada cenário de caso de utilização.

## <a name="how-to-count-messages-for-billing-purpose"></a>Como a contagem de mensagens para fins de faturação?

Estamos apenas a contar as mensagens de saída do serviço SignalR e ignorar as mensagens de ping entre clientes e servidores.

Mensagem é superior a 2 KB é contabilizado como vários mensagens de 2 KB. Gráfico de contagem de mensagens no portal do Azure atualiza cada 100 mensagens por hub.

Por exemplo, tem três clientes e um servidor de aplicativos. Um cliente envia uma mensagem de 4 KB para permitir que o servidor de difusão para todos os clientes. A contagem de mensagens é 8: Uma mensagem a partir do serviço para o servidor de aplicativos, três mensagens do serviço para clientes e cada mensagem é contabilizada como duas mensagens de 2 KB.

Contagem de mensagens apresentada no portal do Azure é ainda 0, até que ele acumula para ser mais do que 100.

## <a name="how-to-count-connections"></a>Como contar ligações?

Existem ligações de servidor e ligações de cliente. Por predefinição, cada servidor de aplicativos tem cinco ligações por hub com o serviço SignalR e cada cliente tem uma ligação de cliente com o serviço SignalR.

Contagem de ligação apresentada no portal do Azure inclui ligações de servidor e ligações de cliente.

Por exemplo, dois servidores de aplicativos e definir os hubs de cinco em códigos. Total de ligações de servidor é 50: 2 servidores de aplicações * 5 hubs * 5 ligações/hub.

O SignalR de ASP.NET é diferente no cálculo de ligações de servidor. Ele tem um hub de padrão, além de hubs de definidos pelo cliente. Cada servidor de aplicativos tem 5 mais ligações de servidor por predefinição. A contagem de ligação do hub padrão mantém consistente com outros hubs.

## <a name="how-to-count-inbound-traffic--outbound-traffic"></a>Como contar o tráfego de entrada / saída de tráfego

Entrada / saída é a partir da perspetiva de serviço SignalR. O tráfego é calculado em Bytes. Como o número de mensagens, o tráfego também tem sua taxa de amostragem. A entrada / saído gráfico no portal do Azure atualiza cada 100 KB por hub.

## <a name="related-resources"></a>Recursos relacionados

- [Tipo de agregação no Azure Monitor](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [Configuração do ASP.NET Core SignalR](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)