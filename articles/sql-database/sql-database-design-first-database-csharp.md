---
title: Criar a sua primeira base de dados SQL do Azure - C# | Microsoft Docs
description: Saiba como criar a sua primeira base de dados SQL do Azure e ligar a esta com um programa C# a utilizar ADO.NET.
services: sql-database
author: MightyPen
manager: craigg-msft
ms.reviewer: CarlRabeler
ms.service: sql-database
ms.custom: develop databases, mvc, devcenter
ms.topic: tutorial
ms.date: 06/07/2018
ms.openlocfilehash: 4e2238968d29bc0081a472c9c05662cb7813d866
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38697718"
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Criar uma base de dados SQL do Azure e estabelecer ligação com C&#x23; e ADO.NET

A Base de Dados SQL do Azure é uma base de dados relacional como serviço (DBaaS) no Microsoft Cloud (Azure). Neste tutorial, vai aprender a utilizar o portal do Azure e o ADO.NET com o Visual Studio para: 

> [!div class="checklist"]
> * Criar uma base de dados no portal do Azure
> * Configurar uma regra de firewall ao nível do servidor no portal do Azure
> * Ligar à base de dados com o ADO.NET e o Visual Studio
> * Criar tabelas com o ADO.NET
> * Inserir, atualizar e eliminar os dados com o ADO.NET 
> * Consultar dados com o ADO.NET

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Uma instalação do [Visual Studio Community 2017, Visual Studio Professional 2017 ou Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu tarefas de base de dados básicas, como criar uma base de dados e tabelas, carregar e consultar dados e restaurar a base de dados para um ponto anterior no tempo. Aprendeu a:
> [!div class="checklist"]
> * Criar uma base de dados
> * Configurar uma regra de firewall
> * Ligar à base de dados com o [Visual Studio e o C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Criar tabelas
> * Inserir, atualizar e eliminar dados
> * Consultar dados

Avance para o próximo tutorial para saber como migrar os seus dados.

> [!div class="nextstepaction"]
> [Migre a sua base de dados do SQL Server para a Base de Dados SQL do Azure](sql-database-migrate-your-sql-server-database.md)

