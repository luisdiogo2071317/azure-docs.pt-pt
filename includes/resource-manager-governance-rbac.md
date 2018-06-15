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
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31433254"
---
Deve certificar-se de que os utilizadores na sua organização tiverem o nível adequado de acesso a estes recursos. Não pretende conceder acesso ilimitado a utilizadores, mas terá também de certificar-se de que podem fazer o seu trabalho. O controlo de acesso baseado em funções (RBAC) permite-lhe gerir os utilizadores que têm permissão para concluir as ações específicas a um âmbito. Uma função define um conjunto de ações permitidas. Pode atribuir a função a um âmbito e especificar os utilizadores que pertencem a essa função para o âmbito.

Quando planear a estratégia de controlo de acesso, conceda aos utilizadores o menor privilégio para fazer o seu trabalho. A imagem seguinte mostra um padrão para atribuição de RBAC sugerido.

![Âmbito](./media/resource-manager-governance-rbac/role-examples.png)

Existem três funções que se aplicam a todos os recursos - proprietário, leitor e contribuinte. As contas atribuídas à função de proprietário devem ser controladas rigorosamente e raramente utilizadas. Os utilizadores que apenas tem de observar o estado das soluções deverão ser concedidos a função de leitor.

São concedidas a maior parte dos utilizadores [funções específicas do recurso](../articles/role-based-access-control/built-in-roles.md) ou [funções personalizadas](../articles/role-based-access-control/custom-roles.md) ao nível do grupo a subscrição ou recurso. Estas funções totalmente definem as ações permitidas. Ao atribuir utilizadores a estas funções, pode conceder o acesso necessário para os utilizadores sem que permite a demasiada controlo. Pode atribuir uma conta a mais do que uma função, e que o utilizador obtém as permissões das funções combinadas. Conceder acesso ao nível do recurso, muitas vezes, é demasiado restritivo para os utilizadores, mas pode funcionar para um processo automatizado foi concebido para tarefa específica.

### <a name="who-can-assign-roles"></a>Quem pode atribuir funções

Para criar e remover atribuições de funções, os utilizadores devem ter `Microsoft.Authorization/roleAssignments/*` acesso. Este acesso é concedido através de funções de proprietário ou administrador de acesso de utilizador.