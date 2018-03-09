---
title: "Adicionar um principal de serviço à função de administrador do servidor de Analysis Services do Azure | Microsoft Docs"
description: "Saiba como adicionar um principal de serviço de automatização para a função de administrador do servidor"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: owend
ms.openlocfilehash: 8e51b80e184b2b1ff24b1051b55088fbc54c271c
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="add-a-service-principle-to-the-server-administrator-role"></a>Adicionar um principal de serviço à função de administrador do servidor 

 Para automatizar as tarefas do PowerShell autónomas, tem de ter um principal de serviço **administrador do servidor** privilégios no servidor do Analysis Services que está a ser geridos. Este artigo descreve como adicionar um principal de serviço para a função de administradores de servidor num servidor AS do Azure.

## <a name="before-you-begin"></a>Antes de começar
Antes de concluir esta tarefa, tem de ter um principal de serviço registado no Azure Active Directory.

[Criar o principal de serviço - portal do Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Criar principal de serviço - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Permissões obrigatórias
Para concluir esta tarefa, tem de ter [administrador do servidor](analysis-services-server-admins.md) permissões no servidor AS do Azure. 

## <a name="add-service-principle-to-server-administrators-role"></a>Adicionar o princípio de serviço à função de administradores de servidor

1. No SSMS, ligue ao servidor AS do Azure.
2. No **propriedades do servidor** > **segurança**, clique em **adicionar**.
3. No **selecionar um utilizador ou grupo**, procure a aplicação registada por nome, selecione e, em seguida, clique em **adicionar**.

    ![Pesquisa de conta do principal de serviço](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Verifique o ID de conta do principal de serviço e, em seguida, clique em **OK**.
    
    ![Pesquisa de conta do principal de serviço](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> Para operações de servidores utilizando AzureRm cmdlets, princípio de serviço com o programador tem também de pertencer à **proprietário** função para o recurso de [controlo de acesso em funções do Azure (RBAC)](../active-directory/role-based-access-control-what-is.md). 

## <a name="related-information"></a>Informações relacionadas

* [Transferir o módulo do PowerShell do SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Transferir o SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


