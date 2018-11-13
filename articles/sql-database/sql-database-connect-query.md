---
title: Inícios rápidos de Ligação e Consulta da Base de Dados SQL do Azure | Microsoft Docs
description: Inícios rápidos da Base de Dados SQL do Azure que mostram como ligar e consultar uma base de dados SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: 01f1ac19cbab5ec60034b75fb15ccdb45df8541e
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913313"
---
# <a name="quickstarts-azure-sql-database-connect-and-query"></a>Inícios Rápidos: Ligação e Consulta da Base de Dados SQL do Azure

O seguinte documento inclui ligações para exemplos do Azure que mostram como ligar e consultar uma base de dados SQL do Azure. Fornece também algumas recomendações para Segurança a Nível de Transporte.

## <a name="quickstarts"></a>Inícios rápidos

| |  |
|---|---|
|[SQL Server Management Studio](sql-database-connect-query-ssms.md)|Este guia de introdução demonstra como utilizar o SSMS para ligar a uma base de dados SQL do Azure e, em seguida, utilizar as declarações de Transact-SQL para consultar, inserir, atualizar e eliminar dados na base de dados.|
|[Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)|Este guia de início rápido demonstra como utilizar o Azure Data Studio para ligar a uma base de dados SQL do Azure e, em seguida, utilizar instruções Transact-SQL (T-SQL) para criar o TutorialDB utilizado nos tutoriais do Azure Data Studio.|
|[Portal do Azure](sql-database-connect-query-portal.md)|Este início rápido demonstra como utilizar o Editor de consultas para ligar a uma base de dados SQL e, em seguida, utilizar as instruções de Transact-SQL para consultar, inserir, atualizar e eliminar dados na base de dados.|
|[Visual Studio Code](sql-database-connect-query-vscode.md)|Este guia de introdução demonstra como utilizar o Visual Studio Code para ligar a uma base de dados SQL do Azure e, em seguida, utilizar as declarações de Transact-SQL para consultar, inserir, atualizar e eliminar dados na base de dados.|
|[.NET com o Visual Studio](sql-database-connect-query-dotnet-visual-studio.md)|Este início rápido demonstra como utilizar o .NET Framework para criar um programa C# com o Visual Studio para ligar a uma base de dados SQL do Azure e como utilizar instruções Transact-SQL para consultar dados.|
|[.NET Core](sql-database-connect-query-dotnet-core.md)|Este início rápido demonstra como utilizar o .NET Core em Windows/Linux/macOS para criar um programa C# para ligar a uma base de dados SQL do Azure e como utilizar instruções Transact-SQL para consultar dados.|
|[Go](sql-database-connect-query-go.md)|Este início rápido explica como utilizar o Go para ligar a uma base de dados SQL do Azure. As declarações Transact-SQL para consultar e modificar dados também são explicadas.|
|[Java](sql-database-connect-query-java.md)|Este início rápido demonstra como utilizar o Java para ligar a uma base de dados SQL do Azure e, em seguida, utilizar as instruções Transact-SQL para consultar dados.|
|[Node.js](sql-database-connect-query-nodejs.md)|Este início rápido demonstra como utilizar o Node.js para criar um programa para ligar a uma base de dados SQL do Azure e como utilizar instruções Transact-SQL para consultar dados.|
|[PHP](sql-database-connect-query-php.md)|Este início rápido demonstra como utilizar o PHP para criar um programa para ligar a uma base de dados SQL do Azure e como utilizar instruções Transact-SQL para consultar dados.|
|[Python](sql-database-connect-query-python.md)|Este início rápido demonstra como utilizar o Python para ligar a uma base de dados SQL do Azure e utilizar as instruções Transact-SQL para consultar dados. |
|[Ruby](sql-database-connect-query-ruby.md)|Este início rápido demonstra como utilizar o Ruby para criar um programa para ligar a uma base de dados SQL do Azure e como utilizar instruções Transact-SQL para consultar dados.|
|||

## <a name="tls-considerations-for-sql-database-connectivity"></a>Considerações de TLS para conectividade de Base de Dados SQL
A Transport Layer Security (TLS) é utilizada por todos os controladores que a Microsoft fornece ou suporta para ligar à Base de Dados SQL do Azure. Não é necessária qualquer configuração especial. Para todas as ligações ao SQL Server ou à Base de Dados SQL do Azure, recomendamos que todas as aplicações definam as seguintes configurações ou os respetivos equivalentes:

 - **Encriptar = Ativado**
 - **TrustServerCertificate = Desativado**

Alguns sistemas utilizam palavras-chave diferentes mas equivalentes para as palavras-chave de configuração. Estas configurações garantem que o controlador do cliente verifica a identidade do certificado TLS recebido do servidor.

Recomendamos também que desative a TLS 1.1 e 1.0 no cliente, se tiver de estar em conformidade com a Data Security Standard (DSS) da Payment Card Industry (PCI).

Os controladores que não são da Microsoft podem não utilizar o TLS por predefinição. Isto poderá ser um fator ao ligar à Base de Dados SQL do Azure. As aplicações com controladores incorporados podem não permitir que controle estas definições de ligação. Recomendamos que analise a segurança de tais aplicações e controladores antes de os utilizar em sistemas que interajam com dados confidenciais.

## <a name="next-steps"></a>Passos seguintes

Para obter informações sobre a arquitetura de conectividade, veja [Arquitetura de Conectividade de Base de Dados SQL do Azure](sql-database-connectivity-architecture.md).