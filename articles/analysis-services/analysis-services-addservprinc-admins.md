---
title: Adicionar um principal de serviço à função de administrador de servidor Azure Analysis Services | Documentos da Microsoft
description: Saiba como adicionar um principal de serviço de automatização para a função de administrador do servidor
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d6d276895534b3fcfb7f58f8fa51620c65c600a3
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427659"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Adicionar um principal de serviço para a função de administrador do servidor 

 Para automatizar autónomas tarefas do PowerShell, tem de ter um principal de serviço **administrador do servidor** privilégios no servidor do Analysis Services a ser gerido. Este artigo descreve como adicionar um principal de serviço para a função de administradores de servidor num servidor do Azure.

## <a name="before-you-begin"></a>Antes de começar
Antes de concluir esta tarefa, tem de ter um principal de serviço registado no Azure Active Directory.

[Criar principal de serviço - portal do Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Criar principal de serviço - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Permissões obrigatórias
Para concluir essa tarefa, tem de ter [administrador do servidor](analysis-services-server-admins.md) permissões ao servidor do Azure. 

## <a name="add-service-principal-to-server-administrators-role"></a>Adicionar principal de serviço à função de administradores de servidor

1. No SSMS, ligue ao seu servidor do Azure.
2. Na **propriedades do servidor** > **segurança**, clique em **adicionar**.
3. Na **selecionar um utilizador ou grupo**, procure a aplicação registada por nome, selecione e, em seguida, clique em **Add**.

    ![Pesquisar por conta do principal de serviço](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Verifique se o ID de conta do principal de serviço e, em seguida, clique em **OK**.
    
    ![Pesquisar por conta do principal de serviço](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> Para operações de servidor com os cmdlets do AzureRm, o agendador em execução principal de serviço também deve pertencer aos **proprietário** função para o recurso na [controlo de acesso de controlo (RBAC)](../role-based-access-control/overview.md). 

## <a name="related-information"></a>Informações relacionadas

* [Transferir o módulo de PowerShell do SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Baixe o SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


