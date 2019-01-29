---
title: Início rápido - base de dados SQL do Azure | Documentos da Microsoft
description: Saiba como começar a utilizar rapidamente com o Azure SQL Database - Singleton
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: dad9f004b49ee98e6efdd7d10b779ec3bd81fa47
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/28/2019
ms.locfileid: "55105957"
---
# <a name="getting-started-with-azure-sql-database-singleton"></a>Introdução ao Azure SQL da base de dados Singleton

[SQL Database do Azure - Singleton](https://docs.microsoft.com/azure/sql-database/sql-database-single-index) ou base de dados totalmente gerido base de dados de PaaS como um serviço (DbaaS), que é o mecanismo de armazenamento ideal para as aplicações modernas na cloud. Nesta seção, aprenderá como configurar rapidamente e criar a base de dados SQL.

## <a name="quickstart-overview"></a>Descrição geral do guia de introdução

Nesta secção, irá ver uma descrição geral dos artigos disponíveis que pode ajudá-lo a começar a utilizar rapidamente com a base de dados. A maneira mais fácil para criar a sua primeira base de dados SQL é usar [do portal do Azure](sql-database-get-started-portal.md) onde pode configurar parâmetros necessários.
Após a criação, precisaria [proteger a sua base de dados através da configuração de regras de firewall](sql-database-get-started-portal-firewall.md). 

Se tiver uma base de dados no SQL Server que pretende migrar para o Azure, deve instalar [Assistente de migração de dados (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) que irá analisar seus bancos de dados no SQL Server e encontrar qualquer problema que poderia bloquear a migração para o único Base de dados. Se não encontrar algum problema, pode exportar a base de dados como `.bacpac` ficheiros e [importá-lo a utilizar o portal do Azure ou o SqlPackage](sql-database-import.md).

Estes inícios rápidos permitem-lhe configurar rapidamente, criar e importar as suas bases de dados na cloud do Azure.

## <a name="automating-management-operations"></a>Automatização de operações de gestão

O portal do Azure permite-lhe facilmente criar e modificar a sua base de dados do Azure. Como alternativa, pode utilizar [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md), ou [CLI do Azure](scripts/sql-database-create-and-configure-database-cli.md) para criar bases de dados.
Também pode atualizar seus recursos de base de dados e dimensionamento através de [PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md) ou [CLI do Azure](scripts/sql-database-monitor-and-scale-database-cli.md).

## <a name="migrating-to-azure-single-database-with-minimal-downtime"></a>Migrar a base de dados do Azure com o tempo de inatividade mínimo

Artigos nestes inícios rápidos permitem-lhe rapidamente criar ou importar a base de dados para o Azure com `.bacpac`. No entanto, `.bacpac` e `.dacpack` ficheiros foram concebidos para mover rapidamente a base de dados nas diferentes versões do SQL Server e o Azure SQL Database (base de dados ou à instância gerida) ou para implementar a integração contínua no seu pipeline de DevOps. No entanto, esse método não foi concebido para a migração de seus bancos de dados de produção com o tempo de inatividade mínimo, uma vez que terá de aguardar para exportar a base de dados de origem como `.bacpac` de ficheiro e importá-lo para o Azure SQL Database, o que poderia causar alguns períodos de inatividade do seu aplicação, especialmente se a base de dados é maior. Se estiver a mover a base de dados de produção, provavelmente terá uma melhor forma de migrar, que irá garantir que o período de indisponibilidade mínimo de migração. [Serviço de migração de dados](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json) é um serviço que pode migrar a sua base de dados com o período de indisponibilidade mínimo. Dessa forma, pode alternar rapidamente seu aplicativo de origem para a base de dados de destino com o período de indisponibilidade mínimo.

## <a name="next-steps"></a>Passos Seguintes

* Encontrar um [lista de alto nível das funcionalidades suportadas na base de dados do Azure SQL](sql-database-features.md). 
* Saiba como tornar sua [base de dados mais seguro](sql-database-security-tutorial.md). 
* Encontre mais avançadas tutoriais [como secção](sql-database-howto-single-database.md). 
* Encontre mais de exemplos de scripts escritos em [PowerShell](sql-database-powershell-samples.md) e [CLI do Azure](sql-database-cli-samples.md). 
* Saiba mais sobre o [gestão de API](sql-database-single-databases-manage.md) que pode utilizar para configurar seus bancos de dados. 
