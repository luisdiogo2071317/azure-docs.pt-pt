---
title: "Auditoria no armazém de dados SQL do Azure | Microsoft Docs"
description: "Introdução ao Azure SQL Data Warehouse de auditoria"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 0e6af148-b218-4b43-bb5f-907917d20330
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 01/16/2018
ms.author: rortloff;barbkess
ms.openlocfilehash: 5400f29d8c7579809ef7b2a084115473df7baa85
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Auditoria no armazém de dados SQL do Azure

Auditoria de SQL Data Warehouse permite-lhe para registo de eventos na base de dados para uma auditoria iniciar sessão na sua conta do Storage do Azure. Auditoria pode ajudar a manter a conformidade de regulamentação, compreender a atividade de base de dados e obter informações sobre discrepâncias e anomalias que poderão indicar preocupações para a empresa ou suspeitas de violação de segurança. Auditoria de SQL Data Warehouse também integra-se com o Microsoft Power BI para análise e relatórios.

As ferramentas de auditoria ativarem e facilitam a conformidade com as normas de conformidade, mas não garantir a compatibilidade. Para obter mais informações sobre o Azure programas que conformidade de padrões de suporte, consulte o <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Centro de fidedignidade do Azure</a>.

## <a id="subheading-1"></a>Noções básicas de auditorias
Auditoria de base de dados do SQL Data Warehouse permite-lhe:

* **Manter** um registo de auditoria de eventos selecionados. Pode definir categorias de ações de base de dados a serem auditados.
* **Relatório** na atividade de base de dados. Pode utilizar relatórios pré-configurados e um dashboard para começar a trabalhar rapidamente com atividade e o relatório de eventos.
* **Analisar** relatórios. Pode encontrar eventos suspeitos, atividade invulgar e tendências.

Pode configurar a auditoria para as seguintes categorias de evento:

**SQL simples** e **parametrizadas SQL** para o qual os registos de auditoria recolhidos são classificados como  

* **Acesso a dados**
* **Alterações do esquema (DDL)**
* **Alterações de dados (DML)**
* **Contas, funções e permissões (DCL)**
* **Procedimento armazenado**, **início de sessão** e, **transação gestão**.

Para cada categoria de evento de auditoria de **êxito** e **falha** operações configuradas em separado.

Para obter mais informações sobre os eventos auditados e atividades, consulte o <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">referência de formatos de registo de auditoria (transferência de ficheiro do documento)</a>.

Os registos de auditoria são armazenados na sua conta do storage do Azure. Pode definir um período de retenção do registo de auditoria.

Pode definir uma política de auditoria para uma base de dados específica ou como uma política de servidor de predefinição. Uma política de auditoria do servidor predefinido aplica-se a todas as bases de dados num servidor que não dispõe de uma substituição da base de dados auditoria política específica definida.

Antes de configurar a auditoria de auditoria de verificação se estiver a utilizar um ["Clientes de nível inferior."](sql-data-warehouse-auditing-downlevel-clients.md)

## <a id="subheading-2"></a>Configurar a auditoria da base de dados
1. Inicie o <a href="https://portal.azure.com" target="_blank">portal do Azure</a>.
2. Aceda a **definições** para o armazém de dados do SQL Server que pretende de auditoria. Selecione **deteção de ameaças e auditoria**.
   
    ![][1]
3. Em seguida, ativar a auditoria clicando a **ON** botão.
   
    ![][3]
4. No painel de configuração de auditoria, selecione **detalhes armazenamento** para abrir o painel de armazenamento de registos de auditoria. Selecione a conta de armazenamento do Azure para os registos e o período de retenção. 
>[!TIP]
>Utilize a mesma conta de armazenamento para todas as bases de dados auditadas para tirar o máximo partido de modelos de relatórios pré-configurados.
   
    ![][4]
