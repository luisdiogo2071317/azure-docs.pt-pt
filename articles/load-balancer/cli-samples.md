---
title: Exemplos CLI do Azure para o Balanceador de carga | Documentos da Microsoft
description: Exemplos da CLI do Azure
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: jeconnoc
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 06/14/2018
ms.author: kumud
ms.openlocfilehash: 41e80e04051a24fd32086c61c65bc3eec1564c57
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35947723"
---
# <a name="azure-cli-samples-for-load-balancer"></a>Exemplos CLI do Azure para o Balanceador de carga

A tabela seguinte inclui ligações para scripts bash criados com a CLI do Azure.

| | |
|-|-|
| [Balancear carga de tráfego para VMs de elevada disponibilidade](./scripts/load-balancer-linux-cli-sample-nlb.md) | Cria várias máquinas virtuais numa elevada disponibilidade e a configuração de balanceamento de carga. |
| [Balancear carga de VMs por zonas de disponibilidade](./scripts/load-balancer-linux-cli-sample-zone-redundant-frontend.md) | Cria três VMs em zonas de disponibilidade diferentes dentro de uma região e o Balanceador de carga Standard com um endereço IP de front-end com redundância de zona. Esta configuração de Balanceador de carga ajuda a proteger as aplicações e dados de uma falha pouco provável ou perda de todo o datacenter. |
|[Balanceamento de carga de VMs dentro de uma zona de disponibilidade específica](./scripts/load-balancer-linux-cli-sample-zonal-frontend.md)|Cria três VMs, um balanceador de carga Standard com o IP de front-end zonal que ajuda a alinhar o caminho de dados e recursos numa única zona para uma determinada região.|
| [Vários Web sites em VMs com balanceamento de carga](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md) | Cria duas VMs com várias configurações de IP, associadas ao Azure conjunto de disponibilidade, acessível através de um balanceador de carga do Azure. |
| | |

