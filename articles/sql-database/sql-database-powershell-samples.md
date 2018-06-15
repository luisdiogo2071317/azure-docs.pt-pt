---
title: Exemplos de scripts do Azure PowerShell para a Base de Dados SQL | Microsoft Docs
description: Exemplos de scripts do Azure PowerShell para ajudar a criar e gerir servidores de Base de Dados SQL do Azure, conjuntos elásticos, bases de dados e firewalls.
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: craigg
editor: tysonn
tags: azure-service-management
ms.service: sql-database
ms.custom: overview-samples, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 2245964cab3361e0f298cf35d26e8b0a1b8456f0
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30924124"
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Exemplos do Azure PowerShell para a Base de Dados SQL do Azure

A tabela seguinte inclui ligações para scripts de exemplo do Azure PowerShell para a Base de Dados SQL do Azure.

| |  |
|---|---|
|**Criar uma base de dados e um conjunto elástico**||
| [Criar uma base de dados e configurar uma regra de firewall](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Este script do PowerShell cria uma base de dados SQL do Azure e configura uma regra de firewall ao nível do servidor. |
| [Criar conjuntos elásticos e mover bases de dados agrupadas](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Este script do PowerShell cria conjuntos elásticos da Base de Dados SQL do Azure, move bases de dados agrupadas e altera os níveis de desempenho.|
|**Configurar georreplicação e ativação pós-falha**||
| [Configurar e efetuar a ativação pós-falha de uma base de dados através de georreplicação ativa](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Este script do PowerShell configura a georreplicação ativa para uma base de dados SQL do Azure e realiza a ativação pós-falha para uma réplica secundária. |
| [Configurar e efetuar a ativação pós-falha de uma base de dados agrupada através de georreplicação ativa](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Este script do PowerShell configura a georreplicação ativa para uma base de dados SQL do Azure num conjunto elástico SQL e realiza a ativação pós-falha para uma réplica secundária. |
| [Configurar e efetuar a ativação pós-falha de um grupo de ativação pós-falha para uma base de dados (pré-visualização)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Este script do PowerShell configura um grupo de ativação pós-falha para uma instância de servidor da Base de Dados SQL do Azure, adiciona uma base de dados ao grupo de ativação pós-falha e efetua a ativação pós-falha para o servidor secundário |
|**Dimensionar uma base de dados e um conjunto elástico**||
| [Dimensionar uma base de dados](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Este script do PowerShell monitoriza as métricas de desempenho de uma base de dados SQL do Azure, dimensiona-a para um nível de desempenho superior e cria uma regra de alerta numa das métricas de desempenho. |
| [Dimensionar um conjunto elástico](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Este script do PowerShell monitoriza as métricas de desempenho de um conjunto elástico da Base de Dados SQL do Azure, dimensiona-o para um nível de desempenho superior e cria uma regra de alerta numa das métricas de desempenho.  |
| **Auditoria e deteção de ameaças** |
| [Configurar a auditoria e a deteção de ameaças](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Este script do PowerShell configura as políticas de auditoria e deteção de ameaças para uma base de dados SQL do Azure. |
| **Restaurar, copiar e importar uma base de dados**||
| [Restaurar uma base de dados](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Este script do PowerShell restaura uma base de dados SQL do Azure a partir de uma cópia de segurança georredundante e restaura uma base de dados SQL do Azure eliminada para a cópia de segurança mais recente. |
| [Copiar uma base de dados para um novo servidor](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Este script do PowerShell cria uma cópia de uma base de dados SQL do Azure existente num novo servidor SQL do Azure. |
| [Importar uma base de dados de um ficheiro bacpac](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Este script do PowerShell importa uma base de dados para um servidor SQL do Azure a partir de um ficheiro bacpac. |
| **Sincronizar dados entre bases de dados**||
| [Sincronizar dados entre bases de dados SQL](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Este script do PowerShell configura a Sincronização de Dados entre várias bases de dados SQL do Azure. |
| [Sincronizar dados entre a Base de Dados SQL e o SQL Server no local](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Este script do PowerShell configura a Sincronização de Dados entre uma base de dados SQL do Azure e uma base de dados do SQL Server no local. |
| [Atualizar o esquema de Sincronização de Dados SQL](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Este script do PowerShell adiciona ou remove itens do esquema de Sincronização de Dados. |
|||
