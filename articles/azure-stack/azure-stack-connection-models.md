---
title: Pilha do Azure integrado modelos de ligação de sistemas | Microsoft Docs
description: Determine as decisões sobre a pilha do Azure de vários nós de planeamento de implementação.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: e6c94ef1172ea6380a94d5907c24069ed8c48ff5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
ms.locfileid: "29118793"
---
# <a name="azure-stack-integrated-systems-connection-models"></a>Modelos de ligação de sistemas de pilha integrada do Azure
Se estiver interessado num sistema de pilha do Azure integrado, terá de compreender [várias considerações de integração do Centro de dados](azure-stack-datacenter-integration.md) para a implementação de pilha do Azure determinar a forma como o sistema adequarão seu centro de dados. Além disso, terá de decidir exatamente como irá integrar pilha do Azure para o seu ambiente de nuvem híbrida. Este artigo fornece uma descrição geral destas decisões principais, incluindo a ligação do Azure, o arquivo de identidade e decisões de modelo de faturação.

Se optar por adquirir um sistema integrado, o seu fornecedor de hardware de fabricante de equipamento original (OEM) ajuda a ajudá-lo através de grande parte do processo de planeamento em mais detalhe. Também irá efetuar a implementação real.

## <a name="choose-an-azure-stack-deployment-connection-model"></a>Escolha um modelo de ligação de implementação de pilha do Azure
Pode optar por implementar pilha do Azure ligado à internet (e para o Azure) ou desligada. Para obter a vantagem da maioria da pilha do Azure, incluindo cenários híbridos entre pilha do Azure e o Azure, seria aconselhável implementar ligado ao Azure. Esta opção define que opções estão disponíveis para o arquivo de identidade (Azure Active Directory ou serviços de Federação do Active Directory) e o modelo de faturação (pagar enquanto baseada na utilização com capacidade de faturação ou faturação) conforme resumido no diagrama e tabela seguintes: 

![Implementação de pilha do Azure e cenários de faturação](media/azure-stack-connection-models/azure-stack-scenarios.png)  
  
> [!IMPORTANT]
> Este é um ponto de decisão principais! Serviços de Federação do Active Directory (AD FS) ou o Azure Active Directory (Azure AD) é uma decisão única que é necessário efetuar no momento da implementação. Não é possível alterar esta definição posteriormente sem o Reimplementar todo o sistema.  


|Opções|Ligado ao Azure|Desligado do Azure|
|-----|-----|-----|
|Azure AD|![Suportadas](media/azure-stack-connection-models/check.png)| |
|AD FS|![Suportadas](media/azure-stack-connection-models/check.png)|![Suportadas](media/azure-stack-connection-models/check.png)|
|Com base no consumo de faturação|![Suportadas](media/azure-stack-connection-models/check.png)| |
|Com base na capacidade de faturação|![Suportadas](media/azure-stack-connection-models/check.png)|![Suportadas](media/azure-stack-connection-models/check.png)|
|Transferir pacotes de atualização diretamente a pilha do Azure|![Suportadas](media/azure-stack-connection-models/check.png)|  |

Depois de decidido no modelo de ligação do Azure a utilizar para a implementação da pilha do Azure, decisões adicionais, dependentes da ligação têm de ser efetuadas para o arquivo de identidade e o método de faturação. 

## <a name="next-steps"></a>Passos Seguintes

[Decisões de implementação de pilha do Azure ligadas do Azure](azure-stack-connected-deployment.md)

[Azure desligado decisões de implementação de pilha do Azure](azure-stack-disconnected-deployment.md)
