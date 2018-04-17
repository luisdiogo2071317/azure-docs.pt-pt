---
title: Gerir serviços de análise do Azure com o PowerShell | Microsoft Docs
description: Gestão de Analysis Services do Azure com o PowerShell.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: reference
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c7315835bca446c4cae592f4bdd58a733b203655
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Gerir serviços de análise do Azure com o PowerShell

Este artigo descreve os cmdlets do PowerShell utilizados para executar o servidor de Analysis Services do Azure e tarefas de gestão de base de dados. 

Tarefas de gestão de servidor, tais como criar ou eliminar um servidor, suspender ou retomar as operações do servidor ou alterar o nível de serviço (camada) utilizam cmdlets do Azure Resource Manager (AzureRM). Outras tarefas para gerir bases de dados, tais como adicionar ou remover membros da função, processamento, ou criação de partições utilizar cmdlets incluídos no mesmo módulo SqlServer como SQL Server Analysis Services.

## <a name="permissions"></a>Permissões
A maioria das tarefas de PowerShell necessitam de que ter privilégios de administrador no servidor do Analysis Services que está a gerir. Tarefas agendadas do PowerShell são operações autónomas. A conta que executa o programador tem de ter privilégios de administrador no servidor do Analysis Services. 

Para operações de servidores utilizando AzureRm cmdlets, a conta ou a conta que executa o programador tem também de pertencer à função de proprietário para o recurso no [controlo de acesso em funções do Azure (RBAC)](../role-based-access-control/overview.md). 

## <a name="server-operations"></a>Operações do servidor 
Cmdlets de Analysis Services do Azure estão incluídos no [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) módulo de componente. Para instalar os módulos do cmdlet AzureRM, consulte [cmdlets do Azure Resource Manager](/powershell/azure/overview) na galeria do PowerShell.

|Cmdlet|Descrição| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azurerm.analysisservices/add-azureanalysisservicesaccount)|Adiciona uma conta a utilizar para pedidos de cmdlet do Azure Analysis Services server autenticada.| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Obtém os detalhes de uma instância de servidor.|  
|[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Cria uma instância de servidor.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Remove uma instância de servidor.|  
|[Restart-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Reinicia uma instância do servidor do Analysis Services no ambiente atualmente com sessão iniciado; especificados no comando Adicionar AzureAnalysisServicesAccount.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Retoma uma instância de servidor.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Suspende uma instância de servidor.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Modifica uma instância de servidor.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Testa a existência de uma instância de servidor.| 

## <a name="database-operations"></a>Operações de base de dados

Operações de base de dados de Analysis Services do Azure utilizam o mesmo [SqlServer](https://www.powershellgallery.com/packages/SqlServer) módulo como SQL Server Analysis Services. No entanto, nem todos os cmdlets são suportados para o Azure Analysis Services. 

O módulo de SqlServer fornece cmdlets de gestão de base de dados específicas de tarefas, bem como o cmdlet Invoke-ASCmd para fins gerais que aceita uma consulta da linguagem de Scripting de modelo em tabela (TMSL) ou script. Os seguintes cmdlets no módulo SqlServer são suportados para o Azure Analysis Services.

  
|Cmdlet|Descrição|
|------------|-----------------| 
|[Add-RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Adicione um membro a uma função de base de dados.| 
|[Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|Cópia de segurança uma base de dados do Analysis Services.|  
|[Remove-RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Remova um membro de uma função de base de dados.|   
|[Invoke-ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Execute um script TMSL.|
|[Invoke-ProcessASDatabase](https://msdn.microsoft.com/library/mt651773.aspx)|Processar uma base de dados.|  
|[ProcessPartition invocar](https://msdn.microsoft.com/library/hh510164.aspx)|Processar uma partição.| 
|[Invoke-ProcessTable](https://msdn.microsoft.com/library/mt651774.aspx)|Processo de uma tabela.|  
|[Merge-Partition](https://msdn.microsoft.com/library/hh479576.aspx)|Uma partição de intercalação.|  
|[Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|Restaure uma base de dados do Analysis Services.| 
  

## <a name="related-information"></a>Informações relacionadas

* [Transferir o módulo do PowerShell do SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Transferir o SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [Módulo de SqlServer na galeria do PowerShell](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabela modelo de programação para 1200 de nível de compatibilidade e superiores](https://msdn.microsoft.com/library/mt712541.aspx)
