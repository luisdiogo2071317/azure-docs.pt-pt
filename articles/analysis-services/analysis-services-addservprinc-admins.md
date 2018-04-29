---
title: Adicionar um principal de serviço à função de administrador do servidor de Analysis Services do Azure | Microsoft Docs
description: Saiba como adicionar um principal de serviço de automatização para a função de administrador do servidor
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f1cc563cc13a9102dbdac7bd505b4dd844ff8247
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Adicionar um principal de serviço à função de administrador do servidor 

 Para automatizar as tarefas do PowerShell autónomas, tem de ter um principal de serviço **administrador do servidor** privilégios no servidor do Analysis Services que está a ser geridos. Este artigo descreve como adicionar um principal de serviço para a função de administradores de servidor num servidor AS do Azure.

## <a name="before-you-begin"></a>Antes de começar
Antes de concluir esta tarefa, tem de ter um principal de serviço registado no Azure Active Directory.

[Criar serviço principal - portal do Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Criar o principal de serviço - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Permissões obrigatórias
Para concluir esta tarefa, tem de ter [administrador do servidor](analysis-services-server-admins.md) permissões no servidor AS do Azure. 

## <a name="add-service-principal-to-server-administrators-role"></a>Adicionar o principal de serviço à função de administradores de servidor

1. No SSMS, ligue ao servidor AS do Azure.
2. No **propriedades do servidor** > **segurança**, clique em **adicionar**.
3. No **selecionar um utilizador ou grupo**, procure a aplicação registada por nome, selecione e, em seguida, clique em **adicionar**.

    ![Pesquisa de conta do principal de serviço](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Verifique o ID de conta do principal de serviço e, em seguida, clique em **OK**.
    
    ![Pesquisa de conta do principal de serviço](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> Para operações de servidores utilizando AzureRm cmdlets, Programador de execução principal de serviço têm também de pertencer à **proprietário** função para o recurso de [controlo de acesso em funções do Azure (RBAC)](../role-based-access-control/overview.md). 

## <a name="related-information"></a>Informações relacionadas

* [Transferir o módulo do PowerShell do SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Transferir o SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


