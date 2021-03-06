---
title: incluir ficheiro
description: incluir ficheiro
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b4b06119b9d46781b967fc8d98808c60d2b41ccb
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38753671"
---
Deve confirmar que os utilizadores na sua organização possuem o nível adequado de acesso a estes recursos. Não vai querer conceder acesso ilimitado aos utilizadores, mas também precisa de confirmar que estes podem fazer o seu trabalho. Controlo de acesso baseado em funções (RBAC) permite-lhe gerir os utilizadores que tem permissão para concluir as ações específicas a um âmbito. Uma função define um conjunto de ações permitidas. Atribuir a função a um âmbito e especificar os utilizadores que pertencem a essa função para o âmbito.

Ao planejar sua estratégia de controlo de acesso, conceda aos utilizadores o menor privilégio para fazer o seu trabalho. A imagem seguinte mostra um padrão sugerido para atribuir o RBAC.

![Âmbito](./media/resource-manager-governance-rbac/role-examples.png)

Existem três funções que se aplicam a todos os recursos - proprietário, Contribuidor e leitor. Todas as contas atribuídas à função de proprietário devem ser controladas rigorosamente e raramente utilizadas. Os utilizadores que só tem de observar o estado das soluções devem ser concedidos a função de leitor.

A maioria dos usuários são concedidos [funções de recursos específicos](../articles/role-based-access-control/built-in-roles.md) ou [funções personalizadas](../articles/role-based-access-control/custom-roles.md) ao nível da subscrição ou o recurso do grupo. Estas funções definem rigidamente as ações permitidas. Ao atribuir utilizadores a estas funções, pode conceder o acesso necessário para os utilizadores sem permitir muito controle. Pode atribuir uma conta a mais de uma função e que o utilizador obtém as permissões combinadas das funções. Conceder acesso ao nível do recurso, muitas vezes, é restritiva demais para os utilizadores, mas pode funcionar para um processo automatizado, concebido para a tarefa específica.

### <a name="who-can-assign-roles"></a>Quem pode atribuir funções

Para criar e remover atribuições de funções, os utilizadores devem ter acesso `Microsoft.Authorization/roleAssignments/*`. Este acesso é concedido através das funções Proprietário ou Administrador de Acesso dos Utilizadores.