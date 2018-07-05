---
title: Auditoria de funções de recursos para recursos do Azure com o Privileged Identity Management | Documentos da Microsoft
description: Explica como obter uma vista de todas as atividades de função para a um determinado recurso.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 2740297337e1de0d041a80c7860324175413c833
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447718"
---
# <a name="audit-resource-roles-for-azure-resources-by-using-privileged-identity-management"></a>Funções de recursos de auditoria para recursos do Azure com o Privileged Identity Management 

Auditoria de recurso dá-lhe uma vista de todas as atividades de função para o recurso. Pode filtrar as informações utilizando uma data predefinida ou um intervalo personalizado.
![Informações de filtro](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Auditoria de recurso também fornece acesso rápido aos detalhes de atividade de um utilizador. Sob **tipo de auditoria**, selecione **ativar**. Selecione **(atividade)** para ver as ações do usuário em recursos do Azure.
![Detalhe da atividade](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Mais detalhes de atividade](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>A minha auditoria

A minha auditoria dá-lhe uma vista de atividade de funções pessoais de um utilizador. Pode filtrar as informações utilizando uma data predefinida ou um intervalo personalizado.
![Atividade de funções pessoais](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Ativação do modo de exibição e a atividade de recursos do Azure

Para ver as ações realizou de um utilizador específico em vários recursos, pode rever a atividade de recursos do Azure que está associada um período de ativação fornecido. Comece por selecionar um utilizador a partir da **membros** vista ou a partir da lista de membros numa função específica. O resultado mostra uma visão gráfica das ações do usuário em recursos do Azure por data. Ela também mostra as ativações de função recente no mesmo período de tempo.

![Detalhes do utilizador](media/azure-pim-resource-rbac/rbac-user-details.png)

Selecionar uma ativação de função específica mostra os detalhes de ativação de função e atividade de recursos do Azure correspondente que ocorreu enquanto que o utilizador esteve ativo.

![Selecione a ativação de função](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

