---
title: Recomendações do Advisor custo do Azure | Microsoft Docs
description: Utilize o Advisor do Azure para otimizar o custo das implementações do Azure.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: ade6ef996c00c0c06d5b8e44815520e6e4ab7e9f
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34735872"
---
# <a name="advisor-cost-recommendations"></a>Recomendações de custo do Assistente

Ajuda-o Advisor otimizar e reduzir o Azure geral passam por identificar recursos inativos e subutilizados. Pode obter custo recomendações do **custo** separador no dashboard do Advisor.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Otimizar a máquina virtual se gasta, redimensionamento ou encerrar subutilizadas instâncias 
Apesar de determinados cenários de aplicação podem resultar numa utilização baixa por predefinição, muitas vezes, pode poupar dinheiro ao gerir o tamanho e número de máquinas virtuais. Advisor monitoriza a utilização da máquina virtual nos últimos 14 dias e, em seguida, identifica as máquinas virtuais de baixa utilização. Máquinas virtuais cuja utilização do CPU é 5 porcento ou menos e utilização de rede é 7 MB ou menos, de quatro ou mais dias são considerados máquinas de virtuais baixa utilização.

Advisor mostra o custo estimado continuar a executar a máquina virtual, para que possa escolher encerrá-la ou redimensioná-la.

Se pretender ser mais agressiva em identificar subutilizadas máquinas virtuais, pode ajustar a regra de utilização da CPU média numa base por subscrição.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Reduzir os custos, eliminando não aprovisionados circuitos do ExpressRoute
Advisor identifica os circuitos ExpressRoute que tenham sido do Estado de fornecedor de *não aprovisionado* para mais de um mês e recomenda a eliminar o circuito, se não estão a planear para aprovisionar o circuito com a conectividade fornecedor.

## <a name="buy-virtual-machine-reserved-instances-to-save-money-over-pay-as-you-go-costs"></a>Compre instâncias reservadas de máquinas virtuais para poupar dinheiro sobre os custos pay as you go
Advisor irá rever a utilização da máquina virtual durante os últimos 30 dias e determinar se foi poupar dinheiro pelo compra instâncias reservadas. Advisor mostrará a regiões e tamanhos potencialmente tenha as maioria das reduções e irá mostrar-lhe as poupanças estimadas de compra instâncias reservadas. 

Com instâncias reservadas, pode comprar previamente os custos de base para as máquinas virtuais. Descontos serão aplicado automaticamente para as VMs novas ou existentes com o mesmo tamanho e região, como as instâncias reservadas. [Saiba mais sobre reservado instâncias de VM do Azure.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Como aceder recomendações de custo no Azure Advisor

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, abra [Advisor](https://aka.ms/azureadvisordashboard).

2.  No dashboard do Advisor, clique em de **custo** separador.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as recomendações do assistente, consulte:
* [Introdução ao Advisor](advisor-overview.md)
* [Introdução](advisor-get-started.md)
* [Recomendações de desempenho do Assistente](advisor-cost-recommendations.md)
* [Recomendações de elevada disponibilidade do Assistente](advisor-cost-recommendations.md)
* [Recomendações de segurança do Advisor](advisor-cost-recommendations.md)
