---
title: "Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory | Microsoft Docs"
description: "Como selecionar uma aplicação empresarial para atribuir um utilizador ou grupo ao mesmo no Azure Active Directory"
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: daveba
ms.reviewer: luleon
ms.openlocfilehash: 827d5c8669eb54630adbaba0306e73b5de6a7137
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory
Para atribuir um utilizador ou grupo a uma aplicação da empresa, tem de ter as permissões adequadas para gerir a aplicação da empresa e tem de ser administrador global do diretório.
> [!NOTE]
> Para Applications da Microsoft (tais como aplicações do Office 365), utilize o PowerShell para atribuir utilizadores a uma aplicação da empresa.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app-in-the-azure-portal"></a>Como atribuir acesso de utilizador para uma aplicação da empresa no portal do Azure?
1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2. Selecione **mais serviços**, introduza Azure Active Directory na caixa de texto e, em seguida, selecione **Enter**.
3. No **Azure Active Directory - *directoryname***  painel (ou seja, o Azure AD painel para o diretório que está a gerir), selecione **aplicações empresariais**.

    ![Aplicações da empresa ao abrir](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. No **aplicações empresariais** painel, selecione **todas as aplicações**. Isto apresenta uma lista de aplicações que pode gerir.
5. No **aplicações da empresa - todas as aplicações** painel, selecione uma aplicação.
6. No ***appname*** painel (ou seja, o painel com o nome da aplicação selecionada no título), selecione **utilizadores e grupos**.

    ![Selecionar o todos os comandos de aplicações](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. No ***appname*** **-atribuição de grupos de & utilizador** painel, selecione o **adicionar** comando.
8. No **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![Atribuir um utilizador ou grupo para a aplicação](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. No **utilizadores e grupos** painel, selecione um ou mais utilizadores ou grupos na lista e, em seguida, selecione o **selecione** na parte inferior do painel.
10. No **adicionar atribuição** painel, selecione **função**. Em seguida, no **selecionar função** painel, selecione uma função para aplicar a utilizadores ou grupos selecionados e, em seguida, selecione o **OK** na parte inferior do painel.
11. No **adicionar atribuição** painel, selecione o **atribuir** na parte inferior do painel. Os utilizadores atribuídos ou grupos tem as permissões definidas pela função selecionada para esta aplicação empresarial.

## <a name="how-do-i-assign-a-user-to-an-enterprise-app-using-powershell"></a>Como atribuir um utilizador para uma aplicação da empresa através do PowerShell?

1. Abra uma linha de comandos elevada do Windows PowerShell.

    >[!NOTE] 
    > Tem de instalar o módulo de AzureAD (utilize o comando `Install-Module -Name AzureAD`). Se lhe for pedido para instalar um módulo NuGet ou do novo módulo do Azure Active Directory V2 PowerShell, escreva Y e prima ENTER.

2. Executar `Connect-AzureAD` e inicie sessão com uma conta de utilizador de Administrador Global.
3. Utilize o seguinte script para atribuir um utilizador e a função a uma aplicação:

    ```powershell
    # Assign the values to the variables
    $username = "<You user's UPN>"
    $app_name = "<Your App's display name>"
    $app_role_name = "<App role display name>"
    
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```     

Para obter mais informações sobre como atribuir um utilizador a uma função de aplicação, visite a documentação para [AzureADUserAppRoleAssignment novo](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0)

### <a name="example"></a>Exemplo

Neste exemplo atribui o utilizador Britta Simon para o [Microsoft à área de trabalho Analytics](https://products.office.com/en-us/business/workplace-analytics) aplicação através do PowerShell.

1. No PowerShell, atribua os valores correspondentes para as variáveis $username, $APP_NAME>.azurewebsites.NET e $app_role_name. 

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

2. Neste exemplo, não sabemos qual é o nome da função de aplicação que pretende atribuir a Britta Simon exato. Execute os seguintes comandos para obter o utilizador ($user) e nomes a apresentar o principal de serviço ($sp) utilizando o UPN do utilizador e o principal de serviço.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```
        
3. Execute o comando `$sp.AppRoles` para apresentar as funções disponíveis para a aplicação de análise da área de trabalho. Neste exemplo, queremos de atribuir Britta Simon a função do analista (acesso limitado).
    
    ![Função de análise da área de trabalho](media/active-directory-coreapps-assign-user-azure-portal/workplace-analytics-role.png)

4. Atribuir o nome de função para o `$app_role_name` variável.
        
    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    ```

5. Execute o seguinte comando para atribuir o utilizador à função de aplicação:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="next-steps"></a>Passos seguintes
* [Ver todos os meus grupos](active-directory-groups-view-azure-portal.md)
* [Remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Desativar o utilizador inícios de sessão para uma aplicação empresarial](active-directory-coreapps-disable-app-azure-portal.md)
* [Alterar o nome ou logótipo de uma aplicação empresarial](active-directory-coreapps-change-app-logo-user-azure-portal.md)