5. Clique em de **OK** botão para guardar a configuração de detalhes de armazenamento.
6. Em **registo pelo evento**, clique em **êxito** e **falha** para registar todos os eventos ou selecione as categorias de eventos individuais.
7. Se estiver a configurar auditoria para uma base de dados, poderá ter de alterar a cadeia de ligação do seu cliente para garantir a auditoria de dados é capturado corretamente. Verifique o [modificar FDQN de servidor na cadeia de ligação](sql-data-warehouse-auditing-downlevel-clients.md) tópico para ligações de cliente de nível inferior.
8. Clique em **OK**.

## <a id="subheading-3"></a>Analisar registos de auditoria e relatórios
Os registos de auditoria são agregados de uma coleção de tabelas de arquivo com um **SQLDBAuditLogs** prefixo na conta do storage do Azure que escolheu durante a configuração. Pode ver ficheiros de registo utilizando uma ferramenta como <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Explorador de armazenamento do Azure</a>.

Um modelo de relatório do dashboard pré-configurada está disponível como uma <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">transferível folha de cálculo do Excel</a> para ajudar a analisar rapidamente os dados de registo. Para utilizar o modelo nos seus registos de auditoria, terá de Excel 2013 ou posterior e o Power Query, que pode transferir <a href="http://www.microsoft.com/download/details.aspx?id=39379">aqui</a>.

O modelo tem dados de exemplo fictícias no mesmo e, pode configurar o Power Query para importar o registo de auditoria diretamente a partir da sua conta do storage do Azure.

## <a id="subheading-4"></a>Regeneração da chave de armazenamento
Na produção, que é provável que atualize periodicamente as chaves de armazenamento. Ao atualizar as suas chaves, terá de guardar a política. O processo é o seguinte:

1. Na auditoria de painel de configuração, o que é descrito a auditoria secção de configuração anterior, altere o **chave de acesso de armazenamento** de *primário* para *secundário* e  **GUARDAR**.

   ![][4]
2. Aceda ao painel de configuração de armazenamento e **regenerar** o *chave de acesso primária*.
3. Volte ao painel Configuração de auditoria 
4. mudar o **chave de acesso de armazenamento** de *secundário* para *primário* e prima **guardar**.
4. Volte ao armazenamento da IU e **regenerar** o *chave de acesso secundária* (como preparação para a próxima chaves atualizar ciclo.

## <a id="subheading-5"></a>Automatização (do PowerShell/REST API)
Também pode configurar a auditoria no Azure SQL Data Warehouse, utilizando as ferramentas de automatização seguintes:

* **Cmdlets do PowerShell**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy)
   * [Get-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy)
   * [Remove-AzureRMSqlDatabaseAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing)
   * [Remove-AzureRMSqlServerAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing)
   * [Set-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy)
   * [Set-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy)
   * [Use-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy)


## <a name="downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Suportem de clientes de nível inferior para auditoria e dados dinâmicos máscara
Auditoria funciona com os clientes de SQL que suportam redirecionamento de TDS.

Qualquer cliente que implementa TDS 7.4 também deve suportar redirecionamento. Exceções a este incluem JDBC 4.0 em que a funcionalidade de redirecionamento não é totalmente suportada e Tedious para Node.JS no qual redirecionamento não foi implementada.

"Em clientes" que suportam TDS versão 7.3 e abaixo, modifique o FQDN do servidor na cadeia de ligação da seguinte forma:

- Original FQDN do servidor na cadeia de ligação: <*nome do servidor*>. database.windows.net
- Modificação FQDN do servidor na cadeia de ligação: <*nome do servidor*> .database. **segura**. windows.net

Uma lista parcial de "Clientes de nível inferior" inclui:

* O .NET 4.0 e abaixo
* ODBC 10.0 e abaixo.
* JDBC (enquanto JDBC suporta TDS 7.4, a funcionalidade de redirecionamento do TDS não é totalmente suportada)
* Tedious (para Node.JS)

**Remark:** o modificação do FDQN de servidor anterior poderá ser útil também aplicar uma política de auditoria de nível de servidor de SQL, sem necessidade de uma configuração passo em cada base de dados (mitigação temporária).     




<!--Anchors-->
[Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


