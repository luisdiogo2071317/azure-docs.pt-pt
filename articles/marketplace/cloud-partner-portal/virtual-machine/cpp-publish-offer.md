---
title: Publicar uma oferta de máquina virtual no Azure Marketplace | Documentos da Microsoft
description: Listas os passos necessários para publicar uma máquina virtual existente da oferta no Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 08/17/2018
ms.author: pbutlerm
ms.openlocfilehash: 3cf6a3d9bcb9470fd3a6bd4fef964c1966adfa1a
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639913"
---
# <a name="publish-a-virtual-machine-offer"></a>Publicar uma oferta de máquina virtual

 A última etapa, depois de ter definido a oferta no portal e criado os ativos técnicos associados, é submeter a oferta para publicação. O diagrama seguinte descreve os passos principais no processo de publicação para "entrar em direto":

![Passos de publicação para a máquina virtual da oferta](./media/publishvm_013.png)

A tabela seguinte descreve estes passos e fornece uma estimativa de tempo máximo de sua conclusão: <!-- we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Etapa de publicação**           | **tempo**    | **Descrição**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Validar a pré-requisitos         | 15 min   | Oferecer informações e oferecer definições são validadas.                        |
| Teste de validação de unidade (opcional) | 2 horas | Se tiver selecionado para ativar a versão de teste, a Microsoft valida a configuração da unidade de teste, sua implementação e replicação através de regiões selecionadas. |
| Certificação                  | 3 dias | Oferta é analisada pela equipe de certificação do Azure. Este passo irá realizar verificações de vírus, malware, conformidade de segurança e problemas de segurança. Comentários é fornecido se não for encontrado um problema. |
| Aprovisionamento                   | 4 dias   | Oferta de VM é replicada em sistemas de produção do marketplace.               |
| Empacotamento e registo de geração de oportunidades potenciais | \< 1 hora  | Ativos de técnicos da oferta são empacotados para uso do cliente e os sistemas de oportunidades potenciais estão configurados e de configuração. |
|  Aprovação do publicador             |  -        | Revisão do publicador final e a confirmação antes da oferta entra no ar. Pode implementar a sua oferta nas subscrições selecionadas (nos passos de informações oferta) para verificar que cumpre todos os seus requisitos.  |
| Aprovisionamento                   | 4 dias | Oferta de VM finalizada é replicada nas regiões e sistemas de produção do marketplace. | 
| Em direto                           | 4 dias | Oferta de VM é lançada, replicada para as regiões necessárias e disponibilizada ao público. |
|  |  |

Permitir que até 16 dias para este processo ser concluído.  Depois de seguir estes passos de publicação, a oferta VM será listada na [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). 

