---
title: Gerir administradores de servidor no Azure Analysis Services | Documentos da Microsoft
description: Saiba como gerenciar administradores de servidor para um servidor de Analysis Services no Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 00a4de5f463133054ff4821cce74b3dc859a07c2
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603571"
---
# <a name="manage-server-administrators"></a>Gerir administradores de servidor

Os administradores de servidor tem de ser um grupo de segurança ou de utilizador válido no Azure Active Directory (Azure AD) para o inquilino no qual reside o servidor. Pode usar **administradores do Analysis Services** para o seu servidor no portal do Azure, propriedades do servidor no SSMS, PowerShell ou REST API para gerir os administradores de servidor. 

> [!NOTE]
> Grupos de segurança tem de ter o `MailEnabled` definida como `True`.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Para adicionar os administradores de servidor utilizando o portal do Azure

1. No portal, do servidor, clique em **administradores do Analysis Services**.
2. Na  **\<servername >-administradores do Analysis Services**, clique em **Add**.
3. Na **adicionar administradores de servidor**, selecione as contas de utilizador do seu Azure AD ou convidar utilizadores externos por endereço de e-mail.

    ![Administradores de servidor no portal do Azure](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>Para adicionar os administradores de servidor com o SSMS

1. Com o botão direito do servidor > **propriedades**.
2. Na **propriedades do servidor de Analysis**, clique em **segurança**.
3. Clique em **adicionar**e, em seguida, introduza o endereço de e-mail para um utilizador ou grupo no seu Azure AD.
   
    ![Adicionar administradores de servidor no SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="powershell"></a>PowerShell

Uso [New-AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) cmdlet para especificar o parâmetro de administrador ao criar um novo servidor. <br>
Uso [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver) cmdlet para modificar o parâmetro de administrador para um servidor existente.

## <a name="rest-api"></a>API REST

Uso [criar](https://docs.microsoft.com/rest/api/analysisservices/servers/create) para especificar a propriedade asAdministrator ao criar um novo servidor. <br>
Uso [atualização](https://docs.microsoft.com/rest/api/analysisservices/servers/update) para especificar a propriedade asAdministrator quando modificar um servidor existente. <br>



## <a name="next-steps"></a>Passos Seguintes 

[Autenticação e permissões de utilizador](analysis-services-manage-users.md)  
[Gerir funções de base de dados e utilizadores](analysis-services-database-users.md)  
[Controlo de Acesso Baseado em Funções](../role-based-access-control/overview.md)  

