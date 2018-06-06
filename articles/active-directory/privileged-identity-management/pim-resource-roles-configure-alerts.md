---
title: Gerir alertas de segurança de recursos do Azure utilizando o Privileged Identity Management | Microsoft Docs
description: Descreve os alertas de segurança do PIM.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 0ad4cde6a59a8867f9ff3b3cb9d9bded06e34256
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699588"
---
# <a name="manage-security-alerts-for-azure-resources-by-using-privileged-identity-management"></a>Gerir alertas de segurança de recursos do Azure utilizando o Privileged Identity Management
Privileged Identity Management (PIM) para os recursos do Azure gera alertas quando existe atividade suspeita ou insegura no seu ambiente. Quando um alerta é acionado, aparece a página de alertas. 

![Página de alertas](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Rever os alertas
Selecione um alerta para ver um relatório que lista os utilizadores ou funções que acionou o alerta, juntamente com conselhos remediação.

![Relatório de alerta](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Alertas
| Alerta | Gravidade | Acionador | Recomendação |
| --- | --- | --- | --- |
| **Demasiados proprietários atribuídos a um recurso** |Médio |Demasiados utilizadores tem a função de proprietário. |Reveja os utilizadores na lista e reatribuir algumas funções com menos privilégios. |
| **Demasiados proprietários permanentes atribuídos a um recurso** |Médio |Demasiados utilizadores permanentemente são atribuídos a uma função. |Reveja os utilizadores na lista e atribuir algumas para requerer a ativação de função, utilize novamente. |
| **Função duplicada criada** |Médio |Várias funções tem dos mesmos critérios. |Utilize apenas uma dessas funções. |


### <a name="severity"></a>Gravidade
* **Elevada**: necessita de uma ação imediata devido a uma violação de política. 
* **Média**: não requerem ação imediata, mas sinalizar uma potencial violação de política.
* **Baixa**: não requerem ação imediata, mas sugere uma alteração de política preferencial.

## <a name="configure-security-alert-settings"></a>Configurar definições de alerta de segurança
Na página alertas, aceda a **definições**.
![Definições](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Personalize as definições de alertas de diferentes para trabalhar com o seu ambiente e os objetivos de segurança.
![Personalizar as definições](media/azure-pim-resource-rbac/rbac-alert-settings.png)
