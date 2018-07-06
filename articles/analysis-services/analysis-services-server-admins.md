---
title: Gerir administradores de servidor no Azure Analysis Services | Documentos da Microsoft
description: Saiba como gerenciar administradores de servidor para um servidor de Analysis Services no Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9d8f74bd66fc7c980c4fc5f83492aad7d8a4aa5c
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866972"
---
# <a name="manage-server-administrators"></a>Gerir administradores de servidor
Os administradores de servidor tem de ser um grupo de segurança ou de utilizador válido no Azure Active Directory (Azure AD) para o inquilino no qual reside o servidor. Pode usar **administradores do Analysis Services** para o seu servidor no portal do Azure, ou como propriedades do servidor no SSMS para gerir os administradores de servidor. 

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

## <a name="next-steps"></a>Passos Seguintes 
[Autenticação e permissões de utilizador](analysis-services-manage-users.md)  
[Gerir funções de base de dados e utilizadores](analysis-services-database-users.md)  
[Controlo de Acesso Baseado em Funções](../role-based-access-control/overview.md)  

