---
title: Atribuir funções para recursos do Azure utilizando o Privileged Identity Management | Microsoft Docs
description: Descreve como atribuir funções no PIM.
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
ms.openlocfilehash: c27f5432facc34384fce72ffff26e414cbeb7d96
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699313"
---
# <a name="assign-roles-for-azure-resources-by-using-privileged-identity-management"></a>Atribuir funções para recursos do Azure utilizando o Privileged Identity Management

## <a name="assign-roles"></a>Atribuir funções

Para atribuir um utilizador ou grupo a uma função quando estiver a visualizar a **funções** painel, selecione a função e, em seguida, selecione **adicionar utilizador**. 

![Painel "Funções" com o botão "Adicionar utilizador"](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

Também pode selecionar **adicionar utilizador** do **membros** painel.

![Painel "Membros" com o botão "Adicionar utilizador"](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Se estiver a adicionar um utilizador ou grupo a partir de **membros** painel, tem de: 

1. Escolher uma função do **selecionar uma função** painel para poder selecionar um utilizador ou grupo.

   ![Painel "Selecionar uma função"](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Escolha um utilizador ou grupo a partir do diretório.

3. Selecione o tipo de atribuição adequada no menu de lista pendente: 

   - **Apenas no tempo**: disponibiliza os membros do grupo ou utilizador elegível mas não persistente acesso para a função durante um período especificado ou indefinidamente (se configurada nas definições de função). 
   - **Direto**: não necessita que os membros de utilizador ou grupo ativar a atribuição de função (conhecida como acesso persistente). Recomendamos que utilize a atribuição direta para a utilização de curto prazo, onde não irá ser necessário acesso quando a tarefa estiver concluída. Os exemplos são na chamada desvia e atividades sensíveis ao tempo.

4. Se a atribuição permanente (permanentemente elegível para uma atribuição de just-in-time ou permanentemente ativa para uma atribuição direta), selecione a caixa de verificação abaixo o **tipo de atribuição** caixa.

   ![Painel "Definições de associação" com a caixa "Tipo de atribuição" e a caixa de verificação relacionada](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

   >[!NOTE]
   >A caixa de verificação pode ser unmodifiable se outro administrador tiver especificado a duração máxima de atribuição para cada tipo de atribuição nas definições de função.

   Especifique uma duração de atribuição específico, desmarque a caixa de verificação e modificar as caixas data e hora de início e/ou de fim.

   ![Painel "Definições de associação" com as caixas de data de início, hora de início, a data de fim e a hora de fim](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Gerir atribuições de funções

Os administradores podem gerir atribuições de função, selecionando um **funções** ou **membros** no painel esquerdo. Selecionar **funções** permite aos administradores definir o âmbito das respetivas tarefas de gestão para uma função específica. Selecionar **membros** apresenta todas as atribuições de função de utilizador e grupo para o recurso.

![Painel "Funções"](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![Painel "Membros"](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Se tiver uma função pendentes de ativação, uma faixa de notificação é apresentada na parte superior do painel quando estiver a visualizar a associação.


## <a name="modify-existing-assignments"></a>Altere as atribuições existentes

Para modificar atribuições existentes da vista de detalhes de utilizador/grupo, selecione **alterar definições** na barra de ação. Alterar o tipo de atribuição para **apenas no tempo** ou **direta**.

![Painel "Detalhes de utilizador" com o botão "Alterar definições"](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)
