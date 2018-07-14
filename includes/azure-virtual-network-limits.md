---
title: incluir ficheiro
description: incluir ficheiro
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 06/20/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: 9ba9bc993832350f6b6ce1c642e2dc852731b6f0
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030045"
---
<a name="virtual-networking-limits-classic"></a>Os limites seguintes só se aplicam a recursos de rede geridos através do modelo de implementação clássica por subscrição. Saiba como [ver a utilização de recursos atual em relação a seus limites de subscrição](../articles/networking/check-usage-against-limits.md).

| Recurso | Limite predefinido | Limite máximo |
| --- | --- | --- |
| Redes virtuais |50 |100 |
| Sites de rede local |20 |contactar o suporte |
| Servidores de DNS por rede virtual |20 |100 |
| Endereços de IP privados por rede virtual |4096 |4096 |
| Em simultâneo TCP ou UDP flui por NIC de uma máquina virtual ou instância de função |500 K |500 K |
| Grupos de Segurança de Rede (NSG) |100 |200 |
| Regras do NSG por NSG |200 |1000 |
| Tabelas da rota definida pelo utilizador |100 |200 |
| Rotas definidas pelo utilizador por tabela de rota |100 |400 |
| Endereços IP públicos (dinâmico) |5 |contactar o suporte |
| Endereços IP públicos reservados |20 |contactar o suporte |
| VIP público por implementação |5 |contactar o suporte |
| VIP privado (ILB) por implementação |1 |1 |
| Listas de Controlo de Acesso de Ponto Final (ACLs) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Limites de Rede - Azure Resource Manager
Os seguintes limites só se aplicam aos recursos de rede geridos através do Azure Resource Manager por região por subscrição. Saiba como [ver a utilização de recursos atual em relação a seus limites de subscrição](../articles/networking/check-usage-against-limits.md).

| Recurso | Limite predefinido | Limite Máximo |
| --- | --- | --- |
| Redes virtuais |50 |1000 |
| Sub-redes por rede virtual |1000 |10000 |
| Peerings de rede virtual por rede Virtual |50 * * |100 |
| Servidores de DNS por rede virtual |9 |25 |
| Endereços de IP privados por rede virtual |16384 * * |16384 |
| Endereços IP privados por interface de rede |256 |256 |
| Em simultâneo TCP ou UDP flui por NIC de uma máquina virtual ou instância de função |500 K |500 K |
| Interfaces de Rede (NIC) |24000 * * |24000 |
| Grupos de Segurança de Rede (NSG) |100 |5000 |
| Regras do NSG por NSG |1000 * * |1000 |
| Endereços IP e intervalos especificados para a origem ou destino num grupo de segurança |2000 |4000 |
| Grupos de segurança de aplicações |500 |3000 |
| Grupos de segurança de aplicação por configuração de IP, por NIC |10 |20 |
| Configurações de IP por grupo de segurança de aplicações |1000 |4000 |
| Grupos de segurança de aplicações que podem ser especificados em todas as regras de segurança de um grupo de segurança de rede |50 |100 |
| Tabelas da rota definida pelo utilizador |100 |200 |
| Rotas definidas pelo utilizador por tabela de rota |400 * * |400 |
| Endereços IP públicos - dinâmico |60 (básico) |contactar o suporte |
| Endereços IP públicos - estáticos |20 (básico) |contactar o suporte |
| Endereços IP públicos - estáticos |20 (standard) |contactar o suporte |
| Certificados de Raiz do Ponto-a-Site por Gateway de VPN |20 |20 |

* * Estas atualizado padrão limites aplicam-se a subscrições que não tenham tido anteriormente estes limites aumentados através do suporte. Se tiver tem estes limites aumentados em suporte no passado e gostaria de obtê-los atualizados para as predefinições de novo [abra um pedido de suporte do cliente online sem custos](../articles/azure-resource-manager/resource-manager-quota-errors.md)

#### <a name="load-balancer"></a>Limites do Balanceador de carga
Os seguintes limites só se aplicam aos recursos de rede geridos através do Azure Resource Manager por região por subscrição. Saiba como [ver a utilização de recursos atual em relação a seus limites de subscrição](../articles/networking/check-usage-against-limits.md)

| Recurso | Limite predefinido | Limite Máximo |
| --- | --- | --- |
| Balanceadores de Carga | 100 | 1000 |
| Regras por recurso, Basic | 150 | 250 |
| Regras por recurso, Standard | 1250 | 1500 |
| Regras por configuração de IP | 299 |299 |
| Configurações de IP de front-end, Basic | 10 | 200 |
| Configurações de IP de front-end, Standard | 10 | 600 |
| Conjunto de back-end, Basic | Conjunto de disponibilidade de único de 100, | Conjunto de disponibilidade de único de 100, |
| Conjunto de back-end, Standard | 1000, único VNet | 1000, único VNet |
| Recursos de back-end por Balanceador de carga Standard &ast; | 50 | 150 |
| HA portas, Standard | 1 por front-end interno | 1 por front-end interno |

&ast; Até 150 recursos, qualquer combinação de máquinas virtuais autónomas, conjuntos de disponibilidade e conjuntos de dimensionamento de máquina virtual.

Caso seja necessário aumentar os limites para lá da predefinição, [contacte o suporte](../articles/azure-supportability/resource-manager-core-quotas-request.md ).

