---
title: Gerir administradores de servidor no Azure Analysis Services | Microsoft Docs
description: Saiba como gerir administradores de servidor para um servidor de Analysis Services no Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 59aaf07edf387d40f47d7d82dcb116802a003dc1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34596173"
---
# <a name="manage-server-administrators"></a>Gerir administradores de servidor
Os administradores de servidores tem de ser um utilizador ou grupo válido no Azure Active Directory (Azure AD) para o inquilino em que reside o servidor. Pode utilizar **administradores de serviços de análise** para o servidor no portal do Azure ou propriedades do servidor no SSMS para gerir os administradores de servidores. 

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Para adicionar os administradores de servidores utilizando o portal do Azure
1. No portal, para o servidor, clique em **administradores de serviços de análise**.
2. No  **\<servername >-administradores de serviços de análise**, clique em **adicionar**.
3. No **adicionar administradores de servidor**, selecione as contas de utilizador do seu Azure AD ou convidar utilizadores externos ao endereço de correio eletrónico.

    ![Administradores de servidor no portal do Azure](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>Para adicionar os administradores de servidores com o SSMS
1. Clique com o botão direito do servidor > **propriedades**.
2. No **propriedades do Analysis Server**, clique em **segurança**.
3. Clique em **adicionar**e, em seguida, introduza o endereço de e-mail para um utilizador ou grupo no Azure AD.
   
    ![Adicionar administradores de servidor no SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Passos Seguintes 
[Autenticação e permissões de utilizador](analysis-services-manage-users.md)  
[Gerir utilizadores e funções de base de dados](analysis-services-database-users.md)  
[Controlo de Acesso Baseado em Funções](../role-based-access-control/overview.md)  

