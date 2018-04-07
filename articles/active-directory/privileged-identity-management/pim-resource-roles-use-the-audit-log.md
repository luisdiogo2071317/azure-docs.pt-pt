---
title: Privileged Identity Management para recursos do Azure - auditoria recursos | Microsoft Docs
description: Explica como obter uma vista de todas as atividades de função para a um recursos específico.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: b8fa7d5600c0de8a3319ea4de785281372959937
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---audit"></a>Privileged Identity Management - funções dos recursos - auditoria

Auditoria de recurso dá-lhe uma vista de todas as atividades de função para o recurso. Pode filtrar as informações utilizando uma data predefinida ou um intervalo personalizado.
![](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Auditoria de recursos também fornece acesso rápido para ver detalhes de atividade do utilizador. Em "Tipo de auditoria" selecione "Ativar". Clique em "(atividade)" para ver as ações desse utilizador nos recursos do Azure.
![](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>A minha auditoria

A minha auditoria dá-lhe uma vista de actividade de função pessoais do utilizador. Pode filtrar as informações utilizando uma data predefinida ou um intervalo personalizado.
![](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Ver a ativação e a atividade de recursos do Azure

No caso de necessitar de ver as ações que um utilizador específico demorou em vários recursos, pode rever a atividade de recursos do Azure associada um período de activação fornecido (para os utilizadores elegíveis). Comece por selecionar um utilizador da vista de membros ou da lista de membros de uma função específica. O resultado mostra uma visualização gráfica de ações do utilizador nos recursos do Azure por data e as ativações de função recente esse mesmo período de tempo.

![](media/azure-pim-resource-rbac/rbac-user-details.png)

A ativação de uma função específica a seleção irá mostrar os detalhes de ativação de função e atividade correspondente de recursos do Azure que ocorreram durante esse utilizador Active Directory.

![](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

