---
title: Adicionar uma nova conta de inquilino do Azure Stack no Azure Active Directory | Documentos da Microsoft
description: Depois de implementar o Development Kit do Microsoft Azure Stack, terá de criar a conta de utilizador, pelo menos, um inquilino para que possa explorar o portal de inquilinos.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: c312658750c1e9ef024a837ccc16e5cd5be8a5ef
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35949776"
---
# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Adicionar uma nova conta de inquilino do Azure Stack no Azure Active Directory
Após [implementar o Development Kit do Azure Stack](azure-stack-run-powershell-script.md), terá de uma conta de utilizador do inquilino para que possa explorar o portal de inquilinos e suas ofertas e planos de teste. Pode criar uma conta de inquilino por [com o portal do Azure](#create-an-azure-stack-tenant-account-using-the-azure-portal) ou pelo [com o PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Criar uma conta de inquilino do Azure Stack com o portal do Azure
Tem de ter uma subscrição do Azure para utilizar o portal do Azure.

1. Inicie sessão no [Azure](https://portal.azure.com).
2. Na barra de navegação à esquerda do Microsoft Azure, clique em **do Active Directory**.
3. Na lista de diretório, clique no diretório que pretende utilizar para o Azure Stack ou criar um novo.
4. Nesta página do diretório, clique em **utilizadores**.
5. Clique em **Adicionar utilizador**.
6. Na **adicionar utilizador** assistente, na **tipo de utilizador** lista, escolha **novo utilizador na sua organização**.
7. Na **nome de utilizador** , escreva um nome para o utilizador.
8. Na **@** caixa, escolha a entrada apropriada.
9. Clique na seta seguinte.
10. Na **perfil de utilizador** página do assistente, escreva um **FirstName**, **Apelido**, e **nome a apresentar**.
11. Na **função** lista, escolha **utilizador**.
12. Clique na seta seguinte.
13. Sobre o **palavra-passe temporária de Get** página, clique em **criar**.
14. Copiar o **nova palavra-passe**.
15. Inicie sessão no Microsoft Azure com a nova conta. Altere a palavra-passe quando lhe for pedido.
16. Inicie sessão no `https://portal.local.azurestack.external` com a nova conta para ver o portal de inquilinos.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Criar uma conta de inquilino do Azure Stack com o PowerShell
Se não tiver uma subscrição do Azure, não é possível utilizar o portal do Azure para adicionar uma conta de utilizador do inquilino. Neste caso, pode utilizar em vez disso, o módulo Azure Active Directory para Windows PowerShell.

> [!NOTE]
> Se estiver a utilizar Account Microsoft (Live ID) para implementar o Development Kit do Azure Stack, é possível utilizar o AAD PowerShell para criar a conta de inquilino. 
> 
> 

1. Instalar o [Assistente de início de sessão para os profissionais de TI RTW Microsoft Online Services](https://www.microsoft.com/en-us/download/details.aspx?id=41950).
2. Instalar o [módulo Azure Active Directory para Windows PowerShell (versão de 64 bits)](http://go.microsoft.com/fwlink/p/?linkid=236297) e abri-lo.
3. Execute os seguintes cmdlets:

    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack Development Kit

            $msolcred = get-credential

    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".

            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

    ```

1. Inicie sessão no Microsoft Azure com a nova conta. Altere a palavra-passe quando lhe for pedido.
2. Inicie sessão no `https://portal.local.azurestack.external` com a nova conta para ver o portal de inquilinos.

