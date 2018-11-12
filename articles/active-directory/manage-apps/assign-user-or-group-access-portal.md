---
title: Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory | Documentos da Microsoft
description: Como selecionar uma aplicação empresarial para atribuir um utilizador ou grupo a ele no Azure Active Directory
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
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: barbkess
ms.reviewer: luleon
ms.openlocfilehash: ee0b14123e193f219e403d2608368c27f953013d
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037979"
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory
Para atribuir um utilizador ou grupo a uma aplicação empresarial, tem de ter as permissões adequadas para gerir a aplicação da empresa e tem de ser administrador global do diretório.

> [!NOTE]
> Para os requisitos para os recursos discutidos neste artigo de licenciamento, consulte a [Azure Active Directory, página de preços](https://azure.microsoft.com/pricing/details/active-directory).

> [!NOTE]
> For Microsoft Applications (por exemplo, aplicações do Office 365), utilize o PowerShell para atribuir utilizadores a uma aplicação empresarial.


## <a name="how-do-i-assign-user-access-to-an-enterprise-app-in-the-azure-portal"></a>Como atribuir acesso de utilizador para uma aplicação empresarial no portal do Azure?
1. Iniciar sessão no [Portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2. Selecione **todos os serviços**, introduza Azure Active Directory na caixa de texto e, em seguida, selecione **Enter**.
3. Selecione **aplicações empresariais**.

    ![Aplicações empresariais de abertura](./media/assign-user-or-group-access-portal/open-enterprise-apps.png)
4. Sobre o **aplicações empresariais** painel, selecione **todos os aplicativos**. Isto apresenta uma lista de aplicações que pode gerir.
5. Sobre o **aplicações empresariais - todas as aplicações** painel, selecione uma aplicação.
6. Sobre o ***appname*** painel (ou seja, o painel com o nome da aplicação selecionada no título), selecione **utilizadores e grupos**.

    ![Selecionar o todos os comandos de aplicativos](./media/assign-user-or-group-access-portal/select-app-users.png)
7. Sobre o ***appname*** **-atribuição de grupos de & utilizador** painel, selecione o **adicionar** comando.
8. Sobre o **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![Atribuir um utilizador ou grupo à aplicação](./media/assign-user-or-group-access-portal/assign-users.png)
9. Sobre o **utilizadores e grupos** painel, selecione um ou mais utilizadores ou grupos a partir da lista e, em seguida, selecione a **selecione** na parte inferior do painel.
10. Sobre o **adicionar atribuição** painel, selecione **função**. Em seguida, no **selecionar função** painel, selecione uma função para aplicar a utilizadores ou grupos selecionados e, em seguida, selecione a **OK** na parte inferior do painel.
11. Sobre o **adicionar atribuição** painel, selecione a **atribuir** na parte inferior do painel. Os utilizadores atribuídos ou grupos têm as permissões definidas pela função selecionada para esta aplicação empresarial.

## <a name="how-do-i-assign-a-user-to-an-enterprise-app-using-powershell"></a>Como atribuir um utilizador para uma aplicação empresarial com o PowerShell?

1. Abra uma linha de comandos elevada do Windows PowerShell.

    >[!NOTE] 
    > Tem de instalar o módulo AzureAD (utilize o comando `Install-Module -Name AzureAD`). Se lhe for pedido para instalar um módulo de NuGet ou o novo módulo do Azure Active Directory V2 PowerShell, digite s e pressione ENTER.

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

Para obter mais informações sobre como atribuir um utilizador a uma função de aplicação, visite a documentação para [New-AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0)

Para atribuir um grupo a uma aplicação empresarial, tem de substituir `Get-AzureADUser` com `Get-AzureADGroup`.

### <a name="example"></a>Exemplo

Este exemplo atribui ao utilizador Eduarda Almeida para o [Microsoft Workplace Analytics](https://products.office.com/business/workplace-analytics) aplicação com o PowerShell.

1. No PowerShell, atribua os valores correspondentes às variáveis $username, $app_name e $app_role_name. 

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

2. Neste exemplo, não sabemos o que é o nome exato da função de aplicação que pretende atribuir a Eduarda Almeida. Execute os seguintes comandos para obter o utilizador ($user) e o principal de serviço ($sp) com o UPN do utilizador e o principal de serviço nomes a apresentar.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```
        
3. Execute o comando `$sp.AppRoles` para apresentar as funções disponíveis para a aplicação de análise de área de trabalho. Neste exemplo, queremos atribuir a Eduarda Almeida a função de analista (acesso limitado).
    
    ![Função de análise de área de trabalho](./media/assign-user-or-group-access-portal/workplace-analytics-role.png)

4. Atribuir o nome de função para o `$app_role_name` variável.
        
    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    ```

5. Execute o seguinte comando para atribuir o utilizador à função de aplicação:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="next-steps"></a>Passos Seguintes
* [Ver todos os meus grupos](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial](remove-user-or-group-access-portal.md)
* [Desativar utilizador-inícios de sessão de uma aplicação empresarial](disable-user-sign-in-portal.md)
* [Alterar o nome ou logótipo de uma aplicação empresarial](change-name-or-logo-portal.md)
