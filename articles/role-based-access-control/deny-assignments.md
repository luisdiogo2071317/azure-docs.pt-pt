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
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: fa1a979c01999bd79c45d24e4c7771edaf346dd8
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632420"
---
# <a name="understand-deny-assignments"></a>Compreender as atribuições de negação

Semelhante a uma atribuição de função, um *negar atribuição* anexa um conjunto de ações de negação para um utilizador, grupo ou principal de serviço num determinado âmbito para efeitos de negar o acesso. Negar atribuições impedir que os utilizadores de executar ações específicas, mesmo se uma atribuição de função lhes concede acesso. Alguns recursos de fornecedores no Azure incluem agora negar atribuições. Atualmente, as atribuições de negação são **só de leitura** e só podem ser definidas pelo Azure.

De certa forma, negar atribuições são diferentes de atribuições de funções. Negar atribuições podem excluir entidades de segurança e impedir a herança para âmbitos subordinados. Negar atribuições também se aplicam ao [administrador de subscrição clássica](rbac-and-directory-admin-roles.md) atribuições.

Este artigo descreve como negar atribuições são definidas.

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
> | `Principals[i].Id` | Sim | String] | Uma matriz de IDs (utilizador, grupo, principal de serviço ou identidade gerida) aos quais se aplica a atribuição de negação de objeto de entidade do Azure AD. Definido como um GUID vazio `00000000-0000-0000-0000-000000000000` para representar todos os principais. |
> | `Principals[i].Type` | Não | String] | Uma matriz de tipos de objeto representado pelo Direi principais [i]. Definido como `SystemDefined` para representar todos os principais. |
> | `ExcludePrincipals[i].Id` | Não | String] | Uma matriz de IDs (utilizador, grupo, principal de serviço ou identidade gerida) para o qual não se aplica a atribuição de negação de objeto de entidade do Azure AD. |
> | `ExcludePrincipals[i].Type` | Não | String] | Uma matriz de tipos de objeto representado pelo Direi ExcludePrincipals [i]. |
> | `IsSystemProtected` | Não | Booleano | Especifica se negar atribuição foi criada pelo Azure e não pode ser editada ou eliminada. Atualmente, todos negar atribuições são protegido do sistema. |

## <a name="system-defined-principal"></a>Principal de definidos pelo sistema

Para suporte negar atribuições, o **System-Defined Principal** foi introduzido. Este principal de representa todos os utilizadores, grupos, os principais de serviço e identidades geridas num diretório do Azure AD. Se o ID de principal é um zero GUID `00000000-0000-0000-0000-000000000000` e o tipo de principal é `SystemDefined`, o principal representa todas as entidades. `SystemDefined` pode ser combinado com `ExcludePrincipals` para negar a todos os principais, exceto alguns usuários. `SystemDefined` tem as seguintes restrições:

- Pode ser utilizada apenas num `Principals` e não é possível utilizar `ExcludePrincipals`.
- `Principals[i].Type` tem de ser definido como `SystemDefined`.

## <a name="next-steps"></a>Passos Seguintes

* [Lista negar atribuições com o RBAC e a API REST](deny-assignments-rest.md)
* [Compreender as definições de função](role-definitions.md)
