---
title: Compreender negar atribuições no RBAC do Azure | Documentos da Microsoft
description: Saiba mais sobre negar atribuições no controlo de acesso baseado em funções (RBAC) para recursos no Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 8ef3a2ec44c5eff80d3a50a6c56805667e164ba8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980173"
---
# <a name="understand-deny-assignments"></a>Compreender as atribuições de negação

Semelhante a uma atribuição de função, um *negar atribuição* une um conjunto de ações de negação para um utilizador, grupo ou principal de serviço num determinado âmbito para efeitos de negar o acesso. Uma atribuição de negação também pode excluir entidades de segurança e impedir a herança para âmbitos subordinados, que é diferente de atribuições de funções. Atualmente, negar atribuições são **só de leitura** e só pode ser definido pelo Azure. Este artigo descreve como negar atribuições são definidas.

## <a name="deny-assignment-properties"></a>Propriedades de atribuição de negação

 Uma atribuição de negações tem as seguintes propriedades:

> [!div class="mx-tableFixed"]
> | Propriedade | Necessário | Tipo | Descrição |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Sim | Cadeia | O nome a apresentar da atribuição de negação. Os nomes têm de ser exclusivos para um determinado âmbito. |
> | `Description` | Não | Cadeia | A descrição da atribuição de negação. |
> | `Permissions.Actions` | Ações, pelo menos, um ou um DataActions | String] | Uma matriz de cadeias de caracteres que especificam as operações de gestão aos quais a atribuição de negar bloqueia o acesso. |
> | `Permissions.NotActions` | Não | String] | Uma matriz de cadeias de caracteres que especificam as operações de gestão para excluir da atribuição de negação. |
> | `Permissions.DataActions` | Ações, pelo menos, um ou um DataActions | String] | Uma matriz de cadeias de caracteres que especificam as operações de dados aos quais a atribuição de negar bloqueia o acesso. |
> | `Permissions.NotDataActions` | Não | String] | Uma matriz de cadeias de caracteres que especificam as operações de dados para impedir que a atribuição de negação. |
> | `Scope` | Não | Cadeia | Uma cadeia de caracteres que especifica o âmbito que se aplica a atribuição de negação. |
> | `DoNotApplyToChildScopes` | Não | Booleano | Especifica se a atribuição de negar aplica-se a âmbitos subordinados. Valor predefinido é false. |
> | `Principals[i].Id` | Sim | String] | Uma matriz de IDs (utilizador, grupo ou principal de serviço) aos quais se aplica a atribuição de negação de objeto de entidade do Azure AD. Definido como um GUID vazio `00000000-0000-0000-0000-000000000000` para representar todos os utilizadores. |
> | `Principals[i].Type` | Não | String] | Uma matriz de tipos de objeto representado pelo Direi principais [i]. Definido como `Everyone` para representar todos os utilizadores. |
> | `ExcludePrincipals[i].Id` | Não | String] | Uma matriz de IDs (utilizador, grupo ou principal de serviço) para o qual não se aplica a atribuição de negação de objeto de entidade do Azure AD. |
> | `ExcludePrincipals[i].Type` | Não | String] | Uma matriz de tipos de objeto representado pelo Direi ExcludePrincipals [i]. |
> | `IsSystemProtected` | Não | Booleano | Especifica se negar atribuição foi criada pelo Azure e não pode ser editada ou eliminada. Atualmente, todos negar atribuições são protegido do sistema. |

## <a name="everyone-principal"></a>Todos os utilizadores principal

Para suporte negar atribuições, a principal todas as pessoas foi introduzida. O principal everyone representa todos os utilizadores, grupos e principais de serviço no diretório do Azure AD. Se o ID de principal é um zero GUID `00000000-0000-0000-0000-000000000000` e o tipo de principal é `Everyone`, o principal representa todas as pessoas. Todos principal pode ser combinado com `ExcludePrincipals` para negar todos, exceto alguns usuários. O principal todos têm as seguintes restrições:

- Pode ser utilizada apenas num `Principals` e não é possível utilizar `ExcludePrincipals`.
- `Principals[i].Type` tem de ser definido como `Everyone`.

## <a name="next-steps"></a>Passos Seguintes

* [Lista negar atribuições com o RBAC e a API REST](deny-assignments-rest.md)
* [Compreender as definições de função](role-definitions.md)
