---
title: Privileged Identity Management para recursos do Azure - alertas de segurança | Microsoft Docs
description: Descreve os alertas de segurança do PIM.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 86c9a0f12b2598ffbd02810a11622b13b0363a1f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---alerts"></a>Privileged Identity Management - funções dos recursos - alertas
PIM para recursos do Azure gera alertas quando existe atividade suspeita ou insegura no seu ambiente. Quando um alerta é acionado, aparece a página de alertas. 

![](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Rever os alertas
Selecione um alerta para ver um relatório que lista os utilizadores ou funções que acionou o alerta, bem como conselhos remediação.
![](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Alertas
| Alerta | Gravidade | Acionador | Recomendação |
| --- | --- | --- | --- |
| **Demasiados proprietários atribuídos a um recurso** |Médio |Demasiados utilizadores tem a função de proprietário. |Reveja os utilizadores na lista e voltar a atribuir algumas funções com menos privilégios. |
| **Demasiados proprietários permanentes atribuídos a um recurso** |Médio |Demasiados utilizadores permanentemente são atribuídos a uma função. |Reveja os utilizadores na lista e atribuir algumas para requerer a ativação de função, utilize novamente. |
| **Função duplicada criada** |Médio |Várias funções tem dos mesmos critérios. |Utilize apenas uma dessas funções. |


### <a name="severity"></a>Gravidade
* **Elevada**: necessita de uma ação imediata devido a uma violação de política. 
* **Média**: não requerem ação imediata, mas sinalizar uma potencial violação de política.
* **Baixa**: não requerem ação imediata, mas sugere uma alteração de política preferrable.

## <a name="configure-security-alert-settings"></a>Configurar definições de alerta de segurança
A página de alertas, navegue para as definições.
![](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Personalize as definições de alertas de diferentes para trabalhar com o seu ambiente e os objetivos de segurança.
![](media/azure-pim-resource-rbac/rbac-alert-settings.png)
