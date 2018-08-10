---
title: Descrição geral da capacidade dedicado de Hubs de eventos do Azure | Documentos da Microsoft
description: Descrição geral da capacidade do Microsoft Azure Hubs de eventos dedicados.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: shvija
ms.openlocfilehash: 1a7a7593e80f08296e3163e528e880f343366b8a
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005709"
---
# <a name="overview-of-event-hubs-dedicated"></a>Descrição geral dos Hubs de eventos dedicados

*Os Hubs de eventos dedicados* capacidade oferece implementações de inquilino único para os clientes com os requisitos mais exigentes. Em escala completa, os Hubs de eventos do Azure pode receber mais de 2 milhões de eventos por segundo ou até 2 GB por segundo de telemetria com latência totalmente durável de armazenamento e de frações de segundos. Isto também permite que soluções integradas por lote no mesmo sistema e de processamento em tempo real. Com o [captura de Hubs de eventos](event-hubs-capture-overview.md) incluído na oferta, pode reduzir a complexidade da sua solução fazendo com que um único fluxo suporte pipelines em tempo real e com base no batch.

A tabela seguinte compara os escalões de serviço disponível dos Hubs de eventos. A oferta de Hubs de eventos dedicados é um preço mensal fixo, em comparação comparado a utilização de preços para a maioria das funcionalidades do Standard. O escalão dedicado oferece todas as funcionalidades do plano padrão, mas com capacidade de dimensionamento de enterprise para clientes com cargas de trabalho exigentes. 

| Funcionalidade | Standard | Dedicado |
| --- |:---:|:---:|:---:|
| Eventos de entrada | Pagar por milhão de eventos | Incluída |
| Unidade de débito (entrada de 1 MB/s, saída 2 MB/seg) | Pagar por hora | Incluída |
| Tamanho da Mensagem | 256 KB | 1 MB |
| Políticas do publicador | Sim | Sim |   
| Grupos de consumidores | 20 | 20 |
| Repetição de mensagens | Sim | Sim |
| Unidades de débito máximas | 20 (flexível para 100)   | 1 unidade de capacidade (CU) ≈ 50 |
| Ligações mediadas | 1000 incluídos | 100 mil incluídos |
| Ligações Mediadas adicionais | Sim | Sim |
| Retenção de Mensagens | 1 dia incluído | Até 7 dias incluídos |
| Captura | Pagar por hora | Incluída |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Benefícios da capacidade de Hubs de eventos dedicados

Os seguintes benefícios estão disponíveis ao utilizar os Hubs de eventos dedicados:

* Inquilino único, hospedagem de com não ruído de outros inquilinos.
* Tamanho da mensagem aumenta para 1 MB em comparação com a 256 KB para Standard.
* Desempenho repetível cada vez.
* A garantia de capacidade de atender às suas necessidades de rajada.
* Inclui a [capturar](event-hubs-capture-overview.md) funcionalidade dos Hubs de eventos, para fornecer integração com retenção a longo prazo e de micro-lotes.
* Manutenção de zero: O serviço gere o balanceamento de carga, OS atualizações, patches de segurança e criação de partições.
* Corrigido preços por hora.
* Retenção de mensagens de cópia de segurança a 7 dias sem encargos extra.

Os Hubs de eventos dedicados também remove algumas das limitações de débito da oferta padrão. Unidades de débito no escalão Standard lhe dá direito a 1000 eventos por segundo ou 1 MB por segundo de entrada por TU e double essa quantidade de saída. A oferta de dedicadas de dimensionamento tem sem restrições na entrada e contagem de eventos de saída. Estes limites são regidos apenas com a capacidade de processamento de hubs de eventos adquiridas.

Neste ambiente dedicado, reservado fornece outras capacidades exclusivas para esta camada, tais como:

* Controla o número de espaços de nomes no seu cluster.
* Especifica os limites de débito em cada um dos seus espaços de nomes.
* Configura o número de hubs de eventos em cada espaço de nomes.
* Determina o limite no número de partições.

Este serviço é direcionado para os utilizadores de telemetria maiores e está disponível para clientes com um contrato enterprise.

## <a name="how-to-onboard"></a>Como integrar

Pode aumentar sua capacidade ou reduzir verticalmente ao longo do mês para atender às suas necessidades ao adicionar ou remover CUs. O plano dedicado é o único que irá ocorrer uma integração mais prática da equipe de produto dos Hubs de eventos para obter a implementação flexível, que é adequada para si. A carregar para este SKU [contacte o suporte de faturação](https://ms.portal.azure.com/#create/Microsoft.Support) ou seu representante da Microsoft.

## <a name="next-steps"></a>Passos Seguintes

Contacte o seu representante de vendas da Microsoft ou Support da Microsoft para obter detalhes adicionais sobre a capacidade de dedicado de Hubs de eventos. Também pode saber mais sobre os Hubs de eventos escalões de preço, visitando os links a seguir:

- [Preços de dedicado de Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/). Também pode contactar o seu representante de vendas da Microsoft ou Support da Microsoft para obter detalhes adicionais sobre a capacidade de Hubs de eventos dedicados.
- O [FAQ dos Hubs de eventos](event-hubs-faq.md) contém informações sobre preços e responde a algumas perguntas frequentes sobre os Hubs de eventos. 
