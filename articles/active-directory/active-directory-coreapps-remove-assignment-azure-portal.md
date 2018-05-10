---
title: Remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial no Azure Active Directory | Microsoft Docs
description: Como remover a atribuição de acesso de um utilizador ou grupo a partir de uma aplicação empresarial no Azure Active Directory
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: b3c5c6c4bc1cfd5a9b68277e21fc06f37fa18f3b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial no Azure Active Directory
É fácil remover um utilizador ou um grupo de que está a ser atribuído o acesso a uma das suas aplicações da empresa no Azure Active Directory (Azure AD). Tem de ter as permissões adequadas para gerir a aplicação da empresa e tem de ser administrador global do diretório.

> [!NOTE]
> Para Applications da Microsoft (tais como aplicações do Office 365), utilize o PowerShell para remover utilizadores de uma aplicação da empresa.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Como remover um utilizador ou a atribuição de grupo para uma aplicação da empresa no portal do Azure?
1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2. Selecione **mais serviços**, introduza **do Azure Active Directory** na caixa de texto e, em seguida, selecione **Enter**.
3. No **Azure Active Directory - *directoryname***  página (ou seja, a Azure AD página para o diretório que está a gerir), selecione **aplicações empresariais**.

    ![Aplicações da empresa ao abrir](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. No **aplicações empresariais** página, selecione **todas as aplicações**. Verá uma lista de aplicações que pode gerir.
5. No **aplicações da empresa - todas as aplicações** página, selecione uma aplicação.
6. No ***appname*** página (ou seja, a página com o nome da aplicação selecionada no título), selecione **utilizadores e grupos**.

    ![Selecionar utilizadores ou grupos](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. No ***appname*** **-atribuição de grupos de & utilizador** página, selecione uma das mais utilizadores ou grupos e, em seguida, selecione o **remover** comando. Confirme a sua decisão de linha.

    ![Selecionar o comando Remove](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Como remover um utilizador ou a atribuição de grupo para uma aplicação da empresa através do PowerShell?
1. Abra uma linha de comandos elevada do Windows PowerShell.

    >[!NOTE] 
    > Tem de instalar o módulo de AzureAD (utilize o comando `Install-Module -Name AzureAD`). Se lhe for pedido para instalar um módulo NuGet ou do novo módulo do Azure Active Directory V2 PowerShell, escreva Y e prima ENTER.

2. Executar `Connect-AzureAD` e inicie sessão com uma conta de utilizador de Administrador Global.
3. Utilize o seguinte script para atribuir um utilizador e a função a uma aplicação:

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ``` 
## <a name="next-steps"></a>Passos Seguintes

- [Ver todos os meus grupos](active-directory-groups-view-azure-portal.md)
- [Atribuir um utilizador ou grupo a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)
- [Desativar o utilizador inícios de sessão para uma aplicação empresarial](active-directory-coreapps-disable-app-azure-portal.md)
- [Alterar o nome ou logótipo de uma aplicação empresarial](active-directory-coreapps-change-app-logo-user-azure-portal.md)
