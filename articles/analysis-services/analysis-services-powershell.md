---
title: Gerir serviços de análise do Azure com o PowerShell | Microsoft Docs
description: Gestão de Analysis Services do Azure com o PowerShell.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 06/25/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5c347a024af385e04bfdf3631ddcbaec89df4f40
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937369"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Gerir serviços de análise do Azure com o PowerShell

Este artigo descreve os cmdlets do PowerShell utilizados para executar o servidor de Analysis Services do Azure e tarefas de gestão de base de dados. 

Tarefas de gestão de servidor, tais como criar ou eliminar um servidor, suspender ou retomar as operações do servidor ou alterar o nível de serviço (camada) utilizam cmdlets do Azure Resource Manager (recurso) e os cmdlets do Analysis Services (servidor). Outras tarefas para gerir bases de dados, tais como adicionar ou remover membros da função, processamento, ou criação de partições utilizar cmdlets incluídos no mesmo módulo SqlServer como SQL Server Analysis Services.

## <a name="permissions"></a>Permissões
A maioria das tarefas de PowerShell necessitam de que ter privilégios de administrador no servidor do Analysis Services que está a gerir. Tarefas agendadas do PowerShell são operações autónomas. O princípio de conta ou a um serviço com o programador tem de ter privilégios de administrador no servidor do Analysis Services. 

Para operações de servidores utilizando AzureRm cmdlets, a conta ou a conta que executa o programador tem também de pertencer à função de proprietário para o recurso no [controlo de acesso em funções do Azure (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Operações de gestão de recursos 
Módulo - [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)

|Cmdlet|Descrição| 
|------------|-----------------| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Obtém os detalhes de uma instância de servidor.|  
|[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Cria uma instância de servidor.|   
|[Novo AzureRmAnalysisServicesFirewallConfig](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallconfig)|Cria uma nova configuração de firewall de Analysis Services.|   
|[Novo AzureRmAnalysisServicesFirewallRule](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallrule)|Cria uma nova regra de firewall de Analysis Services.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Remove uma instância de servidor.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Retoma uma instância de servidor.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Suspende uma instância de servidor.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Modifica uma instância de servidor.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Testa a existência de uma instância de servidor.| 

## <a name="server-management-operations"></a>Operações de gestão de servidor

Módulo - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|Descrição| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azure.analysisservices/add-azureanalysisservicesaccount)|Adiciona uma conta a utilizar para pedidos de cmdlet do Azure Analysis Services server autenticada.| 
|[Exportação AzureAnalysisServicesInstance]()|Exporta um registo a partir de uma instância do servidor de Analysis Services no atualmente com sessão iniciada ambiente conforme especificado no comando Adicionar AzureAnalysisServicesAccount|  
|[Restart-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Reinicia uma instância do servidor do Analysis Services no ambiente atualmente com sessão iniciado; especificados no comando Adicionar AzureAnalysisServicesAccount.|  
|[Sincronização AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Sincroniza a base de dados especificada na instância especificada do servidor do Analysis Services para todas as consultas scaleout instâncias a atualmente com sessão iniciada ambiente conforme especificado no comando Adicionar AzureAnalysisServicesAccount|  

## <a name="database-operations"></a>Operações de base de dados

Operações de base de dados de Analysis Services do Azure utilizam o mesmo [SqlServer módulo](https://www.powershellgallery.com/packages/SqlServer) como SQL Server Analysis Services. No entanto, nem todos os cmdlets são suportados para o Azure Analysis Services. Para saber mais, consulte [PowerShell do SQL Server](https://docs.microsoft.com/sql/powershell/sql-server-powershell).

O módulo de SqlServer fornece cmdlets de gestão de base de dados específicas de tarefas, bem como o cmdlet Invoke-ASCmd para fins gerais que aceita uma consulta da linguagem de Scripting de modelo em tabela (TMSL) ou script. Os seguintes cmdlets no módulo SqlServer são suportados para o Azure Analysis Services.

  
|Cmdlet|Descrição|
|------------|-----------------| 
|[Add-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Adicione um membro a uma função de base de dados.| 
|[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Cópia de segurança uma base de dados do Analysis Services.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Remova um membro de uma função de base de dados.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Execute um script TMSL.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Processar uma base de dados.|  
|[ProcessPartition invocar](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Processar uma partição.| 
|[Invoke-ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Processo de uma tabela.|  
|[Merge-Partition](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Uma partição de intercalação.|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Restaure uma base de dados do Analysis Services.| 
  

## <a name="related-information"></a>Informações relacionadas

* [Transferir o módulo do PowerShell do SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Transferir o SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [Módulo de SqlServer na galeria do PowerShell](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabela modelo de programação para 1200 de nível de compatibilidade e superiores](https://msdn.microsoft.com/library/mt712541.aspx)
