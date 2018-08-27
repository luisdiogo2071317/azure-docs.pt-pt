---
title: Gerir o Azure Analysis Services com o PowerShell | Documentos da Microsoft
description: Gestão de Analysis Services do Azure com o PowerShell.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 06/25/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 6c648a9cb6b8d8dbfb60f1a5a6ebc386c57460b0
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887251"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Gerir o Azure Analysis Services com o PowerShell

Este artigo descreve os cmdlets do PowerShell utilizados para realizar tarefas de gerenciamento de banco de dados e de servidor Azure Analysis Services. 

Tarefas de gestão de servidor, como criar ou eliminar um servidor, suspender ou retomar as operações de servidor ou alterar o nível de serviço (escalão) utilizam cmdlets do Azure Resource Manager (recurso) e os cmdlets do Analysis Services (servidor). Outras tarefas para gerir bases de dados, como adicionar ou remover membros de funções, processamento ou criação de partições de utilizar os cmdlets incluídos no mesmo módulo SqlServer como SQL Server Analysis Services.

## <a name="permissions"></a>Permissões
A maioria das tarefas do PowerShell necessitam de que ter privilégios de administrador no servidor do Analysis Services que está a gerir. As tarefas agendadas do PowerShell são operações autónomas. O princípio de conta ou o serviço em execução o scheduler tem de ter privilégios de administrador no servidor do Analysis Services. 

Para operações de servidor com os cmdlets do AzureRm, sua conta ou a conta que executa o scheduler também tem de pertencer à função de proprietário para o recurso no [controlo de acesso de controlo (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Operações de gestão de recursos 
Módulo - [azurerm. Analysisservices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)

|Cmdlet|Descrição| 
|------------|-----------------| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Obtém os detalhes de uma instância de servidor.|  
|[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Cria uma instância de servidor.|   
|[Novo AzureRmAnalysisServicesFirewallConfig](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallconfig)|Cria uma nova configuração de firewall do Analysis Services.|   
|[Novo AzureRmAnalysisServicesFirewallRule](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallrule)|Cria uma nova regra de firewall do Analysis Services.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Remove uma instância de servidor.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Retoma uma instância de servidor.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Suspende uma instância de servidor.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Modifica uma instância de servidor.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Testa a existência de uma instância de servidor.| 

## <a name="server-management-operations"></a>Operações de gestão de servidor

Módulo - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|Descrição| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azure.analysisservices/add-azureanalysisservicesaccount)|Adiciona uma conta autenticada a utilizar para pedidos de cmdlet do servidor Azure Analysis Services.| 
|[AzureAnalysisServicesInstance de exportação](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|Exporta um registo de uma instância do servidor do Analysis Services no atualmente conectado no ambiente conforme especificado no comando de Add-AzureAnalysisServicesAccount|  
|[Restart-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Reinicia uma instância do servidor do Analysis Services no ambiente de atualmente com sessão iniciado; especificado no comando de Add-AzureAnalysisServicesAccount.|  
|[AzureAnalysisServicesInstance de sincronização](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Sincroniza um banco de dados especificado na instância especificada do servidor do Analysis Services para todas as instâncias de aumento horizontal de consulta no atualmente conectado no ambiente conforme especificado no comando de Add-AzureAnalysisServicesAccount|  

## <a name="database-operations"></a>Operações de base de dados

Operações de base de dados do Analysis Services do Azure, utilize o mesmo [módulo SqlServer](https://www.powershellgallery.com/packages/SqlServer) como SQL Server Analysis Services. No entanto, nem todos os cmdlets são suportados para o Azure Analysis Services. Para saber mais, veja [PowerShell do SQL Server](https://docs.microsoft.com/sql/powershell/sql-server-powershell).

O módulo SqlServer fornece cmdlets de gestão de base de dados específicos a tarefas, bem como o cmdlet Invoke-Scmd para fins gerais que aceita uma consulta de linguagem de scripts de modelo em tabela (TMSL) ou um script. Os seguintes cmdlets no módulo SqlServer são suportados para o Azure Analysis Services.

  
|Cmdlet|Descrição|
|------------|-----------------| 
|[Add-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Adicione um membro a uma função de base de dados.| 
|[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Cópia de segurança uma base de dados do Analysis Services.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Remova um membro de uma função de base de dados.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Execute um script TMSL.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Processe uma base de dados.|  
|[ProcessPartition invocar](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Processe uma partição.| 
|[Invoke-ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Processo de uma tabela.|  
|[Merge-Partition](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Uma partição de intercalação.|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Restaure uma base de dados do Analysis Services.| 
  

## <a name="related-information"></a>Informações relacionadas

* [Transferir o módulo de PowerShell do SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Baixe o SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [Módulo SqlServer na galeria do PowerShell](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabela modelo de programação para 1200 de nível de compatibilidade e superior](https://msdn.microsoft.com/library/mt712541.aspx)
