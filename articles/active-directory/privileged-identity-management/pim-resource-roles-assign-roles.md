---
title: Atribuir funções para recursos do Azure com o Privileged Identity Management | Documentos da Microsoft
description: Descreve como atribuir funções no PIM.
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
ms.openlocfilehash: 52320fe160fc1df2997dd419b406fce24f43b786
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622981"
---
# <a name="assign-roles-for-azure-resources-by-using-privileged-identity-management"></a>Atribuir funções para recursos do Azure com o Privileged Identity Management

## <a name="assign-roles"></a>Atribuir funções

Para atribuir um utilizador ou grupo a uma função ao visualizar o **funções** painel, selecione a função e, em seguida, selecione **adicionar utilizador**. 

![Painel de "Funções" com o botão "Adicionar utilizador"](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

Também pode selecionar **adicionar utilizador** partir do **membros** painel.

![Painel "Membros" com o botão "Adicionar utilizador"](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Se estiver a adicionar um utilizador ou grupo a partir da **membros** painel, terá de: 

1. Escolher uma função a partir do **selecionar uma função** painel antes de poder selecionar um utilizador ou grupo.

   ![Painel "Selecionar uma função"](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Escolha um utilizador ou grupo a partir do diretório.

3. Escolha o tipo de atribuição apropriada no menu pendente: 

   - **Just-in-time**: fornece os membros de utilizador ou grupo com acesso de elegível mas não persistente para a função para um período específico ou indefinidamente (se configurada nas definições de função). 
   - **Direto**: não requer que os membros de utilizador ou grupo ativar a atribuição de função (conhecida como acesso persistente). Recomendamos que utilize a atribuição direta para a utilização de curto prazo, onde o acesso não ser necessário quando a tarefa estiver concluída. Os exemplos são na chamada mudanças e atividades de sensíveis ao tempo.

4. Se a atribuição permanente (permanentemente elegível para uma atribuição de just-in-time ou permanentemente ativa para uma atribuição direta), selecione a caixa de verificação abaixo a **tipo de atribuição** caixa.

   ![Painel "Definições de associação" com a caixa de "Tipo de atribuição" e a caixa de verificação relacionada](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

   >[!NOTE]
   >A caixa de verificação pode ser unmodifiable se outro administrador tiver especificado a duração máxima de atribuição para cada tipo de atribuição de definições da função.

   Especifique uma duração de atribuição específica, desmarque a caixa de verificação e modificar as caixas data e hora de início e/ou final.

   ![Painel "Definições de associação" com caixas de data de início, hora de início, data de fim e a hora de fim](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Gerir atribuições de funções

Os administradores podem gerir atribuições de funções, selecionando o **funções** ou **membros** no painel à esquerda. Selecionando **funções** permite que os administradores definir o âmbito suas tarefas de gestão a uma função específica. Selecionando **membros** apresenta todas as atribuições de função utilizador e grupo para o recurso.

![Painel "Funções"](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![Painel "Membros"](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Se tiver uma função pendentes de ativação, é apresentada uma faixa de notificação na parte superior do painel quando estiver a ver associação.


## <a name="modify-existing-assignments"></a>Modificar atribuições existentes

Para modificar atribuições de existentes a partir da vista de detalhes de utilizador/grupo, selecione **alterar definições** a partir da barra de ação. Altere o tipo de atribuição para **Just in time** ou **direto**.

![Painel "Detalhes do utilizador" com o botão "Alterar definições"](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)
