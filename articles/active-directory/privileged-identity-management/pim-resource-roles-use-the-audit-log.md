---
title: Auditar funções dos recursos para recursos do Azure através de Privileged Identity Management | Microsoft Docs
description: Explica como obter uma vista de todas as atividades de função para a um recursos específico.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 211b8c69a1462f7efdcb4002269d96d1d5cf2ae6
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233789"
---
# <a name="audit-resource-roles-for-azure-resources-by-using-privileged-identity-management"></a>Funções de recursos de auditoria para recursos do Azure utilizando o Privileged Identity Management 

Auditoria de recurso dá-lhe uma vista de todas as atividades de função para o recurso. Pode filtrar as informações utilizando uma data predefinida ou um intervalo personalizado.
![Informações de filtro](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Auditoria de recursos também fornece acesso rápido aos detalhes de atividade do utilizador. Em **auditoria tipo**, selecione **ativar**. Selecione **(atividade)** para ver as ações desse utilizador nos recursos do Azure.
![Detalhe de atividade](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Mais detalhe de atividade](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>A minha auditoria

A minha auditoria dá-lhe uma vista de actividade de função pessoais do utilizador. Pode filtrar as informações utilizando uma data predefinida ou um intervalo personalizado.
![Actividade de função pessoal](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Ver a ativação e a atividade de recursos do Azure

Para ver as ações que um utilizador específico demorou em vários recursos, pode rever a atividade de recursos do Azure que está associada a um período de ativação fornecido. Comece por selecionar um utilizador do **membros** vista ou a partir da lista de membros de uma função específica. O resultado mostra uma visualização gráfica de ações do utilizador em recursos do Azure por data. Também mostra as ativações de função recente esse mesmo período de tempo.

![Detalhes do utilizador](media/azure-pim-resource-rbac/rbac-user-details.png)

Selecionar a ativação de uma função específica mostra os detalhes de ativação de função e atividade correspondente do recurso do Azure que ocorreram durante a Active Directory que o utilizador.

![Selecione a ativação da função](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

