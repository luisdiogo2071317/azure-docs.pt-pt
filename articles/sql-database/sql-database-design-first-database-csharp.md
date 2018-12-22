---
title: Criar a sua primeira base de dados SQL do Azure - C# | Microsoft Docs
description: Saiba como criar a sua primeira base de dados SQL do Azure e ligar a esta com um programa C# a utilizar ADO.NET.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: MightyPen
ms.author: genemi
ms.reviewer: carlrab
manager: craigg-msft
ms.date: 12/10/2018
ms.openlocfilehash: cf180f6e2970ac4435602f1cceeb98a4dd9e8724
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53727170"
---
# <a name="tutorial-design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Tutorial: Criar uma base de dados SQL do Azure e estabelecer ligação com C&#x23; e ADO.NET

Base de dados SQL do Azure é um relacional da base de dados-como-serviço (DBaaS) no Microsoft Cloud (Azure). Neste tutorial, vai aprender a utilizar o portal do Azure e o ADO.NET com o Visual Studio para:

> [!div class="checklist"]
> * Criar uma base de dados no portal do Azure
> * Configurar uma regra de firewall ao nível do servidor no portal do Azure
> * Ligar à base de dados com o ADO.NET e o Visual Studio
> * Criar tabelas com o ADO.NET
> * Inserir, atualizar e eliminar os dados com o ADO.NET
> * Consultar dados com o ADO.NET

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Uma instalação do [Visual Studio 2017](https://www.visualstudio.com/downloads/)

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]

<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu tarefas de base de dados básica, tais como criam uma base de dados e tabelas, ligar à base de dados, carregar dados e executar consultas. Aprendeu a:

> [!div class="checklist"]
> * Criar uma base de dados
> * Configurar uma regra de firewall
> * Ligar à base de dados com o [Visual Studio e o C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Criar tabelas
> * INSERT, update, delete e consultar dados

Avance para o próximo tutorial para saber mais sobre a migração de dados.

> [!div class="nextstepaction"]
> [Migrar o SQL Server para a Base de Dados SQL do Azure offline com o DMS](../dms/tutorial-sql-server-to-azure-sql.md)
