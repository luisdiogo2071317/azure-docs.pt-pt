---
title: Privileged Identity Management para recursos do Azure - atribuir funções | Microsoft Docs
description: Descreve como atribuir funções no PIM.
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
ms.openlocfilehash: 9a9046afe2ee1e578333ff9d29f6fb21e95a0f22
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---assign"></a>Atribuir Privileged Identity Management - funções dos recursos-

## <a name="assign-roles"></a>Atribuir funções

Para atribuir um utilizador ou grupo a uma função, selecione a função (se visualizar funções) 

![](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

ou clique em Adicionar na barra de ação (se na vista de membros).

![](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Se adicionar um utilizador ou grupo a partir do separador Membros, terá: 

1. Escolha uma função de menu Adicionar para poder selecionar um utilizador ou grupo.

![](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Escolha um utilizador ou grupo a partir do diretório.

3. Escolha o tipo de atribuição adequada no menu de lista pendente. 

    - **Apenas na atribuição de tempo:** fornece os membros do grupo ou utilizador com acesso elegível, mas não persistente para a função para um determinado período de tempo ou indefinidamente (se configurada nas definições de função). 
    - **A atribuição direta:** que não é necessário que os membros de utilizador ou grupo ativar a atribuição de função (conhecida como acesso persistente). A Microsoft recomenda utilizando a atribuição direta para utilização de curto prazo, como no chamada desvia ou atividades confidenciais do tempo, em que não irá ser necessário acesso quando a tarefa estiver concluída.

Uma caixa de verificação abaixo da lista pendente Tipo de atribuição permite-lhe especificar se a atribuição de deve ser permanente (permanentemente elegível para ativar imediatamente no tempo atribuição/permanentemente Active Directory para a atribuição direta).

![](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

>[!NOTE]
>A caixa de verificação pode ser unmodifiable se outro administrador tiver especificado a duração máxima de atribuição para cada tipo de atribuição nas definições de função.

 Para especificar uma duração de atribuição específico, desmarque a caixa de verificação e modificar o início e/ou campos de data e hora de fim.

![](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Gerir atribuições de funções

Os administradores podem gerir atribuições de função, selecionando funções ou membros do painel de navegação esquerdo. Selecionar funções permite aos administradores para definir o âmbito das respetivas tarefas de gestão para uma função específica, enquanto membros apresenta todas as atribuições de função de utilizador e grupo para o recurso.

![](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Se tiver uma função pendentes de ativação, uma faixa de notificação é apresentada na parte superior da página, ao visualizar associação.


## <a name="modify-existing-assignments"></a>Altere as atribuições existentes

Para modificar atribuições existentes da vista de detalhes de utilizador/grupo, selecione a alterar as definições na barra de ação na parte superior da página. Altere o tipo de atribuição para apenas na hora de atribuição ou atribuição direta.

![](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)
