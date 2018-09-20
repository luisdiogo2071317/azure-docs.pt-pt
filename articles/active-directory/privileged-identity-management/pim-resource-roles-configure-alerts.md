---
title: Configurar alertas de segurança para funções de recursos do Azure no PIM | Documentos da Microsoft
description: Saiba como configurar alertas de segurança para funções de recursos do Azure no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 5d990d86124a7360dbc0398cf4250f9b088c183b
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465576"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Configurar alertas de segurança para funções de recursos do Azure no PIM
Privileged Identity Management (PIM) para recursos do Azure gera alertas quando houver atividade suspeita ou não segura no seu ambiente. Quando for acionado um alerta, este aparece na página de alertas. 

![Página de alertas](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Rever alertas
Selecione um alerta para ver um relatório que lista os utilizadores ou funções que acionou o alerta, juntamente com conselhos de remediação.

![Relatório de alerta](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Alertas
| Alerta | Gravidade | Acionador | Recomendação |
| --- | --- | --- | --- |
| **Existem demasiados proprietários atribuídos a um recurso** |Médio |Muitos utilizadores têm a função de proprietário. |Reveja os utilizadores na lista e reatribuir alguns para funções com menos privilégios. |
| **Existem demasiados proprietários permanentes atribuídos a um recurso** |Médio |Demasiados utilizadores permanentemente são atribuídos a uma função. |Reveja os utilizadores na lista e atribua novamente à requer ativação para utilização de função. |
| **Função duplicada criada** |Médio |Várias funções têm os mesmos critérios. |Utilize apenas uma destas funções. |


### <a name="severity"></a>Gravidade
* **Alta**: requer uma ação imediata devido a uma violação de política. 
* **Médio**: não requerer ação imediata, mas sinaliza uma potencial violação de política.
* **Baixa**: não requerer ação imediata, mas sugere uma alteração de política preferencial.

## <a name="configure-security-alert-settings"></a>Configurar definições de alerta de segurança
A partir da página de alertas, aceda a **definições**.
![Definições](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Personalize as configurações nos alertas diferentes para trabalhar com o seu ambiente e os objetivos de segurança.
![Personalizar as definições](media/azure-pim-resource-rbac/rbac-alert-settings.png)

## <a name="next-steps"></a>Passos Seguintes

- [Configurar alertas de segurança para funções de recursos do Azure no PIM](pim-resource-roles-configure-alerts.md)
