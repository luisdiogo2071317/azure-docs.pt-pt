---
title: Pilha do Azure de política de manutenção | Microsoft Docs
description: Saiba mais sobre a pilha do Azure de manutenção política, como manter um sistema integrado num estado suportado.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: brenduns
ms.reviewer: harik
ms.openlocfilehash: c3cc8857373238079fee06c61faec962d7e3a6b2
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796431"
---
# <a name="azure-stack-servicing-policy"></a>Pilha do Azure de política de manutenção
Este artigo descreve a política de manutenção para sistemas de pilha do Azure integrada e que terá de efetuar para manter o seu sistema num estado suportado. 

## <a name="update-package-types"></a>Tipos de pacote de atualização

Existem dois tipos de pacotes de atualização para sistemas integrados: 

- **As atualizações de software da Microsoft**. Microsoft é responsável pelo ciclo de vida manutenção de ponto a ponto para os pacotes de atualização de software da Microsoft. Estes pacotes podem incluir as mais recentes atualizações de segurança do Windows Server, não-atualizações de segurança e atualizações de funcionalidade da pilha do Azure. Pode transferir os pacotes de atualização de presumem diretamente a partir de Microsoft.

- **Atualizações de fornecido de fornecedor de hardware do OEM**. Parceiros de hardware de pilha do Azure são responsáveis pelo ponto-a-ponto manutenção ciclo de vida (incluindo orientações) para o firmware relacionados com o hardware e pacotes de atualização de controladores. Além disso, os parceiros de hardware de pilha do Azure proprietários e manter a documentação de orientação para todo o software e hardware no anfitrião de ciclo de vida de hardware. O fornecedor de hardware do OEM aloja estes pacotes num site suas próprias transferências de atualização.


## <a name="update-package-release-cadence"></a>Cadência de versão do pacote de atualização
A Microsoft espera libertar os pacotes de atualização de software uma cadência mensal. No entanto, é possível ter várias ou não existem versões de atualização num mês. As atualizações em conta as necessidades de versão de fornecedores de hardware do OEM. 

Encontrar documentação sobre como planear e gerir as atualizações e como determinar a sua versão atual na [gerir atualizações de descrição geral](azure-stack-updates.md). Para obter informações sobre uma atualização específica, incluindo como transferi-lo, consulte as notas de versão de atualização para que: 
- [Atualização de pilha 1805 do Azure](azure-stack-update-1805.md)
- [Atualização de pilha 1804 do Azure](azure-stack-update-1804.md)
- [Atualização de pilha 1803 do Azure](azure-stack-update-1803.md)


## <a name="hotfixes"></a>Correções
Ocasionalmente, a Microsoft disponibiliza correções para o Azure pilha esse endereço de um problema específico que é frequentemente preventivas ou sensíveis ao tempo.  Cada correção é fornecida com um artigo de Base de dados de Conhecimento Microsoft correspondente que fornece detalhes sobre o problema, a causa e a resolução. 

Correções são transferidas e instaladas, tal como os pacotes de atualização completa regular para pilha do Azure. No entanto, ao contrário de uma atualização completa, podem instalar correções em minutos. Recomendamos que os operadores do Azure pilha definir janelas de manutenção ao instalar as correções. Correções atualizar a versão da sua nuvem de pilha do Azure, pelo que pode facilmente determinar se a correção foi aplicada. É fornecida uma correção separada para cada versão de pilha do Azure que ainda está a ser suporte. Cada correção para uma iteração específica é cumulativa e inclui as atualizações anteriores para essa mesma versão. Pode ler mais sobre a aplicabilidade de uma correção específicos num correções correspondente a Base de dados de conhecimento artigo.  


## <a name="keep-your-system-under-support"></a>Manter o seu sistema de suporte
Para continuar a receber suporte, tem de manter a implementação de pilha do Azure atual. A política de diferimento por atualizações é: para a implementação de pilha do Azure que deverá permanecer disponível no suporte, tem de executar a versão de atualização mais recentemente lançada ou executar qualquer uma das duas versões de atualização anterior. Correções não são consideradas versões de atualização principal. Se a nuvem de pilha do Azure se encontrar atrás por *mais de duas atualizações*, que é considerado em conformidade e tem de atualizar para, pelo menos, a versão mínima suportada para receber suporte. 

Por exemplo, se a versão de atualização mais recentemente disponível é 1805 e dois pacotes de atualização anteriores foram versões 1804 e 1803, 1803 e 1804 permanecem no suporte. No entanto, 1802 está fora do suporte. A política se aplica quando não existe nenhuma versão de um mês ou dois. Por exemplo, se a versão atual é 1805 e não ocorreu nenhuma versão 1804, os pacotes de atualização de dois anterior de 1803 e 1802 permanecem suporte.

Pacotes de atualização de software Microsoft são não cumulativa e necessitam que o pacote de atualização anteriores como pré-requisito. Se decidir diferir atualizações de um ou mais, considere o tempo de execução global, se pretender obter a versão mais recente. 

## <a name="get-support"></a>Obter suporte
Pilha do Azure segue o mesmo processo de suporte do Azure. Os clientes empresariais podem seguir o processo descrito no [como criar um pedido de suporte do Azure](/azure/azure-supportability/how-to-create-azure-support-request). Se for um cliente de um fornecedor de serviço em nuvem (CSP), contacte o seu CSP para obter suporte.  Para obter mais informações, consulte o [perguntas frequentes do suporte do Azure](https://azure.microsoft.com/support/faq/). 


## <a name="next-steps"></a>Passos Seguintes

- [Gerir as atualizações na pilha do Azure](azure-stack-updates.md)


