---
title: O Azure Stack a política de manutenção | Documentos da Microsoft
description: Saiba mais sobre o Azure Stack de manutenção política e como manter um sistema integrado num estado suportado.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: sethm
ms.reviewer: harik
ms.openlocfilehash: 523be365e8b66c157546470a70ddec73d3e1e5d1
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630686"
---
# <a name="azure-stack-servicing-policy"></a>O Azure Stack a política de manutenção
Este artigo descreve a política de manutenção para sistemas integrados do Azure Stack e o que deve fazer para manter seu sistema num estado suportado. 

## <a name="update-package-types"></a>Tipos de pacote de atualização

Existem dois tipos de pacotes de atualização para os sistemas integrados: 

- **Atualizações de software da Microsoft**. A Microsoft é responsável pelo ciclo de vida manutenção de ponto-a-ponto para os pacotes de atualização de software Microsoft. Esses pacotes podem incluir as mais recentes atualizações de segurança do Windows Server, não-atualizações de segurança e as atualizações de funcionalidades do Azure Stack. Pode baixar pacotes de atualização de theses diretamente da Microsoft.

- **Atualizações de fornecidos pelo fornecedor de hardware de OEM**. Parceiros de hardware do Azure Stack são responsáveis pelo ponto-a-ponto manutenção ciclo de vida (incluindo documentação de orientação) para o firmware relacionado ao hardware e os pacotes de atualização de driver. Além disso, os parceiros de hardware do Azure Stack proprietário e manter a documentação de orientação para todo o software e hardware do anfitrião de ciclo de vida do hardware. O fornecedor de hardware de OEM aloja estes pacotes em seu próprio site de download de atualização.


## <a name="update-package-release-cadence"></a>Cadência de lançamento do pacote de atualização
A Microsoft espera libertar os pacotes de atualização de software a uma cadência mensal. No entanto, é possível ter várias ou nenhuma versões de atualização num mês. Fornecedores de hardware de OEM da versão suas atualizações de forma conforme necessário. 

Encontrar documentação sobre como planear e gerir atualizações e como determinar sua versão atual na [descrição geral de atualizações de gerir](azure-stack-updates.md). Para obter informações sobre uma atualização específica, incluindo como transferi-lo, consulte as notas de versão para que atualizar: 
- [Atualização de 1808 de pilha do Azure](azure-stack-update-1808.md)
- [Atualização de 1807 de pilha do Azure](azure-stack-update-1807.md)
- [Atualização de 1805 de pilha do Azure](azure-stack-update-1805.md)


## <a name="hotfixes"></a>Correções
Ocasionalmente, a Microsoft fornece correções para o Azure Stack esse endereço de um problema específico que é muitas vezes preventiva ou sensíveis ao tempo.  Cada correção é fornecida com um artigo correspondente na Base de dados de conhecimento da Microsoft que explica em detalhe o problema, a causa e a resolução. 

Correções são transferidas e instaladas como os pacotes de atualização completa para regular para o Azure Stack. No entanto, ao contrário de uma atualização completa, podem instalar correções em minutos. Recomendamos que operadores do Azure Stack definir janelas de manutenção ao instalar as correções. Correções atualizem a versão de cloud do Azure Stack para que possa determinar facilmente se a correção foi aplicada. Uma correção separada é fornecida para cada versão do Azure Stack que ainda está em suporte. Cada correção para uma iteração específica é cumulativa e inclui as atualizações anteriores para essa mesma versão. Pode ler mais sobre a aplicabilidade de uma correção específicos num correções correspondentes a Base de dados de conhecimento artigo.  


## <a name="keep-your-system-under-support"></a>Manter seu sistema em suporte
Para continuar a receber suporte, deve manter a implementação do Azure Stack atual. A política de diferimento de atualizações é: para a sua implementação do Azure Stack permanecer no suporte, tem de executar a versão de atualização lançada recentemente ou executar qualquer uma das duas versões de atualização anterior. Correções não são consideradas versões de atualização importante. Se a cloud do Azure Stack está por trás por *mais de duas atualizações*, ele é considerado em conformidade e tem de atualizar para, pelo menos, a versão mínima suportada para receber suporte. 

Por exemplo, se a versão de atualização mais recentemente disponível é 1805 e dois pacotes de atualização anteriores foram versões 1804 e versão 1803, versão 1803 e 1804 permanecem no suporte. No entanto, 1802 está fora do suporte. A política se aplica quando não existe nenhuma versão de um mês ou dois. Por exemplo, se a versão atual é 1805 e não havia nenhuma versão 1804, os anterior dois pacotes de atualização de versão 1803 e 1802 permanecem no suporte.

Pacotes de atualização de software Microsoft são não cumulativas e necessitam que o pacote de atualização anterior como pré-requisito. Se decidir diferir atualizações de um ou mais, considere o tempo de execução geral caso queira obter para a versão mais recente. 

## <a name="get-support"></a>Obter suporte
O Azure Stack segue o mesmo processo de suporte do Azure. Os clientes empresariais podem seguir o processo descrito em [como criar um pedido de suporte do Azure](/azure/azure-supportability/how-to-create-azure-support-request). Se for um cliente de um fornecedor de serviços Cloud (CSP), contacte o seu CSP para obter suporte.  Para obter mais informações, consulte a [FAQ do suporte do Azure](https://azure.microsoft.com/support/faq/). 


## <a name="next-steps"></a>Passos Seguintes

- [Gerir atualizações no Azure Stack](azure-stack-updates.md)


