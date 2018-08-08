---
title: Gerir alertas de segurança para recursos do Azure com o Privileged Identity Management | Documentos da Microsoft
description: Descreve os alertas de segurança do PIM.
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
ms.openlocfilehash: 46b19866ec3f276c8337cf9501b779701377d0a5
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620421"
---
# <a name="manage-security-alerts-for-azure-resources-by-using-privileged-identity-management"></a>Gerir alertas de segurança para recursos do Azure com o Privileged Identity Management
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
