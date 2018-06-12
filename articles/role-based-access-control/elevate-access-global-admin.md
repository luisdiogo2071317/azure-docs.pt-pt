---
title: Elevar o acesso de Administrador Global no Azure Active Directory | Microsoft Docs
description: Descreve como elevar o acesso de Administrador Global no Azure Active Directory utilizando o portal do Azure ou a REST API.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: bagovind
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e1e46d5fb786b09a4c006b61f52b3ac99aafd555
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266510"
---
# <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Elevar o acesso de Administrador Global no Azure Active Directory

Se for um [Administrador Global](../active-directory/active-directory-assign-admin-roles-azure-portal.md#global-administrator) no Azure Active Directory (Azure AD), poderá haver horas em que pretende fazer o seguinte:

- Recuperar o acesso a uma subscrição do Azure quando um utilizador perdeu o acesso
- Conceder a outro utilizador ou por si aceder a uma subscrição do Azure
- Ver todas as subscrições do Azure numa organização
- Permitir que uma aplicação de automatização (por exemplo, uma aplicação de faturação ou auditoria) para aceder a todas as subscrições do Azure

Por predefinição, as funções de administrador do Azure AD e o acesso baseado em funções do Azure controlam efetue de funções (RBAC) do Azure AD não span e o Azure. No entanto, se for um Administrador Global no Azure AD, pode elevar o acesso para gerir as subscrições do Azure e grupos de gestão. Quando elevar o acesso, será atribuído o [administrador de acesso de utilizador](built-in-roles.md#user-access-administrator) função (uma função RBAC) em todas as subscrições para um determinado inquilino. A função de administrador de acesso de utilizador permite-lhe de conceder acesso a recursos do Azure no âmbito de raiz de outros utilizadores (`/`).

Este elevação deve ser temporário e apenas efetuada quando for necessário.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="elevate-access-for-a-global-administrator-using-the-azure-portal"></a>Elevar o acesso de Administrador Global no portal do Azure

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) ou [Centro de administração do Azure Active Directory](https://aad.portal.azure.com).

1. Na lista de navegação, clique em **do Azure Active Directory** e, em seguida, clique em **propriedades**.

   ![Propriedades do Azure AD - captura de ecrã](./media/elevate-access-global-admin/aad-properties.png)

1. Em **Administrador Global pode gerir subscrições do Azure e grupos de gestão**, defina o comutador para **Sim**.

   ![Administrador global pode gerir subscrições do Azure e grupos de gestão - captura de ecrã](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Quando configurar o comutador **Sim**, a sua conta de Administrador Global (atualmente com sessão iniciada no utilizador) está adicionada à função de administrador de acesso de utilizador no Azure RBAC no âmbito de raiz (`/`), que concede acesso para ver e o relatório no todas as subscrições Azure associadas inquilino do Azure AD.

   Quando configurar o comutador **não**, a conta de Administrador Global (atualmente com sessão iniciada no utilizador) é removida da função de administrador de acesso de utilizador no RBAC do Azure. Não é possível ver todas as subscrições do Azure que estão associadas ao inquilino do Azure AD e pode ver e gerir apenas as subscrições do Azure para que lhe foi concedido acesso.

1. Clique em **guardar** para guardar a definição.

   Esta definição não é uma propriedade global e aplica-se apenas ao utilizador atualmente com sessão iniciado.

1. Execute as tarefas que necessita para fazer o acesso elevados. Quando tiver terminado, defina o comutador para **não**.

## <a name="list-role-assignment-at-the-root-scope--using-powershell"></a>Lista de atribuição de função no âmbito de raiz (/) com o PowerShell

Para listar a atribuição de função de administrador de acesso de utilizador para um utilizador no âmbito de raiz (`/`), utilize o [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) comando.

```azurepowershell
Get-AzureRmRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
  -and $_.SignInName -eq "<username@example.com>" -and $_.Scope -eq "/"}
```

```Example
RoleAssignmentId   : /providers/Microsoft.Authorization/roleAssignments/098d572e-c1e5-43ee-84ce-8dc459c7e1f0
Scope              : /
DisplayName        : username
SignInName         : username@example.com
RoleDefinitionName : User Access Administrator
RoleDefinitionId   : 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9
ObjectId           : d65fd0e9-c185-472c-8f26-1dafa01f72cc
ObjectType         : User
```

## <a name="remove-a-role-assignment-at-the-root-scope--using-powershell"></a>Remover uma atribuição de função no âmbito de raiz (/) com o PowerShell

Para remover uma atribuição de função de administrador de acesso de utilizador para um utilizador no âmbito de raiz (`/`), utilize o [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) comando.

```azurepowershell
Remove-AzureRmRoleAssignment -SignInName <username@example.com> `
  -RoleDefinitionName "User Access Administrator" -Scope "/"
```

## <a name="elevate-access-for-a-global-administrator-using-the-rest-api"></a>Elevar o acesso de Administrador Global utilizando a API REST

Utilize os seguintes passos básicos para elevar o acesso de Administrador Global utilizando a API REST.

1. Utilizar REST, chamar `elevateAccess`, que atribui a função de administrador de acesso de utilizador no âmbito de raiz (`/`).

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Criar um [atribuição de função](/rest/api/authorization/roleassignments) atribuir qualquer função em qualquer âmbito. O exemplo seguinte mostra as propriedades para atribuir a função {em roleDefinitionID} no âmbito de raiz (`/`):

   ```json
   { 
     "properties": {
       "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionID}",
       "principalId": "{objectID}",
       "scope": "/"
     },
     "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
     "type": "Microsoft.Authorization/roleAssignments",
     "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
   }
   ```

1. Enquanto administrador de acesso de utilizador, pode também remover atribuições de função no âmbito de raiz (`/`).

1. Remova os privilégios de administrador de acesso do utilizador até que forem necessárias novamente.

## <a name="list-role-assignments-at-the-root-scope--using-the-rest-api"></a>Listar atribuições de função no âmbito de raiz (/) utilizando a API REST

Pode listar todas as atribuições de funções para um utilizador no âmbito de raiz (`/`).

- Chamar [GET roleAssignments](/rest/api/authorization/roleassignments/listforscope) onde `{objectIdOfUser}` é o ID de objeto do utilizador cujas atribuições de função que pretende obter.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

## <a name="remove-elevated-access-using-the-rest-api"></a>Remover o acesso elevado utilizando a API REST

Quando chamar `elevateAccess`, criar uma atribuição de função para si, por isso, para revogar esses privilégios tem de remover a atribuição.

1. Chamar [GET roleDefinitions](/rest/api/authorization/roledefinitions/get) onde `roleName` é igual ao administrador de acesso de utilizador para determinar o ID de nome da função de administrador de acesso de utilizador.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter=roleName+eq+'User Access Administrator'
    ```

    ```json
    {
      "value": [
        {
          "properties": {
        "roleName": "User Access Administrator",
        "type": "BuiltInRole",
        "description": "Lets you manage user access to Azure resources.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "*/read",
              "Microsoft.Authorization/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedOn": "2016-05-31T23:14:04.6964687Z",
        "createdBy": null,
        "updatedBy": null
          },
          "id": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
          "type": "Microsoft.Authorization/roleDefinitions",
          "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"
        }
      ],
      "nextLink": null
    }
    ```

    Guardar o ID do `name` parâmetro, neste caso, `18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`.

2. Também precisa de listar a atribuição de função de administrador inquilino no âmbito de inquilino. Lista todas as atribuições de âmbito de inquilino para o `principalId` do administrador do inquilino que efetuou o acesso de elevate chamada. Isto irá listar todas as atribuições de inquilino para o objectid.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Um administrador inquilino não deve ter muitos atribuições, se a consulta anterior devolve demasiadas atribuições, também pode consultar para todas as atribuições de apenas ao nível do âmbito de inquilino e filtrar os resultados: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. As chamadas anteriores devolvem uma lista de atribuições de funções. Localizar a atribuição de função em que o âmbito é `"/"` e `roleDefinitionId` termina com o ID de nome de função encontrada no passo 1 e `principalId` corresponde o objectId do administrador inquilino. 
    
    Atribuição de função de exemplo:

        ```json
        {
          "value": [
            {
              "properties": {
                "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                "principalId": "{objectID}",
                "scope": "/",
                "createdOn": "2016-08-17T19:21:16.3422480Z",
                "updatedOn": "2016-08-17T19:21:16.3422480Z",
                "createdBy": "93ce6722-3638-4222-b582-78b75c5c6d65",
                "updatedBy": "93ce6722-3638-4222-b582-78b75c5c6d65"
              },
              "id": "/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
              "type": "Microsoft.Authorization/roleAssignments",
              "name": "e7dd75bc-06f6-4e71-9014-ee96a929d099"
            }
          ],
          "nextLink": null
        }
        ```
        
    Novamente, guardar o ID do `name` parâmetro, neste caso e7dd75bc-06f6-4e71-9014-ee96a929d099.

    3. Por fim, utilize o ID de atribuição de função para remover a atribuição adicionada por `elevateAccess`:

    ```http
    DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
    ```

## <a name="next-steps"></a>Passos Seguintes

- [Controlo de acesso baseado em funções com REST](role-assignments-rest.md)
- [Gerir atribuições de acesso](role-assignments-users.md)
