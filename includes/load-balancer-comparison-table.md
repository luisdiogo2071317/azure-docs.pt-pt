---
title: incluir ficheiro
description: incluir ficheiro
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 8/8/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 6514bcdbe79db4a22b2a4bf13e5808bbb9abbb06
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40025959"
---
| | SKU Standard | SKU Básico |
| --- | --- | --- |
| Tamanho do conjunto de back-end | Suporta até 1000 instâncias | Suporta até 100 instâncias |
| Pontos finais de conjunto de back-end | qualquer máquina virtual numa única rede virtual, incluindo blend de máquinas virtuais, conjuntos de disponibilidade, conjuntos de dimensionamento de máquina virtual. | Conjunto de máquinas virtuais numa escala de máquina virtual ou conjunto de disponibilidade única. |
| [Sondas de estado de funcionamento](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Sonda de estado de funcionamento para baixo de comportamento](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | Ligações de TCP permanecem ativas na instância de pesquisa para baixo __e__ em todas as sondas para baixo. | Ligações de TCP permanecem ativas na instância de pesquisa para baixo. Terminam todas as conexões TCP em todas as sondas para baixo. |
| Zonas de Disponibilidade | SKU básico, com redundância de zona e zonais front-ends para entrada e mapeamentos de fluxos de saída, saída resistir a falhas de zona, balanceamento de carga entre zonas. | n/d|
| Diagnóstico | Monitor do Azure, incluindo bytes e contadores de pacotes, estado de funcionamento de métricas multidimensionais sonda de estado, as tentativas de ligação (TCP SYN), estado de funcionamento da ligação de saída (SNAT com êxito ou falhados fluxos), medidas de plano de dados do Active Directory | O Azure Log Analytics para o Balanceador de carga público apenas, alerta de esgotamento de SNAT, contagem de estado de funcionamento do conjunto de back-end. |
| HA portas | Balanceador de Carga Interno | n/d |
| Seguro por padrão | Predefinição fechada para pontos finais públicos do Balanceador de carga e de IP e um grupo de segurança de rede deve ser usado para explicitamente lista de permissões para o tráfego para o fluxo. | Padrão aberto, grupo de segurança de rede opcional. |
| [Ligações de saída](../articles/load-balancer/load-balancer-outbound-connections.md) | Vários front-ends com por sair em regra de balanceamento de carga. Um cenário de saída _tem_ explicitamente criado para a máquina virtual para conseguir utilizar a conectividade de saída.  Pontos finais de serviço de rede virtual pode ser contatados sem conectividade de saída e não são considerados dados processados.  Quaisquer endereços IP públicos, incluindo serviços de PaaS do Azure não está disponíveis como pontos finais de serviço de VNet, tem de ser contatados por meio de conectividade de saída e contagem para dados processados. Quando apenas um balanceador de carga interno está a servir uma máquina virtual, as ligações de saída através do padrão SNAT não estão disponíveis. Programação de SNAT de saída é o protocolo de transporte específico com base no protocolo de regra de balanceamento de carga de entrada. | Único front-end, selecionado aleatoriamente quando vários front-ends estão presentes.  Quando uma máquina virtual está a servir apenas Balanceador de carga interno, é utilizada a predefinição SNAT. |
| [Vários front-ends](../articles/load-balancer/load-balancer-multivip-overview.md) | Entrada e [saída](../articles/load-balancer/load-balancer-outbound-connections.md) | Apenas de entrada |
| Operações de gestão | A maioria dos segundos de < a 30 de operações | 60-90 segundos típico. |
| SLA | 99,99% para o caminho de dados com duas máquinas virtuais em bom estado. | Implícitas no SLA da VM. | 
| Preços | Cobradas com base no número de regras, dados processados de entrada ou saída associados a recursos.  | sem custos |
|  |  |  |
