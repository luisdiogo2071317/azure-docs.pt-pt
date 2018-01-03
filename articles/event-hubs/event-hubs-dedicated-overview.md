---
title: "Descrição geral da capacidade dedicado de Hubs de eventos do Azure | Microsoft Docs"
description: "Descrição geral da capacidade dedicado do Microsoft Azure Event Hubs."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: sethm;babanisa
ms.openlocfilehash: 563152a019464f3d0342383ff13e6ee1c87a22fe
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="overview-of-event-hubs-dedicated"></a>Descrição geral dos Event Hubs dedicado

*Event Hubs dedicado* capacidade oferece único inquilino implementações para clientes com os requisitos mais demanding. Em escala completa, Event Hubs do Azure pode a entrada mais de 2 milhões de eventos por segundo ou até 2 GB por segundo de telemetria com uma latência de armazenamento e o segundo secundárias totalmente durável. Isto também permite que soluções integradas por lote no mesmo sistema e de processamento em tempo real. Com [capturar os Hubs de eventos](event-hubs-capture-overview.md) incluído na oferta, pode reduzir a complexidade da sua solução, fazendo com que um fluxo único suporta pipelines em tempo real e baseada no batch.

A tabela seguinte compara os escalões de serviço dos Event Hubs. A oferta dedicado de Hubs de eventos é um preço mensal fixo, comparada com a utilização de preço para a maioria das funcionalidades do padrão. A camada dedicada oferece todas as funcionalidades do plano padrão, mas com capacidade de escala empresarial para os clientes com cargas de trabalho pedir o seu trabalho. 

| Funcionalidade | Standard | Dedicado |
| --- |:---:|:---:|:---:|
| Eventos de entrada | Pagar por milhões de eventos | Incluída |
| Unidade de débito (entrada de 1 MB/seg, saída 2 MB por segundo) | Pagar por hora | Incluída |
| Tamanho da Mensagem | 256 KB | 1 MB |
| Políticas do publicador | Sim | Sim |   
| Grupos de consumidores | 20 | 20 |
| Repetição de mensagens | Sim | Sim |
| Unidades de débito máximas | 20 (flexível para 100)   | 1 unidade de capacidade (CU) ≈ 50 |
| Ligações mediadas | 1000 incluídos | 100 K incluído |
| Ligações Mediadas adicionais | Sim | Sim |
| Retenção de Mensagens | 1 dia incluído | Até 7 dias incluídos |
| Captura | Pagar por hora | Incluída |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Vantagens da capacidade dedicado de Hubs de eventos

As seguintes vantagens estão disponíveis quando utilizar Event Hubs dedicado:

* Único inquilino aloja com nenhuma ruído de outros inquilinos.
* Aumenta o tamanho da mensagem para 1 MB por comparação a 256 KB para Standard.
* Desempenho repetíveis sempre.
* Capacidade para satisfazer as necessidades de rajada de garantida.
* Inclui o [capturar](event-hubs-capture-overview.md) funcionalidade dos Event Hubs, para fornecer a integração com retenção micro batch e a longo prazo.
* Zero manutenção: O serviço gere o balanceamento de carga, SO de atualizações, patches de segurança e a criação de partições.
* Corrigido preço por hora.
* Mensagem retenção de cópias de segurança para 7 dias com sem encargos adicionais.

Dedicado de Hubs de eventos também remove a algumas das limitações de débito de oferta de padrão. Unidades de débito no escalão Standard entitle é 1000 eventos por segundo ou de 1 MB por segundo de entrada por TU e duplo que quantidade de saída. Contagens de evento de saída e a oferta de escala dedicado tem sem restrições na entrada. Estes limites são regidos apenas pela capacidade de processamento dos hubs de eventos adquiridas.

Este ambiente dedicado reservado fornece outras capacidades exclusivas para esta camada, tais como:

* Controla o número de espaços de nomes no seu cluster.
* Especifica os limites de débito em cada um dos seus espaços de nomes.
* Configura o número de hubs de eventos em cada espaço de nomes.
* Determina o limite no número de partições.

Este serviço é direcionado para os utilizadores de telemetria maiores e está disponível para clientes com um enterprise agreement.

## <a name="how-to-onboard"></a>Como integrar

Pode dimensionar a capacidade ou reduzir verticalmente durante todo o mês para satisfazer as necessidades adicionando ou removendo Comunidade. O plano dedicado é exclusivo na medida em que irá ocorrer uma integração mais prática da equipa de produto do Event Hubs para obter a implementação flexível que é adequada para si. Para carregar para este SKU [contacte o suporte à faturação](https://ms.portal.azure.com/#create/Microsoft.Support) ou o seu representante da Microsoft.

## <a name="next-steps"></a>Passos Seguintes

Contacte o seu representante da Microsoft ou Support da Microsoft para obter detalhes adicionais sobre a capacidade de dedicado de Hubs de eventos. Também pode saber mais sobre os Event Hubs escalões de preço, visitando as seguintes ligações:

- [Event Hubs dedicado preços](https://azure.microsoft.com/pricing/details/event-hubs/). Também pode contactar o seu representante da Microsoft ou Support da Microsoft para obter detalhes adicionais sobre a capacidade de dedicado de Hubs de eventos.
- O [FAQ de Hubs de eventos](event-hubs-faq.md) contém informações sobre preços e responde a algumas perguntas frequentes sobre os Event Hubs. 
